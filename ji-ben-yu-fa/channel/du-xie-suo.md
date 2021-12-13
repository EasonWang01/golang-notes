# 讀寫鎖

當大量 go routine 同時執行時，例如放在 for loop，會產生 race condition，不一定照著順序執行。

此時可用 sync.Mutex，但可能會造成讀取也變得沒效率，可改用 sync.RWMutex，讓你可以多次同時讀並保證單次寫。

```go
mux.RWLock()
// 要執行的代碼
mux.RWUnlock()
```

可參考：[https://clouding.city/go/mutex-rwmutex/](https://clouding.city/go/mutex-rwmutex/)
