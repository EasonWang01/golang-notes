# regExp

找出字串

```go
myRegex, _ := regexp.Compile("c([a-z]+)ll")
found := myRegex.FindAllString("Can golang be called godlang?", -1)
fmt.Printf("found  =%v\n", found)
```

