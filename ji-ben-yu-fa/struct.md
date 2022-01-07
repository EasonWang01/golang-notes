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

```
type Account struct {
    Id      int
    Name    int
}
```

## 初始化

如果只有一個欄位最後要加逗點

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

## Struct inside Struct

```go
type testS struct {
	cc int
}

type ss struct {
	testS
	ccc int
}

func main() {
	v := ss{testS: testS{cc: 22}, ccc: 1}
	fmt.Println(v)
}
```

推薦: [https://ithelp.ithome.com.tw/articles/10227592](https://ithelp.ithome.com.tw/articles/10227592)

## 把func 加入到 struct內

有兩種方法：

#### 第一種：

直接放入 struct

```go
type ss struct {
	ccc      int
	getApple func()
}

func main() {
	v := ss{ccc: 1, getApple: func() {
		fmt.Println(22)
	}}
	fmt.Println(v)
}

```

#### 第二種：

之後寫 func ，然後將 struct 放在 func 名稱前

[https://go.dev/tour/methods/4](https://go.dev/tour/methods/4)

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

例如原本要把參數傳進去才能呼叫 test ：

```go
package main

import "fmt"

type connection struct {
	message chan string
}

var conn = connection{
	message: make(chan string),
}

func main() {
	go test(conn.message)

	msg := <-conn.message
	fmt.Println(msg)
}

func test(messages chan string) {
	messages <- "ping"
}
```

可以改為以下：讓test 變成 conn 的方法

```go
package main

import "fmt"

type connection struct {
	message chan string
}

var conn = connection{
	message: make(chan string),
}


func (c *connection) test() {
	c.message <- "ping"
}


func main() {
	go conn.test()

	msg := <-conn.message
	fmt.Println(msg)
}

```

## Func 傳值與傳址

```go
type MyStruct struct {
    Val int
}

func myfunc() MyStruct {
    return MyStruct{Val: 1}
}

func myfunc() *MyStruct {
    return &MyStruct{}
}

func myfunc(s *MyStruct) {
    s.Val = 1
}
```

> The first returns a copy of the struct, the second a pointer to the struct value created within the function, the third expects an existing struct to be passed in and overrides the value.

[https://stackoverflow.com/questions/23542989/pointers-vs-values-in-parameters-and-return-values](https://stackoverflow.com/questions/23542989/pointers-vs-values-in-parameters-and-return-values)

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

{% embed url="https://stackoverflow.com/a/53474756/4622645" %}

