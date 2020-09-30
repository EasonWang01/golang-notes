# gin 框架

一個 server 框架：[https://github.com/gin-gonic/gin](https://github.com/gin-gonic/gin)

## 範例：

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {

	router := gin.New()
	router.LoadHTMLFiles("./public/index.html")

	router.GET("test", func(c *gin.Context) {
		res := []string{"foo", "bar"}
		c.JSON(200, res)
	})

	router.GET("/room/:roomId", func(c *gin.Context) {
		c.HTML(200, "index.html", nil)
	})

	router.GET("/ws/:roomId", func(c *gin.Context) {
		roomId := c.Param("roomId")
		serveWs(c.Writer, c.Request, roomId)
	})

	router.Run("0.0.0.0:8080")
}

```

