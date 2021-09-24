# go-jwt

[https://godoc.org/github.com/dgrijalva/jwt-go\#example-New--Hmac](https://godoc.org/github.com/dgrijalva/jwt-go#example-New--Hmac)

簽章

```go
import "github.com/dgrijalva/jwt-go"

//以下為簽章
token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
  "foo": "bar",
  "nbf": time.Now().Unix(),
})

// Sign and get the complete encoded token as a string using the secret
tokenString, err := token.SignedString([]byte("test")) //這裡是secret 一定要是byte array

fmt.Println(tokenString, err)
```

驗證

```go
//以下為認證verify   下面為剛產生的token hash
tokenString1 := "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJuYmYiOjE0NDQ0Nzg0MDB9.oWULuaWiGsPIvfnnVpmA8kRcUbDA7WKei_EyHB4cUAc"

token1, err := jwt.Parse(tokenString1, func(token *jwt.Token) (interface{}, error) {
  // Don't forget to validate the alg is what you expect:
  if _, ok := token.Method.(*jwt.SigningMethodHMAC); !ok {
     return nil, fmt.Errorf("Unexpected signing method: %v", token.Header["alg"])
  }
  return []byte("test"), nil  //第一個return值要填入我們寫的secret
})

if claims, ok := token1.Claims.(jwt.MapClaims); ok && token1.Valid {
  fmt.Println(claims["foo"], claims["nbf"])
} else {
  fmt.Println(err)
}
```

### 單獨抽出 Verify token func

```go
func VerifyToken(tokenString string) (bool, interface{}) {
	parsedToken, err := jwt.Parse(tokenString, func(_token *jwt.Token) (interface{}, error) {
		if _, ok := _token.Method.(*jwt.SigningMethodHMAC); !ok {
			return nil, fmt.Errorf("Unexpected signing method: %v", _token.Header["alg"])
		}
		return hmacSampleSecret, nil
	})
	if parsedToken == nil {
		return false, "not valid"
	}
	if claims, ok := parsedToken.Claims.(jwt.MapClaims); ok && parsedToken.Valid {
		return parsedToken.Valid, claims["address"]
	} else {
		fmt.Println(err)
		return false, "not valid"
	}
}
```

使用：放入某個需要驗證的 route 裡面

```go
	if c.Get("Authorization") == "" {
		return c.Status(fiber.StatusOK).JSON(fiber.Map{
			"success": false,
			"message": "Please provide Authorization header",
		})
	}
	isValid, userAddress := VerifyToken(c.Get("Authorization"))
	if !isValid || userAddress != Profile.Blockchain_address {
		return c.Status(403).JSON(fiber.Map{
			"success": false,
			"message": "User not authorized",
		})
	}
```

