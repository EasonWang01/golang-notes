# Sync

{% embed url="https://golang.org/pkg/sync/" %}

## WaitGroup

一般來說如果有go routine 再跑必須用 `time.Sleep()` 或 `WaitGroup` 來避免程式結束但go routine還沒跑完。

```javascript
func main() {
    wg := sync.WaitGroup{}
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go func(i int) {
            fmt.Println(i)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

有三個方法

```text
Add(), Done(), Wait()
```

WaitGroup 對象內部有一個計數器，最初從0開始，它有三個方法：Add\(\), Done\(\), Wait\(\) 用來控制計數器的數量。Add\(n\) 把計數器設置為n ，Done\(\) 每次把計數器-1 ，wait\(\) 會阻塞代碼的運行，直到計數器地值減為0

## Mutex

可以控制多個 goroutine 不會同時存取相同變數，產生 data races。

> 如果把 inc 裡面的 lock 移除，則每次產生的結果都會不同

```go
package main

import (
	"fmt"
	"sync"
)

var total int
var wg sync.WaitGroup
var lock sync.Mutex

// Inc increments the counter for the given key.
func inc(num int) {
	lock.Lock()
	defer lock.Unlock()
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

