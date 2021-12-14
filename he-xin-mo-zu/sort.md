---
description: https://golang.org/pkg/sort/
---

# Sort

{% embed url="https://yourbasic.org/golang/how-to-sort-in-go/" %}

## 基本用法

```go
sort.Slice
sort.SliceStable // 將相同值的元素保持原順序
sort.Ints
sort.Float64s
sort.Strings
```

## 基本 sort

```go
package main

import (
    "fmt"
    "sort"
)

func main() {

    strs := []string{"c", "a", "b"}
    sort.Strings(strs)
    fmt.Println("Strings:", strs)

    ints := []int{7, 2, 4}
    sort.Ints(ints)
    fmt.Println("Ints:   ", ints)

    s := sort.IntsAreSorted(ints)
    fmt.Println("Sorted: ", s)
}
```

## slice sort&#x20;

```go
sort.Slice(people, func(i, j int) bool {
  return people[i].Age > people[j].Age
})
```

## 實作 slice sort 方法

自行實作 slice 的 sort ，實作時必須要實作三個 func (Len, Less, Swap) 缺一不可。

實作後即可使用 `sort.Sort` 會依照實作時的 Less 方法排序。

```go
type Person struct {
    Name string
    Age  int
}

// ByAge implements sort.Interface based on the Age field.
type ByAge []Person

func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }

func main() {
    family := []Person{
        {"Alice", 23},
        {"Eve", 2},
        {"Bob", 25},
    }
    sort.Sort(ByAge(family))
    fmt.Println(family) // [{Eve 2} {Alice 23} {Bob 25}]
}
```

###
