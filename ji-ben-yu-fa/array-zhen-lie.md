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

{% embed url="https://go.dev/ref/spec#Slice_expressions" %}

## 有關 slice 或 array 當成參數時

slice 如果當參數傳遞不傳 pointer 的話仍會改變原先的變數，但 array 不會改變到原先的變數。

更詳細的說明可參考：

{% embed url="https://kitecloud-backend.coderbridge.io/2020/08/15/golang-slice-%E4%BD%9C%E7%82%BA%E5%8F%83%E6%95%B8%E5%82%B3%E9%81%9E%E6%99%82%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A0%85" %}

[https://stackoverflow.com/questions/39993688/are-slices-passed-by-value](https://stackoverflow.com/questions/39993688/are-slices-passed-by-value)
