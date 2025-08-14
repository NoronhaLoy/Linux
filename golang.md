# Comprehensive Golang Guide for SRE / DevOps / Platform / Cloud Engineers with Examples

## 1. Basics

* Installation: `sudo apt install golang-go` or `brew install go`
* Run script: `go run script.go`
* Compile: `go build script.go`
* Package: `package main`
* Import: `import "fmt"`

**Example:**

```go
package main

import "fmt"

func main() {
    name := "Server"
    fmt.Println("Hello", name)
}
```

---

## 2. Variables & Constants

```go
var x int = 10
y := 20
const PI = 3.14
fmt.Println(x, y, PI)
```

---

## 3. Data Types

* int, float64, string, bool
* Array: `[5]int{1,2,3,4,5}`
* Slice: `s := []int{1,2,3}`
* Map: `m := map[string]int{"a":1, "b":2}`
* Structs for complex objects

**Example:**

```go
type Server struct {
    Name string
    Status string
}

srv := Server{Name: "web01", Status: "active"}
fmt.Println(srv.Name, srv.Status)
```

---

## 4. Conditionals

```go
x := 7
if x > 5 {
    fmt.Println("x > 5")
} else if x == 5 {
    fmt.Println("x = 5")
} else {
    fmt.Println("x < 5")
}
```

---

## 5. Loops

```go
// For loop
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// Range over slice
fruits := []string{"apple","banana"}
for i, f := range fruits {
    fmt.Println(i, f)
}

// Infinite loop
for {
    break
}
```

---

## 6. Functions

```go
func greet(name string) string {
    return "Hello " + name
}
fmt.Println(greet("World"))

// Multiple return
func swap(a, b int) (int,int) {
    return b, a
}
x, y := swap(1,2)
```

---

## 7. Pointers

```go
x := 10
p := &x
fmt.Println(*p)  // Dereference
*p = 20
fmt.Println(x)   // 20
```

---

## 8. Structs & Methods

```go
type Server struct {
    Name string
}

func (s Server) greet() {
    fmt.Println("Hello from", s.Name)
}

srv := Server{Name:"web01"}
srv.greet()
```

---

## 9. Interfaces

```go
type Notifier interface {
    Notify(msg string)
}

type Email struct{}

func (e Email) Notify(msg string) {
    fmt.Println("Email:", msg)
}

var n Notifier = Email{}
n.Notify("Server down")
```

---

## 10. Concurrency

```go
// Goroutine
go func() { fmt.Println("Hello from goroutine") }()

// Channels
ch := make(chan string)
go func() { ch <- "ping" }()
msg := <-ch
fmt.Println(msg)

// Buffered channel
buf := make(chan int, 2)
buf <- 1
buf <- 2
```

---

## 11. Error Handling

```go
f, err := os.Open("file.txt")
if err != nil {
    fmt.Println("Error:", err)
} else {
    fmt.Println(f.Name())
}
```

---

## 12. File I/O

```go
// Write file
os.WriteFile("file.txt", []byte("Hello World"), 0644)

// Read file
data, _ := os.ReadFile("file.txt")
fmt.Println(string(data))
```

---

## 13. Packages & Modules

```bash
go mod init myapp
go get github.com/gin-gonic/gin
```

```go
import "github.com/gin-gonic/gin"

r := gin.Default()
r.GET("/", func(c *gin.Context) { c.String(200,"OK") })
r.Run()
```

---

## 14. Networking & HTTP

```go
// HTTP GET
resp, _ := http.Get("https://example.com")
body, _ := io.ReadAll(resp.Body)
fmt.Println(string(body))
```

```go
// Simple HTTP Server
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request){
    fmt.Fprintf(w, "Hello World")
})
http.ListenAndServe(":8080", nil)
```

---

## 15. Logging

```go
log.Println("Server started")
log.Fatalf("Fatal error: %v", err)
```

---

## 16. Testing

```go
func Add(a,b int) int { return a+b }

func TestAdd(t *testing.T) {
    result := Add(2,3)
    if result != 5 { t.Error("Expected 5") }
}
```

---

## 17. SRE / DevOps Examples

**Health Check:**

```go
services := []string{"nginx","mysql"}
for _, svc := range services {
    cmd := exec.Command("systemctl","is-active",svc)
    out,_ := cmd.Output()
    fmt.Println(svc,string(out))
}
```

**Deployment Automation:**

```go
src := "/var/app/releases/v1"
dst := "/var/app/current"
os.RemoveAll(dst)
os.MkdirAll(dst,0755)
exec.Command("cp","-r",src+"/*",dst).Run()
```

**Log Parsing:**

```go
data,_ := os.ReadFile("/var/log/syslog")
lines := strings.Split(string(data), "\n")
for _, line := range lines {
    if strings.Contains(line,"ERROR") {
        fmt.Println(line)
    }
}
```

---

## 18. Best Practices

* Always handle errors
* Use `go fmt` and `golint`
* Modular packages
* Use goroutines and channels for concurrency
* Avoid global variables
* Use `context` for cancellations and timeouts
* Proper logging
