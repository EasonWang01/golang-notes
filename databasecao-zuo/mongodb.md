# MongoDB

目前推薦使用 [mongo-go-driver](https://github.com/mongodb/mongo-go-driver) 而不是 mgo

[https://github.com/mongodb/mongo-go-driver](https://github.com/mongodb/mongo-go-driver)

## 不錯的文章：

[https://juejin.cn/post/6908063164726771719](https://juejin.cn/post/6908063164726771719)

## 使用 FindOne

```go
func GetProfile(c *fiber.Ctx) error {
	var result bson.M
	userCollection := config.MI.DB.Collection("user")
	ctx, _ := context.WithTimeout(context.Background(), 10*time.Second)
	findOneOpts := options.FindOne()
	singleResult := userCollection.FindOne(ctx, bson.M{"blockchain_address": c.Query("address")}, findOneOpts)

	if err := singleResult.Decode(&result); err == nil {
		fmt.Printf("result: %+v\n", result)
	}

	return c.Status(fiber.StatusOK).JSON(fiber.Map{
		"data":    result,
		"success": true,
		"message": "User profile query successfully",
	})
}
```

## 使用 Push

如果結構中有 array 可使用

```go

	opts1 := options.Update().SetUpsert(true)
	filter1 := bson.M{"blockchain_address": Follower.Follower_blockchain_address}
	update1 := bson.M{
		"$push": bson.M{
			"Following": bson.M{
				"blockchain_address": Follower.Following_blockchain_address,
				"create_time":        utils.MakeTimestamp(),
			},
		},
	}
	result1, err1 := userCollection.UpdateOne(ctx, filter1, update1, opts1)
```

## 完整範例：

![](../.gitbook/assets/jie-tu-20210901-xia-wu-12.11.51.png)

`config/db.go`

```go
package config

import (
	"context"
	"fmt"
	"log"
	"os"
	"time"

	"github.com/joho/godotenv"
	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
	"go.mongodb.org/mongo-driver/mongo/readpref"
)

type MongoInstance struct {
	Client *mongo.Client
	DB     *mongo.Database
}

var MI MongoInstance

func ConnectDB() {
	if os.Getenv("APP_ENV") != "production" {
		err := godotenv.Load()
		if err != nil {
			log.Fatal("Error loading .env file")
		}
	}

	client, err := mongo.NewClient(options.Client().ApplyURI(os.Getenv("MONGO_URI")))
	if err != nil {
		log.Fatal(err)
	}

	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()

	err = client.Connect(ctx)
	if err != nil {
		log.Fatal(err)
	}

	err = client.Ping(ctx, readpref.Primary())
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("Database connected!")

	MI = MongoInstance{
		Client: client,
		DB:     client.Database(os.Getenv("DB")),
	}
}
```

`models/user.go`

```go
package models

import (
	"go.mongodb.org/mongo-driver/bson/primitive"
)

type User struct {
	ID                primitive.ObjectID `json:"_id,omitempty" bson:"_id,omitempty"`
	UserId            string             `json:"userId,omitempty" bson:"userId,omitempty"`
	BlockchainName    string             `json:"blockchainName,omitempty" bson:"blockchainName,omitempty"`
	BlockchainAddress string             `json:"blockchainAddress,omitempty" bson:"blockchainAddress,omitempty"`
	Nickname          string             `json:"nickname,omitempty" bson:"nickname,omitempty"`
	Email             string             `json:"email,omitempty" bson:"email,omitempty"`
	Avatar            string             `json:"avatar,omitempty" bson:"avatar,omitempty"`
	Cover             string             `json:"cover,omitempty" bson:"cover,omitempty"`
	Bio               string             `json:"bio,omitempty" bson:"bio,omitempty"`
	Register_time     string             `json:"register_time,omitempty" bson:"register_time,omitempty"`
	Last_login        string             `json:"last_login,omitempty" bson:"last_login,omitempty"`
	Ip                string             `json:"ip,omitempty" bson:"ip,omitempty"`
}
```

`controllers/userController.go`

```go
package controllers

import (
	config "project_name/config"
	models "project_name/models"
	"context"
	"log"
	"math"
	"strconv"
	"time"

	"github.com/gofiber/fiber/v2"
	"go.mongodb.org/mongo-driver/bson"
	"go.mongodb.org/mongo-driver/mongo/options"
)

func GetAllUser(c *fiber.Ctx) error {
	userCollection := config.MI.DB.Collection("user")
	ctx, _ := context.WithTimeout(context.Background(), 10*time.Second)

	var user []models.User

	filter := bson.M{}
	findOptions := options.Find()

	page, _ := strconv.Atoi(c.Query("page", "1"))
	limitVal, _ := strconv.Atoi(c.Query("limit", "10"))
	var limit int64 = int64(limitVal)

	total, _ := userCollection.CountDocuments(ctx, filter)

	findOptions.SetSkip((int64(page) - 1) * limit)
	findOptions.SetLimit(limit)

	cursor, err := userCollection.Find(ctx, filter, findOptions)
	defer cursor.Close(ctx)

	if err != nil {
		return c.Status(fiber.StatusNotFound).JSON(fiber.Map{
			"success": false,
			"message": "user Not found",
			"error":   err,
		})
	}

	for cursor.Next(ctx) {
		var User models.User
		cursor.Decode(&User)
		user = append(user, User)
	}

	last := math.Ceil(float64(total / limit))
	if last < 1 && total > 0 {
		last = 1
	}

	return c.Status(fiber.StatusOK).JSON(fiber.Map{
		"data":      user,
		"total":     total,
		"page":      page,
		"last_page": last,
		"limit":     limit,
	})
}

func AddUser(c *fiber.Ctx) error {
	userCollection := config.MI.DB.Collection("user")
	ctx, _ := context.WithTimeout(context.Background(), 10*time.Second)
	User := new(models.User)

	if err := c.BodyParser(User); err != nil {
		log.Println(err)
		return c.Status(400).JSON(fiber.Map{
			"success": false,
			"message": "Failed to parse body",
			"error":   err,
		})
	}

	result, err := userCollection.InsertOne(ctx, User)
	if err != nil {
		return c.Status(500).JSON(fiber.Map{
			"success": false,
			"message": "User failed to insert",
			"error":   err,
		})
	}
	return c.Status(fiber.StatusCreated).JSON(fiber.Map{
		"data":    result,
		"success": true,
		"message": "User inserted successfully",
	})

}

```

`main.go`

```go
package main

import (
	"log"
	"os"

	config "project_name/config"
	"project_name/routes"

	"github.com/gofiber/fiber/v2"
	"github.com/gofiber/fiber/v2/middleware/cors"
	"github.com/gofiber/fiber/v2/middleware/logger"
	"github.com/joho/godotenv"
)

func setupRoutes(app *fiber.App) {

	api := app.Group("/api")

	routes.UserRoute(api.Group("/user"))
}

func main() {
	if os.Getenv("APP_ENV") != "production" {
		err := godotenv.Load()
		if err != nil {
			log.Fatal("Error loading .env file")
		}
	}

	app := fiber.New()

	app.Use(cors.New())
	app.Use(logger.New())

	config.ConnectDB()

	setupRoutes(app)

	port := os.Getenv("PORT")
	err := app.Listen(":" + port)

	if err != nil {
		log.Fatal("Error app failed to start")
		panic(err)
	}
}
```

routes/user.go

```go
package routes

import (
	controllers "project-name/controllers"

	"github.com/gofiber/fiber/v2"
)

func UserRoute(route fiber.Router) {
	route.Get("/", controllers.GetAllUser)
	route.Post("/", controllers.AddUser)
}

```

.env

```text
MONGO_URI=mongodb://localhost:27017
DB=project-name
PORT=3000
APP_ENV=development
```

 

