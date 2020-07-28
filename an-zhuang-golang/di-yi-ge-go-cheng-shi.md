# 第一個 GO 程式

輸入以下來建立資料夾

```text
mkdir -p $GOPATH/src/github.com/user/hello
```

新增 hello.go

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world.")
}
```

執行：

```text
go run hello.go
```

