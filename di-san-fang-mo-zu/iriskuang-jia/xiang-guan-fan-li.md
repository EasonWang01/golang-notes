# 相關範例

{% embed url="https://github.com/kataras/iris/tree/master/\_examples" %}

## 搭配xorm

```go
package main

import (
	"fmt"
	"time"

	"github.com/go-xorm/xorm"
	"github.com/kataras/iris/v12"
	_ "github.com/mattn/go-sqlite3"
)

// User describes a user
type User struct {
	Id      int64
	Name    string
	Created time.Time `xorm:"created"`
	Updated time.Time `xorm:"updated"`
}

func main() {
	app := iris.New()

	f := "conversion.db"

	orm, err := xorm.NewEngine("sqlite3", f)
	if err != nil {
		fmt.Println(err)
		return
	}
	app.Get("/", func(ctx iris.Context) {
		users := make([]User, 0)
		err = orm.Find(&users)
		if err != nil {
			fmt.Println(err)
			return
		}
		ctx.JSON(users)
	})
	app.Post("/", func(ctx iris.Context) {
		fmt.Println(ctx.Request().Body)
	})
	app.Listen(":8080")

}

```

