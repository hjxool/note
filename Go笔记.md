## 常用占位符

```go
%d    // 整数
%f    // 浮点数  
%s    // 字符串
%v    // 默认格式
%t    // 布尔值
%q    // 带引号字符串
%T    // 类型
```

## 准备

### 项目结构

```
myproject/
 ├── go.mod
 ├── main.go        # 入口 go build编译 或者go run .运行
 └── utils/ 				# 工具包
      └── utils.go  # 因为没有main函数 所以只能当库包 不影响go build时 可执行程序必须同属一个包 的规则
# 如果是微服务
myproject/
 ├── go.mod
 ├── cmd/						# 不同服务 每个都是单独go build ./cmd/xxx编译 或者go run ./cmd/xxx独立运行
 │    ├── mainapp/
 │    │    └── main.go
 │    ├── utils/
 │    │    └── main.go
 │    └── other/
 │         └── main.go
 └── internal/
      └── common.go
# 分层结构
project/
├── main.go              // 程序入口
├── config/              // 配置相关
│   └── config.go
├── db/                  // 数据库初始化与封装
│   └── mysql.go
├── models/              // 数据模型定义
│   └── user.go
├── handlers/            // 路由处理函数
│   └── user_handler.go
├── routes/              // 路由注册
│   └── routes.go
```

#### 分层结构示例

```go
// 配置相关 config.go
package config
import "fmt"
type DBConfig struct {
    User     string
    Password string
    Host     string
    Port     int
    Name     string
}
func (c DBConfig) DSN() string {
    return fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?parseTime=true",
        c.User, c.Password, c.Host, c.Port, c.Name)
}

// 数据库初始化与封装 mysql.go
package db
import (
    "database/sql"
    "log"
    _ "github.com/go-sql-driver/mysql"
    "your_project/config"
)
var DB *sql.DB
func Init(cfg config.DBConfig) {
    var err error
    DB, err = sql.Open("mysql", cfg.DSN())
    if err != nil {
        log.Fatal("Failed to connect to DB:", err)
    }
    // 设置连接池参数
    DB.SetMaxOpenConns(20)   // 最大打开连接数
    DB.SetMaxIdleConns(10)   // 最大空闲连接数
    DB.SetConnMaxLifetime(0) // 连接可复用的最大时间，0 表示无限制
    if err = DB.Ping(); err != nil {
        log.Fatal("Failed to ping DB:", err)
    }
}

// 数据模型定义 user.go
package models
type User struct {
    ID       int
    Username string
    Password string
}

// 路由处理函数 user_handler.go
package handlers
import (
    "net/http"
    "your_project/db"
    "github.com/gin-gonic/gin"
)
func Register(c *gin.Context) {
    username := c.PostForm("username")
    password := c.PostForm("password")
    _, err := db.DB.Exec("INSERT INTO users (username, password) VALUES (?, ?)", username, password)
    if err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        return
    }
    c.JSON(http.StatusOK, gin.H{"message": "User registered successfully"})
}

// 路由注册 routes.go
package routes
import (
    "your_project/handlers"
    "github.com/gin-gonic/gin"
)
func SetupRouter() *gin.Engine {
    r := gin.Default()
    r.POST("/register", handlers.Register)
    return r
}

// 程序入口 main.go
package main
import (
    "your_project/config"
    "your_project/db"
    "your_project/routes"
)
func main() {
    cfg := config.DBConfig{
        User:     "root",
        Password: "123456",
        Host:     "127.0.0.1",
        Port:     3306,
        Name:     "testdb",
    }
    db.Init(cfg) // 初始化数据库连接池
    r := routes.SetupRouter()
    r.Run(":8080")
}
```

### 初始化

```
go mod init myproject // 初始化 这会生成一个 go.mod 文件 记录项目的模块名和依赖
go get github.com/gin-gonic/gin // 安装依赖
go mod tidy // 删除依赖
go mod download // 拉取依赖到本地

// 项目编译成一个二进制文件（例如 Linux 下是 myproject，Windows 下是 myproject.exe）
// 是一个完整的可执行程序，可以在没有 Go 环境的机器上运行
go build
```

- main.go

  ```go
  // 包名 同一目录下所有 .go 文件必须属于同一个包 且整个包只能有一个 main() 函数
  // 同一个包的多个 .go 文件会被编译器视为一个整体
  // 所以只要函数、变量、结构体是 公开的（首字母大写）就能在同包的其他文件里直接使用
  package main
  // 不同包则要引入后使用
  import (
    // 项目名/域名 包名(文件目录)
      "myproject/utils"  // 引入 utils 包
  )
  
  func main() {
    // 选择要运行的逻辑
    // RunTest()
    // RunGinServer()
    RunHttpServer()
    
    // 使用引入的包 包名.方法/变量
    utils.PrintHello("侯")
  }
  ```

## 变量与函数

```go
package main
import "fmt"
type User struct {
	Name string
	Age int
}
func (u *User) SayHello() string {
	return fmt.Sprintf("Hello, 我是 %s, 我今年 %d 岁", u.Name, u.Age)
}
func main() {
	users := []User{
		{Name: "test1", Age: 10},
		{Name: "test2", Age: 20},
		{Name: "test3", Age: 30},
	}
	total := 0
	for _,user := range users {
		fmt.Println(user.SayHello())
		total += user.Age
	}
	fmt.Printf("平均年龄是：%d\n", total / len(users))
}
```

## http服务

```go
package main
import (
	"fmt"
	"net/http"
)
func homeHandler(w http.ResponseWriter, r *http.Request)  {
	fmt.Fprintf(w, "Welcome to Home!")
}
func userHandler(w http.ResponseWriter, r *http.Request)  {
	if r.Method == http.MethodGet {
		fmt.Fprintf(w, "Get User Info")
	} else if r.Method == http.MethodPost {
		fmt.Fprintf(w, "Create User")
	} else {
		http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
	}
}
func main()  {
	http.HandleFunc("/", homeHandler) // 访问根路径时 调用对应函数
	http.HandleFunc("/user", userHandler)

	fmt.Println("Server running on :8080")
	http.ListenAndServe(":8080", nil) // 启动服务 监听 8080 端口
}
```

## Gin框架服务

```go
package main
import (
    "github.com/gin-gonic/gin"
    "net/http"
)
type Head struct {
	Code    int    `json:"code"`
	Message string `json:"message"`
}
type Data map[string]any
type Response struct {
	Head Head `json:"head"`
	Data Data `json:"data"`
}
func GinRegisterAndLogin() {
	router := gin.Default()
	router.POST("/register", func(ctx *gin.Context) {
		username := ctx.PostForm("username")
		password := ctx.PostForm("password")
		res := Response{
			Head: Head{
				Code:    http.StatusOK,
				Message: "",
			},
			Data: Data{
				"total": 10,
				"data":  []any{},
			},
		}
		// 校验
		if username == "" || password == "" {
			res.Head.Code = http.StatusBadRequest
			res.Head.Message = "缺失用户名或密码"
			res.Data = Data{}
			ctx.JSON(http.StatusBadRequest, res)
			return
		}
		res.Head.Message = "写入"
		// 写入
		ctx.JSON(http.StatusOK, res)
	})
	router.POST("/login", func(ctx *gin.Context) {
		username := ctx.PostForm("username")
		password := ctx.PostForm("password")
		// 校验
		if username == "admin" && password == "123" {
			ctx.JSON(http.StatusOK, gin.H{"message": "登录成功"})
		} else {
			ctx.JSON(http.StatusUnauthorized, gin.H{"error": "密码错误"})
		}
	})
	router.Run(":8080")
}
```