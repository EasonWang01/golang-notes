# 型別

### 1.型別定義在變數名稱後

```go
var appleNum int // 一般變數
const appleNum int // 常數
```

### 2.一次定義多個同型別的變數

```go
var AppleNum1, AppleNum2, AppleNum3 int
```

或是

```go
var (
    isIt   bool      = false
    initInt uint64   = 1000
    z      complex128 = -5 + 12i
)
```

### 3.直接定義並賦值

```go
var AppleNum int = 5
```

### 4.同時定義多個變數並賦值

```go
var AppleNum1, AppleNum2, AppleNum3 int = 1, 2, 3
```

> 上面幾點如果直接賦值但沒有加型別，go語言會自動判斷

### 5.使用:= 宣告 可省略var和type

```go
func main() {
  appleNum := 5
}
```

> := 只可用在函式內，放在top level會出現錯誤; 如果用 := 又加了var 和型別也會錯誤

**所以用var 或const 通常用來定義全域變數才使用**

### 6.有關 \_

> \_（下劃線）是個特殊的變量名，任何賦予它的值都會被丟棄
>
> 可用於部分回傳值參數不想處理的時候

### 7.型別轉換

使用 `type(要轉換的變數)，例如int(..), float64(..) 等等`

```go
func main() {
    var x, y int = 3, 4
    var f float64 = math.Sqrt(float64(x*x + y*y))
    var z int = int(f)
    fmt.Println(x, y, z)
}
```

> int8 與int32也不可直接運算，需要轉換

### 8.型別別名\(Type alias\)

```go
type (
    水果數 int8
    水果名稱 string
)

func main() {
    var fruitNum 水果數
    var fruitName 水果名稱
    fruitNum = 5
    fruitName = "香蕉"
    fmt.Println(fruitNum)
    fmt.Println(fruitName)
}
```

[https://play.golang.org/p/HHs6xDipuZ](https://play.golang.org/p/HHs6xDipuZ)

## Golang所有可用型別

```go
bool  // 布林值

string // 字串

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 
uintptr // 保存指針用的類型，在32平台下為4字節(bytes)，在64位平台下是8字節。
// var p *int

byte // uint8 的別名

rune // int32 的別名
     // 代表一個Unicode碼

float32 float64

complex64 // 複數（32位實數+32位虛數）
complex128 //（64位實數+64位虛數）
```

### Rune

範例：

```go
var str = "hello 你好"
fmt.Println("len(str):", len(str))  // 12
fmt.Println("rune:", len([]rune(str))) // 8
```

> golang中string底層實作為byte。中文字符在unicode下占2個byte，在utf-8編碼為3個byte，而golang默認編碼為utf8。
>
> 所以用rune才能正確算出中文字長度。

## byte array to string

```go
string(someByteArray[:])
```

如果有轉為 hex string

```go
hex.EncodeToString(someByteArray[:])
```

