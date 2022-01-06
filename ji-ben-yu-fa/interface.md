# Interface

### Empty Interface

可用來作為不指定 func 參數的方法

```go
func test(i interface{}) {
  fmt.Println(i.type)
}
```

不指定型態的 slice

```go
	var cc []interface{}

	fmt.Println(append(cc, 123, "345"))
```

## Function Interface

在 Interface 裡面只能宣告沒實作的 func，之後傳入 interface 當 func 的參數 type。

用法為：有多個不同的 struct 需要傳入當某個 func 的參數，但每個 struct 參數的屬性不同，但每個 struct 卻有實作相同的 func 名稱時可以使用。

```go
package main

import (
    "fmt"
    "math"
)

// interface for shapes:
type geometry interface {
    area() float64
    perim() float64
}

type rect struct {
    width, height float64
}

type circle struct {
    radius float64
}

// implement all methods for interface:
func (r rect) area() float64 {
    return r.width * r.height
}

func (r rect) perim() float64 {
    return 2*r.width + 2*r.height
}

func (c circle) area() float64 {
    return math.Pi * c.radius * c.radius
}

func (c circle) perim() float64 {
    return 2 * math.Pi * c.radius
}

func measure(g geometry) {
    fmt.Println(g)
    fmt.Println(g.area())
    fmt.Println(g.perim())
}

func main(){

    r := rect{width: 5, height: 10}
    c := circle{radius: 5}

    measure(r)
    measure(c)
}
```
