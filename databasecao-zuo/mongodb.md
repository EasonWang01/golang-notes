# MongoDB

以下為使用`mgo`之簡單範例

[https://labix.org/mgo](https://labix.org/mgo)

```go
package main

import (
    "fmt"
    "log"
    "time"

    "gopkg.in/mgo.v2"
)

type Person struct {
    Name       string
    Phone      string
    CreateTime time.Time
}

func main() {
    session, err := mgo.Dial("mongodb://test:1234@ds013898.mlab.com:13898/forclass")
    if err != nil {
        panic(err)
    }
    defer session.Close()

    // Optional. Switch the session to a monotonic behavior.
    session.SetMode(mgo.Monotonic, true)

    c := session.DB("forclass").C("apple")
    // err = c.Insert(&Person{"Ale", "+55 53 8116 9639", time.Now()},
    //     &Person{"Cla", "+55 53 8402 8510", time.Now()})
    // if err != nil {
    //     log.Fatal(err)
    // }

    result := make([]Person, 0, 10)
    err = c.Find(nil).All(&result)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("Result:", result)
}
```

