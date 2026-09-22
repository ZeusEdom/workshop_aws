---
title : "Tạo DynamoDB Table & TTL"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL quản lý hoàn toàn (Fully Managed), mang lại hiệu năng cao với độ trễ tính bằng mili-giây ở mọi quy mô.

---

### Bước 1: Khởi tạo DynamoDB Table `WeatherData`

1. Truy cập **AWS Management Console** -> Tìm kiếm **DynamoDB**.
2. Tại bảng điều khiển DynamoDB, chọn **Create table**.
3. Điền thông tin cấu hình Table:
   - **Table name:** `WeatherData`
   - **Partition key:** `city` (String)
   - **Sort key:** `timestamp` (Number)
4. Mục **Table capacity settings**, chọn **On-demand** (chế độ tự động mở rộng theo lưu lượng, chỉ trả phí cho các truy vấn thực tế).

![Create DynamoDB Table](/images/5-workshop/5.1-dynamodb/01-create-table.png?featherlight=false&width=90pc)

5. Nhấn **Create table** và đợi khoảng vài giây cho đến khi trạng thái Table chuyển sang **Active**.

![DynamoDB Table Active](/images/5-workshop/5.1-dynamodb/02-table-active.png?featherlight=false&width=90pc)

---

### Bước 2: Cấu hình Time-To-Live (TTL) tự động xóa dữ liệu cũ

Tính năng **Time-To-Live (TTL)** giúp tự động định thời điểm hết hạn cho các bản ghi trong Table để DynamoDB tự động xóa chúng mà không tốn chi phí write hay gọi API thủ công.

1. Chọn Table `WeatherData` vừa tạo -> Chuyển sang tab **Additional settings**.
2. Tìm đến mục **Time-to-Live (TTL)** -> Nhấn **Enable**.
3. Tại ô **TTL attribute name**, nhập `ttl`.
4. Nhấn **Save changes** để hoàn tất kích hoạt.

![Enable TTL Attribute](/images/5-workshop/5.1-dynamodb/03-enable-ttl.png?featherlight=false&width=90pc)

{{% notice note %}}
**Giải thích cơ chế TTL:** Trong hàm Lambda Fetcher, thuộc tính `ttl` sẽ được tính bằng:
`ttl = current_epoch_timestamp + (7 * 86400)` (7 ngày sau). Sau thời điểm này, DynamoDB sẽ tự động quét và xóa bản ghi trong nền hoàn toàn miễn phí.
{{% /notice %}}
