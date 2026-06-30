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

## interface

- **多态**是“**行为**的通用”
- **注意**！不只是结构体可以继承实现，任何自定义类型只要实现了接口中定义的内容，它就实现了该接口
  - 除了结构体，基础类型、切片、map映射、甚至func函数，只要通过`type`关键字进行了重新定义，都可以实现接口

### 作为接口限制行为

```go
// 虽然go语言中没有显示继承声明 但这是为了极致的解耦(开闭原则) 显式声明必须知道接口的存在
// 如第三方库中有个结构体没有显式声明实现任何接口 在Java中就拿它毫无办法
// 但go中只要引入该结构体 声明的方法正好符合接口 就可以将该结构体当作对应interface多态使用

// interface多态示例
type Payer interface { // 统一的行为标准
    Pay(amount int) bool
}
type WeChatPay struct{}
func (w *WeChatPay) Pay(amount int) { /* 微信支付逻辑 */ }
type Alipay struct{}
func (a *Alipay) Pay(amount int) { /* 支付宝支付逻辑 */ }
func HandlePayment(p Payer, amount int) {
    p.Pay(amount) // 只要实现了 Pay 方法的结构体 都能传进来
}

// 但隐式继承也有其不便 为了检查是否完全继承实现某个接口 可以用如下方法编译时检查
// 用 _匿名变量 避免编译器因没使用该变量而报错
// (*...)() 对传入的变量进行 强制类型转换
// 使用 nil空指针 是因为它在内存里不占空间 不需要实际去 new 一个巨大的结构体
var _ 某个interface = (*自定义结构体)(nil)
```

### 作为类型约束/类型集合

```go
// 1.18版后的go语言 interface不止作为方法的集合 还可以作为类型集合
type Number interface {
    int64 | float64
}
// 注意！只能用于泛型的类型约束 绝对不能用来声明普通的变量
func Sum[T Number](n T) T {...}
// 也视作类型集合 包含所有实现了内部方法的类型
type ReadWriter interface {
    Read()
    Write()
}

// 不要写到一起！
// 违背单一职责原则 interface {int64 | float64} 这样的写法就应该用于泛型 而不是接口实现
// 类型约束就是类型约束 不要和接口实现混到一起用
type SerializableNumber interface {
    int64 | float64
    Serialize() string
}
```

## 泛型

- **泛型**是“**数据类型**的通用”
- 任何泛型参数后都**必须有类型约束**

```go
// comparable 不是具体类型 是interface类型约束 表示该类型的值必须支持 比较运算符
func SumIntsOrFloats[K comparable, V int64 | float64](m map[K]V) V {
	var sum V
	for _, v := range m {
		sum += v
	}
	return sum
}
ints := map[string]int64{
  "first":  34,
  "second": 12,
}
floats := map[string]float64{
  "first":  35.98,
  "second": 26.99,
}
fmt.Printf("泛型求和: %v 和 %v\n", SumIntsOrFloats[string, int64](ints), SumIntsOrFloats[string, float64](floats))
// 也可以不传入类型参数 由编译器推断类型 但注意 对于不接收参数的泛型函数 就需要传入类型参数
fmt.Printf("自动推断类型的泛型求和: %v 和 %v\n", SumIntsOrFloats(ints), SumIntsOrFloats(floats))

// 如果想不限制泛型约束 用any
func Index[T any](s []T, x T) int {}

// 泛型结构体
type Node[T any] struct {
	Val  T
	Next *Node[T]
}
type List[T any] struct {
	Head *Node[T]
}
// 注意：给泛型结构体绑定方法时 接收者必须写成 Xxx[T]
func (l *List[T]) Push(val T) {}
// 实例化
// 必须显式地用方括号 [...] 指定具体的类型！与泛型函数不同 函数可以依靠编译器自动推导 结构体必须写明
intList := List[int]{} 
intList.Push(10)
intList.Push(20)
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
├── my_project/                  # 你的整个大项目根目录
│   ├── common/                  # ❌ 注意：这里面绝对不能有 package main
│   │   ├── go.mod               # module 名为 example.com/common
│   │   └── utils/
│   │       └── crypto.go        # 包名是 package utils (公共加密工具)
│   │
│   ├── user_service/            # 微服务 A：用户服务
│   │   ├── go.mod               # module 名为 example.com/usersvc
│   │   ├── main.go              # package main 👈 这里是用户服务的启动入口
│   │   └── handler/
│   │       └── user.go          # package handler (普通的业务包)
│   │
│   └── order_service/           # 微服务 B：订单服务
│       ├── go.mod               # module 名为 example.com/ordersvc
│       ├── main.go              # package main 👈 这里是订单服务的启动入口
│       └── handler/
│           └── order.go         # package handler (普通的业务包)
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

### 赋值

```go
// go语言中 方括号 [] 有特殊且固定的语法用途 因此不能用来给数组赋值
// 定义类型：[]string声明切片/数组 map[string]int声明Map类型
// 访问索引/键：arr[0]读取或修改数组元素 myMap["name"]访问字典里的键
// 如果写成 []string["xxx"] Go编译器会认为是对一个名叫 []string 的类型进行“索引取值”操作 语法逻辑上冲突

// 因此对于 复合类型（切片、数组、结构体、Map）初始化和值构造一律使用 类型名{元素...} 的格式
// 字符串切片
names := []string{"张三", "李四"} 
// 整型数组
primes := [3]int{2, 3, 5}

// 键值对初始化 直接在花括号里填入 key: value
userAge := map[string]int{"alice": 18, "bob": 20} 
// 创建一个空的 map 
emptyMap := map[string]string{}

type User struct {
    Name string
    Age  int
}
// 结构体赋值依然是用 {}
u := User{Name: "王五", Age: 30}

// 创建map
// make 函数形式
messages := make(map[string]string)
// 字面量形式
messages := map[string]string{}
// 虽然两种写法在底层完全等价 但 make 函数还支持第二个参数用来指定 Map 的初始容量 因此更常用
// 因为Map底层是哈希表 随着数据增加 Map会自动扩容 每次扩容Go底层都需要重新申请更大的内存并把老数据搬过去 这非常消耗性能
// 用make可以预估Map里面会装多少数据 提前分配空间 后续超出依然会自动扩容
```

### 是否可以使用指针

- 值类型：需要通过指针才能修改原值，都则只进行拷贝，尤其对于结构体性能较差，所以一般用指针引用
  - 基础数据类型
  - 结构体
  - 固定长度的数组
- 自带指针的容器：底层设计已经实现了指针，不需要再显式声明指针引用，也不会进行拷贝
  - interface接口
  - 切片
  - map
  - channel
  - func

## channel通道与Goroutines协程

- 注意！`chan T`与`[]T`、`map[K]V`、`*T`、`func(...)`一样是类型构造语法，不是泛型，字面量语法就是这样

```go
// 协程就一个关键字 go fn 即可
func say(s string){...}
go say("world")

// 创建通道 只能用make
ch := make(chan int)
// 通道是协程间传递数据的队列与同步机制
ch <- value   // 把 value 放进通道
value := <-ch // 从通道取出一个值

// make(chan int) 默认通道容量为0 这就是 无缓冲通道
// 无缓冲通道没有存储空间 因此必须同步交接
// 缓冲通道 容量 > 0
// 队列满时 ch <- v 存操作阻塞 直到有人 <-ch
// 队列为空时 <-ch 取操作阻塞 直到有人 ch <- v

// 通道如何让 goroutine 协作？
go sum(s[:len(s)/2], c) // 两个 goroutine 并行计算
go sum(s[len(s)/2:], c) // 每个 goroutine 计算完后把结果发送到通道
x, y := <-c, <-c // 主 goroutine 通过 <-c 等待两个结果

// 关闭通道
go func xxx(ch chan int){
  close(ch)
}(ch)
// 接收方可以通过第二个返回值判断通道是否关闭
v, ok := <-ch // ok == true 成功取到值(无论通道是否关闭) ok == false 没有值了且通道已关闭
// for range 会持续接收直到通道关闭
for i := range ch
// 通道关闭不是资源释放 而是“发送结束”的信号 因此不需要经常关闭

// 只读通道
func consumer(ch <-chan int)
// 只写通道
func producer(ch chan<- int)
```

### 退出通道模式

```go
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
    // select 会阻塞 直到某个 case 可以执行
		select {
      // 如果多个 case 同时就绪 会随机选择一个执行
      case c <- x: // x值填入通道
        x, y = y, x+y // 更新x y值
      case <-quit: // 当前示例中只有go func()取够10次后才会触发
        fmt.Println("quit")
        return
      default: // 如果所有通道操作都阻塞 会立即执行 default
      
		}
	}
}

func main() {
	c := make(chan int) // 用于正常工作的通道
	quit := make(chan int) // 作为退出信号
  // 协程执行一个匿名函数
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c) // 无缓冲通道 阻塞等待有值填入
		}
		quit <- 0 // 全部取完以后 quit通道填入一个值作为信号
	}()
	fibonacci(c, quit)
}
```

## 并发锁

```go
// sync.Mutex 互斥锁
type SafeCounter struct {
	mu sync.Mutex // 必须要用sync.Mutex控制
	v  map[string]int
}

func (c *SafeCounter) Inc(key string) {
  // 使用 Lock/Unlock 包围临界区
  // Lock 和 Unlock 之间的代码只能被一个 goroutine 执行
	c.mu.Lock()
	c.v[key]++
	c.mu.Unlock()
}

func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	defer c.mu.Unlock() // 使用 defer 确保 Unlock 一定执行 否则锁无法被释放
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}

// sync.RWMutex 读写锁
type SafeMap struct {
    mu  sync.RWMutex
    m   map[string]int
}
func (s *SafeMap) Get(key string) int {
    s.mu.RLock() // 读锁：允许多个读者并发
    defer s.mu.RUnlock()
    return s.m[key]
}
func (s *SafeMap) Set(key string, value int) {
    s.mu.Lock() // 写锁：必须独占
    defer s.mu.Unlock()
    s.m[key] = value
}
func main() {
    s := &SafeMap{m: make(map[string]int)}
    s.Set("count", 0)

    // 启动 5 个读者 goroutine
    for i := 0; i < 5; i++ {
        go func(id int) {
            for {
                v := s.Get("count")
                fmt.Printf("Reader %d read: %d\n", id, v)
                time.Sleep(100 * time.Millisecond)
            }
        }(i)
    }

    // 启动 1 个写者 goroutine
    go func() {
        for i := 1; i <= 5; i++ {
            time.Sleep(500 * time.Millisecond)
            fmt.Println("Writer updating...")
            s.Set("count", i)
        }
    }()

    time.Sleep(4 * time.Second)
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
      // JSON 返回格式{"id":"1","title":"Blue Train"} 更紧凑
			ctx.JSON(http.StatusOK, gin.H{"message": "登录成功"})
      // IndentedJSON 返回带数据结构的格式 适合开发调试时使用
      ctx.IndentedJSON(...)
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