# map

使用方式:

```go
test := make(map[int]string)

test := map[int]string{
    1: "a",
    2: "b",
    3: "c",
}
```

map 跟 struct 最大的差別在於:

```text
map 的所有 key 要同型別，所有的value 也要同型別
```

## 新增key

```go
test[123] = "as"
```

## 刪除key

```go
delete (test, 123)
```

## 查詢key

```go
test[123]
```

## iterate Map 的 value

```go
package main

import (
	"fmt"
)

func main() {
	test := map[string]int{
		"jason": 12,
		"eason": 15,
	}
	for idx := range test {
		fmt.Println(test[idx])
	}
}
```

