# 安裝Golang

## 使用 1.11 版本後的不需要設置 GOPATH, GOROOT

因為已經有 go mod，解決之前專案都必須放在 GOPATH 的問題。

## Windows

到此頁面下載並安裝[https://golang.org/dl/](https://golang.org/dl/)

建議安裝路徑預設選為C:\go

之後安裝程式會自動添加PATH變數 `C:\go\bin`

1.接著我們要添加一個變數`GOROOT`

```text
setx goroot C:\go
```

2.再來設定`GOPATH`

```text
setx gopath C:\Go\gopathfold
```

GOPATH為之後用來放專案的路徑 未來通常會有三個目錄`src、bin、pkg`

`src`放原始碼（比如：.go .c .h .s等）

`pkg` 存放編譯後生成的文件（比如：.a）

`bin` 存放編譯後生成的可執行文件

> 可以輸入go env 來查看目前Go 的相關環境路徑與變量

## MacOS

1.到此下載頁面下載安裝檔案：[https://golang.org/dl/](https://golang.org/dl/)

2.之後一樣新增`GOPATH`

```text
export GOPATH=/Users/...
```

## Linux

```text
wget -c https://dl.google.com/go/go1.14.2.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local

vim ~/.profile
export PATH=$PATH:/usr/local/go/bin

source ~/.profile
```

