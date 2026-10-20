---
title: "Business Model and Business Domain Knowledge"
date: 2026-10-10
draft: false
tags: []
description: "Tuần 41"
---
# Business Foundation

## Business Unit & Channel

Hiện tại có 2 **BU (Business Unit)** là Pico và Nguyễn Kim. Mỗi BU có các kênh bán hàng (Channel) khác nhau như Web, Telesales, Marketplace hoặc Bán hàng tại kho (riêng Pico).

- **Channel:** Kênh bán hàng chính.
- **Sub-channel:** Kênh bán hàng chi tiết thuộc Channel.

## Product Hierarchy

- **Category (Cat):** Ngành hàng chính.
- **Subcategory (Subcat):** Nhóm hàng chi tiết.
- **Brand:** Thương hiệu.
- **SKU (Stock Keeping Unit):** Mã định danh đơn vị hàng hóa, tùy quy ước của doanh nghiệp.

Các dimension thường dùng trong báo cáo:

`BU` · `Channel` · `Sub-channel` · `Cat` · `Subcat` · `Brand` · `SKU` · `Product Name`

## Time Intelligence

- **MTD (Month to Date):** Lũy kế từ đầu tháng đến ngày chốt dữ liệu (thường T-1), dùng theo dõi tiến độ KPI.
- **LMonth (Last Month):** Toàn bộ tháng trước, dùng so sánh hiệu suất MoM.
- **LYear (Last Year):** Toàn bộ năm trước, dùng đánh giá hiệu suất năm.

Các đại lượng cần quan tâm: `Quantity`, `Value`, `Stock`.

---

# Sales — Performance & Profitability

Trả lời câu hỏi: **Bán được bao nhiêu, doanh thu và lợi nhuận thế nào?**

## Sales Funnel

| Metric                            | Ý nghĩa                                    |
|-----------------------------------|--------------------------------------------|
| GMV (Gross Merchandise Value)     | Tổng giá trị đơn hàng phát sinh            |
| CMV (Confirmed Merchandise Value) | Giá trị đơn hàng đã xác nhận               |
| NMV (Net Merchandise Value)       | Giá trị đơn hàng thuần theo quy ước nội bộ |

**Lưu ý:** Theo nghiệp vụ hiện tại, NMV được quy về tháng khách đặt hàng, còn Revenue ghi nhận theo tháng hoàn thành/giao hàng. Cần xác nhận thêm quy tắc hoàn/hủy và thanh toán.

## Order Fulfillment — Quy trình xử lý đơn hàng

Mỗi đơn hàng được định danh bằng `Order ID` (mã đơn hàng) và trải qua các bước:

`Order Placed → CNOW Confirmation → Warehouse Confirmation → Preparing → Shipping → Delivered → Completed`

- **Order Placed:** Khách hàng đặt đơn, hệ thống tạo mã đơn và chuyển về CNOW.
- **CNOW Confirmation:** Nhân viên xác nhận đơn hàng.
- **Warehouse Confirmation:** Kho phù hợp/gần khách xác nhận khả năng xuất hàng.
- **Preparing:** Kho chuẩn bị, đóng gói hàng hóa.
- **Shipping:** Đơn hàng được bàn giao để vận chuyển.
- **Delivered:** Giao hàng thành công.
- **Completed:** Hệ thống xác nhận đơn hoàn thành.

**Lưu ý:** `Order Status` cần phân biệt với `Payment Status` và `Delivery Status`. Đơn hàng cũng có thể phát sinh các trạng thái ngoại lệ như hủy đơn, hết hàng hoặc giao thất bại.

## Sales Metrics

- **ASP (Average Selling Price)** = Revenue / Quantity.
- **AOV (Average Order Value)** = Revenue / Orders.
- **Revenue:** Doanh thu được ghi nhận trong kỳ.
- **COGS (Cost of Goods Sold):** Giá vốn hàng bán.
- **GP (Gross Profit)** = Revenue − COGS.
- **GP Margin (%)** = GP / Revenue × 100%.

## Front GP & Back GP

- **Front GP:** Lợi nhuận gộp trực tiếp từ bán hàng.
- **Back GP:** Front GP cộng các khoản tài trợ/rebate đủ điều kiện theo quy ước nội bộ.

Nguồn Rebate:
- **Sell-in:** Thưởng theo hoạt động nhập hàng.
- **Sell-out:** Thưởng theo kết quả bán hàng.
- **Marketing:** Tài trợ quảng bá, trưng bày sản phẩm.

## Price Volume Mix (PVM)

Phân rã biến động doanh thu thành ba yếu tố:

`Revenue Change = Price Effect + Volume Effect + Mix Effect`

- **Price Effect (PE):** Ảnh hưởng do thay đổi giá bán.
- **Volume Effect (VE):** Ảnh hưởng do thay đổi tổng số lượng bán.
- **Mix Effect (ME):** Ảnh hưởng do thay đổi cơ cấu sản phẩm bán ra.

---

# Traffic — Acquisition & Conversion

Trả lời câu hỏi: **Khách hàng đến từ đâu, tương tác và chuyển đổi như thế nào?**

## Traffic Acquisition

- **Organic:** Lượt truy cập tự nhiên.
- **Paid Traffic:** Lượt truy cập từ quảng cáo trả phí.
- **Affiliate:** Lượt truy cập/chuyển đổi thông qua đối tác tiếp thị liên kết.

### Affiliate Model

`Advertiser → Affiliate Network → Publisher → End User`

- **Advertiser:** Doanh nghiệp muốn quảng bá và bán sản phẩm.
- **Affiliate Network:** Nền tảng trung gian kết nối và theo dõi chuyển đổi (ví dụ nội bộ: AT, MO).
- **Publisher:** KOL, KOC, Banking hoặc đối tác phân phối.
- **End User:** Khách hàng cuối.

## Conversion Funnel

`Page View → Product Detail View → Add to Cart → Purchase`

- **Page View:** Lượt xem trang.
- **Product Detail View:** Lượt xem chi tiết sản phẩm.
- **Add to Cart (ATC):** Thêm sản phẩm vào giỏ.
- **Purchase:** Hoàn tất mua hàng.
- **CR (Conversion Rate):** Tỷ lệ chuyển đổi.

## Marketing Efficiency

- **CIR (Cost to Income Ratio)** = Cost / Income.
- **ROAS (Return on Ad Spend)** = Revenue attributed to Ads / Ad Spend.
- **ROI (Return on Investment)** = Net Profit from Investment / Investment Cost.
- **Repeat Purchase Rate:** Tỷ lệ khách hàng mua lại.
- **Purchase Frequency:** Tần suất mua hàng.

## Revenue Drivers

`Revenue = Sessions × CR × AOV`

Trong đó CR = Orders / Sessions, với cùng phạm vi đo lường.

Doanh thu còn chịu ảnh hưởng từ hoàn/hủy đơn, giá bán, tồn kho và khả năng chuyển đổi.

---

# Assortment (Selection) — Product & Availability

Trả lời câu hỏi: **Có bao nhiêu sản phẩm để bán, có đủ hàng và giá có cạnh tranh không?**

## Selection & Product Status

- **Selection:** Tập hợp SKU thuộc danh mục kinh doanh.
- **EOL (End of Line):** Sản phẩm cuối vòng đời.
- **OOS (Out of Stock):** Hết tồn kho.
- **Saleable:** Sản phẩm đủ điều kiện bán.
- **Unsaleable:** Sản phẩm không đủ điều kiện bán.
- **Hero SKU:** SKU trọng điểm, được ưu tiên kinh doanh.
- **Normal SKU:** SKU thông thường.

Các trạng thái trên thuộc những nhóm khác nhau (Lifecycle, Availability, Saleability, Priority), không loại trừ lẫn nhau.

## Stock & Availability

- **Stock:** Số lượng hàng tồn kho.
- **Available Stock:** Số lượng hàng thực tế có thể bán sau khi xét các khoản giữ chỗ/hạn chế.
- **Availability:** Khả năng đáp ứng nhu cầu mua hàng.

## Pricing & Competitive Strategy

Ba yếu tố cạnh tranh chính:

- **Price:** Mức giá và khả năng cạnh tranh về giá.
- **Differentiation:** Khác biệt về sản phẩm, dịch vụ và trải nghiệm.
- **Mix:** Cơ cấu và tỷ trọng các nhóm sản phẩm kinh doanh.

Giá bán ảnh hưởng trực tiếp đến Sales, nhưng hoạt động quản lý giá niêm yết và so sánh giá đối thủ thuộc Assortment.

---

**Business Overview:** Assortment quyết định *có gì để bán*, Traffic tạo ra *cơ hội bán hàng*, còn Sales đo lường *kết quả doanh thu và lợi nhuận*.