# make, new

用來初始化變數。

## Make

```text
buf := make([]byte, 16) // [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
buf := []byte{0, 1, 3} // [0 1 3]
```

以上兩種寫法都可以初始化。

## New

new 會自動用 zeroed value 來初始化型別，字串會是""，number 會是 0，`channel, func, map, slice` 等等則會是 nil

```go
person := new([]byte)
```

> new 會回傳指標，並且不用設定初始化大小

