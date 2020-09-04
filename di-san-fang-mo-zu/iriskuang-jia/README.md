# Iris框架

一個web 框架

```go
package main

import (
	"github.com/kataras/iris/v12"
)

func main() {
	app := iris.New()
	app.Get("/", info)
	app.Listen(":8080")
}

func info(ctx iris.Context) {
	ctx.Params().Visit(func(name string, value string) {
		ctx.Writef("%s = %s\n", name, value)
	})
	type User struct {
		Firstname string `json:"firstname"`
		Lastname  string `json:"lastname"`
		City      string `json:"city"`
		Age       int    `json:"age"`
	}
	peter := User{
		Firstname: "John",
		Lastname:  "Doe",
		City:      "hi",
		Age:       22,
	}
	ctx.JSON(peter)
}
```

