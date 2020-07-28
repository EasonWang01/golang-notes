# 測試

執行以上後會顯示出如下

```text
f1     10000        120860 ns/op        2433 B/op         28 allocs/op
f2     10000        120288 ns/op        2288 B/op         26 allocs/op
```

其中分別為

```text
1. 疊代次数
2. ns/op 是一次疊代完成所需的時間奈秒
3. allocs/op 表示每個op發生多少個內存分配
4. B/op 是每個操作分配了多少 bytes 。
```

## 撰寫測試

1.先在目錄內新增一個main.go \(如果沒有其他go檔案test會錯誤\)

2.新增 ..._test.go \(任何名稱加上 \_test.go_\)

3.輸入 `go test <package name>`

> 會自動去尋找目錄內 \_test.go檔案，然後其中 名稱包含Test的func

![](.gitbook/assets/螢幕快照%202020-02-05%20上午10.37.07.png)

新增如下，然後把 1改成 2看看

```go
package hello

import "testing"

func Test123(t *testing.T) {
    if 1 != 1 {
        t.Fail()
    }
}
```

> 其他可用包含
>
> ```text
> t.Skip()
> t.Error()
> t.Log(...)
> ```

### Benchmark

任何以`func BenchmarkXxx(b *testing.B) 都會跑效能測試`

```go
package hello

import "testing"

func Add(a, b int) int {
    return a + b
}
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(1, 2)
    }
}
```

然後輸入`go test hello -bench="."`

> 想看到 Log 的訊息的話，必須加上 -v 引數, 加上 -bench 引數可指定要測試的函式名稱

