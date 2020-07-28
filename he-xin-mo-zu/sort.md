---
description: 'https://golang.org/pkg/sort/'
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

## 自訂排序

自行實作 sort 或 stable 

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

