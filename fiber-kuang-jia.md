# Fiber 框架

類似於 Node.js express 寫法。

{% embed url="https://github.com/gofiber/fiber/" %}

## 回傳 JSON

```go
package main

import "github.com/gofiber/fiber"

func main() {
	app := fiber.New()

	app.Get("/", func(c *fiber.Ctx) {
		c.JSON(fiber.Map{"code": 200, "message": "Hello, World"})
	})

	app.Listen(3000)
}
```

