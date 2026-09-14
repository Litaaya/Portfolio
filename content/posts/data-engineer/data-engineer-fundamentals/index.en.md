---
title: "Data Engineer Fundamentals"
date: 2026-08-04
draft: false
tags: ["data-engineer"]
description: "Some basic knownledge for data engineer"
---
# ELT, ETL and data architecture

---
## ETL and ELT ?

As the names suggest, one is Extract > Transform > Load, and the other is Extract > Load > Transform. The difference lies in where and when the data transformation process, also known as Transform, takes place.

With ETL: data is extracted from the source, transformed and cleaned on an intermediate server, and only then loaded into the data warehouse.

With ELT: data is extracted and raw data is loaded directly into the data warehouse, then the transformation process is performed inside the data warehouse itself. To put it briefly, it uses the computing power of modern cloud data warehouses. There is quite a lot to this, and it will be discussed later.

- In terms of scalability, ELT beats ETL because ETL is limited by the server configuration, and the loading speed is also slower because it has to wait for the transformation.
- In terms of infrastructure requirements, ETL is more suitable for on-premise systems or traditional data warehouses, while ELT requires a cloud data warehouse capable of processing big data.
- In terms of security, ETL is more secure because the data is transformed before being stored, while ELT requires better access control because it stores all raw data.
- In terms of cost, ELT storage is relatively cheap, but when it comes to the cloud, it still depends; compute cost can increase quickly if the transformations are poorly optimized.

With modern data stacks, most companies are moving toward ELT combined with reverse ETL (pushing processed data from the warehouse back to operational applications such as CRM, Marketing Tools, etc.).

---
## When to use ETL and when to use ELT

ETL:
- Strict security requirements must be considered.
- Old systems - Legacy systems: Oracle, SQL Server with limited computing capacity.
- Small data and fixed formats.

ELT:
- Using cloud data warehouses: Snowflake, Google BigQuery, AWS Redshift, Databricks.
- Large and diverse data (big data).
- Data democratization: high flexibility is needed, allowing DAs/DSs to query raw data themselves and write their own pipelines for each specific problem.

---
## Why cloud data warehouses make ELT more popular

Decoupled Storage and Compute: an architectural model that separates storage and computing.

MPP - Massively Parallel Processing: parallel processing capability.

Thanks to these two factors, a CDW (cloud data warehouse) can transform big data directly inside the warehouse at a low cost, removing the performance barriers that traditional SQL systems faced in the past.

Let's go deeper:
- With Decoupled Storage and Compute: in the past, storage capacity and computing power came together. If too much raw data was loaded into the warehouse, it could fill up the hard drive and make reporting queries unstable. Now, cloud services such as S3 and Google Cloud Storage are inexpensive, so large amounts of raw big data can be loaded into the warehouse without worrying too much about storage costs or affecting computing power. When transformation is needed, the system turns on compute resources to process it.
- With MPP: a CDW uses multiple distributed virtual machines underneath to run queries at the same time. In simple terms, having more workers means it is faster than an ETL server.

I also found another reason: instead of having to write complex Java, Python, or Scala code on Spark/Hadoop to transform data before loading it, we can now use SQL to transform data directly inside the CDW (dbt is a specific example). In addition, keeping raw data in the cloud helps businesses avoid losing the original information. The business problem also changes: DAs only need to modify the SQL logic to create a new reporting table instead of changing the pipeline from the source system as in the old ETL model.

---
## Batch processing and stream processing ?

Batch: data is collected into a large batch, which may run periodically by hour or day, and is then processed all at once. It has high latency and is optimized for large data volumes and statistical or periodic reporting tasks.

Stream: data is processed immediately when it is generated, with very low latency, and is optimized for real-time/near-real-time use cases.

---
## How is near-real-time different from real-time?

As the names suggest, one is real-time and the other is near-real-time. With real-time, latency is measured in milliseconds or microseconds, etc., while near-real-time is measured in seconds.

Examples:
- Real: car airbag systems, stock trading, medical-device control systems, autonomous aircraft, etc.
- Near: fraud detection (this is a bit specific; if it were real-time, it might alert immediately for every small corresponding action, which would be odd. In general, it depends and clearer signs may be needed, which is why near-real-time may be used), positioning systems (these can be real-time or near-real-time depending on the company/application), order dashboards, etc.

---
## Data lake, data warehouse and lakehouse ?

Data warehouse: a data store containing structured data, designed and optimized for fast SQL and dashboards (BI)

Data lake: everything, structured or unstructured, is stored here, cheap :)), serving DS and ML/DL

Data lakehouse: uses the inexpensive storage infrastructure of a data lake but adds the management layer (ACID transactions, schema) of a data warehouse

---
## Bronze, Silver and Golf ?

Source > Bronze > Silver > Gold > Consumer

Bronze contains raw data and performs minimal transformations. It may add an ingestion timestamp, source name, or batch ID. This layer should not contain business logic.

Silver contains cleaned and standardized data. This is where data can be reused for multiple use cases.

Gold contains data organized by business use case. The Gold layer is optimized for "reading."

---
## Dataset grain

Grain describes what each row in a dataset represents. Roughly speaking, it answers the question: "What is this data?"

---
## Business key and surrogate key

BK means a key that has meaning in the business system and is used to identify a business entity
```
customer_code
product_code
employee_number
```

SK is used to identify one record or one version of a record in the system
``` 
customer_code
valid_from
```

---
## Operational Data Store

It is used as an integrated data store that serves operational needs over a relatively short period of time. For example, the customer service department may need to view the current status of an order, payment, and delivery on one screen, and this data can be taken from the ODS. An ODS is not mandatory in every architecture; it should only be built when there is a real need to integrate operational data from multiple sources.

---
## Data mart

A dataset organized for a specific department, domain, or group of use cases.

There are two ways to build it:
- Dependent data mart: takes data from the data warehouse > departments use shared definitions and consistent data.
- Independent data mart: takes data from the source > fast, but there is a risk that departments may calculate revenue using two different sets of logic, for example.

In a modern data stack, a Gold layer or semantic model for each business domain can also be considered a type of data mart.

---
## What are the disadvantages of Medallion Architecture?

First, what is medallion architecture? It is a model for organizing and designing data in layers in data lakehouse systems. In short, it consists of the three Bronze, Silver, and Gold layers :))

So what are its disadvantages?
- Creates multiple copies of data.
- Increases latency (because data must pass through multiple layers).
- Makes it easy to create unnecessary transformations.
- The boundaries between layers are not always clear.
- It can become rigid; sometimes going directly from the source to the dashboard is much faster.
- Cost :))

---
## Should transformations be placed in ingest, Silver or Gold?

Ingest: only perform the minimum necessary work, such as adding timestamps, source metadata, and blocking corrupted files.

Silver: technical work such as standardizing timestamps, deduplicating, validating nulls, and validating schemas > clean data.

Gold: business rules such as calculating revenue, KPIs, fact/dimension tables, and creating tables for BI dashboards.

---
## When should a lakehouse not be built?

A lakehouse may not be needed when:
- The data is small or medium-sized.
- The data is mainly structured.
- The main need is BI and SQL analytics.
- PostgreSQL or a cloud data warehouse already meets the requirements.
- There is no need to process files, machine learning, or big data.
- The team is small and does not have enough capacity to operate a complex system.
- There is no need to store raw data at a large scale.
- There is no need for multiple engines to read the same dataset.
- The operating cost of the lakehouse is higher than the value it provides.

A lakehouse is more suitable when:
- The data is large-scale.
- There are many different types of data.
- BI, data science, and machine learning are needed on the same platform.
- Long-term raw data history needs to be stored.
- Multiple compute engines need to access the same dataset.
- There is a desire to avoid being completely dependent on proprietary warehouse storage.

---
## Lambda Architecture and Kappa Architecture ?

Lambda:
- The source goes through two separate pipelines, the batch layer and the speed layer, and they are combined at the serving layer
- The batch layer processes all historical data to produce accurate results. The speed layer processes new data with low latency to provide temporary results before the batch layer finishes.

Advantages:
- It can provide low latency while still being able to recompute the entire history.
- The batch layer can correct errors in the results of the speed layer.

Disadvantages:
- Two pipelines must be maintained.
- The same business logic may have to be written twice.
- It is difficult to ensure that batch and streaming produce the same results.
- Development and operating costs are high.

Kappa:
- The source log goes through stream processing to the serving layer
- Historical data is also processed by replaying events from the log.

Advantages:
- Only one processing model needs to be maintained.
- Avoids writing separate batch and stream logic.
- Suitable for event-driven systems.

Disadvantages:
- Replaying a very large amount of historical data can be difficult and time-consuming.
- The source log must retain enough data.
- State management is complex.
- Not every data source exists as an event log.
- Large batch workloads may still be more suitable for a batch engine.

---
# Reliability and data quality

---
## Idempotency

This term means allowing an operation to be performed multiple times while the final result remains the same as if it had been performed only once. In the example below, using the first statement may cause duplicates. In the second code example, the MERGE statement applies the idea of "insert if it does not exist, update if it already exists" based on the transaction_id primary key. As a result, only one SQL statement is needed instead of writing separate UPDATE and INSERT statements.

```postgres-sql
INSERT INTO transactions VALUES ('TXN-1001', 500000);
---
MERGE INTO transactions AS target
USING staging_transactions AS source
ON target.transaction_id = source.transaction_id

WHEN MATCHED THEN
    UPDATE SET amount = source.amount

WHEN NOT MATCHED THEN
    INSERT (transaction_id, amount) VALUES (source.transaction_id, source.amount);
```

---
## Why can retries create duplicates?

Because the system is not sure whether the previous operation succeeded or not. Retry improves reliability, but it is only safe when the processing operation is idempotent or uses deduplication.

---
## Safe pipeline ?

Each record should have a stable key that helps the pipeline check whether the record has already been processed.

Overwrite partitions. With a daily batch, the pipeline can reprocess the entire day and overwrite the corresponding partition.

Store checkpoints.

Separate staging and production.

Atomic commit.

Batch details.

---
## At-most-once, at-least-once and exactly-once ?

At most once: A message is processed at most once. The system does not retry when an error occurs, so it does not create duplicates, but data may be lost. [0 or 1]

At least once: A message is processed at least once. It may be processed once or multiple times. The system retries if it is not sure whether the message was processed successfully. Therefore, there is less risk of data loss, but duplicates may be created. [1 or more]

Exactly-once: Each message creates a business effect exactly once. No data is lost and no duplicate effect is created.

---
## Is exactly-once absolute across the entire system?

It should only be guaranteed within a defined scope, such as within one Kafka cluster, between a Kafka source and sink, in a stateful streaming job, ...

---
## Backfill ?

Run the pipeline to process historical data or add data that was missing in the past.

It differs from retry because backfill processes a larger range of historical data.

---
## Late-arriving data

Data reaches the processing system later than the time when it actually occurred.

---
## Dead-Letter-Queue

DLQ: a place that stores messages that cannot be processed successfully after a number of retries.

---
## Quarantine table

Similar to a DLQ, but quarantine usually belongs to a data platform or analytical pipeline

---
## Which data-quality rules should block the pipeline and which should only trigger warnings?

Not every data-quality failure should stop the pipeline.

A rule should block the pipeline when incorrect data could:
- Make financial reports incorrect.
- Cause a loss of referential integrity.
- Cause downstream jobs to crash.
- Lead to dangerous business decisions.
- Make it impossible to recover the meaning of a record.
- Violate a mandatory data contract.
- Make the entire batch unreliable.

A rule should only trigger a warning when:
- The data can still be used.
- A small difference is within tolerance.
- An unimportant field is null.
- Freshness is delayed but still within an acceptable range.
- The distribution changes slightly.
- A new category appears but does not break the schema.
- A small percentage of records is quarantined.

---
## How can duplicates be detected without a reliable event ID?

A temporary key can be created from relatively stable columns such as user_id, event_time, event_type, and amount, and then hashed for comparison. However, this method is not completely accurate because two valid events may still be identical. Therefore, the columns should be selected based on business characteristics, and a certain time window may also be included for deduplication.

---
## What is a data contract?

A data contract is an agreement between the data producer and the data consumer.

---
## What is schema evolution?

Schema evolution is when a schema changes over time in a controlled manner. Adding an optional column usually has little impact, while deleting a column or changing a data type may cause old consumers to fail. Therefore, compatibility needs to be checked before making changes.

---
## What is schema drift?

Schema drift is when the structure or format of data changes unexpectedly.

For example, the amount column used to be numeric, but the source suddenly sends it as a string: 100000 > "100,000 VND"

Schema drift can cause a pipeline to fail or create incorrect data. It can be detected using schema validation, a schema registry, or a data contract.

---
## What is data lineage used for?

Data lineage shows where data comes from, which transformation steps it passes through, and where it is ultimately used.

Example: Source > Bronze > Silver > Gold > Dashboard

It helps investigate the cause of incorrect data, check the impact before changing a schema, and identify which tables or dashboards depend on a dataset.

---
## What types of signals are included in data observability?

Data observability helps monitor the condition of data and pipelines.

Some common signals:
- Freshness: whether the data is updated on time.
- Volume: whether the number of records is abnormal.
- Schema: whether the data structure has changed.
- Data quality: whether the data contains nulls, duplicates, or incorrect formats.
- Pipeline health: whether jobs fail, run slowly, or retry too often.

The goal is to detect problems early before they affect reports or downstream systems.

---
## Freshness, completeness, uniqueness, validity and consistency ?

Freshness: whether the data is recent enough.

Completeness: whether the data is complete, for example, whether records or required fields are missing.

Uniqueness: whether the data contains duplicates.

Validity: whether the data follows the correct format and rules.

Consistency: whether the data is consistent with related tables or systems.

---
## How is reconciliation performed between source and target?

Reconciliation is checking whether the data after the pipeline matches the source.

The following can be compared:
- Number of records.
- Number of distinct keys.
- Total amount or quantity.
- Minimum and maximum timestamps.
- Number of filtered, duplicate, or quarantined records.

If the pipeline performs deduplication or filtering, row count alone cannot be compared. Records that were validly removed must also be taken into account.

---
## How should partial failure across multiple sinks be handled?

Partial failure occurs when writing to one sink succeeds but writing to another sink fails.

Example:
- Write to database: successful
- Send to API: failed

It can be handled by:
- Designing each sink to be idempotent.
- Storing the success or failure status of each sink.
- When retrying, rerunning only the failed sink.
- Using a transaction if the steps are in the same database.
- Using the outbox pattern when both updating a database and sending a message are required.

In a distributed system, it is usually difficult to roll back everything, so the system may accept eventual consistency and continue retrying the unfinished step.

---
## How can the Saga pattern be related to a multi-step pipeline?

The Saga pattern divides a large process into multiple small steps, with each step having its own transaction.

Example:

Ingest > Transform > Publish > Send API

If one step fails, the system can retry that step separately or perform a compensating action, called compensation. For example, if data has already been created but the publish step fails, the system can mark the data as incomplete and rerun the publish step later.

Saga is suitable when a pipeline includes multiple different systems and the entire process cannot be placed within a single transaction.

---
# CDC, partitioning and scale

---
## CDC - Change Data Capture

A way to capture changes in the source database such as insert, delete, and update.

Instead of retrieving the entire table, the pipeline only retrieves recently changed data. CDC usually reads the transaction log of the database and sends the changes to Kafka, a warehouse, or a lake

---
## Full load and incremental load ?

A full load reads and reloads all data from the source into the target.

An incremental load only retrieves new data or data that has changed since the previous run.

For example, a table contains 10 million records, but only 10,000 new records were added today > A full load reads all 10 million records again. An incremental load reads only the 10,000 new records.

A full load is simpler but consumes more time and resources. An incremental load is more efficient but requires a mechanism to determine which data has changed.

---
## Watermark column in batch incremental load

A watermark column is a column used to identify new data since the previous run.

For example, the previous run processed up to:
```
updated_at = 2026-08-03 10:00:00
```
The next run only retrieves:
```
WHERE updated_at > '2026-08-03 10:00:00'
```
After the pipeline runs successfully, the new watermark is stored for the next run.

---
## How does partitioning help queries and ingestion ?

Partitioning divides a large dataset into smaller parts based on a column, usually a date, month, or region. If a query only needs data from August 3, the system only needs to read the corresponding partition instead of scanning the entire table.

---
## What happens if partitioning is too small ?

If partitions are too small, the system creates a large number of partitions and small files. For example, partitioning by second can create thousands of partitions each day even if each partition contains only a few records. Partitions should be large enough to reduce the amount of data that needs to be read, but they should not be so small that they create too many files and too much metadata.

---
## Partition prunning
Partition pruning means that the query engine only reads partitions that match the query condition.

Example:
```
SELECT *
FROM sales
WHERE sale_date = '2026-08-03';
```
If the table is partitioned by sale_date, the system can skip other dates and only read the August 3 partition.

---
## Hot partition

A hot partition is a partition that receives too much data or too many requests compared with other partitions. For example, Kafka is partitioned by country, but 90% of events come from Vietnam, so the Vietnam partition must process most of the data while the other partitions are almost idle. A hot partition can be reduced by choosing a more evenly distributed key or adding an additional value to the partition key.

---
## What is the small-file problem?

The small-file problem occurs when a data lake contains too many small files. For example, a streaming job writes a file of only a few KB or a few MB every few seconds. As a result, queries may be slower than when the data is stored in fewer files of a reasonable size.

---
## What problem does compaction solve?

Compaction combines many small files into a smaller number of larger files. Compaction helps reduce the number of files, reduce metadata overhead, and speed up queries. Systems such as Iceberg, Delta Lake, or Hudi usually provide compaction or file-optimization mechanisms.

---
## How should a partition key be selected?

A partition key should be a column that:
- Frequently appears in filter conditions.
- Has neither too few nor too many distinct values.
- Distributes data relatively evenly.
- Matches the way the data is written and read.

For event data, time values such as day or month are common choices.

A column with too many distinct values, such as user_id, should not be used for partitioning because it may create millions of small partitions.

---
## When data grows by 100 times, in what order should bottlenecks be checked?

Measure first instead of immediately adding resources.

They can be checked in the following order:
- Can the source read data quickly enough?
- Is the network or message broker congested?
- Does compute lack CPU or memory?
- Are transformations or joins too heavy?
- Is the data skewed or is there a hot partition?
- Can the sink write data quickly enough?
- Are there too many small files or partitions?

After identifying the bottleneck, decide whether to optimize the code, change the partitioning, or scale resources.

---
## How are horizontal scaling and vertical scaling different?

Vertical scaling means increasing the resources of one machine:

Add CPU, RAM, or disk storage

Horizontal scaling means adding more machines or workers to divide the work:

1 worker > 5 workers

Vertical scaling is simpler but is limited by the maximum configuration of one machine.

Horizontal scaling offers better scalability, but the system must support distributed processing, partitioning, and coordination among multiple nodes.