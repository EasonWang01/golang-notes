# Data races

{% embed url="https://golang.org/doc/articles/race\_detector.html" %}

假設我們有多個 CPU 同時執行程式，在使用 goroutine 後就容易出現此問題。

原因是多個 CPU 對相同記憶體進行操作最後導致資料不一致所產生。

詳細可見：[https://golang.org/ref/mem](https://golang.org/ref/mem)

## 範例

以下程式每次執行的結果都不同。

> 原因是後面的 goroutine 照理說應該要拿前面加好的數字再往上加，但因為concurrency的關西，前面一個拿完但來不及算好且寫回變數，這時舊的變數就被下一個goroutine拿走要進行加一，於是產生最後加起來數字變少的情況。

```go
package main

import (
	"fmt"
	"sync"
)

var total int
var wg sync.WaitGroup

// Inc increments the counter for the given key.
func inc(num int) {
	total += num
	wg.Done()
}

// Value returns the current value of the counter for the given key.
func getValue() int {
	return total
}

func main() {
	for i := 1; i <= 1000; i++ {
		wg.Add(1)
		go inc(i)
	}
	wg.Wait()
	fmt.Println(getValue())
}

```

可以試著加上 --race 來偵測。

執行： `go run --race app.go`

![](../.gitbook/assets/ying-mu-kuai-zhao-20200820-xia-wu-2.22.12.png)

## 解決方法有三個

1.runtime.GOMAXPROCS\(1\) 

> 設定 CPU 只用一顆執行程式

2.使用 `sync lock 與 unlock`

```go
package main

import (
	"fmt"
	"sync"
)

var total int
var wg sync.WaitGroup
var mu sync.Mutex

// Inc increments the counter for the given key.
func inc(num int) {
	mu.Lock()
	defer mu.Unlock()
	total += num
	wg.Done()
}

// Value returns the current value of the counter for the given key.
func getValue() int {
	return total
}

func main() {
	for i := 1; i <= 1000; i++ {
		wg.Add(1)
		go inc(i)
	}
	wg.Wait()
	fmt.Println(getValue())
}
```

3.使用 `sync/atomic`

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

var total uint64
var wg sync.WaitGroup

func inc(num uint64) {
	atomic.AddUint64(&total, num)
	wg.Done()
}

// Value returns the current value of the counter for the given key.
func getValue() uint64 {
	return atomic.LoadUint64(&total)
}

func main() {
	for i := uint64(1); i <= 1000; i++ {
		wg.Add(1)
		go inc(i)
	}
	wg.Wait()
	fmt.Println(getValue())
}

```

> Javascript 不會有 race condition 的原因是其為 single thread ，並且 worker thread 也不能直接存取到相同變數，都要透過 postMessage 溝通。
>
> [https://stackoverflow.com/questions/21467325/javascript-thread-handling-and-race-conditions](https://stackoverflow.com/questions/21467325/javascript-thread-handling-and-race-conditions)

