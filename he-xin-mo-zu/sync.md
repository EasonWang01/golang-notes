# Sync

{% embed url="https://golang.org/pkg/sync/" %}

## WaitGroup

一般來說如果有go routine 再跑必須用 `time.Sleep()` 或 `WaitGroup` 來避免程式結束但go routine還沒跑完。

```javascript
func main() {
    wg := sync.WaitGroup{}
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go func(i int) {
            fmt.Println(i)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

有三個方法

```text
Add(), Done(), Wait()
```

WaitGroup 對象內部有一個計數器，最初從0開始，它有三個方法：Add\(\), Done\(\), Wait\(\) 用來控制計數器的數量。Add\(n\) 把計數器設置為n ，Done\(\) 每次把計數器-1 ，wait\(\) 會阻塞代碼的運行，直到計數器地值減為0

