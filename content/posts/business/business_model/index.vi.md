---
title: "Business Model and Business Domain Knowledge"
date: 2026-10-10
draft: false
tags: []
description: "Tuần 41"
---
# BU

Với cty hiện tại, mình có 2 BU (Business Unit) khác nhau là Pico và Nguyễn Kim. Ở 2 BU này sẽ có các channel bán hàng khác nhau, với Pico thì có riêng `Bán hàng tại kho`, ngoài ra còn các channel khác như telesales, web và các platform khác nhau.

---
# Cat and Subcate

Hiểu đơn giản thì nó là Category chính của hàng với Sub_Category chi tiết hơn của hàng.

---
# Affiliate

Affiliate (AT và MO), với affiliate thì nó sẽ có các publisher như là KOL, KOC hay Banking. Có thể hình dung như AT và MO là affiliate network, nó như một tech platform trung gian vậy.

Bức tranh tổng thể có thể hiểu như sau:

- Advertiser: chủ sản phẩm hoặc dịch vụ muốn bán hàng hoặc tìm khách mới.
- Affiliate Network: các mạng lưới trung gian đóng vai trò kết nối kỹ thuật và thương mại.
- Publisher: cá nhân hoặc tổ chức sở hữu lượng người dùng, kéo khách hàng thực hiện hành động thông qua link liên kết.
- End_user: người dùng

---
# SKUs

Mã sản phẩm, định danh sản phẩm, cái này còn tùy nghiệp vụ của mỗi công ty, đa phần sẽ là một dãy số.

---
# MTD, LMonth, LYear

3 thuật ngữ trên là yêu cầu quan trọng cần có khi viết báo cáo:
- MTD: month_to_date tương đương với lũy kế từ ngày đầu tiên của tháng hiện tại cho đến ngày hiện tại hoặc ngày chốt dữ liệu gần nhất `T - 1`. Mục đích của nó là theo dõi run_rate và tiến độ KPI của tháng.
- LMonth: last_month tương đương với trọn vẹn toàn bộ tháng trước đó. Mục đích thông thường là chốt sổ hoàn chỉnh tháng gần nhất để đánh giá hiệu suất hoặc so sánh tăng trưởng hằng tháng.
- LYear: last_year tương đương với trọn vẹn toàn bộ năm trước đó.

Các thông số quan trọng mình được dạy hãy để ý là `quantity` và `values`, ngoài ra còn có `stocks` nữa.

---
# Requirements

Yêu cầu tối thiểu khi báo cáo là các metrics + thông số:
```text
cat
sub_cat
skus
name
channel
sub_channel
brand
bu
```

Trong kinh doanh mảng ecommerce, có một vài thông tin quan trọng mà mình sẽ làm việc xoay quanh nó bao gồm `stock`, `price`, `traffic` và `assertment` (`selection`).

Nói riêng về selection: có thể hiểu như nó trả lời cho câu hỏi `có bao nhiêu sản phẩm để bán ?`, một món hàng sẽ có một vài trạng thái như sau:
- EOLine: end_of_line nghĩa là cuối vòng đời của sản phẩm.
- OOS: out_of_stock nghĩa là hết hàng tồn kho.
- Saleable: nghĩa là có thể bán.
- Unsaleable: không thể bán.
- Hassale: có hai trạng thái là Heroes SKU hoặc Normal SKU

---
# Sales

Có các model chính (phễu chuyển đổi doanh thu - Sales Revenue Funnel) là:
- GMV: gross_merchant_value - đơn hàng đã lên - đo lường tổng nhu cầu thị trường (demand size).
- CMV: confirm_merchant_value - khách hàng đã confirm - đo lường chất lượng đơn đầu vào.
- NMV: net_merchant_value - đã thanh toán - doanh thu thực ghi nhận.

Lưu ý NMV với doanh thu trong tháng là khác nhau, doanh thu tháng nghĩa là ghi nhận những gì đã hoàn thành và giao xong trong tháng đó, không quan tâm khách đặt bao giờ, còn NMV nghĩa là dù giao ghi nào, nó đều quy về tháng mà khách đặt.

---
# Traffic

Có các model chính là:
- Page View (landing page)
- Detail View (Product detail)
- Add To Cart
- Purchase

Đối với page view, đa phần sẽ có các hướng 1 là organic (nhu cầu khách hàng), 2 là bỏ tiền chạy add (paid traffic)

---
# Competitive strategy

Các yếu tố chính bao gồm `price`, `sự khác biệt`, `mix`

---
# Formula

`Giá bán trung bình` = `Value` / `Quantity`

`ASP` (average selling price) = `revenue` / `quantity`
- Giá bán trung bình

`AOV` (average order value) = `revenue` / `order`
- Giá trung bình của 1 order (có thể có nhiều sản phẩm), cái này có ảnh hưởng đến yếu tố `LOGISTIC`

`Revenue` = `Traffic` x `Conversion_rate` x `Average_order_value` x `Return`
- Các yếu tốt tác động đến doanh thu:
  - `Traffic` tương ứng là lượng truy cập - quy mô đầu vào
  - `CR` tương ứng là tỷ lệ chuyển đổi, nó sẽ được tính toán bằng các chỉ số:
    - `CIR` (cost_to_income_ratio hoặc cost_to_revenue_ratio) = Chi phí cost chia cho doanh thu mang lại, chỉ số này càng thấp càng tốt.
    - Ngoài ra còn được biết đến như `ROAS` nhưng áp dụng cho chi phí quảng cáo bỏ ra và mang về.
    - Ngoài ra còn có `ROI` nghĩa là return_on_investment, tính bằng cách lấy net_profit (lợi nhuận ròng) chia cho tổng chi phí đầu tư.
  - `AOV`: cao nghĩa là khách mua 1 đơn nhiều hàng hơn = tốt hơn.
  - `Return` hoặc `Repeat_Rate`/`Frequency`: tỉ lệ hoàn đơn hoặc tỷ lệ mua lại/đổi trả.

`PVM`: price volume mix = `Price Effect` + `Volume Effect` + `Mix Effect`
- `PE`: sự thay đổi doanh thu do điều chỉnh giá bán.
- `VF`: sự thay đổi doanh thu do tổng quy mô số lượng sản phẩm bán ra.
- `ME`: sự thay đổi doanh thu do sự dịch chuyển tỷ trọng bán giữa các dòng sản phẩm (đắt vs rẻ).

`Gross Point` (`GP`) = `Revenue` - `COGS` và `% GP` = (`GP` / `Revenue`) x 100%.
- `COGS` ở đây nghĩa là chi phí (giá vốn), bao gồm định phí (fixed costs) và biến phí (variable costs).
- Còn có 2 khái niệm là `front GP` và `back GP`:
  - `front GP` = `Revenue` - `COGS`
  - `back GP` = `front GP` + `rebate` (tiền tài trợ)
- Với `rebate`, nó có thể tới từ sell in (tiền thưởng khi nhập hàng), sell out (tiền thưởng dựa trên những gì mình bán cho khách hàng, nhận từ hãng) và marketing (hãng trả khi mình cho họ thực hiện các hoạt động quảng bá, trưng bày sản phẩm tại điểm bán).