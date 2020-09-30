---
description: 有宣告固定長度為 Array，沒宣告固定長度則為 Slice
---

# Array, Slice 陣列

## 宣告方法

```go
arr2 := [5]int{1, 2, 3}
arr3 := [...]int{1, 2, 3, 4, 5}
```

或

```go
var scores [10]int
scores[0] = 90
scores[1] = 89
```

## 巢狀陣列

```go
arr1 := [2][3]int{[3]int{1, 2, 3}, [3]int{4, 5, 6}}
fmt.Println(arr1) // [[1 2 3] [4 5 6]]
```

## 建立 slice

不初始長度的array及為 slice

```go
langs := []string{"Go", "Python", "Ruby", "PHP"}
```

合併 slice

{% tabs %}
{% tab title="Go" %}
```go
package main

import (
	"fmt"
)

func main() {
	low := []string{"1", "2", "3"}
	high := []string{"11", "22", "33"}
	c := append(low, high...)
	fmt.Println(c)
}

```
{% endtab %}
{% endtabs %}

### 不指定型態

```go
	var cc []interface{}

	fmt.Println(append(cc, 123, "345"))
```

### Assign any slice to an `[]interface{}`

[https://github.com/golang/go/wiki/InterfaceSlice](https://github.com/golang/go/wiki/InterfaceSlice)

