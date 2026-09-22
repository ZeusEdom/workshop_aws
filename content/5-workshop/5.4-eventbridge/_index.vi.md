---
title : "Lịch tự động EventBridge"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---

Amazon EventBridge là dịch vụ Serverless Event Bus cho phép kết nối các ứng dụng thông qua dữ liệu sự kiện hoặc lập lịch thực thi công việc (Schedule Cron).

---

### Bước 1: Khởi tạo EventBridge Schedule Rule

1. Truy cập **AWS Management Console** -> Tìm kiếm **Amazon EventBridge**.
2. Tại menu bên trái, chọn **Rules** (hoặc **Schedules**) -> Nhấn **Create rule**.
3. Nhập tên Rule: `weather-fetch-schedule` -> Chọn Rule type: **Schedule**.

![Create EventBridge Rule](/workshop_aws/images/5-workshop/5.4-eventbridge/01-create-rule.png)

---

### Bước 2: Thiết lập Tần suất chạy (Schedule Pattern)

1. Chọn mục **A schedule that runs at a regular rate**.
2. Thiết lập thông số:
   - **Value:** `30`
   - **Unit:** `Minutes` (hoặc biểu thức `rate(30 minutes)`).
3. Nhấn **Next**.

![Schedule Pattern 30 Minutes](/workshop_aws/images/5-workshop/5.4-eventbridge/02-schedule-pattern.png)

---

### Bước 3: Gắn Target gọi AWS Lambda Function

1. Ở bước **Select target**, chọn Target types: **AWS service**.
2. Danh sách service chọn **Lambda function**.
3. Mục Function chọn hàm: `weather-fetcher`.

![Attach Lambda Target](/workshop_aws/images/5-workshop/5.4-eventbridge/03-target-lambda.png)

4. Nhấn **Create rule** để hoàn tất.

Từ thời điểm này, hệ thống sẽ tự động gọi hàm Lambda `weather-fetcher` định kỳ **30 phút/lần** để cập nhật dữ liệu thời tiết mới nhất vào DynamoDB mà không cần sự can thiệp của con người.
