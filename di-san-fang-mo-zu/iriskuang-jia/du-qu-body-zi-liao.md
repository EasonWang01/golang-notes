# 讀取 Body 資料

### Raw JSON

```go
		app.Post("/", func(ctx iris.Context) {
		  rawBodyAsBytes, err := ioutil.ReadAll(ctx.Request().Body)
		  if err != nil { /* handle the error */
		  	ctx.Writef("%v", err)
		  }
		  rawBodyAsString := string(rawBodyAsBytes)
		  ctx.JSON(rawBodyAsString)
		}
```

### x-www-form-urlencode

```go
app.Post("/", func(ctx iris.Context) {
		username := ctx.PostValue("age")
		fmt.Println(username)
		ctx.Writef(username)
}
```

### form-data

```go
app.Post("/", func(ctx iris.Context) {		
		type Visitor struct {
			Name string `form:"name"`
		}
		visitor := Visitor{}
		err := ctx.ReadForm(&visitor)
		if err != nil {
			ctx.StatusCode(iris.StatusInternalServerError)
			ctx.WriteString(err.Error())
		}
		ctx.Writef("%s", visitor)
}
```

