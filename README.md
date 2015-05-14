WEB
==

把自己的java写的web框架改了撑了golong的,比较轻量级

## Web配置

```go
type ServerConfig struct {
	Addr           string
	Port           int
	ReadTimeout    time.Duration // 读的最大Timeout时间
	WriteTimeout   time.Duration // 写的最大Timeout时间
	MaxHeaderBytes int           // 请求头的最大长度
	TLSConfig      *tls.Config   // 配置TLS
}
```

## 例子

```go
package web

import (
	"errors"
	"net/http"
	"testing"
	"time"
)

func TestServer(t *testing.T) {
	server := NewServer(nil)
	server.Regedit("/a/{name}/123", GET, Service)
	server.Regedit("/a/123/{name}", GET, testService)
	server.AddFilter("/*", AccessLogFilter)
	server.Start()
	time.Sleep(time.Second * 20)
	server.Stop()
}

func Service(request *http.Request, param map[string]string, reply *Reply) {
	reply.With("123" + param["name"])
	panic(errors.New("test error"))
}

func testService(request *http.Request, param map[string]string, reply *Reply) {
	reply.With(param["name"])
}
```






