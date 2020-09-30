# 自動重啟 server 工具

> 也有自己的 web 框架：[https://github.com/gin-gonic/gin](https://github.com/gin-gonic/gin)

類似於 nodemon 的 live-reload 工具。

[https://github.com/codegangsta/gin](https://github.com/codegangsta/gin)

### 自訂 port

> 之後輸入 localhost:8081 會執行 port 8080 server

```text
gin -p 8081 -a 8080
```

### 如果是用預設 port

預設 proxy 為 3001 指向 3000，所以 app 會佔用 3000，如果要存取 server 要存取 3001

如果安裝新模組要重新執行 gin

