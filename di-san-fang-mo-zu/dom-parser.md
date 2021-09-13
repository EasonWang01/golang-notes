---
description: 'https://github.com/PuerkitoBio/goquery'
---

# Dom Parser

可以執行類似解析 DOM 的工作。

```go
res, err := http.Get("https://test.io/overview")
	if err != nil {
		log.Fatal(err)
	}
	defer res.Body.Close()
	if res.StatusCode != 200 {
		log.Fatalf("status code error: %d %s", res.StatusCode, res.Status)
	}

	// Load the HTML document
	doc, err := goquery.NewDocumentFromReader(res.Body)
	if err != nil {
		log.Fatal(err)
	}
	doc.Find("[class^=card-number]").Each(func(i int, s *goquery.Selection) {
		// For each item found, get the title
		title := s.Text()
		fmt.Printf("Review %d: %s\n", i, title)
	})
```

