# crypto

## Crypto

[https://golang.org/pkg/crypto/](https://golang.org/pkg/crypto/)

### SHA256

包含了sha224 與 sha256

```go
package main

import (
    "crypto/sha256"
    "fmt"
)

func main() {
    sum := sha256.Sum256([]byte("hello world\n"))
    fmt.Printf("%x", sum)
}
```

[https://play.golang.org/p/Ky4kmHLqly](https://play.golang.org/p/Ky4kmHLqly)

### SHA3

\(包含原生sha3和SHAKE\)

> The SHAKE instances are faster than the SHA3 instances

[https://godoc.org/golang.org/x/crypto/sha3](https://godoc.org/golang.org/x/crypto/sha3)

```go
package main

import (
    "fmt"
    "golang.org/x/crypto/sha3"
)

func main() {
    buf := []byte("some")
    h := make([]byte, 32)
    d := sha3.NewShake256()
    d.Write(buf)
    d.Read(h)
    fmt.Printf("%x\n", h)
}
```

等同於

```go
package main

import (
    "fmt"
    "golang.org/x/crypto/sha3"
)

func main() {
    buf := []byte("some")
    // A hash needs to be 64 bytes long to have 256-bit collision resistance.
    h := make([]byte, 32)
    // Compute a 64-byte hash of buf and put it in h.
    sha3.ShakeSum256(h, buf)
    fmt.Printf("%x\n", h)
}
```

### RIPEMD160

```go
package main

import (
    "golang.org/x/crypto/ripemd160"
    "fmt"
)

func main() {
    h := ripemd160.New()
    h.Write([]byte("hello"))
    fmt.Printf("%x\n", h.Sum(nil))
}
```

### HMAC

[https://golang.org/pkg/crypto/hmac/](https://golang.org/pkg/crypto/hmac/)

```go
package main

import (
        "crypto/hmac"  
        "crypto/sha256"  
    "fmt"
)

func main() {
    key := []byte("I am key")
    mac := hmac.New(sha256.New, key)
    mac.Write([]byte("msg"))
    fmt.Printf("%x\n", mac.Sum(nil))
}
```

### ECDSA

```go
package main

import (
    "crypto/ecdsa"
    "crypto/elliptic"
    "crypto/md5"
    "crypto/rand"
    "fmt"
    "hash"
    "io"
    "math/big"
    "os"
)

func main() {

    Curve := elliptic.P256() //see http://golang.org/pkg/crypto/elliptic/#P256

    privatekey, err := ecdsa.GenerateKey(Curve, rand.Reader) // this generates a public & private key pair

    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    pubkey := privatekey.PublicKey
    fmt.Printf("%+v\n", privatekey.PublicKey.X)
    fmt.Println("Private Key :")
    fmt.Printf("%x \n", privatekey)

    fmt.Println("Public Key :")
    fmt.Printf("%x \n", pubkey)
    fmt.Printf("%+v\n", pubkey) // 印出struct

    // Sign ecdsa style

    var h hash.Hash
    h = md5.New()
    r := big.NewInt(0)
    s := big.NewInt(0)

    io.WriteString(h, "This is a message to be signed and verified by ECDSA!")
    signhash := h.Sum(nil)

    r, s, serr := ecdsa.Sign(rand.Reader, privatekey, signhash)
    if serr != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    // Verify
    verifystatus := ecdsa.Verify(&pubkey, signhash, r, s)
    fmt.Println(verifystatus) // should be true
}
```

## AES

```go
package main

import (
	"bytes"
	"crypto/aes"
	"crypto/cipher"
	"crypto/rand"
	"encoding/base64"
	"errors"
	"fmt"
	"io"
	"strings"
)

func addBase64Padding(value string) string {
	m := len(value) % 4
	if m != 0 {
		value += strings.Repeat("=", 4-m)
	}

	return value
}

func removeBase64Padding(value string) string {
	return strings.Replace(value, "=", "", -1)
}

func Pad(src []byte) []byte {
	padding := aes.BlockSize - len(src)%aes.BlockSize
	padtext := bytes.Repeat([]byte{byte(padding)}, padding)
	return append(src, padtext...)
}

func Unpad(src []byte) ([]byte, error) {
	length := len(src)
	unpadding := int(src[length-1])

	if unpadding > length {
		return nil, errors.New("unpad error. This could happen when incorrect encryption key is used")
	}

	return src[:(length - unpadding)], nil
}

func encrypt(key []byte, text string) (string, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return "", err
	}

	msg := Pad([]byte(text))
	ciphertext := make([]byte, aes.BlockSize+len(msg))
	iv := ciphertext[:aes.BlockSize]
	if _, err := io.ReadFull(rand.Reader, iv); err != nil {
		return "", err
	}

	cfb := cipher.NewCFBEncrypter(block, iv)
	cfb.XORKeyStream(ciphertext[aes.BlockSize:], []byte(msg))
	finalMsg := removeBase64Padding(base64.URLEncoding.EncodeToString(ciphertext))
	return finalMsg, nil
}

func decrypt(key []byte, text string) (string, error) {
	block, err := aes.NewCipher(key)
	if err != nil {
		return "", err
	}

	decodedMsg, err := base64.URLEncoding.DecodeString(addBase64Padding(text))
	if err != nil {
		return "", err
	}

	if (len(decodedMsg) % aes.BlockSize) != 0 {
		return "", errors.New("blocksize must be multipe of decoded message length")
	}

	iv := decodedMsg[:aes.BlockSize]
	msg := decodedMsg[aes.BlockSize:]

	cfb := cipher.NewCFBDecrypter(block, iv)
	cfb.XORKeyStream(msg, msg)

	unpadMsg, err := Unpad(msg)
	if err != nil {
		return "", err
	}

	return string(unpadMsg), nil
}

func main() {
	key := []byte("LKHlhb899Y09olUi")
	encryptMsg, _ := encrypt(key, "test")
	msg, _ := decrypt(key, encryptMsg)
	fmt.Println(msg) // Hello World
}
```

## RSA

> 包含 OAEP 與 PKCS

先產生公鑰與私鑰

```text
openssl genrsa -out private.pem 1024
openssl rsa -in private.pem -pubout -out public.pem
```

```go
package main

import (
	"crypto/rand"
	"crypto/rsa"
	"crypto/x509"
	"encoding/pem"
	"errors"
	"fmt"
)

func rsaEncrypt(origData []byte) ([]byte, error) {
	block, _ := pem.Decode(publicKey)
	if block == nil {
		return nil, errors.New("public key error")
	}
	pubInterface, err := x509.ParsePKIXPublicKey(block.Bytes)
	if err != nil {
		return nil, err
	}
	pub := pubInterface.(*rsa.PublicKey)
	return rsa.EncryptPKCS1v15(rand.Reader, pub, origData)
}

func rsaDecrypt(ciphertext []byte) ([]byte, error) {
	block, _ := pem.Decode(privateKey)
	if block == nil {
		return nil, errors.New("private key error")
	}
	priv, err := x509.ParsePKCS1PrivateKey(block.Bytes)
	if err != nil {
		return nil, err
	}
	return rsa.DecryptPKCS1v15(rand.Reader, priv, ciphertext)
}

var privateKey = []byte(`
-----BEGIN RSA PRIVATE KEY-----
MIICXAIBAAKBgQDZyTI4o7pukaJN6sVklkX4sR5IRpLXzPv4/28YVD1xaPn1Q69o
7G6GfHacMtsPZDkEFeGmZBWb4YHLqRczKM3ZPSEA2/FBGZoxKiCq80NMhPngiEWy
gG+J7CyXXO9syHFpSO5AciB0P3r2aewWhVMub9bDa9uwUnMqFCmy65GB/QIDAQAB
AoGARaYZgJGkCr5aeK6vSBbi88C5HYYsagVtQ9l0zwQJzl4zKiPmUhji0/G0AQom
koqLzWmuC4eQfZSl7Nr7x2myQsMxd6W1eZhU/Jki3v/tqG5rDpDz0X0/05uOsZoe
i+6EPhRiT1Y95R2evKpcCwOtUjOD3uQZx6vmytUeEQ7U4sECQQD0rCOdE6OC1LQJ
X2xPqvM9WHeHavhktvCZWkmzfzb/wDtAcbrspS+riBCdiwesP9d7rujmhNUG6vAd
NYVqD4NxAkEA495l33ImdwsyX/IvkastyJ65JDKtS20nHWqzFGW3pVd7liGTvo5k
Iil3vwIM6Mdw/AVUubsEUIvrHC8u2cgJTQJASD9sZL2f0sosP3hF62B3Yu30nbAg
mNzMPvxCNxahjvOci3MJ10cPxH7xKRQct+hCIOuNKkSfAuPs8zMSqjbagQJAUdgI
eRgz7qAL6OBA664zFJLF5tV43tWGrg8r4RCjxHRGhGbs/Q2Bs693PhjLcDRqRWrY
wpkEdLW8rXPY/QnXJQJBAL3uYok6AeKf/UDs1/tR8RqSSWwAQC6FGXbygsLKfywD
v+6/UK1M6tc3S8jHvCZUw5Ybds0KUKMiaATX7RYUDQo=
-----END RSA PRIVATE KEY-----
`)
var publicKey = []byte(`
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDZyTI4o7pukaJN6sVklkX4sR5I
RpLXzPv4/28YVD1xaPn1Q69o7G6GfHacMtsPZDkEFeGmZBWb4YHLqRczKM3ZPSEA
2/FBGZoxKiCq80NMhPngiEWygG+J7CyXXO9syHFpSO5AciB0P3r2aewWhVMub9bD
a9uwUnMqFCmy65GB/QIDAQAB
-----END PUBLIC KEY-----
`)

func main() {
	data, err := rsaEncrypt([]byte("test"))
	if err != nil {
		panic(err)
	}
	decryptedData, err := rsaDecrypt(data)
	if err != nil {
		panic(err)
	}
	fmt.Println(string(decryptedData))
}
```



