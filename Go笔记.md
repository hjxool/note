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
├── main.go  # 程序入口 路由注册
├── config/db.go  # 配置相关
├── models/user.go  # 只定义数据库表映射的结构体
├── repository/user_repo.go  # 封装数据库操作
├── services/user_service.go  # 处理业务逻辑
├── handlers/user_handler.go  # 前/后置拦截器 字段校验 
├── utils/http_client.go  # 通用方法
```

#### 分层结构示例

```go
// 配置相关 db.go
package config
import (
    "database/sql"
    _ "github.com/go-sql-driver/mysql"
    "log"
)
var DB *sql.DB
func InitDB() {
    dsn := "root:123456@tcp(127.0.0.1:3306)/testdb"
    var err error
    DB, err = sql.Open("mysql", dsn)
    defer DB.Close()
    if err != nil {
        log.Fatal("Failed to connect to DB:", err)
    }
    if err = DB.Ping(); err != nil {
        log.Fatal("DB not reachable:", err)
    }
    log.Println("Connected to MySQL successfully!")
}

// 只定义模型结构体 user.go
package models
type User struct {
    ID       int64  `json:"id"`
    Username string `json:"username"`
    Password string `json:"password"`
}

// 封装数据库操作 user_repo.go
package repository
import (
    "database/sql"
    "project/models"
  	"myapp/config"
)
func CreateUser(user models.User) error {
    _, err := config.DB.Exec("INSERT INTO users (username, password) VALUES (?, ?)", user.Username, user.Password)
    return err
}
func GetUserByUsername(username string) (models.User, error) {
    var user models.User
    row := config.DB.QueryRow("SELECT id, username, password FROM users WHERE username = ?", username)
    err := row.Scan(&user.ID, &user.Username, &user.Password)
    return user, err
}

// 业务逻辑层 调用repository user_service.go
package services
import (
    "database/sql"
    "myapp/models"
    "myapp/repository"
)
func RegisterUser(user models.User) error {
    // 这里可以加业务逻辑，比如检查用户名是否存在
    return repository.CreateUser(user)
}
func LoginUser(username, password string) (bool, error) {
    user, err := repository.GetUserByUsername(username)
    if err != nil {
        return false, err
    }
    return user.Password == password, nil
}

// 前/后置拦截器 字段校验 调用service user_handler.go
package handlers
import (
    "database/sql"
    "net/http"
    "myapp/models"
    "myapp/services"
    "github.com/gin-gonic/gin"
)
func RegisterHandler() gin.HandlerFunc {
    return func(c *gin.Context) {
        username := c.PostForm("username")
        password := c.PostForm("password")
        user := models.User{Username: username, Password: password}
        err := services.RegisterUser(user)
        if err != nil {
            c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
            return
        }
        c.JSON(http.StatusOK, gin.H{"message": "User registered successfully"})
    }
}

// 连接数据库 路由注册 将连接池对象传递到handlers 启动服务 main.go
package main
import (
    "database/sql"
    "log"
    "myapp/handlers"
  	"myapp/config"
    "github.com/gin-gonic/gin"
    _ "github.com/go-sql-driver/mysql"
)

func main() {
  	config.InitDB()
    r := gin.Default()
    r.POST("/register", handlers.RegisterHandler)
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

### 路由参数

```go
r := gin.Default()

// 路径参数 /user/123
r.GET("/user/:id", func(c *gin.Context) { 
  id := c.Param("id")
})

// 查询参数 /search?keyword=golang
r.GET("/search", func(c *gin.Context) { 
  keyword := c.Query("keyword")
})

// JSON请求体
r.POST("/login", func(c *gin.Context) { 
  var body struct {
    Username string `json:"username"`
    Password string `json:"password"`
  }
  err := ctx.BindJSON(&body)
  // body.Username body.Password
})

// Form表单参数
r.POST("/login", func(c *gin.Context) { 
  username := c.PostForm("username")
  password := c.PostForm("password")
})
```

## 连接数据库

```go
package main
import (
	"database/sql"
	"fmt"
	"log"
	"net/http"
	"github.com/gin-gonic/gin"
	"github.com/go-playground/validator/v10"
	_ "github.com/go-sql-driver/mysql"
)
// 必须用大写 对外开放才能被JSON解析器解析
// json标签只影响json里字段名 go文件中访问时还是用大写
type RegisterBody struct {
	Username string `json:"username" validate:"required,min=1"`
	Password string `json:"password" validate:"required,min=6"`
}
func LinkToMysql() {
	validate := validator.New()
	db, err := sql.Open("mysql", "root@tcp(127.0.0.1:3306)/maindb")
	if err != nil {
		log.Fatal(err)
	}
	// 用defer关键词延迟关闭连接 这样做的好处是不论后续程序是否异常退出 都必定执行defer
	defer db.Close()
	// 测试连接 db.Ping 返回error 如果为nil表示连接正常 否则表示连接失败
	err = db.Ping()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("连接数据库成功")
	router := gin.Default()
	router.POST("/register", func(ctx *gin.Context) {
		// PostForm 是解析 Form-Data 格式的请求体的
		// username := ctx.PostForm("username")
		// password := ctx.PostForm("password")

		// 解析JSON请求体 需要先定义请求体结构 可以引入库来辅助校验
		var body RegisterBody
		// 然后用 BindJSON 把JSON请求体解析到结构体
		if err := ctx.BindJSON(&body); err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "请求体解析失败",
				},
			})
			return
		}
		// 结构体字段值是否缺失等校验
		if err := validate.Struct(body); err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: err.Error(),
				},
			})
			return
		}
		// 创建表（如果不存在）表不存在插入数据会报错
		// 字段名 类型 限制
		_, err := db.Exec(`CREATE TABLE IF NOT EXISTS users (
			id INT AUTO_INCREMENT PRIMARY KEY,
			username VARCHAR(50) NOT NULL,
			password VARCHAR(100) NOT NULL
		)`)
		if err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "创建表错误",
				},
			})
			return
		}
		// 插入数据
		// (username, password) 选择列 (?, ?) 占位符
		_, err = db.Exec("INSERT INTO users (username, password) VALUES (?, ?)", body.Username, body.Password)
		if err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "插入数据错误",
				},
			})
			return
		}
		ctx.JSON(http.StatusOK, Response{
			Head: Head{
				Code:    http.StatusOK,
				Message: "数据插入成功",
			},
		})
	})
	// Run 会阻塞程序 因此当前函数不会返回 因此defer db.Close()不会执行 除非程序意外结束
	// 所以在路由的回调函数中 db可以接着用
	router.Run(":8080")
}
```

## 登陆接口

```go
func LoginMain() {
	db, err := sql.Open("mysql", "root@tcp(127.0.0.1:3306)/maindb")
	if err != nil {
		fmt.Println("数据库连接失败:", err)
		return
	}
	defer db.Close()
	router := gin.Default()
	router.POST("/login", LoginHandler(db))
	router.Run(":8080")
}
func LoginHandler(db *sql.DB) gin.HandlerFunc {
	validate := validator.New()
	return func(ctx *gin.Context) {
		var body struct {
			Username string `json:"username" validate:"required,min=1"`
			Password string `json:"password" validate:"required,min=6"`
		}
		if err := ctx.BindJSON(&body); err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: err.Error(),
				},
			})
			return
		}
		if err := validate.Struct(body); err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "用户名或密码格式错误",
				},
			})
			return
		}
		ok, err := LoginServer(db, body.Username, body.Password)
		if err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "查询用户错误",
				},
			})
			return
		}
		if !ok {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "密码错误",
				},
			})
			return
		}
		ctx.JSON(http.StatusOK, Response{
			Head: Head{
				Code:    http.StatusOK,
				Message: "登录成功",
			},
		})
	}
}
func LoginServer(db *sql.DB, username, password string) (bool, error) {
	user, err := LoginRepo(db, username)
	if err != nil {
		return false, err
	}
	return user.Password == password, nil
}
type LoginUser struct {
	ID       int
	Username string
	Password string
}
func LoginRepo(db *sql.DB, username string) (LoginUser, error) {
	var user LoginUser
	// QueryRow 单行查询
	row := db.QueryRow("SELECT id, username, password FROM users WHERE username = ?", username)
	// Scan 按 SELECT顺序赋值
	err := row.Scan(&user.ID, &user.Username, &user.Password)
	return user, err
}
```

## JWT 登录认证

```go
secretKey := []byte("sercet_key")
getToken := func(user LoginUser) (string, error) {
		claims := jwt.MapClaims{
			"username": user.Username,
			"password": user.Password,
			"exp":      time.Now().Add(time.Second * 20).Unix(), // 20秒后过期
		}
		// 还没有签名的Token对象 记录了签名算法和声明
		token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
		// 签名并获取完整的token字符串 对称加密算法要求传入[]byte参数作为密钥
		return token.SignedString(secretKey)
	}
	loginHandler := func(ctx *gin.Context) {
		var body LoginUser
		if err := ctx.BindJSON(&body); err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: err.Error(),
				},
			})
			return
		}
		token, err := getToken(body)
		if err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: err.Error(),
				},
			})
			return
		}
		ctx.JSON(http.StatusOK, Response{
			Head: Head{
				Code:    http.StatusOK,
				Message: "success",
			},
			Data: Data{ // type Data map[string]any
				// 这里要用小写 这样在返回给前端时 json里才是小写 不需要遵守导出规则大写
				"token": token,
			},
		})
	}
	verifyToken := func(ctx *gin.Context) {
		token := ctx.GetHeader("Authorization")
		// TrimPrefix会删除前缀 如果没有对应前缀则返回原字符串
		token = strings.TrimPrefix(token, "Bearer ")
		if token == "" {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "token不能为空",
				},
			})
			ctx.Abort()
			return
		}
		claims := jwt.MapClaims{} // 声明空结构体
		// 验证token 将解密后的参数放到 claims 里
		_, err := jwt.ParseWithClaims(token, claims, func(t *jwt.Token) (any, error) {
			return secretKey, nil
		})
		if err != nil {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "无效token",
				},
			})
			ctx.Abort()
			return
		}
		// 把解密出的信息放到 当前请求链上的 *gin.Context中
		ctx.Set("userInfo", map[string]any{
			"username": claims["username"],
			"password": claims["password"],
		})
		ctx.Next()
	}

	r.POST("/login", loginHandler)
	r.GET("/verify", verifyToken, func(ctx *gin.Context) {
		userInfo, exist := ctx.Get("userInfo")
		if !exist {
			ctx.JSON(http.StatusOK, Response{
				Head: Head{
					Code:    http.StatusBadRequest,
					Message: "用户信息不存在",
				},
			})
			return
		}
		ctx.JSON(http.StatusOK, Response{
			Head: Head{
				Code:    http.StatusOK,
				Message: "success",
			},
			Data: Data{
				"username": userInfo.(map[string]any)["username"],
				"password": userInfo.(map[string]any)["password"],
			},
		})
	})
```

## SQL语句

```sql
CREATE TABLE permissions ( 
  id INT AUTO_INCREMENT PRIMARY KEY, # 单字段直接声明主键
  name VARCHAR(50) NOT NULL UNIQUE # UNIQUE 表示唯一值 再插入相同值会报错
);

CREATE TABLE user_roles (
    user_id INT,
    role_id INT,
    PRIMARY KEY (user_id, role_id), # 组合式主键 单独一行声明
    FOREIGN KEY (user_id) REFERENCES users(id), # 声明外键 以及关联表的唯一性约束键
    FOREIGN KEY (role_id) REFERENCES roles(id)
);
```