# UnBuffered channel 與 Buffered channel

## UnBuffered channel

> unbuffered channel 意思 make 時沒有傳第二個參數
>
> 特性是在主程式內，需要等到讀或寫都完成，不然會產生錯誤
>
> 如果寫入了就必須要讀出

首先我們先來試一下以下程式

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan int)
	c <- 10
	fmt.Println(<-c)
}
```

創建一個 channel，然後傳入 10，之後再讀出。

> 會看到出現 fatal error: all goroutines are asleep - deadlock 錯誤

原因是 unBuffered channel 讀跟寫需要在不同 goroutine 才不會被 block，也就是把讀或寫放在一個goroutine環境即可解決這個問題。

所以我們調整程式為以下

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan int)
	go func() {
		c <- 10
	}()
	fmt.Println(<-c)
}
```

> 即可正常讀出 10

但這時我們替換一下順序，把go func 寫在最後，也就是讓 go routine 在程式最後面才執行

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan int)
	c <- 10
	go func() {
		fmt.Println(<-c)
	}()
}
```

> 會發現再次出現了 fatal error: all goroutines are asleep，因為 main func 在 goroutines 還沒完成前就 return 了，只有寫入但沒有讀出

但還是有解決方法：使用 sync.waitGroup 來等待 go routine

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	c := make(chan int)
	var wg sync.WaitGroup
	wg.Add(1)
	go func() {
		defer wg.Done()
		c <- 10
	}()
	wg.Add(1)
	go func() {
		defer wg.Done()
		fmt.Println(<-c)
	}()
	wg.Wait()
}
```

那接著我們在試著把讀跟寫都放到同一個 Goroutine 內

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	c := make(chan int)
	var wg sync.WaitGroup
	wg.Add(1)
	go func() {
		defer wg.Done()
		c <- 10
		fmt.Println(<-c)
	}()
	wg.Wait()
}
```

> 會發現還是出現了 fatal error: all goroutines are asleep - deadlock

原因是 UnBuffered Channel 讀寫必須在不同的 Goroutine環境下

## Buffered Channel

> 在 make 時加入第二個參數即為 Buffered Channel。
>
> 第二個參數代表 buffered channel 的容量

所以我們把剛才上面的程式 make 部分加上第二個參數，即可看到能正常執行。

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	c := make(chan int, 1)
	var wg sync.WaitGroup
	wg.Add(1)
	go func() {
		defer wg.Done()
		c <- 10
		fmt.Println(<-c)
	}()
	wg.Wait()
}
```

這時我們讀寫 channel 可以不用使用 goroutine 也能正常執行

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan int, 1)
	c <- 10
	fmt.Println(<-c)
}
```

但因為目前設定的 channel 容量只有 1 所以以下程式寫入兩次才讀出會出現錯誤

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan int, 1)
	c <- 10
	c <- 20
	fmt.Println(<-c)
}
```

所以等到寫入讀出後，再次進行寫入即可

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan int, 1)
	c <- 10
	fmt.Println(<-c)
	c <- 20
	fmt.Println(<-c)
}
```

