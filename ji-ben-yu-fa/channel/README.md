# Channel 與Goroutine

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
> go func(c chan string) { c <- "ping" }(messages)
> ```

## Goroutine

```go
go func() {....}()
```

以上及為Goroutine一般的樣子，go code 執行時通常為同步的，所以加上 go 等於是 async，讓他變為非同步執行。

如果有多個Goroutine 做相同的事，放在越後面的會先執行完畢

## Channel

使用 make 創建 channel

```go
messages := make(chan string)
```

然後用 `<-`傳訊息到channel

> make 第二個參數可以放channel大小

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

## 用 func 接收多個 channel 

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

