# context

包含以下方法

```go
WithCancel
WithDeadline
WithTimeout
WithValue
```

[https://golang.org/pkg/context/](https://golang.org/pkg/context/)

使用前需要把 Background 傳進去

```text
context.Background()
```

範例：

```go
func someHandler() {
    ctx, cancel := context.WithCancel(context.Background()) // 利用context產生cancel方法
    go doStuff(ctx)
    time.Sleep(10 * time.Second)
    cancel()
}
```

