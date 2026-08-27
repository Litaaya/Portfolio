---
title: "REST API & HTTP"
date: 2026-08-27
draft: false
tags: ["REST-API", "HTTP"]
description: "Một vài kiến thức cơ bản về rest api và http"
---
> Http là protocol còn REST là một phong cách kiến trúc dùng để thiết kế API

---
# API

Trong lĩnh vực công nghệ thông tin thì thuật ngữ `API` được nhắc đến khá nhiều, vậy câu hỏi ở đây là `API` là gì ? `API` là viết tắt cho `Application Programming Interface`, hiểu là một giao diện cho phép hai chương trình hoặc hệ thống giao tiếp với nhau. Có thể hình dung theo kiểu application A gửi request tới application B thông qua hoặc bằng API vậy.

Ví dụ ở dưới cho thấy ứng dụng thời tiết gửi request để lấy nhiệt độ của thành phố Hồ Chí Minh, nó sẽ gửi request như bên dưới và nhận về kết quả:
```http request
GET /weather?city=ho-chi-minh
```
```json
{
    "city": "Ho Chi Minh city",
    "temperature": 30
}
```

Nói ngắn gọn hơn tí thì `API` nó giống như một hợp đồng giao tiếp, client phải gửi dữ liệu theo cách nào và server sẽ phản hồi theo cấu trúc nào.

---
# Http

`Http` là viết tắt cho `HyperText Tranfer Protocol` - giao thức ở tầng application dùng để trao đổi dữ liệu trên web. Mô hình `http` sẽ chủ yếu là: `client -> http request -> server` và `client <- http response <- server`.

Ví dụ mình gửi một request là:
```http request
GET /user/123 HTTP/1.1
Host: api.example.com
```

Và nhận kết quả từ server là:
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```json
{
  "id": 123,
  "name": "Litaaya"
}
```

---
# Http: Stateless

Một đặc điểm quan trọng của http là stateless - nghĩa là mỗi http request được xử lý độc lập so với request trước đó. Nếu muốn hệ thống nhớ trạng thái đăng nhập hoặc session thì sẽ cần có cơ chế bổ sung như cookie, sessiond ID, access token, jwt, etc.

---
# Http Request

Một http request sẽ có các thành phần chính là `method`, `url/path`, `headers` và `body`(optional). Ví dụ:
```http request
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer abc123

{
    "name": "Litaaya",
    "age": 25
}
```

Ở ví dụ trên, `method` chính là `POST`, nó cho biết client muốn thực hiện hành động gì. `path` là `/users HTTP/1.1` cho biết resource cần thao tác. `headers` chứa metadata liên quan đến request là những dòng dưới. Cuối cùng là `body` là dữ liệu gửi lên server là phần trong `{}`. Lưu ý ở chỗ là không phải request nào cũng cần body, ví dụ `GET` thì sử dụng để lấy dữ liệu chứ không cần chứa request content.

---
# Http Response

Một http response sẽ có các thành phần chính bao gồm `status code`, `headers` và `body`. Ví dụ:
```
HTTP/1.1 201 Created
Content-Type: application/json
```
```json
{
  "id": 111,
  "name": "Litaaya",
  "age": 25
}
```

Ở ví dụ trên, `201` chính là `http status code`, nó cho biết resource mới đã được tạo thành công. `headers` là phần dưới cho biết response đang ở định dạng json. Còn `body` là dữ liệu thực tế mà server trả lại là phần trong `{}`.

---
# Http Methods

Http định nghĩa nhiều method khác nhau. Trong REST API quan trọng nhất là:

| Method    | Ý nghĩa thường dùng                               |
|:----------|:--------------------------------------------------|
| `GET`     | Lấy dữ liệu                                       |
| `POST`    | Tạo dữ liệu mới / gửi dữ liệu để xử lý            |
| `PUT`     | Thay thế toàn bộ resource                         |
| `PATCH`   | Cập nhật một phần resource                        |
| `DELETE`  | Xóa resource                                      |
| `HEAD`    | Giống GET nhưng không lấy body                    |
| `OPTIONS` | Hỏi server hỗ trợ những phương thức/giao tiếp nào |

---
# Http Status Code

Server sử dụng `status code` để cho client biết kết quả xử lý request, nó sẽ chia thành 5 nhóm chính bao gồm:

| Nhóm  | Ý nghĩa      |
|:------|:-------------|
| `1xx` | Information  |
| `2xx` | Success      |
| `3xx` | Redirection  |
| `4xx` | Client error |
| `5xx` | Server error |

Để cho dễ hình dung thì nếu code trả về là `2**` nghĩa là `OK`, `4**` nghĩa là vấn đề nằm ở phía request/client, `5**` nghĩa là vấn đề nằm ở server.

---
# URL, URI, Endpoint và Resource

Mình có một ví dụ như sau:
```
https://api.example.com/users/123
```

`Resource` là đối tượng hoặc dữ liệu mà API quản lý, trong ví dụ trên nó sẽ là `users`, ở các trường hợp khác nó có thể là `orders`, `products`, `payments`.

`URL` là địa chỉ của resource, nó chính là cả cái ví dụ. Mọi `URL` đều là `URI` nhưng ngược lại thì không, đối với `URI` thì nó dùng để nhận diện hoặc định danh một tài nguyên ví dụ như `urn:isbn:...`. Với `URL` thì nó bắt buộc phải chứa thông tin giao thức như `http://` và `https://`

`Endpoint` là một địa chỉ cụ thể mà client có thể gọi ví dụ như `GET /users/123` hay `DELETE /users/456`. Một `Endpoint` có thể hiểu là `HTTP Method` + `URL`.

---
# JSON và REST API

`REST` không nhất thiết phải xài json, tuy nhiên json hiện là một trong những format phổ biến nhất để trao đổi dữ liệu trong web api vì dễ đọc và không phụ thuộc ngôn ngữ lập trình. 

Vậy `REST` là gì ? `REST` là viết tắt cho `Representational State Transfer`, đây là một dạng architectural style tức là một tập hợp các nguyên tắc thiết kế hệ thống phân tán. `REST` đưa ra các architectual constraints để xây dựng hệ thống có khả năng mở rộng, đơn giản và dễ tương tác, và các API tuân theo nguyên tác `REST` thường được gọi là `REST API` hoặc `RESTful API`.

---
# REST và Resource

Hệ thống được mô hình hóa dưới dạng các resource, ví dụ như:
```http request
GET /users
```

Thay vì:
```http request
/getAllUsers
```

---
# Resource và Representation

Giả sử db có user `User #123`, user đó là một resource. Khi client gọi:
```http request
GET /users/123
```

Server sẽ không gửi bản thân resource theo nghĩa vật lý mà là gửi một representation của resource. Ví dụ dưới chính là representation của resource `user 123`:
```json
{
  "id": 123,
  "name": "Litaaya"
}
```

---
# Nguyên tắc của REST

Client - Server tách biệt nhau, ở giữa là REST API, client không cần biết server lưu dữ liệu bằng Postgresql, mongoDB hay file, server cũng không cần biết client là web, mobile hay python, etc.

Stateless: mỗi request phải chứa đủ thông tin cần thiết để server xử lý, server không nên phụ thuộc vào một request trước đó để hiểu request này.

Cacheable: response có thể được đánh dấu để client hoặc intermediary cache lại, nếu dữ liệu vẫn còn hợp lệ, cache có thể trả kết quả mà không cần gọi server lại, việc này giúp giảm latency, giảm tải server và network traffic.

Uniform Interface: Các thành phần giao tiếp với nhau thông qua một interface nhất quán.

Layered System: Client không nhất thiết phải giao tiếp trực tiếp với server cuối cùng, client không cần biết phía sau API có bao nhiều layer.

Code on Demand: Server có thể gửi executable code cho client.

---
# Query Parameter và Path Parameter

Query Parameter thường dùng để filter, sort, paginate hoặc bổ sung điều kiện, ví dụ như:
```http request
GET /users?country=vn&active=true
GET /products?page=10&limit=1
```

Path Parameter dùng để xác định một resource cụ thể, ví dụ như:
```http request
GET /users/{userId}
```

---
# Headers

Http headers mang metadata của request hoặc response, ví dụ như:
```http request
Content-Type: application/json
```

nghĩa là nội dung body là JSON
```http request
Accept: application/json
```

nghĩa là client muốn nhận json.

---
# Http và Https

`HTTPS` cơ bản là `HTTP` được truyền qua một kết nối được bảo vệ bằng `TLS`, vì vậy gần như mọi trang web đều sẽ sử dụng `https://`.

`REST API` không đồng nghĩa với `HTTP API`, có nhiều api sử dụng http nhưng không tuân thủ đầy đủ REST constrains.

---
# CRUD

`CRUD` tương ứng là `Create`, `Read`, `Update`, `Delete`.

| CRUD   | HTTP        | Example             |
|:-------|:------------|:--------------------|
| Create | `POST`      | `POST /users`       |
| Read   | `GET`       | `GET /users/123`    |
| Update | `PUT/PATCH` | `PATCH /users/123`  |
| Delete | `DELETE`    | `DELETE /users/123` |
