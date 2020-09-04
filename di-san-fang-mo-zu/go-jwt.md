# go-jwt

[https://godoc.org/github.com/dgrijalva/jwt-go\#example-New--Hmac](https://godoc.org/github.com/dgrijalva/jwt-go#example-New--Hmac)

Example

```go
import "github.com/dgrijalva/jwt-go"

//以下為簽章
token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
        "foo": "bar",
        "nbf": time.Date(2015, 10, 10, 12, 0, 0, 0, time.UTC).Unix(),
    })

    // Sign and get the complete encoded token as a string using the secret
    tokenString, err := token.SignedString([]byte{'g', 'o', 'l', 'a', 'n', 'g'}) //這裡是secret 一定要是byte array

    fmt.Println(tokenString, err)

//以下為認證verify   下面為剛產生的token hash
    tokenString1 := "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJuYmYiOjE0NDQ0Nzg0MDB9.oWULuaWiGsPIvfnnVpmA8kRcUbDA7WKei_EyHB4cUAc"

    // Parse takes the token string and a function for looking up the key. The latter is especially
    // useful if you use multiple keys for your application.  The standard is to use 'kid' in the
    // head of the token to identify which key to use, but the parsed token (head and claims) is provided
    // to the callback, providing flexibility.
    token1, err := jwt.Parse(tokenString1, func(token *jwt.Token) (interface{}, error) {
        // Don't forget to validate the alg is what you expect:
        if _, ok := token.Method.(*jwt.SigningMethodHMAC); !ok {
            return nil, fmt.Errorf("Unexpected signing method: %v", token.Header["alg"])
        }

        // hmacSampleSecret is a []byte containing your secret, e.g. []byte("my_secret_key")
        return []byte{'g', 'o', 'l', 'a', 'n', 'g'}, nil  //第一個return值要填入我們寫的secret
    })

    if claims, ok := token1.Claims.(jwt.MapClaims); ok && token1.Valid {
        fmt.Println(claims["foo"], claims["nbf"])
    } else {
        fmt.Println(err)
    }
```

