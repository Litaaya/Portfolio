---
title: "Google Cloud Platform"
date: 2026-10-14
draft: false
tags: ["bigquery", "gcp"]
description: "Hệ sinh thái cloud của google"
---
# Google Cloud Platform

Google Cloud Platform - GCP là nền tảng Cloud của Google, cung cấp hạ tầng và các dịch vụ để doanh nghiệp có thể chạy application, lưu trữ dữ liệu, xử lý dữ liệu và xây dựng các hệ thống phân tích mà không cần tự sở hữu toàn bộ server vật lý.

---
# GCP Structure

Trước khi đi vào từng service, cần hiểu GCP tổ chức resource theo một số khái niệm cơ bản.

`Project`

Project có thể xem là một "workspace" chứa các resource của một hệ thống.  Ví dụ:
```text
data-dev
data-staging
data-prod
```

Trong mỗi project có thể chứa:
```text
VM
BigQuery
Cloud Storage
Cloud Run
Service Account
Network
```

Project cũng là nơi quản lý:
```text
Billing
Permission
API
Resource
```

Ví dụ thực tế, một công ty có thể tách (và mình cũng đang sử dụng theo hướng này):
```text
data-dev dùng để development

data-prod chạy hệ thống production
```

Việc tách như vậy giúp tránh trường hợp một developer đang test nhưng vô tình ảnh hưởng tới dữ liệu production.

`Region và Zone`

GCP có data center tại nhiều khu vực trên thế giới.

`Region` là một khu vực địa lý lớn.

Ví dụ:
```text
asia-southeast1
```

là Region Singapore.

Bên trong mỗi Region có nhiều `Zone`.

Ví dụ:
```text
asia-southeast1-a
asia-southeast1-b
asia-southeast1-c
```

Hình dung:
```text
Region = một thành phố

Zone = các tòa nhà khác nhau trong thành phố đó
```

Một số service hoạt động theo Region, một số hoạt động theo Zone. Ví dụ:
```text
Compute Engine VM thường nằm trong một Zone

Subnet thuộc một Region
```

Việc có nhiều Zone giúp hệ thống có thể tránh phụ thuộc vào một vị trí vật lý duy nhất.

---
# Compute Engine

Compute Engine là dịch vụ cho phép tạo Virtual Machine - VM trên Google Cloud. Có thể hiểu VM như một chiếc máy tính hoặc server chạy trên Cloud. Ví dụ tạo VM:
```text
OS: Ubuntu
CPU: 4 vCPU
RAM: 16 GB
Disk: 100 GB
```

Sau khi tạo, mình có thể SSH vào VM và thao tác gần giống một server Linux bình thường. Ví dụ:
```bash
python main.py
```

hoặc chạy:
```text
Crawler
API
Database
Scheduled Script
Internal Application
```

Có thể hình dung rằng nếu Cloud là một tòa nhà server rất lớn thì Compute Engine giống như mình thuê riêng một chiếc máy tính bên trong đó, google chịu trách nhiệm phần hardware vật lý, còn mình vẫn quản lý:
```text
OS
Package
Application
Security configuration
Process đang chạy
```

---
# VPC Network

VPC - Virtual Private Cloud là mạng riêng ảo trong Google Cloud. Nó giúp các resource như:
```text
VM
Database
Application
Internal Service
```

có thể giao tiếp với nhau thông qua network.

Hình dung VPC giống như một khuôn viên công ty riêng, các server và application nằm bên trong khuôn viên đó có thể được phân chia thành từng khu vực khác nhau.

`Subnet - Subnetwork`:

Subnet là việc chia network của VPC thành các vùng nhỏ hơn.

Hình dung:
```text
VPC = một tòa nhà

Subnet = từng tầng trong tòa nhà
```

Ví dụ:
```text
Tầng 1
-> Application

Tầng 2
-> Data Platform

Tầng 3
-> Database
```

Trong GCP, mỗi Subnet thuộc một Region. Ví dụ:
```text
Subnet:
data-prod-sg

Region:
Singapore
```

Mỗi subnet được cấp một dải IP riêng. Ví dụ:
```text
10.10.0.0/24
```

là một khoảng địa chỉ IP nội bộ dành cho các resource nằm trong subnet đó.

Ví dụ cụ thể chi tiết như sau, mình có:
```text
VPC: production-network

Subnet: data-prod-sg
Region: Singapore
IP range: 10.10.0.0/24
```

Trong subnet này có:
```text
Airflow Worker
PostgreSQL Staging
Internal Data Service
```

---
# Firewall

Firewall dùng để quy định ai được phép kết nối tới resource nào và thông qua port nào.

Ví dụ VM chạy PostgreSQL ở port:
```text
5432
```

Không nên mở:
```text
Internet -> 5432 -> PostgreSQL
```

cho tất cả mọi người.

Thay vào đó có thể quy định chỉ application trong internal network mới được kết nối PostgreSQL.

---
# Route

Route quyết định đường mà network traffic sẽ đi. Ví dụ như resource A muốn gửi dữ liệu tới resource B thì network cần biết đường để đưa traffic tới đúng nơi - Route giúp thực hiện việc đó.

---
# Google Cloud Storage

Google Cloud Storage - GCS là dịch vụ lưu file trên Cloud.

Nếu máy tính có:
```text
C:\data\
```

thì trên GCS có thể có:
```text
gs://company-data/
```

Trong GCS, dữ liệu được lưu trong `Bucket`. Ví dụ:
```text
Bucket:
company-data
```

bên trong có:
```text
raw/orders/orders.csv
raw/products/products.json
processed/sales/sales.parquet
```

---
# Cloud Run Functions / Cloud Functions

Cloud Function là cách chạy một đoạn code nhỏ mà không cần tự tạo và quản lý VM. Ví dụ mình có function:
```python
def hello():
    print("Hello")
```

Google sẽ chịu trách nhiệm:
```text
Server
Runtime
Scaling
Infrastructure
```

`Event-driven`: Function thường được kích hoạt khi có một event. Ví dụ:
```text
GCS nhận file mới -> Function gửi notification
```

hoặc:

```text
API Request -> Function -> Insert data
```

---
# Google BigQuery

BigQuery là Data Warehouse của Google Cloud, được thiết kế để phân tích lượng dữ liệu lớn bằng SQL. Vậy thì BigQuery khác PostgreSQL như thế nào ?

PostgreSQL thường phù hợp cho transaction của application. Ví dụ:
```text
Create Order
Update Customer
Delete Cart
```

BigQuery phù hợp cho analytics:
```text
Tổng doanh thu 3 năm
Doanh thu từng tỉnh
Top 100 sản phẩm
Phân tích hàng triệu transaction
```

---
# Data Storage

Không phải dữ liệu nào cũng nên lưu cùng một chỗ, tùy mục đích mà sử dụng storage khác nhau. Ví dụ như:
```text
File raw
-> Cloud Storage

Application transaction
-> PostgreSQL / Cloud SQL

Analytics
-> BigQuery
```

---
# Cost-effectiveness

`Cloud = money`, trên đời không có gì miễn phí cả, đụng tới cloud là đã đụng tới tiền rồi vậy nên việc quản lý chi phí rất quan trọng khi sử dụng cloud. Lý do đơn giản là vì mình xài bao nhiêu thì mình sẽ phải trả bấy nhiêu

Ví dụ BigQuery:
```sql
SELECT *
FROM sales;
```

nếu bảng rất lớn thì có thể scan rất nhiều dữ liệu.

Trong khi:
```sql
SELECT
    product_id,
    revenue
FROM sales
WHERE order_date = '2026-10-24';
```

sẽ chỉ lấy dữ liệu cần thiết hơn.

---
# Data Governance and Management

Data Governance là cách doanh nghiệp quản lý dữ liệu sao cho:
```text
đúng người
được truy cập đúng dữ liệu
với đúng quyền
và dữ liệu được hiểu đúng
```

---
# Data Ownership

Dataset nên có người hoặc team chịu trách nhiệm. Ví dụ:
```text
Sales Dataset -> Owner: Commercial Data Team
```

Nếu dữ liệu sai hoặc cần thay đổi business logic thì biết team nào chịu trách nhiệm.

---
# Access Control

Access Control quyết định:
```text
Ai
được làm gì
trên dữ liệu nào
```

Ví dụ Analyst:
```text
được SELECT
```

nhưng không được:
```text
DROP TABLE
DELETE TABLE
```

Nguyên tắc phổ biến là:

```text
Least Privilege
```

nghĩa là chỉ cấp đúng quyền cần thiết.

---
# Data Classification

Dữ liệu có mức độ nhạy cảm khác nhau ví dụ như:
```text
Public
Internal
Confidential
Restricted
```

Thông tin như:
```text
Email
Phone
Address
Identity Number
```

thường cần bảo vệ nhiều hơn dữ liệu như:
```text
Product Category
Store Name
Public Price
```

---
# Metadata

Metadata là thông tin mô tả dữ liệu. Ví dụ table:
```text
fact_sales
```

Metadata có thể bao gồm:
```text
Table này dùng để làm gì?
Owner là ai?
Mỗi column có ý nghĩa gì?
Data update bao lâu một lần?
```

---
# Data Lineage

Data Lineage cho biết dữ liệu đi từ đâu tới đâu. Nếu dashboard sai doanh thu, mình có thể lần ngược để tìm lỗi.

---
# Data Warehouse

Data Warehouse là nơi lưu dữ liệu được tổ chức để phục vụ:
```text
Analytics
Reporting
BI
Historical Analysis
```

Ví dụ doanh nghiệp có nhiều hệ thống:
```text
ERP
CRM
E-commerce
POS
```

Mỗi hệ thống có database riêng và `Data Warehouse` sẽ gom dữ liệu từ các hệ thống này lại:

---
# Data Modeling

Data Modeling là việc thiết kế cách dữ liệu được tổ chức. Ví dụ hệ thống bán hàng có:
```text
Customer
Order
Product
Store
```

Ta cần xác định:
```text
Customer liên quan Order như thế nào?
Order chứa Product nào?
Store nào bán Order đó?
```

---
# Dimensional Modeling

Trong Data Warehouse, một cách thiết kế rất phổ biến là chia dữ liệu thành:
```text
Fact
Dimension
```

`Fact`

Fact chứa sự kiện hoặc con số cần phân tích. Ví dụ:
```text
fact_sales
```

có:
```text
product
customer
date
quantity
revenue
```

Trong đó:
```text
quantity
revenue
```

là các measurement.

`Dimension`

Dimension mô tả thêm context cho Fact. Ví dụ:
```text
dim_product

product_id
product_name
brand
category
```

hoặc:
```text
dim_customer

customer_id
customer_name
region
```

---
# Grain

Grain là mức độ chi tiết của một row trong `Fact Table`, xác định grain rất quan trọng vì nó quyết định ý nghĩa của dữ liệu.

---
# Surrogate Key

Surrogate Key là một ID do Data Warehouse tự tạo. Ví dụ mình có:
```text
product_key = 1
```

Trong khi source có:
```text
product_id = SKU001
```

Có thể có:
```text
product_key | product_id
1           | SKU001
2           | SKU002
```

Lý do tạo surrogate key là để Data Warehouse không bị phụ thuộc hoàn toàn vào ID của source.

---
# Slowly Changing Dimension

Dimension có thể thay đổi theo thời gian. Ví dụ khách hàng Litaaya:
```text
2025
Region = HCM
```

sang năm sau:

```text
2026
Region = US
```

Có hai cách cơ bản để xử lý vấn đề này.

`Type 1`

Ghi đè và không giữ lịch sử.

`Type 2`

Giữ cả dữ liệu cũ và mới:
```text
Customer | Region | From | To
A        | HCM    | 2025 | 2026
A        | Hanoi  | 2026 | NULL
```

Nhờ vậy có thể biết:
```text
Tại thời điểm năm 2025
khách hàng thuộc region nào?
```

---
# Data Processing in Warehouse

Dữ liệu thường không đi trực tiếp từ source lên dashboard.

Nó thường qua nhiều layer, thông thường mình sẽ sử dụng mô hình `gold medilions`. Dashboard có thể query trực tiếp từ Mart thay vì phải tự xử lý Raw Data.

---
# Partitioning

Partitioning là chia một bảng lớn thành nhiều phần nhỏ.

Ví dụ mình có bảng:
```text
sales
```

có dữ liệu nhiều năm.

Có thể chia theo:
```text
order_date
```

thành:
```text
2026-10-20
2026-10-21
2026-10-22
2026-10-23
```

Nếu query:
```sql
WHERE order_date = '2026-10-23'
```

thì hệ thống có thể chỉ đọc phần dữ liệu của ngày đó thay vì toàn bộ bảng.

---
# Data Quality

Pipeline chạy thành công không có nghĩa dữ liệu đúng. Ví dụ:

```text
Pipeline Status:
SUCCESS
```

nhưng dữ liệu:
```text
Revenue = NULL
```

thì vẫn là vấn đề.