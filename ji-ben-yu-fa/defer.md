# defer

defer 會在外層 function return 後才執行，但會記得當下defer的狀態

> 即使發生錯誤或panic，`defer` 還是會執行

### 範例1:

```go
package main

import "fmt"

func deferredFunc() {
    fmt.Println("deferredFunc")
}

func main() {
    defer deferredFunc()
    fmt.Println("Hello, 世界")
}
```

### 範例2:

```javascript
func a() {
    i := 0
    defer fmt.Println(i)
    i++
    return
}
```

> 如上的 defer 會在 a return 後印出 0

假設是 for 迴圈的 defer 則會推入類似 stack 內，先進後出

```javascript
func b() {
    for i := 0; i < 4; i++ {
        defer fmt.Print(i)
    }
}
```

> 印出 3 2 1 0

再來試著讀取回傳值的defer

```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println(c())
}

func c() (i int) {
    defer func() { i++ }()
    return 3
}
```

> 會印出4，因為 c 的 return 為 i

## 如果多個defer會相反方向執行

```go
defer fmt.Println("1")    
defer fmt.Println("2")
// 2 1
```

## defer 會保留當下狀態

```go
package main

import "fmt"

func main() {
    i := 10
    defer fmt.Println(i)    
    i++
    fmt.Println(i) 
}
// 11 10
```

