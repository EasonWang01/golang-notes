---
description: 變數
---

# Variable

宣告變數有兩種方式

#### 1.在 main 或 func 裡面使用 := 宣告

```go
func main {
  count := 12
}
```

> 一定要在 func 或 main 裡面才可用&#x20;

#### 2.使用 var 宣告

可以放在 package scope 或 function scope，但後面一定要給型別。

```go
var count int;
var count1 int = 12; // 可以先賦予值
var (
    foo string
    bar int
) // 簡寫
func main () {
  var count2 int;
}
```

### Constant

用來宣告固定不變的常數

> iota 的下一個會自動 +1

```go
const ccc = 12

const (
	test11 = iota
	test22
	test3
)

func main() {
	fmt.Println(test11, test22, ccc) // 0 1 12
}
```
