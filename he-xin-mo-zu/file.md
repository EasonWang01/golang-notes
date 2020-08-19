# File

File 相關 API 會使用 os 模組創建檔案或開啟檔案。

## 創建檔案並寫入

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	f, err := os.Create("test.txt")
	if err != nil {
		fmt.Println(err)
	}
	l, err := f.WriteString("apple")
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println("How many bytes write to file: ", l)
}
```

### Append 檔案

加上新字段在舊檔案，`os.OpenFile` 一定要給三個參數，然後第二個參數如果要append必須是`os.O_APPEND|os.O_WRONLY`

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	f, err := os.OpenFile("test.txt", os.O_APPEND|os.O_WRONLY, 0600)
	if err != nil {
		fmt.Println(err)
	}
	f.WriteString("apple")
}

```

