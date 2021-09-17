# 時間相關

```go
now := time.Now() // 2009-11-10 23:00:00 +0000 UTC m=+0.000000001
sec := now.Unix() // timestamp
```

## Ticker 與 Timer

ticker 類似時 setInterval 間隔持續執行，timer 類似 setTImeout 只執行一次

```go
package main
 
import (
    "fmt"
    "sync"
    "time"
)
 
func main() {
    var wg sync.WaitGroup
    wg.Add(2)
    timer1 := time.NewTimer(2 * time.Second)
    ticker1 := time.NewTicker(2 * time.Second)
 
    go func() {
        defer wg.Done()
        for {
            <-ticker1.C
            fmt.Println("get ticker1", time.Now())
        }
    }()
 
    go func() {
        defer wg.Done()
        for {
            <-timer1.C
            fmt.Println("get timer", time.Now())
            timer1.Reset(2 * time.Second)
        }
    }()
 
    wg.Wait()
}
```

間隔時間發送

```go
package main
import (
    "fmt"
    "time"
)

func main() {
    ticker := time.NewTicker(1 * time.Second)
    for range ticker.C {
      fmt.Println("test")  
    }
}
```

等同於

```go
package main
import (
    "fmt"
    "time"
)

func main() {
    ticker := time.NewTicker(1 * time.Second)
	for {
	  select {
	    case <-ticker.C:
		  fmt.Println("test")  
	  }
	}
}
```

## 產生 unix timestamp 

```go
func makeTimestamp() int64 {
    return time.Now().Round(time.Millisecond).UnixNano() / (int64(time.Millisecond)/int64(time.Nanosecond))
}
```

