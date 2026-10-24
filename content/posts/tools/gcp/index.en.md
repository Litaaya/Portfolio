---
title: "Google Cloud Platform"
date: 2026-10-14
draft: false
tags: ["bigquery", "gcp"]
description: "Google's cloud ecosystem"
---
# Google Cloud Platform

Google Cloud Platform - GCP is Google's Cloud platform, providing infrastructure and services so businesses can run applications, store data, process data and build analytical systems without having to own all physical servers themselves.

---
# GCP Structure

Before going into each service, it is necessary to understand how GCP organizes resources according to some basic concepts.

`Project`

Project can be seen as a "workspace" containing the resources of a system.  Example:
```text
data-dev
data-staging
data-prod
```

Each project can contain:
```text
VM
BigQuery
Cloud Storage
Cloud Run
Service Account
Network
```

Project is also the place to manage:
```text
Billing
Permission
API
Resource
```

In practice, a company can separate them (and I am also using this approach):
```text
data-dev used for development

data-prod runs the production system
```

Separating them like this helps avoid cases where a developer is testing but accidentally affects production data.

`Region and Zone`

GCP has data centers in many regions around the world.

`Region` is a large geographical area.

Example:
```text
asia-southeast1
```

is the Singapore Region.

Inside each Region there are many `Zone`s.

Example:
```text
asia-southeast1-a
asia-southeast1-b
asia-southeast1-c
```

Imagine:
```text
Region = a city

Zone = different buildings in that city
```

Some services operate by Region, some operate by Zone. Example:
```text
Compute Engine VM usually lies in a Zone

Subnet belongs to a Region
```

Having multiple Zones helps the system avoid depending on a single physical location.

---
# Compute Engine

Compute Engine is a service that allows creating Virtual Machine - VM on Google Cloud. A VM can be understood as a computer or server running on the Cloud. Example of creating a VM:
```text
OS: Ubuntu
CPU: 4 vCPU
RAM: 16 GB
Disk: 100 GB
```

After creating it, I can SSH into the VM and operate almost like a normal Linux server. Example:
```bash
python main.py
```

or run:
```text
Crawler
API
Database
Scheduled Script
Internal Application
```

It can be imagined that if Cloud is a very large server building then Compute Engine is like renting a separate computer inside it, google is responsible for the physical hardware part, while I still manage:
```text
OS
Package
Application
Security configuration
Running process
```

---
# VPC Network

VPC - Virtual Private Cloud is a virtual private network in Google Cloud. It helps resources such as:
```text
VM
Database
Application
Internal Service
```

communicate with each other through the network.

Imagine VPC as a private company campus, the servers and applications inside that campus can be divided into different areas.

`Subnet - Subnetwork`:

Subnet is the division of the VPC network into smaller areas.

Imagine:
```text
VPC = a building

Subnet = each floor in the building
```

Example:
```text
Floor 1
-> Application

Floor 2
-> Data Platform

Floor 3
-> Database
```

In GCP, each Subnet belongs to a Region. Example:
```text
Subnet:
data-prod-sg

Region:
Singapore
```

Each subnet is assigned its own IP range. Example:
```text
10.10.0.0/24
```

is a range of internal IP addresses for resources located in that subnet.

A specific detailed example is as follows, I have:
```text
VPC: production-network

Subnet: data-prod-sg
Region: Singapore
IP range: 10.10.0.0/24
```

In this subnet there are:
```text
Airflow Worker
PostgreSQL Staging
Internal Data Service
```

---
# Firewall

Firewall is used to define who is allowed to connect to which resource and through which port.

For example a VM runs PostgreSQL on port:
```text
5432
```

It should not be opened:
```text
Internet -> 5432 -> PostgreSQL
```

to everyone.

Instead, it can be defined that only applications in the internal network are allowed to connect to PostgreSQL.

---
# Route

Route determines the path that network traffic will take. For example if resource A wants to send data to resource B then the network needs to know the path to deliver traffic to the correct place - Route helps perform that.

---
# Google Cloud Storage

Google Cloud Storage - GCS is a file storage service on the Cloud.

If the computer has:
```text
C:\data\
```

then on GCS there can be:
```text
gs://company-data/
```

In GCS, data is stored in a `Bucket`. Example:
```text
Bucket:
company-data
```

inside there are:
```text
raw/orders/orders.csv
raw/products/products.json
processed/sales/sales.parquet
```

---
# Cloud Run Functions / Cloud Functions

Cloud Function is a way to run a small piece of code without having to create and manage a VM yourself. For example I have a function:
```python
def hello():
    print("Hello")
```

Google will be responsible for:
```text
Server
Runtime
Scaling
Infrastructure
```

`Event-driven`: Function is usually triggered when an event occurs. Example:
```text
GCS receives a new file -> Function sends notification
```

or:

```text
API Request -> Function -> Insert data
```

---
# Google BigQuery

BigQuery is Google Cloud's Data Warehouse, designed to analyze large amounts of data using SQL. So how is BigQuery different from PostgreSQL ?

PostgreSQL is usually suitable for application transactions. Example:
```text
Create Order
Update Customer
Delete Cart
```

BigQuery is suitable for analytics:
```text
Total revenue for 3 years
Revenue by province
Top 100 products
Analyze millions of transactions
```

---
# Data Storage

Not all data should be stored in the same place, different storage is used depending on the purpose. For example:
```text
Raw file
-> Cloud Storage

Application transaction
-> PostgreSQL / Cloud SQL

Analytics
-> BigQuery
```

---
# Cost-effectiveness

`Cloud = money`, nothing in this world is free, touching cloud means touching money already so cost management is very important when using cloud. The simple reason is because however much we use, we will have to pay that much

BigQuery example:
```sql
SELECT *
FROM sales;
```

if the table is very large then it can scan a lot of data.

While:
```sql
SELECT
    product_id,
    revenue
FROM sales
WHERE order_date = '2026-10-24';
```

will only get more necessary data.

---
# Data Governance and Management

Data Governance is how a business manages data so that:
```text
the right people
access the right data
with the right permissions
and the data is understood correctly
```

---
# Data Ownership

A Dataset should have a person or team responsible for it. Example:
```text
Sales Dataset -> Owner: Commercial Data Team
```

If the data is wrong or business logic needs to be changed then it is known which team is responsible.

---
# Access Control

Access Control determines:
```text
Who
can do what
on which data
```

For example Analyst:
```text
can SELECT
```

but cannot:
```text
DROP TABLE
DELETE TABLE
```

A common principle is:

```text
Least Privilege
```

meaning only the necessary permissions are granted.

---
# Data Classification

Data has different levels of sensitivity for example:
```text
Public
Internal
Confidential
Restricted
```

Information such as:
```text
Email
Phone
Address
Identity Number
```

usually needs more protection than data such as:
```text
Product Category
Store Name
Public Price
```

---
# Metadata

Metadata is information describing data. Example table:
```text
fact_sales
```

Metadata can include:
```text
What is this Table used for?
Who is the Owner?
What does each column mean?
How often is Data updated?
```

---
# Data Lineage

Data Lineage shows where data comes from and where it goes. If the dashboard has incorrect revenue, I can trace backward to find the error.

---
# Data Warehouse

Data Warehouse is a place to store data organized to serve:
```text
Analytics
Reporting
BI
Historical Analysis
```

For example a business has many systems:
```text
ERP
CRM
E-commerce
POS
```

Each system has its own database and `Data Warehouse` will gather data from these systems together:

---
# Data Modeling

Data Modeling is designing how data is organized. For example a sales system has:
```text
Customer
Order
Product
Store
```

We need to determine:
```text
How is Customer related to Order?
Which Product does Order contain?
Which Store sold that Order?
```

---
# Dimensional Modeling

In Data Warehouse, a very common design approach is to divide data into:
```text
Fact
Dimension
```

`Fact`

Fact contains events or numbers that need to be analyzed. Example:
```text
fact_sales
```

has:
```text
product
customer
date
quantity
revenue
```

In which:
```text
quantity
revenue
```

are measurements.

`Dimension`

Dimension describes additional context for Fact. Example:
```text
dim_product

product_id
product_name
brand
category
```

or:
```text
dim_customer

customer_id
customer_name
region
```

---
# Grain

Grain is the level of detail of a row in a `Fact Table`, determining grain is very important because it decides the meaning of the data.

---
# Surrogate Key

Surrogate Key is an ID created by the Data Warehouse itself. For example I have:
```text
product_key = 1
```

While the source has:
```text
product_id = SKU001
```

There can be:
```text
product_key | product_id
1           | SKU001
2           | SKU002
```

The reason for creating surrogate key is so the Data Warehouse does not depend entirely on the source ID.

---
# Slowly Changing Dimension

Dimension can change over time. For example customer Litaaya:
```text
2025
Region = HCM
```

next year:

```text
2026
Region = US
```

There are two basic ways to handle this problem.

`Type 1`

Overwrite and do not keep history.

`Type 2`

Keep both old and new data:
```text
Customer | Region | From | To
A        | HCM    | 2025 | 2026
A        | Hanoi  | 2026 | NULL
```

Thanks to that it is possible to know:
```text
At the time of the year 2025
which region did the customer belong to?
```

---
# Data Processing in Warehouse

Data usually does not go directly from the source to the dashboard.

It usually goes through many layers, normally I will use the `gold medilions` model. Dashboard can query directly from Mart instead of having to process Raw Data itself.

---
# Partitioning

Partitioning is dividing a large table into smaller parts.

For example I have a table:
```text
sales
```

with many years of data.

It can be divided by:
```text
order_date
```

into:
```text
2026-10-20
2026-10-21
2026-10-22
2026-10-23
```

If query:
```sql
WHERE order_date = '2026-10-23'
```

then the system can only read the data portion of that day instead of the entire table.

---
# Data Quality

A successful pipeline does not mean the data is correct. Example:

```text
Pipeline Status:
SUCCESS
```

but the data:
```text
Revenue = NULL
```

is still a problem.