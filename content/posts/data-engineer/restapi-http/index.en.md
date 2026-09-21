---
title: "REST API & HTTP"
date: 2026-08-27
draft: false
tags: ["REST-API", "HTTP"]
description: "Some basic knowledge about rest api and http"
---

> Http is a protocol while REST is an architectural style used to design APIs

---
# API

In the field of information technology, the term `API` is mentioned quite often, so the question here is what is an `API` ? `API` stands for `Application Programming Interface`, understood as an interface that allows two programs or systems to communicate with each other. It can be imagined as application A sending a request to application B through or by an API.

The example below shows a weather application sending a request to get the temperature of Ho Chi Minh City, it will send the request below and receive the result:
```http request
GET /weather?city=ho-chi-minh HTTP/1.1
```

```json
{
    "city": "Ho Chi Minh city",
    "temperature": 30
}
```

To put it a bit more simply, an `API` is like a communication contract, the client has to send data in what way and the server will respond in what structure.

---
# Http

`Http` stands for `HyperText Tranfer Protocol` - an application-layer protocol used to exchange data on the web. The `http` model will mainly be: `client -> http request -> server` and `client <- http response <- server`.

For example, I send a request:
```http request
GET /user/123 HTTP/1.1
Host: api.example.com
```

And receive the result from the server:
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

An important characteristic of http is stateless - meaning each http request is processed independently from the previous request. If the system needs to remember login state or session then additional mechanisms such as cookie, sessiond ID, access token, jwt, etc. will be needed.

---
# Http Request

An http request will have the main components `method`, `url/path`, `headers` and `body`(optional). For example:
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

In the example above, `method` is `POST`, it indicates what action the client wants to perform. `path` is `/users HTTP/1.1` indicating the resource to operate on. `headers` contain metadata related to the request, which are the lines below. Finally, `body` is the data sent to the server, which is the part inside `{}`. Note that not every request needs a body, for example `GET` is used to retrieve data and does not need to contain request content.

---
# Http Response

An http response will have the main components including `status code`, `headers` and `body`. For example:
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

In the example above, `201` is the `http status code`, it indicates that the new resource has been created successfully. `headers` are the part below indicating that the response is in json format. And `body` is the actual data that the server returns, which is the part inside `{}`.

---
# Http Methods

Http defines many different methods. In REST API the most important ones are:

| Method    | Common meaning                                      |
|:----------|:----------------------------------------------------|
| `GET`     | Retrieve data                                       |
| `POST`    | Create new data / send data for processing          |
| `PUT`     | Replace the entire resource                         |
| `PATCH`   | Update part of a resource                           |
| `DELETE`  | Delete a resource                                   |
| `HEAD`    | Same as GET but without retrieving the body         |
| `OPTIONS` | Ask what methods/communication the server supports  |

---
# Http Status Code

The server uses `status code` to let the client know the result of processing the request, it is divided into 5 main groups including:

| Group | Meaning      |
|:------|:-------------|
| `1xx` | Information  |
| `2xx` | Success      |
| `3xx` | Redirection  |
| `4xx` | Client error |
| `5xx` | Server error |

To make it easier to understand, if the returned code is `2**` it means `OK`, `4**` means the problem is on the request/client side, `5**` means the problem is on the server.

---
# URL, URI, Endpoint and Resource

I have an example as follows:
```
https://api.example.com/users/123
```

`Resource` is the object or data that the API manages, in the example above it will be `users`, in other cases it can be `orders`, `products`, `payments`.

`URL` is the address of the resource, it is the entire example. Every `URL` is a `URI` but not vice versa, for `URI` it is used to identify or designate a resource, for example `urn:isbn:...`. With `URL` it must contain protocol information such as `http://` and `https://`

`Endpoint` is a specific address that the client can call, for example `GET /users/123` or `DELETE /users/456`. An `Endpoint` can be understood as `HTTP Method` + `URL`.

---
# JSON and REST API

`REST` does not necessarily have to use json, however json is currently one of the most popular formats for exchanging data in web api because it is easy to read and independent of programming language. 

So what is `REST` ? `REST` stands for `Representational State Transfer`, this is a type of architectural style, meaning a set of principles for designing distributed systems. `REST` provides architectural constraints to build systems that are scalable, simple and easy to interact with, and APIs that follow `REST` principles are often called `REST API` or `RESTful API`.

---
# REST and Resource

The system is modeled in the form of resources, for example:
```http request
GET /users HTTP/1.1
```

Instead of:
```http request
/getAllUsers
```

---
# Resource and Representation

Suppose the db has user `User #123`, that user is a resource. When the client calls:
```http request
GET /users/123 HTTP/1.1
```

The server will not send the resource itself in the physical sense but will send a representation of the resource. The example below is the representation of resource `user 123`:
```json
{
  "id": 123,
  "name": "Litaaya"
}
```

---
# Principles of REST

Client - Server are separated from each other, in between is the REST API, the client does not need to know whether the server stores data using Postgresql, mongoDB or files, the server also does not need to know whether the client is web, mobile or python, etc.

Stateless: each request must contain enough necessary information for the server to process it, the server should not depend on a previous request to understand this request.

Cacheable: the response can be marked so that the client or intermediary can cache it, if the data is still valid, the cache can return the result without needing to call the server again, this helps reduce latency, server load and network traffic.

Uniform Interface: Components communicate with each other through a consistent interface.

Layered System: The client does not necessarily have to communicate directly with the final server, the client does not need to know how many layers there are behind the API.

Code on Demand: The server can send executable code to the client.

---
# Query Parameter and Path Parameter

Query Parameter is commonly used to filter, sort, paginate or add conditions, for example:
```http request
GET /users?country=vn&active=true HTTP/1.1
```
```http request
GET /products?page=10&limit=1 HTTP/1.1
```

Path Parameter is used to identify a specific resource, for example:
```http request
GET /users/{userId} HTTP/1.1
```

---
# Headers

Http headers carry metadata of the request or response, for example:
```text
Content-Type: application/json
```

means the body content is JSON
```text
Accept: application/json
```

means the client wants to receive json.

---
# Http and Https

`HTTPS` is basically `HTTP` transmitted over a connection protected by `TLS`, therefore almost every website will use `https://`.

`REST API` is not synonymous with `HTTP API`, there are many APIs that use http but do not fully comply with REST constraints.

---
# CRUD

`CRUD` corresponds to `Create`, `Read`, `Update`, `Delete`.

| CRUD   | HTTP        | Example             |
|:-------|:------------|:--------------------|
| Create | `POST`      | `POST /users`       |
| Read   | `GET`       | `GET /users/123`    |
| Update | `PUT/PATCH` | `PATCH /users/123`  |
| Delete | `DELETE`    | `DELETE /users/123` |