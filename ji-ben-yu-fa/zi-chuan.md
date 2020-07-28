# 字串

## 字串

### 1.可用" " 或是 \` \`

```go
var appleColor string = "red"
var appleColor1 string = `red`
```

> \`\`可用來放多行的字串，並且輸出會保留空格
>
> 用' '會錯誤

[https://play.golang.org/p/RnWuWLvDS1](https://play.golang.org/p/RnWuWLvDS1)

### 2. ' ' 單引號只能用在byte array

```go
s := "good"
c := []byte(s)  // 轉為goog
c[3] = 'g'
```

> 用" "或\` \`會錯誤

[https://play.golang.org/p/MMgk12HXFi](https://play.golang.org/p/MMgk12HXFi)

## 核心Strings package

[https://golang.org/pkg/strings/](https://golang.org/pkg/strings/)

### 1.比較字串

可用== 或是 `strings.Compare(a, b)`

> strings.Compare比 == 效率更好
>
> 結果是: 0 假如 a == b,
>
> 結果為 -1 假如 a &lt; b
>
> 結果為 1 假如 a &gt; b.

[https://play.golang.org/p/ia615JYprw](https://play.golang.org/p/ia615JYprw)

### 2.檢查是否包含

`strings.Contains()`

回傳true或false

[https://play.golang.org/p/lj78RhdBYq](https://play.golang.org/p/lj78RhdBYq)

也可用strings.ContainsAny\("failure", "ura & i"\)

> 如果用&在contains\(\) 還是會傳回false

[https://play.golang.org/p/oeGdbA9juD](https://play.golang.org/p/oeGdbA9juD)

### 3.轉為Unicode後比較是否包含

```text
strings.ContainsRune()
```

[https://play.golang.org/p/0puOGCxAdR](https://play.golang.org/p/0puOGCxAdR)

### 4.計算字串中特定字之重複次數

```go
strings.Count()
```

[https://play.golang.org/p/aoJnR5C5ux](https://play.golang.org/p/aoJnR5C5ux)

### 5.忽略大小寫比較

```go
strings.EqualFold()
```

[https://play.golang.org/p/b1VgY3EazY](https://play.golang.org/p/b1VgY3EazY)

### 6.字串轉Array

```go
strings.Fields("  foo bar  baz solid  ")
```

以空格分隔轉為Array

[https://play.golang.org/p/bGsqjtSwMN](https://play.golang.org/p/bGsqjtSwMN)

### 7.替換特殊字元後比較

> ```text
> FieldsFunc第二個參數必須要是function 並return boolean
> ```

```go
    f := func(c rune) bool {
        return !unicode.IsLetter(c) && !unicode.IsNumber(c)
    }
    fmt.Printf("Fields are: %q", strings.FieldsFunc("  foo1; b@ar2,baz3...", f))
```

> 第二個參數例如strings.FieldsFunc\("test", func \(c rune\) bool { return true }\)

### 8.測試開頭是否包含字串

```go
strings.HasPrefix("Gopher", "Go")
```

[https://play.golang.org/p/oyvjtOykj\_](https://play.golang.org/p/oyvjtOykj_)

### 9.測試結尾是否包含

```go
fmt.Println(strings.HasSuffix("banana", "na"))
```

[https://play.golang.org/p/a\_lfSRdz81](https://play.golang.org/p/a_lfSRdz81)

### 10.查看字元在字串中的位置

```go
fmt.Println(strings.Index("paper", "e"))
```

[https://play.golang.org/p/haX84zOA-1](https://play.golang.org/p/haX84zOA-1)

