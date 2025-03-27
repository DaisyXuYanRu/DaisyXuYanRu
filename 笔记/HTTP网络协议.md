# HTTP 网络协议详解

## 1. 什么是 HTTP？

HTTP（**H**yper**T**ext ​**T**ransfer ​**P**rotocol，超文本传输协议）是一种用于分布式、协作式和超媒体信息系统的应用层协议。它是 Web 发展的基石，定义了客户端（如浏览器）与服务器之间的数据通信方式。

---

## 2. HTTP 请求概述

### 2.1 URL 结构

在 HTTP 通信中，​**URL（Uniform Resource Locator，统一资源定位符）​** 用于标识网络资源的位置。一个完整的 URL 由以下几部分组成：
协议://主机名[:端口]/路径[?查询参数#片段]

**示例**：  
`https://www.example.com:443/path/to/resource?key=value#section`

| 组件          | 说明                                 |
| ------------- | ------------------------------------ |
| ​**协议**     | 通常为 HTTP 或 HTTPS                 |
| ​**主机名**   | 服务器域名或 IP 地址                 |
| ​**端口**     | HTTP 默认端口 80，HTTPS 默认端口 443 |
| ​**路径**     | 资源的具体位置                       |
| ​**查询参数** | 以 `?` 分隔的键值对，如 `key=value`  |
| ​**片段**     | 以 `#` 开头的锚点，常用于页面内导航  |

---

### 2.2 HTTP 请求方法

| 方法         | 作用                             |
| ------------ | -------------------------------- |
| ​**GET**     | 请求指定资源，常用于获取数据     |
| ​**POST**    | 提交数据给服务器，常用于表单提交 |
| ​**PUT**     | 更新指定资源                     |
| ​**DELETE**  | 删除指定资源                     |
| ​**HEAD**    | 类似 GET，但不返回响应体         |
| ​**OPTIONS** | 查询服务器支持的 HTTP 方法       |
| ​**PATCH**   | 对资源进行部分更新               |

---

### 2.3 HTTP 请求结构

请求由三部分组成：

```text
请求行：方法 + URL + HTTP版本
请求头：包含元信息，如 `Content-Type`
请求体：可选，主要用于 POST/PUT 请求
```

示例：

```json
POST /api/login HTTP/1.1
Host: www.example.com
Content-Type: application/json
Content-Length: 50

{
  "username": "test",
  "password": "123456"
}
```

## 3. HTTP 响应概述

### 3.1 HTTP 响应结构

服务器的响应由 状态行、响应头部 和 响应体 组成：

```json
状态行：HTTP 版本 + 状态码 + 状态描述
响应头部：服务器返回的元信息，如 `Content-Type`
响应体：服务器返回的数据

```

```json
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 123

<html>
  <body>Hello, World!</body>
</html>

```

### 3.2 HTTP 响应状态码

HTTP 状态码用于指示请求的结果，常见的有：

| 状态码 | 含义                  |
| ------ | --------------------- |
| 200    | OK                    |
| 301    | Moved Permanently     |
| 302    | Found                 |
| 400    | Bad Request           |
| 401    | Unauthorized          |
| 403    | Forbidden             |
| 404    | Not Found             |
| 500    | Internal Server Error |
| 状态码 | 含义                  |
| ------ | --------------        |
| 200    | 请求成功              |
| 301    | 资源永久重定向        |
| 302    | 资源临时重定向        |
| 400    | 请求格式错误          |
| 401    | 需要身份验证          |
| 403    | 服务器拒绝请求        |
| 404    | 资源未找到            |
| 500    | 服务器内部错误        |

## 总结

`HTTP` 是一种无状态协议，意味着每个请求都是独立的，服务器无法记住用户的上一个请求。为了实现用户会话管理，通常使用以下方法：

`Cooki`e：存储在客户端的键值对，用于保存会话信息或标识用户身份。每次请求时，客户端会自动将相关 Cookie 发送给服务器。

`Session`：服务器端保存的会话存储，通过唯一的会话标识符（通常是存储在 Cookie 中的 Session ID）来关联用户的请求和数据。

`JWT（JSON Web Token）`：一种用于身份验证和信息交换的令牌，采用 JSON 格式，签名后传递用户身份信息，通常用于分布式应用中，无需存储会话状态。

此外，`HTTPS`（HTTP Secure） 是在 HTTP 协议的基础上加入 SSL/TLS 加密层，通过加密数据传输，提高了数据的安全性，防止数据在传输过程中被窃取或篡改。
