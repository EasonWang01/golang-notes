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
	defer f.Close()
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
	defer f.Close()
	if err != nil {
		fmt.Println(err)
	}
	f.WriteString("apple")
}

```

### 寫入 Bytes

> 在 txt 檔案內會顯示對應的 ASCII 字元

```go
	d2 := []byte{104, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100, 104, 101, 108, 108, 111}
	f.Write(d2)
```

