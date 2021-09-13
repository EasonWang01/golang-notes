# JSON

## Byte 轉為 JSON

使用  json.Unmarshal 並傳入要轉換的 \[\]byte 格式的 body  即可，如果是用 string\(body\) 則回傳的結果會是 JSON stringify 的型態（多了 \ 在每個 key 前面）

```go
func GetUserAssets(c *fiber.Ctx) error {
	url := "https://api.....io/api/v1/assets...."

	req, _ := http.NewRequest("GET", url, nil)

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	var data interface{}
	json.Unmarshal(body, &data)
	return c.Status(fiber.StatusCreated).JSON(fiber.Map{
		"data":    data,
		"success": true,
		"message": "User assets get successfully",
	})
}
```

## Struct to JSON

記得 struct 的 key 開頭要大寫，然後 Marshal 後要轉為 string 

```go
  var results struct {
		Traders_24H string `json:"Traders_24H"`
	}

  results.Traders_24H = "test"
	
	var data, err1 = json.Marshal(results)
	
	if err1 != nil {
		log.Fatal(err)
	}

	return c.Status(fiber.StatusOK).JSON(fiber.Map{
		"success": true,
		"data":    string(data),
		"message": "success query NFT market data",
	})
```

## 快速 JSON 轉為 struct 的工具

[https://mholt.github.io/json-to-go/](https://mholt.github.io/json-to-go/)

