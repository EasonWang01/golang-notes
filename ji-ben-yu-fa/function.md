# Function

## 基本寫法

```go
func (繼承的方法) funcName(參數)(回傳值型態) {

}
```

> 有關funcName前面的參數可參考：[https://tour.golang.org/methods/1](https://tour.golang.org/methods/1)

## 參數傳遞方式區別（pass by pointer, pass by value）

[http://goinbigdata.com/golang-pass-by-pointer-vs-pass-by-value/](http://goinbigdata.com/golang-pass-by-pointer-vs-pass-by-value/)

### 多個 return 值

```go
package main

import "fmt"

func test(x int) (int, int) {
    return x, x + x
}

func main() {
    person, testtt := test(5)
    fmt.Printf("%v\n", person)
    fmt.Printf("%v", testtt)
}
```

### 彈性多個 argument

```go
func Sum(numbers ...int) int {
    var sum int
    for _, number := range numbers {
        sum += number
    }
    return sum
}

//  fmt.Println(Sum(1, 2, 3))       // 6
//  fmt.Println(Sum(1, 2, 3, 4))    // 10
```

> 宣告參數時用 ...，此參數本質上是個 slice，可以使用 for range 來走訪元素，可接受可變長度的參數只能有一個，而必須是最後一個參數

### 改變參數記憶體位置

```go
package main

import (
    "fmt"
)

func test(x *int) *int {
    *x += 5
    return x
}

func main() {
    apple := 5
    test(&apple)
    fmt.Printf("%v", test(&apple))
}
```

### 宣告函式

以下來宣告函式變數

```go
type testFunc func(int, int) int
或是
type testFunc = func(int, int) int
```

之後可存取記憶體位置

```go
fmt.Println(&maximum) // 0xc00000e028
```

如果是一般func要用以下存取記憶體

```go
func max(a, b int) int {
    if a > b {
    return a
    }
    return b
}
fmt.Println(max) // 0x10994c0
```

### 匿名函式

```go
package main

import "fmt"

func funcA() func(int) int {
    x := 10
    return func(n int) int {
        return x + n
    }
}

func main() {
    fmt.Println(funcA()(2)) // 12
}
```

或

```go
package main

import (
    "fmt"
)

type Int int
type FuncInt func(Int)

func (n Int) Times(f FuncInt) {
    var i Int
    for i = 0; i < n; i++ {
        f(i)
    }
}

func main() {
    var x Int = 10
    x.Times(func(n Int) {
        fmt.Println(n)
    })
}
```

## 將func當參數傳入func

以下寫一個forEach func

```go
package main

import "fmt"

func forEach(elems []int, callback func(int)) {
    for _, elem := range elems {
        callback(elem)
    }
}

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    sum := 0
    forEach(numbers, func(elem int) {
        sum += elem
    })
    fmt.Println(sum) // 15
}
```

## 不指定 argument 型態

傳入空 interface

```go
func Hello(value interface{}) {  
    switch v := value.(type) {
        case string:
            fmt.Println(v)
        case int:
            fmt.Printf("%d", v)
    }
}
```

