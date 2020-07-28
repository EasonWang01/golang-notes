---
description: 'https://golang.org/pkg/fmt/'
---

# fmt

```text
Print: 輸出到控制台,不接受任何格式化操作 
Println: 輸出到控制台並換行 
Printf : 只可以列印出格式化的字符串。只可以直接輸出字符串類型的變量（不可以輸出別的類型） 
Sprintf：格式化並返回一個字符串而不帶任何輸出 
Fprintf：格式化並輸出到 io.Writers 而不是 os.Stdout
```

## printf

```go
package main

import (
    "fmt"
)

type person struct {
    height int
    weight int
}

func main() {
    Jason := person{178, 72}

    fmt.Print(Jason)
    fmt.Println(Jason)        // 不換行
    fmt.Printf("%v\n", Jason) // 可選擇使用什麼格式來印
    fmt.Printf("%+v\n", Jason)
}
```

結果如下：

```text
{178 72}{178 72}
{178 72}
{height:178 weight:72}
```

## sprintf

```go
s := fmt.Sprintf("%s ","string") //不會輸出，只會轉換格式為string
fmt.Println(s)
```

## Fprintf

輸出到其他 io，例如http response

```go
fmt.Fprintf(w, "%d", 123)
```



## 附錄:

```text
        %v 輸出結構體 {10 30}
        %+v 輸出結構體顯示字段名 {one:10 tow:30}
        %#v 輸出結構體源代碼片段 main.Point{one:10, tow:30}
        %T 輸出值的類型     main.Point
        %t 輸出格式化boolean true
        %d`輸出標准的十進制格式化 100
        %b`輸出標准的二進制格式化 99 對應 1100011
        %c`輸出定整數的對應字符  99 對應 c
        %x`輸出十六進制編碼  99 對應 63
        %f`浮點數
        %e`輸出科學  123400000.0 對應 1.234000e+08
        %E`輸出科學  123400000.0 對應 1.234000e+08
        %s 進行基本的字符串輸出   "\"string\""  對應 "string"
        %q  a single-quoted character literal safely escaped with Go syntax.
        %p 輸出一個指針的值   &jgt 對應 0xc00004a090
        % 後面使用數字來控制輸出寬度 默認結果使用右對齊並且通過空格來填充空白部分
        %2.2f  指定浮點型的輸出寬度 1.2 對應  1.20
        %*2.2f  指定浮點型的輸出寬度對齊，使用 `-` 標志 1.2 對應  *1.20
```

[https://golang.org/pkg/fmt/](https://golang.org/pkg/fmt/)

