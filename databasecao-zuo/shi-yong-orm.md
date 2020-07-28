# 使用 ORM

主要是方便我們在不同資料庫時有統一的操作模式，以及預防一些安全性問題。

## xorm

{% embed url="https://github.com/go-xorm/xorm" %}

以下我們已 SQLite 3 為範例

> 記得要安裝對應的資料庫 driver

```text
go get github.com/go-xorm/xorm
go get github.com/mattn/go-sqlite3
```

建立 Table

```go
package main

import (
	"fmt"
	"time"

	"github.com/go-xorm/xorm"
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
	f := "conversion.db"

	orm, err := xorm.NewEngine("sqlite3", f)
	if err != nil {
		fmt.Println(err)
		return
	}
	
	 err = orm.CreateTables(&User{})
	 if err != nil {
	 	fmt.Println(err)
	 	return
	 }
}
```

## 新增

```go
	_, err = orm.Insert(&User{Id: 1, Name: "xlw"})
	if err != nil {
		fmt.Println(err)
		return
	}
```

## 查詢

```go
	users := make([]User, 0)
	err = orm.Find(&users)
	if err != nil {
		fmt.Println(err)
		return
	}
```

