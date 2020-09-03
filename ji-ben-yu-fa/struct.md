# struct

類似於js object。

```go
type Account struct {
    id      int
    name    int
}
```

## 必須大寫

上面範例在 json marshal 後無法解析

原因為 屬性第一個字必須大寫，struct名字第一個字也要大寫。

```text
type Account struct {
    Id      int
    Name    int
}
```

## 初始化

```go
point := struct{ x, y int }{10, 20}
或是
point := new(Account)
```

## 更改值

```go
point.x = 20
point.y = 30
```

## 傳值

```go
package main

import "fmt"

type Point struct {
    X, Y int
}

func main() {
    point1 := Point{X: 10, Y: 20}
    point2 := point1

    point1.X = 20

    fmt.Println(point2) // {20, 20}
}
```

## 傳地址

如果是傳遞指，則會更改到參照的物件

```go
package main

import "fmt"

type Point struct {
    X, Y int
}

func main() {
    point1 := Point{X: 10, Y: 20}
    point2 := &point1

    point1.X = 20

    fmt.Println(*point2) // {20, 20}
}
```

## 把func 加入到 struct內

只要在func 名稱的前面加上該struct 當參數即可

```go
type Account struct {
    id      string
    name    string
    balance float64
}

func (ac *Account) Deposit(amount float64) {
    .....
}

account := &Account{"1234-5678", "Justin Lin", 1000}
account.Deposit(500)
```

## iterate struct 內的值

```go
package main

import (
	"fmt"
	"reflect"
)

type test struct {
	a int
	b int
}

func main() {

	vb := test{a: 12, b: 13}
	v := reflect.ValueOf(vb)
	for idx := 0; idx < v.NumField(); idx++ {
		keyNum := v.Field(idx)
		fmt.Println(keyNum)
	}
}

// 12
// 13
```

## 引入其他 package 的 struct

./user/schema.go

```go
package user

type Schema struct {
	ID           int    `json:"id"`
	Name         string `json:"name"`
	Age          string `json:"age"`
	Account      string `json:"account"`
	Password     string `json:"password"`
	RegisterTime int64  `json:"registerTime"`
	Gender       string `json:"gender"`
	City         string `json:"city"`
}
```

main.go

```go
import (
	USER "./user"
)

func main() {
	type User USER.Schema
	....
}	
```

[https://stackoverflow.com/a/53474756/4622645](https://stackoverflow.com/a/53474756/4622645)

