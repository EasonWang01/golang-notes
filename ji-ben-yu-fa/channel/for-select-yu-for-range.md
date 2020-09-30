# for select 與 for range

可以使用上面兩方法持續監聽 channel

for select

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

for range

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

