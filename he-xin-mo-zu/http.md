# http

## GET request

```go
package main

import (
    "fmt"
    "io/ioutil"
    "net/http"
    "os"
)

func main() {
    response, err := http.Get("http://golang.org/")
    if err != nil {
        fmt.Printf("%s", err)
        os.Exit(1)
    } else {
        defer response.Body.Close()
        contents, err := ioutil.ReadAll(response.Body)
        if err != nil {
            fmt.Printf("%s", err)
            os.Exit(1)
        }
        fmt.Printf("%s\n", string(contents))
    }
}
```

## HTTP Server

```go
// Writing a basic HTTP server is easy using the
// `net/http` package.
package main

import (
    "fmt"
    "net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintf(w, "hello\n")
}

func main() {
    http.HandleFunc("/hello", hello)
    http.ListenAndServe(":8090", nil)
}
```

### ResponseWriter 的寫入方法

```go
w.Write([]byte("OK"))
fmt.Fprintf(w, "OK")
io.WriteString(w, "OK")
```

## 回傳 JSON 的 server

```go
	type User struct { 
     Name  string  `json:"name"` 
}
			
			user := User{Name: "jason"};
			data, err := json.Marshal(user)
			if err != nil {
				log.Fatal(err)
			}
			w.Header().Set("Content-Type", "application/json")
			w.Write(data);
```

## 回傳其他格式

```go
fmt.Fprintf(w, "%d", 123)
```

