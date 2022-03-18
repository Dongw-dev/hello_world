# HTTP 

HTTP（HyperText Transfer Protocal）超文本传输协议

HTTP协议定义Web客户端如何从Web服务端请求页面，以及客户端如何把页面传送给客户端。

**HTTP协议特点**
- 灵活可扩展
- 可靠传输    
  基于TCP协议特点
- 客户端-服务端模型    
  客户端打开链接发送一个请求，等待直到客户端响应。
- 无状态    
  状态指的是通信过程的上下文信息。HTTP协议每次请求都是独立的，默认不保留任何相关状态信息。
  协议本身对于请求和响应之间的通信状态不进行保存。
**HTTP版本**
- http/1.0
- http/1.1
- http/2.0
- http/3.0    
  采用quic协议
  
  
## HTTP报文
用于HTTP协议用于交互的信息叫做HTTP报文。    
客户端的HTTP报文叫请求报文。
服务端的HTTP报文叫响应报文。

**HTTP报文结构**
- 请求报文
  
  请求方法 | URL/URI | 协议版本  --- 请求行
  
  头部字段:| 值                  --- 请求头部
  
  请求数据                       --- 内容实体（可为空）
  
  ```
    GET /test HTTP/1.1
    Host: test.com
    Connection: keep-alive
    content-length: 194
    content-type: text/plain;charset=UTF-8

    
    name=test&age=18
  ```
  
- 响应报文
  
  协议版本 | 状态码 | 状态码描述  --- 状态行
  
  头部字段名:| 值                 --- 响应头部
  
  响应正文                        --- 内容实体
  
  ```
    HTTP/1.1 200 OK
    Date: Tue, 10 Jul 2021
    content-length: 0
    content-type: text/html; charset=utf-8
    
    <html>...
  ```

**HTTP状态码**
- 2XX 成功
  - 200 正确返回
  - 204 正确返回，响应报文不含实体部分
  - 206 返回部分
- 3XX 重定向
  - 301 永久重定向
  - 302 临时重定向
  - 303 
  - 304 Not Modified 表示服务器允许访问资源
  - 307
- 4XX 客户端错误
  - 400 请求报文存在错误
  - 401 没有授权
  - 403 访问资源被拒绝，可在响应报文实体部分
  - 404 在服务器没有找到资源
- 5XX 服务端错误
  - 500 服务器执行发生错误
  - 501 不支持当前请求所需要的操作
  - 503 服务器不可用，无法处理请求
## HTTPS

HTTPS基于HTTP协议，使用SSL/TLS(SSL3.0)协议提供加密数据处理, 验证身份以及数据完整性。

**SSL/TLS实现**
- 非对称算法，实现身份认证和密钥协商
- 对称算法，利用协商的密钥对数据加密
- 散列函数，验证信息完整性

SSL + TLS = 非对称算法 + 对称算法 + 散列函数
            身份认证 ==> 数据加密==>信息校验 

**HTTPS与HTTP的区别**
- HTTPS需要到CA申请证书
- HTTP协议运行在TCP之上，传送内容都是明文;HTTPS协议运行在SSL/TLS之上，SSL/TLS运行在TCP之上，所传输的内容是加密的。
- HTTP协议和HTTPS用的是完全不同的连接方式，端口号也不同, HTTP是80, HTTPS是443。
- HTTP协议是无状态的，HTTPS是HTTP+SSL协议构建的加密传输，身份认证的网络协议，有效防止运行商劫持，比HTTP协议安全


