---
description: 有宣告固定長度為 Array，沒宣告固定長度則為 Slice
---

# Array, Slice 陣列

## Array 宣告方法

當 \[] 內有指定長度或使用 ... 則會是 array

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

{% embed url="https://github.com/golang/go/wiki/InterfaceSlice" %}

## slice 中取值

```go
a[2:]  // same as a[2 : len(a)]
a[:3]  // same as a[0 : 3]
a[:]   // same as a[0 : len(a)]
If a is a pointer to an array, a[low : high] is shorthand for (*a)[low : high].
```

[https://go.dev/ref/spec#Slice\_expressions](https://go.dev/ref/spec#Slice\_expressions)
