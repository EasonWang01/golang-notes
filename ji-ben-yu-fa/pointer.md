# pointer

跟 c 語言的指標相同。

> 至於何時func 的參數要用pointer可參考：
>
> [https://flaviocopes.com/golang-methods-receivers/](https://flaviocopes.com/golang-methods-receivers/)

## 傳值與傳址時機

假設我們今天有範例如下，想改變他的年紀

```go
package main

import "fmt"

type User struct {
	Name string
	Age  int
}

func (c User) GetAge() {
	fmt.Println("Age:", c.Age)
}

func UpdateAge(c User, Age int) {
	c.Age = Age
}

func main() {
	c := User{"age", 20}
	c.GetAge()
	UpdateAge(c, 21)
	c.GetAge()
}

```

但結果都是`Age: 20 Age: 20`

所以我們要用傳指標的方式，才會真正改變到原本的物件

```go
package main

import "fmt"

type User struct {
	Name string
	Age  int
}

func (c User) GetAge() {
	fmt.Println("Age:", c.Age)
}

func UpdateAge(c *User, Age int) {
	c.Age = Age
}

func main() {
	c := User{"age", 20}
	c.GetAge()
	UpdateAge(&c, 21)
	c.GetAge()
}
```

> 在 UpdateAge 第一個參數改為傳指標

## 如果有 struct 傳入 func 建議都用指標

因為耗費資源少。

[https://golang.org/doc/faq\#different\_method\_sets](https://golang.org/doc/faq#different_method_sets)

## Unsafe.pointer

[http://tsunghao.github.io/post/2015/02/manipulating-pointers-in-golang/](http://tsunghao.github.io/post/2015/02/manipulating-pointers-in-golang/)

