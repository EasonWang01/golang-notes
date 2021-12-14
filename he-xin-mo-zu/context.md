---
description: https://pkg.go.dev/context
---

# context

### 簡介：

當我們使用 goroutine 時需要一個地方來儲存互相會共用的變數或是讓 goroutine 在特定情況下停止執行，此時可以使用 context。

包含以下方法

```go
WithCancel
WithDeadline
WithTimeout
WithValue
```

[https://golang.org/pkg/context/](https://golang.org/pkg/context/)

使用前需要把 Background 傳進去

```
context.Background()
```

之後放到 WithDeadline 或 WithTimeout 或 WithCancel，最後如果要傳值再放進 WithValue。

在 goroutine 裡面使用 select，當 context timeout 或 cancel 後會觸發 select 裡面的

&#x20;`case <-ctx.Done():`

範例：

```go
func someHandler() {
    ctx, cancel := context.WithCancel(context.Background()) // 利用context產生cancel方法
    valueCtx := context.WithValue(ctx, key, 1)
    go doStuff(valueCtx)
    time.Sleep(10 * time.Second)
    cancel()
}

func doStuff(ctx context.Context) {
	for {
	  select {
		case <-ctx.Done(): // cancel 時會觸發
			fmt.Println(ctx.Value(key))
			return
		default:
			//取出值
			var value int = ctx.Value(key).(int)
		}
	}
}
```

### Context vs Channel

1.  Use `context` to cancel

    ```go
    type Worker struct {
      Ctx context.Context
      Cancel context.CancelFunc
      Jobs chan Job
    }

    func (w *Worker) Run() {
      for {
        select {
        case <-w.Ctx.Done():
          return
        case j := <-w.Jobs:
          // Do some work
      }
    }

    go w.Run()
    w.Cancel()
    ```



    2\. Use a kill channel to signal cancellation and a done channel to indicate that goroutine has been terminated.

    ```go
    type Worker struct {
      Done chan struct{}
      Kill chan struct{}
      Jobs chan Job
    }

    func (w *Worker) Run() {
      defer func() {
        w.Done <- struct{}{}
      }
      for {
        select {
        case <-w.Kill:
          return
        case j := <-w.Jobs:
          // Do some work
      }
    }

    go w.Run()
    w.Kill <- struct{}{}
    ```

    [https://stackoverflow.com/q/54335907/4622645](https://stackoverflow.com/q/54335907/4622645)
