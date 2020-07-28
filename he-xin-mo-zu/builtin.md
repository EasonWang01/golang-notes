---
description: 'https://golang.org/pkg/builtin/'
---

# BuiltIn

內建函數 \(不需要 import 任何 module\)

```go
func append(slice []Type, elems ...Type) []Type
func cap(v Type) int
func close(c chan<- Type)
func complex(r, i FloatType) ComplexType
func copy(dst, src []Type) int
func delete(m map[Type]Type1, key Type)
func imag(c ComplexType) FloatType
func len(v Type) int
func make(t Type, size ...IntegerType) Type
func new(Type) *Type
func panic(v interface{})
func print(args ...Type)
func println(args ...Type)
func real(c ComplexType) FloatType
func recover() interface{}
```



