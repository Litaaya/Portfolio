---
title: "Data Engineer Fundamentals"
date: 2026-08-03
draft: false
tags: ["data-engineer"]
description: "Một vài thông tin cơ bản của data engineer"
---
# ELT, ETL và kiến trúc dữ liệu

---
## ETL và ELT ?

Như cái tên gọi, một cái Extract > Transform > Load, một cái là Extract > Load > Transform, sự khác biệt nằm ở vị trí và thời điểm diễn ra quá trình biển đổi dữ liệu - hay còn được gọi là Transform.

Với ETL: dữ liệu được trích xuất từ nguồn, biến đổi và làm sạch trên một máy chủ trung gian, sau đó mới được nạp vào kho dữ liệu.

Với ELT: dữ liệu được trích xuất và nạp trực tiếp raw data vào kho dữ liệu, sau đó quá trình biến đổi sẽ được thực hiện ngay bên trong kho dữ liệu, cái này để nói vắng tắc thì nó sử dụng sức mạng tính toán của các cloud data warehouse hiện nay, nói chung nhiều lắm, cái này sẽ được đề cập ở phần sau.

- So về khả năng mở rộng thì ELT ăn ETL do hạn chế của ETL vào cấu hình server, tốc độ nạp cũng chậm do phải chờ transform.
- So về yêu cầu hạ tầng thì ETL sẽ hợp với on premise hoặc data warehouse truyền thống, còn ELT sẽ cần cloud data warehouse có khả năng xử lý bigdata.
- So về bảo mật thì ETL cao hơn vì nó đã biến đổi trước khi lưu, ELT cần phân quyền tốt hơn vì lưu toàn bộ dữ liệu thô.
- So về chi phí thì ELT lưu trữ tương đối rẻ, nhưng đụng tới cloud thì cũng hên xui, compute cose có thể tăng nhanh nếu transform kém tối ưu.

Với các mordern data stack thì các công ty đa số chuyển qua xài ELT kết hợp reverse ETL (đẩy các dữ liệu đã xử lý từ warehouse về lại các ứng dụng vận hành như CRM, Marketing Tool, etc.).

---
## Khi nào ETL khi nào ELT

ETL:
- Cần quan tâm bảo mật khắt khe.
- Hệ thống cũ - Legacy system: oracle, sql server có năng lực tính toán hạn chế.
- Dữ liệu nhỏ và định dạng cố định.

ELT:
- Sử dụng cloud data warehouse: snowflake, gg bigquery, aws redshift, databricks.
- Dữ liệu lớn (big data) và đa dạng.
- Data democratization: cần tính linh hoạt cao, cho phép các DA/DS tự truy vấn dữ liệu thô và tự viết các đường ống phục vụ cho từng vấn đề riêng biệt.

---
## Tại sao cloud data warehouse làm ELT phổ biến hơn

Decoupled Storage and Compute: mô hình kiến trúc tách biệt giữa lưu trữ và tính toán.

MPP - Massively Parallel Processing: khả năng xử lý song song.

Nhờ hai yếu tố trên cdw (cloud data warehouse) có thể biến đổi big data trực tiếp trong kho với chi phí rẻ, xóa bỏ rào cản hiệu năng mà các hệ thống sql truyền thống trước đây gặp phải.

Đi sâu vào nào:
- Với Decoupled Storage and Compute: ngày trước dung lượng lưu trữ và sức mạng tính toán đi chung với nhau, nếu nạp raw data vào kho quá nhiều, nó sẽ làm hết dung lượng đĩa cứng và khiến các truy vấn báo cáo không ổn định. Bây giờ cloud như s3, google cloud storage có chi phí rẻ, có thể nạp big data raw vào kho mà không lo tốn tiền lưu trữ hay ảnh hưởng đến sức mạng tính toán, khi nào cần transform, hệ thống mới bật tài nguyên compute lên để xử lý.
- Với MPP: cdw sử dụng sử dụng nhiều máy ảo phân tán bên dưới để chạy truy vấn cùng 1 lúc, nói ngắn gọn là nhiều người làm hơn đồng nghĩa nhanh hơn so với ETL server.

Mình còn tìm hiểu được một lý do khác, đó là việc thay vì phải code Java, python scala phức tạp trên spark/hadoop để transform dữ liệu trước khi nạp, giờ đây có thể xài sql để transform trực tiếp ngay bên trong cdw (ví dụ cụ thể là dbt nè). Ngoài ra việc giữ raw data trên cloud sẽ giúp doanh nghiệp không lo mất mát thông tin gốc. Bài toán kinh doanh cũng thay đổi thành các DA chỉ cần sửa lại logic SQL để tạo bảng báo cáo mới, thay vì phải sửa lại pipeline từ hệ thống nguồn như mô hình etl cũ.

---
## Batch processing và stream processing ?

Batch: gom lại chạy thành một tập lớn, có thể định kỳ theo giờ ngày, rồi mới xử lý cùng lúc, latency cao, tối ưu cho khối lượng dữ liệu lớn và các bài toán thống kê, báo cáo định kỳ.

Stream: xử lý dữ liệu ngay lập tức khi vừa phát sinh, latency siêu thấp, tối ưu cho real-time/near real-time.

---
## Near-real-time khác real-time như thế nào?

Như tên gọi, một cái thời gian thực, một cái thời gian gần như thực, với realtime, latency tính bằng mili giây hoặc micro giây etc., với near thì tính bằng giây.

Ví dụ:
- Real: Hệ thống túi khí trên oto này, giao dịch chứng khoáng, hệ thống điều khiển thiết bị y tế, máy bay tự hành, etc.
- Near: Phát hiện gian lận (cái này hơi đặc thù, nếu real thì chỉ cần hành động nhỏ tương ứng là nó báo luôn thì hơi kỳ, nói chung hên xui nên phải có dấu hiệu rõ rằng hơn, vậy nên mới cần là near), hệ thống định vị (cái này có real có near, tùy công ty/ứng dụng), dashboard đơn hàng, etc.

---
## Data lake, data warehouse và lakehouse ?

Data warehouse: kho dữ liệu chứa tructured data, thiết kế tối ưu cho sql nhanh và dash (BI)

Data lake: mọi thứ tructured hoặc unstructured đều nằm trong đây, rẻ :)), phục vụ DS và ML/DL

Data lakehouse: dùng hạ tầng lưu trữ giá rẻ của datalake nhưng thêm tầng quản trị (ACID transactions, schema) của data warehouse

---
## Bronze, Silver và Golf ?

Source > Bronze > Silver > Gold > Consumer

Bronze chứa dữ liệu thô, thực hiện trans tối thiểu, có thể thêm ingest timestamp, source name hoặc batch ID, phần này không nên chứa business logic.

Silver chứa dữ liệu đã được làm sạch và chuẩn hóa, đây là nơi dữ liệu có thể tái sử dụng cho nhiều use case.

Gold chứa dữ liệu theo business use case, gold layer là để tối ưu cho việc "đọc".

---
## Grain của dataset

Grain mô tả mỗi dòng trong dataset đại diện cho điều gì, đại khái nó là câu hỏi: "Data này là gì ?"

---
## Business key và surrogate key

BK nghĩa là khóa có ý nghĩa trong hệ thống nghiệp vụ, sử dụng để xác định đối tượng trong nghiệp vụ
```
customer_code
product_code
employee_number
```

SK là để xác định 1 record hoặc 1 phiên bản record trong hệ thống
``` 
customer_code
valid_from
```

---
## Operational Data Store

Sử dụng như kho dữ liệu tích hợp phục vụ nhu cầu vận hành trong thời gian tương đối ngắn. Ví dụ như bộ phận chăm sóc khách hàng cần xem trạng thái hiện tại của đơn hàng, thanh toán và vận chuyển trên một màn hình, dữ liệu này có thể lấy từ ODS. ODS không bắt buộc phải có trong mọi kiến trúc, nó chỉ nên được xây khi thực sự có nhu cầu tích hợp dữ liệu vận hành từ nhiều nguồn.

---
## Data mart

Tập dữ liệu được tổ chức cho một phòng ban, domain hoặc nhóm usecase cụ thể.

Có hai cách xây:
- Dependent data mart: lấy từ data warehouse > các phòng ban sử dụng chung định nghĩa và dữ liệu nhất quán.
- Independent data mart: lấy từ source > nhanh nhưng có rủi ro các phòng ban có thể tính revenue bằng 2 logic khác nhau là 1 ví dụ.

Trong modern data stack, Gold layer hoặc semantic model dành cho từng business domain cũng có thể được xem như một dạng data mart.

---
## Medallion Architecture có nhược điểm gì?

Đầu tiên thì medallion architecture là gì ? Nó là mô hình tổ chức và thiết kế dữ liệu theo lớp trong các hệ thống data lakehouse, nói gọn lại thì nó là 3 lớp bronze, silver và gold :))

Vậy nhược điểm của nó là gì ?
- Tạo nhiều bản sao dữ liệu.
- Tăng latency (do phải đi qua nhiều tầng).
- Dễ tạo transform không cần thiết.
- Ranh giới giữa các tầng không phải lúc nào cũng rõ.
- Dễ bị cứng nhắc, đôi khi đi thẳng từ source qua dash nó sẽ nhanh hơn nhiều.
- Chi phí :))

---
## Transform nên đặt ở ingest, silver hay gold ?

Ingest: chỉ thực hiện tối thiểu mức cần thiết, ví như thêm timestamp, metadata source, chặn file hỏng.

Silver: mang tính kỹ thuật như chuẩn hóa timestamp, deduplicate, valid null, validate schema > dữ liệu sạch.

Gold: mang tính business rule như tính revenue, KPI, fact/dimension, tạo bảng phục vụ dash BI.

---
## Khi nào không nên xây lakehouse

Có thể không cần lakehouse khi:
- Dữ liệu nhỏ hoặc trung bình.
- Chủ yếu là structured data.
- Nhu cầu chính là BI và SQL analytics.
- Một PostgreSQL hoặc cloud data warehouse đã đáp ứng đủ.
- Không có nhu cầu xử lý file, machine learning hoặc big data.
- Đội ngũ nhỏ và không đủ khả năng vận hành hệ thống phức tạp.
- Không cần lưu raw data với quy mô lớn.
- Không cần nhiều engine cùng đọc một tập dữ liệu.
- Chi phí vận hành lakehouse cao hơn giá trị mang lại.

Lakehouse phù hợp hơn khi:
- Dữ liệu có quy mô lớn.
- Có nhiều loại dữ liệu khác nhau.
- Cần BI, data science và machine learning trên cùng nền tảng.
- Muốn lưu lịch sử raw data dài hạn.
- Cần nhiều compute engine truy cập cùng dataset.
- Muốn tránh phụ thuộc hoàn toàn vào proprietary warehouse storage.

---
## Lambda Architecture và Kappa Architecture ?

Lambda:
- Source đi qua 2 pipeline riêng, batch layer và speed layer và gộp lại ở serving layer
- Batch layer xử lý toàn bộ dữ liệu lịch sử để tạo kết quả chính xác. Speed layer xử lý dữ liệu mới với latency thấp để cung cấp kết quả tạm thời trước khi batch layer hoàn thành.

Ưu điểm:
- Có thể vừa đảm bảo latency thấp vừa có khả năng tính lại toàn bộ lịch sử.
- Batch layer có thể sửa sai cho kết quả của speed layer.

Nhược điểm:
- Phải duy trì hai pipeline.
- Có thể phải viết cùng một business logic hai lần.
- Khó đảm bảo batch và streaming cho ra cùng kết quả.
- Chi phí phát triển và vận hành cao.

Kappa:
- Source log qua stream processing tới serving layer
- Dữ liệu lịch sử cũng được xử lý bằng cách replay event từ log.

Ưu điểm:
- Chỉ cần duy trì một processing model.
- Tránh viết logic batch và stream riêng biệt.
- Phù hợp với hệ thống event-driven.

Nhược điểm:
- Replay một lượng dữ liệu lịch sử rất lớn có thể khó và tốn thời gian.
- Source log phải lưu đủ dữ liệu.
- State management phức tạp.
- Không phải mọi nguồn dữ liệu đều tồn tại dưới dạng event log.
- Các bài toán batch lớn đôi khi vẫn phù hợp với batch engine hơn.

---
# Reliability và data quality

---
## Idempotency

Từ này nghĩa là cho phép 1 thao tác được thực hiện nhiều lần nhưng kết quả sau cùng vẫn giống như khi chỉ thực hiện một lần. Ví dụ ở dưới, khi xài dòng lệnh đầu tiên, có thể xảy ra tình trạng duplicate, ở code lệnh 2 câu lệnh MERGE này thực hiện tư duy "Nếu chưa có thì thêm mới, nếu có rồi thì cập nhật" dựa trên khóa chính transaction_id. Nhờ đó chỉ cần chạy duy nhất 1 câu lệnh SQL thay vì phải viết 2 câu lệnh UPDATE và INSERT riêng biệt.

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
## Tại sao retry có thể tạo duplicate

Vì hệ thống sẽ không chắc theo tác trước đó đã thành công hay chưa, retry giúp tăng reliability nhưng chỉ an toàn khi thao tác xử lý có tính idempotent hoặc đeuplication.

---
## Pipeline an toàn ?

Mỗi record nên có 1 stable key giúp pipeline xem coi record đã được xử lý hay chưa.

Ghi đè partition, với batch theo ngày, pipeline có thể xử lý lại toàn bộ ngày và ghi đè partition tương ứng.

Lưu checkpoint.

Tách staging và production.

Atomic commit.

Batch details.

---
## At-most-once, at-least-once và exactly-once ?

At most once : Message được xử lý tối đa một lần. Hệ thống không retry khi có lỗi, do đó không tạo duplicate nhưng có thể mất dữ liệu. [0 or 1]

At least once : Message được xử lý ít nhất một lần. Có thể xử lý 1 lần hoặc nhiều lần. Hệ thống retry nếu chưa chắc message đã được xử lý thành công. Do đó ít có nguy cơ mất dữ liệu nhưng có thể tạo duplicate. [1 or more]

Exactly-once : Mỗi message tạo ra ảnh hưởng nghiệp vụ đúng một lần. Không mất dữ liệu và không tạo ảnh hưởng trùng lặp.

---
## Exactly-one có tuyệt đối trên toàn bộ hệ thống không ?

Chỉ nên đảm bảo trong một phạm vi xác định, chẳng hạn như trong 1 cluster kafka, giữa kafka source và sink, trong stateful streaming job, ...

---
## Backfill ?

Chạy pipeline để xử lý dữ liệu lịch sử hoặc bổ sung dữ liệu bị thiết trong quá khứ.

Nó khác với retry khi backfill sẽ xử lý phạm vụ dữ liệu lịch sử lớn hơn.

---
## Late-arriving data

Dữ liệu đến hệ thống xử lý muộn hơn thời điểm mà nó thực sự xảy ra.

---
## Dead-Letter-Queue

DLQ: nơi lưu trữ các message không thể xử lý thành công sau một số lần retry.

---
## Quarantine table

Tương tự với dlq nhưng quarantine thường thuộc dạng data platform hoặc analytical pipeline

---
## Data-quality rule nào nên block pipeline và rule nào chỉ nên cảnh báo?

Không phải mọi data-quality failure đều nên làm pipeline dừng.

Rule nên block pipeline khi dữ liệu lỗi có thể:
- Làm sai báo cáo tài chính.
- Gây mất referential integrity.
- Làm downstream job crash.
- Tạo quyết định nghiệp vụ nguy hiểm.
- Không thể khôi phục ý nghĩa của record.
- Vi phạm data contract bắt buộc.
- Làm toàn bộ batch không đáng tin cậy.

Rule chỉ nên cảnh báo khi:
- Dữ liệu vẫn có thể sử dụng.
- Sai lệch nhỏ nằm trong tolerance.
- Một trường không quan trọng bị null.
- Freshness chậm nhưng vẫn trong phạm vi có thể chấp nhận.
- Distribution thay đổi nhẹ.
- Một category mới xuất hiện nhưng không phá schema.
- Một tỷ lệ nhỏ record bị quarantine.

---
## Làm sao phát hiện duplicate khi không có event ID đáng tin cậy?

Có thể tạo một khóa tạm từ các cột tương đối ổn định như user_id, event_time, event_type và amount, sau đó hash chúng để so sánh. Tuy nhiên, cách này không chính xác tuyệt đối vì hai event hợp lệ vẫn có thể giống nhau. Vì vậy, cần lựa chọn các cột dựa trên đặc điểm nghiệp vụ và có thể kết hợp thêm một khoảng thời gian nhất định để deduplicate.

---
## Data contract là gì?

Data contract là thỏa thuận giữa bên tạo dữ liệu và bên sử dụng dữ liệu.

---
## Schema evolution là gì?

Schema evolution là việc schema thay đổi theo thời gian một cách có kiểm soát. Thêm một cột optional thường ít ảnh hưởng, còn xóa cột hoặc thay đổi data type có thể làm consumer cũ bị lỗi. Vì vậy sẽ cần kiểm tra compatibility trước khi thay đổi.

---
## Schema drift là gì?

Schema drift là khi cấu trúc hoặc format dữ liệu thay đổi ngoài dự kiến.

Ví dụ, cột amount trước đây là số nhưng source đột nhiên gửi thành chuỗi: 100000 > "100,000 VND"

Schema drift có thể làm pipeline lỗi hoặc tạo ra dữ liệu sai. Có thể phát hiện bằng schema validation, schema registry hoặc data contract.

---
## Data lineage dùng để làm gì?

Data lineage cho biết dữ liệu đến từ đâu, được transform qua những bước nào và cuối cùng được sử dụng ở đâu.

Ví dụ: Source > Bronze > Silver > Gold > Dashboard

Nó giúp điều tra nguyên nhân khi dữ liệu sai, kiểm tra ảnh hưởng trước khi sửa schema và xác định những bảng hoặc dashboard đang phụ thuộc vào một dataset.

---
## Data observability gồm những loại tín hiệu nào?

Data observability giúp theo dõi tình trạng của dữ liệu và pipeline.

Một số tín hiệu phổ biến:
- Freshness: dữ liệu có cập nhật đúng giờ không.
- Volume: số lượng record có bất thường không.
- Schema: cấu trúc dữ liệu có thay đổi không.
- Data quality: dữ liệu có null, duplicate hoặc sai format không.
- Pipeline health: job có lỗi, chạy chậm hoặc retry nhiều không.

Mục tiêu là phát hiện vấn đề sớm trước khi nó ảnh hưởng đến báo cáo hoặc downstream system.

---
## Freshness, completeness, uniqueness, validity và consistency ?

Freshness: dữ liệu có đủ mới không.

Completeness: dữ liệu có đầy đủ không, chẳng hạn có thiếu record hoặc field bắt buộc không.

Uniqueness: dữ liệu có bị trùng không.

Validity: dữ liệu có đúng format và rule không.

Consistency: dữ liệu có thống nhất với các bảng hoặc hệ thống liên quan không.

---
## Làm sao reconciliation giữa source và target?

Reconciliation là kiểm tra dữ liệu sau pipeline có khớp với source hay không.

Có thể so sánh:
- Số lượng record.
- Số lượng distinct key.
- Tổng amount hoặc quantity.
- Min và max timestamp.
- Số record bị loại, duplicate hoặc quarantine.

Nếu pipeline có deduplicate hoặc filter thì không thể chỉ so sánh row count. Cần tính cả những record đã bị loại hợp lệ.

---
## Làm sao xử lý partial failure giữa nhiều sink?

Partial failure xảy ra khi ghi thành công vào một sink nhưng thất bại ở sink khác.

Ví dụ:
- Ghi database: thành công
- Gửi API: thất bại

Có thể xử lý bằng cách:
- Thiết kế mỗi sink có tính idempotent.
- Lưu trạng thái thành công hoặc thất bại của từng sink.
- Khi retry, chỉ chạy lại sink bị lỗi.
- Sử dụng transaction nếu các bước nằm trong cùng một database.
- Dùng outbox pattern khi cần vừa cập nhật database vừa gửi message.

Trong distributed system, thường khó rollback toàn bộ nên hệ thống có thể chấp nhận eventual consistency và tiếp tục retry bước chưa thành công.

---
## Saga pattern có thể liên hệ thế nào với pipeline nhiều bước?

Saga pattern chia một quy trình lớn thành nhiều bước nhỏ, mỗi bước có transaction riêng.

Ví dụ:

Ingest > Transform > Publish > Gửi API

Nếu một bước thất bại, hệ thống có thể retry riêng bước đó hoặc thực hiện một hành động bù, gọi là compensation. Ví dụ nếu đã tạo dữ liệu nhưng bước publish thất bại, hệ thống có thể đánh dấu dữ liệu chưa hoàn thành và chạy lại bước publish sau.

Saga phù hợp khi pipeline gồm nhiều hệ thống khác nhau và không thể đặt toàn bộ quá trình trong một transaction duy nhất.

---
# CDC, partitioning và scale

---
## CDC - Change Data Capture

Cách ghi nhận những thay đổi trong source db như insert, delete, update.

Thay vì lấy toàn bộ bảng, pipeline sẽ chỉ lấy những dữ liệu vừa thay đổi. CDC thường đọc transaction log của db rồi gửi thay đổi sang kafka, ware hoặc lake

---
## Full load và incremental load ?

Full load đọc và nạp lại toàn bộ dữ liệu từ source vào target.

Incremental load chỉ lấy dữ liệu mới hoặc dữ liệu đã thay đổi kể từ lần chạy trước.

Ví dụ, bảng có 10 triệu record nhưng hôm nay chỉ có 10.000 record mới > Full load đọc lại 10 triệu record. Incremental load chỉ đọc 10.000 record mới.

Full load đơn giản hơn nhưng tốn thời gian và tài nguyên. Incremental load hiệu quả hơn nhưng cần cơ chế xác định dữ liệu nào đã thay đổi.

---
## Watermark column trong batch incremental load

Watermark column là cột được dùng để xác định dữ liệu mới kể từ lần chạy trước.

Ví dụ, lần chạy trước đã xử lý đến:
```
updated_at = 2026-08-03 10:00:00
```
Lần chạy tiếp theo chỉ lấy:
```
WHERE updated_at > '2026-08-03 10:00:00'
```
Sau khi pipeline chạy thành công, watermark mới được lưu lại cho lần chạy sau.

---
## Partitioning giúp gì cho query và ingest ?

Partitioning chia một dataset lớn thành nhiều phần nhỏ dựa trên một cột, thường là ngày, tháng hoặc khu vực. Nếu query chỉ cần dữ liệu ngày 3 tháng 8, hệ thống chỉ cần đọc partition tương ứng thay vì quét toàn bộ bảng.

---
## Partioning quá nhỏ thì sao ?

Nếu chia partition quá nhỏ, hệ thống sẽ tạo ra rất nhiều partition và file nhỏ. Ví dụ, partition theo từng giây có thể tạo hàng nghìn partition mỗi ngày dù mỗi partition chỉ có vài record. Partition nên đủ lớn để giúp giảm dữ liệu cần đọc nhưng không nên nhỏ đến mức tạo quá nhiều file và metadata.

---
## Partition prunning
Partition pruning là việc query engine chỉ đọc những partition phù hợp với điều kiện truy vấn.

Ví dụ:
```
SELECT *
FROM sales
WHERE sale_date = '2026-08-03';
```
Nếu bảng được partition theo sale_date, hệ thống có thể bỏ qua các ngày khác và chỉ đọc partition ngày 3 tháng 8.

---
## Hot partition

Hot partition là một partition nhận quá nhiều dữ liệu hoặc request so với các partition khác. Ví dụ, partition Kafka theo country, nhưng 90% event đều đến từ Việt Nam thì Partition Việt Nam sẽ phải xử lý phần lớn dữ liệu trong khi các partition khác gần như rảnh. Có thể giảm hot partition bằng cách chọn key phân phối đều hơn hoặc thêm một giá trị phụ vào partition key.

---
## Small-file problem là gì?

Small-file problem xảy ra khi data lake chứa quá nhiều file có kích thước nhỏ. Ví dụ, streaming job cứ vài giây lại ghi ra một file chỉ vài KB hoặc vài MB. Do đó, query có thể chậm hơn so với khi dữ liệu được lưu trong ít file có kích thước hợp lý.

---
## Compaction giải quyết vấn đề gì?

Compaction gộp nhiều file nhỏ thành một số file lớn hơn. Compaction giúp giảm số file, giảm metadata overhead và tăng tốc query. Các hệ thống như Iceberg, Delta Lake hoặc Hudi thường có cơ chế compaction hoặc file optimization.

---
## Làm sao chọn partition key?

Partition key nên là cột:
- Thường xuất hiện trong điều kiện filter.
- Có số lượng giá trị không quá ít hoặc quá nhiều.
- Phân phối dữ liệu tương đối đều.
- Phù hợp với cách dữ liệu được ghi và đọc.

Với dữ liệu sự kiện, thời gian như ngày hoặc tháng thường là lựa chọn phổ biến.

Không nên partition theo một cột có quá nhiều giá trị như user_id, vì có thể tạo hàng triệu partition nhỏ.

---
## Khi dữ liệu tăng 100 lần, nên kiểm tra bottleneck theo thứ tự nào?

Trước tiên cần đo lường thay vì vội thêm tài nguyên.

Có thể kiểm tra theo thứ tự:
- Source có đọc dữ liệu đủ nhanh không?
- Network hoặc message broker có bị nghẽn không?
- Compute có thiếu CPU hoặc memory không?
- Transformation hoặc join có quá nặng không?
- Dữ liệu có bị skew hoặc hot partition không?
- Sink có ghi dữ liệu đủ nhanh không?
- Có quá nhiều file nhỏ hoặc partition không?

Sau khi xác định bottleneck mới quyết định tối ưu code, thay đổi partitioning hoặc scale tài nguyên.

---
## Horizontal scaling và vertical scaling khác nhau ra sao?

Vertical scaling là tăng tài nguyên cho một máy:

Thêm CPU, RAM hoặc ổ đĩa

Horizontal scaling là thêm nhiều máy hoặc worker để chia công việc:

1 worker > 5 workers

Vertical scaling đơn giản hơn nhưng bị giới hạn bởi cấu hình tối đa của một máy.

Horizontal scaling có khả năng mở rộng tốt hơn nhưng hệ thống phải hỗ trợ xử lý phân tán, chia partition và phối hợp giữa nhiều node.