# Websocket

```go
package main

import (
	"flag"
	"log"
	"net/http"

	"github.com/gorilla/websocket"
)

var addr = flag.String("addr", "localhost:8010", "http service address")

var upgrader = websocket.Upgrader{
	CheckOrigin: func(r *http.Request) bool { return true },// 讓不同 port 能連到
} 

func echo(w http.ResponseWriter, r *http.Request) {
	c, err := upgrader.Upgrade(w, r, nil)
	if err != nil {
		log.Print("upgrade:", err)
		return
	}
	defer c.Close()
	for {
		mt, message, err := c.ReadMessage()
		if err != nil {
			log.Println("read:", err)
			break
		}
		log.Printf("recv: %s", message)
		err = c.WriteMessage(mt, message)
		if err != nil {
			log.Println("write:", err)
			break
		}
	}
}
func main() {

	http.HandleFunc("/echo", echo)
	log.Fatal(http.ListenAndServe(*addr, nil))
}

```

> 不能跟 app 寫在同一個程式，不像 nodejs 可以跟 http server 寫在一起

client.js

```javascript
import React, { useEffect, useState } from "react";

function App() {
  const [ws, setWs] = useState("");
  const [text, setText] = useState("");

  const handleOpen = () => {
    const _ws = new WebSocket("ws://localhost:8010/echo");
    setWs(_ws);
    _ws.onopen = function (evt) {
      console.log("OPEN");
    };
  };
  
  const handleSend = () => {
    ws.send(text);
  };
  
  return (
    <div className="App">
      <header className="App-header">
        test
        <input onChange={(e) => setText(e.target.value)}></input>
        <button onClick={handleOpen}>Open</button>
        <button onClick={handleSend}>Send</button>
      </header>
    </div>
  );
}

export default App;

```

