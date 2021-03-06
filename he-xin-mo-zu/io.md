# IO

包含 io 與  ioutil

![](../.gitbook/assets/ying-mu-kuai-zhao-20200513-xia-wu-10.25.58.png)

## 範例

讀取檔案轉 JSON

```go
package main
import (
	"os"
  "io/ioutil"
	"encoding/json"
)

func main() {
	jsonFile, err := os.Open("conf.json")
	
	if err != nil {
 	 fmt.Println(err)
	}

	defer jsonFile.Close()

	jsonStr, err := ioutil.ReadAll(jsonFile)
  if err != nil {
      log.Fatal(err)
  }
  doc := make(map[string]interface{})
  if err := json.Unmarshal(jsonStr, &doc); err != nil {
      log.Fatal(err)
  }
  fmt.Println(doc)
  fmt.Println(doc["AWS_ID"])
  fmt.Println(doc["AWS_KEY"])
  for k, v := range doc {
  	fmt.Println("%s:%s",k, v.(string))
  }
}

// map[AWS_ID:x12345123r2r4adefds AWS_KEY:test123]

// x12345123r2r4adefds
// test123
```

