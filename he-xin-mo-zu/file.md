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

