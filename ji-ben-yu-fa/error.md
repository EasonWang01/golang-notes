# Error

![](../.gitbook/assets/ying-mu-kuai-zhao-20200513-xia-wu-7.04.23.png)

假設某個 func 會回傳 error，可以用如下印 error

```javascript
	err := http.ListenAndServe("8090", nil);
	if err != nil {
		log.Fatal(err)
	}
```

或是

```go
fmt.Printf("%s", err)
```

#### 一般 log

```text
log.Print("Logging to a file in Go!")
```

#### 自定義

```go
	import (
	  "errors"
	)
	err1 := errors.New("Some error") 
	fmt.Println(err1) 
```

> 其他可參見 panic, recover

