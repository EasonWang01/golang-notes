# Channel 與Goroutine

channel 與 goroutine 是互相搭配的， main function 本身就是一個 goroutine，另外可以使用 go func 創造 goroutine，可以想像 goroutine 是一個異步的 function，然後搭配 channel 使用，來傳遞資料。

當 channel 有資料傳入或需要從某個 channel 變數讀出資料時，那個 channel 作用域上面的 func (該 goroutine func) 會被 block ，然後把執行環境交給另一個 goroutine，直到 channel 有進有出後才會回到該 goroutine 作用環境。

### Channel 用法

```go
package main

import (
	"fmt"
)

func main() {
	var test = make(chan int)
	go func() { test <- 123 }() // 如果傳遞值到 channel 時不在 go func 內程式會卡住
	msg := <-test               // channel 是一個地址，要賦予給一個變數後才能讀出
	fmt.Println(test)
	fmt.Println(msg)
}

// 0xc000062060
// 123
```

因為 Goroutine 是異步 async 的，所以要利用 `channel` 來傳資料

```go
package main

import "fmt"

func main() {

    messages := make(chan string)
    go func() { messages <- "ping" }()
    msg := <-messages
    fmt.Println(msg)  // ping
}
```

goroutine 部分也可加上參數寫為

> ```go
> package main
>
> import "fmt"
>
> func main() {
>
>     messages := make(chan string)
>     go func(c chan string) { c <- "ping" }(messages) 
>     // 功能僅為把原本 messages 參數名改為 c
>     msg := <-messages
>     fmt.Println(msg)  // ping
> }
> ```

## Goroutine

```go
go func() {....}()
```

以上及為Goroutine一般的樣子，go code 執行時通常為同步的，所以加上 go 等於是 async，讓他變為非同步執行。

如果有多個Goroutine 做相同的事，放在越後面的會先執行完畢

#### Goroutine 匿名 function

類似：go func() {} ()

```go
func main() {
  done := make(chan bool, 1)
  go func(c chan bool) {
    time.Sleep(50 * time.Millisecond)
    c <- true
  }(done)
  <-done
}
```

有關於是否要傳參數進去匿名函式的部分可參考：

> 差別在於是否在 go func 內可存取到正確的外部變數

[https://stackoverflow.com/a/30183893/4622645](https://stackoverflow.com/a/30183893/4622645)

## Channel

使用 make 創建 channel

```go
messages := make(chan string)
```

然後用 `<-`傳訊息到channel

> make 第二個參數可以放channel大小

使用 `var test chan int` 這樣宣告的 channel 因為初始為 nil 無法使用

### 等待 Goroutine 執行完才結束程式的三種方法

1. time.Sleep
2. sync.WaitGroup
3. 使用 channel&#x20;

以下利用 c channel 確保 `go test1` 執行後才會結束程式

```go
func test1(c chan int) {
	fmt.Println("test")
	c <- 1
}

func main() {
	c := make(chan int)
	go test1(c)
	<-c
}
```

## Channel select

可以用select 來做channel 的流程控制，select只有 channel可以用，類似於switch

```go
package main

import "time"
import "fmt"

func main() {

    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(time.Second * 1)
        c1 <- "one"
    }()
    go func() {
        time.Sleep(time.Second * 2)
        c2 <- "two"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
}
```

另外

```go
    for i := 0; i < 3; i++ {
        select {
        case a := <-aChan:
            fmt.Println(a)
        case b := <-bChan:
            fmt.Println(b)
        case c := <-cChan:
            fmt.Println(c)

        }
    }
```

```go
與 
a := <-aChan
b := <-bChan
c := <-cChan
fmt.Println(a)
fmt.Println(b)
fmt.Println(c)
相同功能
```

## 有關 channel deadlock

如果 make channel 後 沒給值 則會產生 deadlock

解法為使用 select 或是 給 make 第二個參數

{% embed url="https://www.evanlin.com/go-channels-handle/" %}

## 用 func 接收多個 channel&#x20;

```go
package main

import (
	"fmt"
)

func cc(num int, ch ...chan int) {
	for i := 0; i < len(ch); i++ {
		ch[i] <- num
	}
}

func main() {
	var test1 = make(chan int)
	var test2 = make(chan int)
	go cc(100, test1, test2)
	msg1 := <-test1
	msg2 := <-test2
	fmt.Println(msg1, msg2)
}

```

## 持續接受 channel

當我們這樣寫，程式接收一次channel 就會結束

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(100 * time.Millisecond)
    msg := <-tick
	fmt.Println(msg)
}
```

可以用 for 搭配 select 持續監聽

```go
    for {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
```

或是使用 for range

[https://gobyexample.com/range-over-channels](https://gobyexample.com/range-over-channels)

```go
package main

import "fmt"

func main() {

    queue := make(chan string, 2)
    queue <- "one"
    queue <- "two"
    close(queue)

    for elem := range queue {
        fmt.Println(elem)
    }
}
```

### 推薦閱讀：

[https://peterhpchen.github.io/2020/03/08/goroutine-and-channel.html](https://peterhpchen.github.io/2020/03/08/goroutine-and-channel.html)
