# panic, recover

panic 可以讓執行中的程式直接中斷跳出。

```go
package main

import "fmt"

func main() {
    f()
    fmt.Println("Returned normally from f.")
}

func f() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered in f", r)
        }
    }()
    fmt.Println("Calling g.")
    g(0)
    fmt.Println("Returned normally from g.")
}

func g(i int) {
    if i > 3 {
        fmt.Println("Panicking!")
        panic(fmt.Sprintf("%v", i))
    }
    defer fmt.Println("Defer in g", i)
    fmt.Println("Printing in g", i)
    g(i + 1)
}
```

會印出如下

```text
Calling g.
Printing in g 0
Printing in g 1
Printing in g 2
Printing in g 3
Panicking!
Defer in g 3
Defer in g 2
Defer in g 1
Defer in g 0
Recovered in f 4
Returned normally from f.
```

panic 後會中斷function並立刻執行defer，所以會印出剛才存在defer stack 中的東西

```text
Defer in g 3
Defer in g 2
Defer in g 1
Defer in g 0
```

然後panic 後會再往上拋到 f，執行 f 的 defer

```go
defer func() {
  if r := recover(); r != nil {
    fmt.Println("Recovered in f", r)
  }
}()
```

`recover()` 會讓 f 改為正常執行，從panic復原

並繼續執行

## Recover

如果發生了 panic，可以使用 recover來復原，這個函式必須在被 defer 的函式中執行才有效果，若在被 defer 的函式外執行，recover 會傳回 nil，recover 的 defer 必須寫在panic之前先設定好

```go
    defer func() {
        if err := recover(); err != nil {
            fmt.Println(err)
        }
    }()
```

> 因為panic會讓中斷以外會讓程式結束，所以為了避免程式直接結束會用recover來讓程式繼續執行，並輸出error log

