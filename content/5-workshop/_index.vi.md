---
title : "Bài Hướng Dẫn Thực Hành Workshop"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 1.5 </b> "
---

#### Xây dựng Hệ thống Serverless Weather Dashboard từ cơ bản đến hoàn thiện

Phần này hướng dẫn chi tiết từng bước (Step-by-Step Console Guide) triển khai hệ thống **Serverless Weather Dashboard** với **29 hình ảnh minh họa thực tế** được chụp trực tiếp từ AWS Management Console trong kỳ thực tập.

---

###  Danh sách 7 Bài Lab thực hành (Click vào từng bài để xem chi tiết)

1. [**5.1 Khởi tạo Amazon DynamoDB & Cấu hình TTL**](5.1-dynamodb/)
   - Tạo Table `WeatherData` với Partition Key `city` & Sort Key `timestamp`.
   - Bật tính năng tự động dọn dẹp dữ liệu cũ Time-To-Live (TTL).
2. [**5.2 Cấu hình Phân quyền an toàn AWS IAM**](5.2-iam/)
   - Tạo IAM Execution Role cho Lambda Functions.
   - Gắn Inline Policies truy vấn và ghi dữ liệu DynamoDB tuân thủ Least Privilege.
3. [**5.3 Phát triển AWS Lambda Functions**](5.3-lambda/)
   - Lập trình Python 3.12 cho hàm `weather-fetcher` thu thập dữ liệu OpenWeatherMap.
   - Viết hàm `weather-api-handler` truy vấn DynamoDB & xử lý ép kiểu `DecimalEncoder`.
4. [**5.4 Cấu hình Lịch chạy tự động với EventBridge**](5.4-eventbridge/)
   - Tạo EventBridge Schedule Rule tự động chạy định kỳ `rate(30 minutes)`.
   - Gắn Target gọi hàm `weather-fetcher` Lambda.
5. [**5.5 Thiết lập RESTful API với Amazon API Gateway**](5.5-apigateway/)
   - Tạo HTTP API `weather-dashboard-api` & Route `GET /weather`.
   - Cấu hình Cross-Origin Resource Sharing (CORS `*`) & Stage `$default`.
6. [**5.6 Hosting Giao diện Web trên Amazon S3**](5.6-s3/)
   - Khởi tạo S3 Bucket `weather-dashboard-toih` & bật Static Website Hosting.
   - Cấu hình Bucket Policy Public Read (`s3:GetObject`) & Tải file `index.html`.
7. [**5.7 Kiểm thử Hệ thống & Giao diện Live**](5.7-verification/)
   - Truy cập S3 Website Endpoint kiểm tra hiển thị Dashboard dữ liệu thời tiết thực tế.
