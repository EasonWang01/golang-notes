# Data races

{% embed url="https://golang.org/doc/articles/race\_detector.html" %}

假設我們有多個 CPU 同時執行程式，在使用 goroutine 後就容易出現此問題。

原因是多個 CPU 對相同記憶體進行操作最後導致資料不一致所產生。

詳細可見：[https://golang.org/ref/mem](https://golang.org/ref/mem)

## 範例

以下程式每次執行的結果都不同。

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

