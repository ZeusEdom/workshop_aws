---
title : "Nhật ký Công việc 8 Tuần"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

#### Thời gian: 03/08/2026 – 27/09/2026

Bảng dưới đây tổng hợp toàn bộ lộ trình công việc, kết quả đạt được và các cột mốc chính trong 8 tuần thực tập tại AWS Việt Nam:

| Tuần | Thời gian | Nhiệm vụ chính & Nội dung công việc | Kết quả đạt được |
| :---: | :---: | :--- | :--- |
| **1** | 03/08 - 09/08 | • Tham gia chương trình Onboarding FCAJ.<br>• Tìm hiểu tổng quan về điện toán đám mây AWS và các dịch vụ Serverless cơ bản (IAM, S3, DynamoDB, Lambda). | Hoàn thành định hướng, nắm rõ quy tắc bảo mật và mô hình Cloud Serverless Architecture. |
| **2** | 10/08 - 16/08 | • Nghiên cứu cơ sở dữ liệu NoSQL với Amazon DynamoDB.<br>• Thiết kế Data Schema (Partition Key `city`, Sort Key `timestamp`).<br>• Phân quyền an toàn qua AWS IAM Roles. | Thiết kế xong DynamoDB Schema & cấu hình IAM Role tuân thủ quy tắc Least Privilege. |
| **3** | 17/08 - 23/08 | • Nghiên cứu RESTful API của OpenWeatherMap.<br>• Lập trình Python 3.12 với AWS SDK `boto3`.<br>• Viết hàm AWS Lambda `weather-fetcher` thu thập dữ liệu thời tiết 10 thành phố lớn. | Chạy thử nghiệm thành công `weather-fetcher` Lambda, ghi dữ liệu chuẩn `Decimal` vào DynamoDB. |
| **4** | 24/08 - 30/08 | • Cấu hình tính năng Time-To-Live (TTL) trên DynamoDB để tự động dọn dẹp dữ liệu cũ quá 7 ngày.<br>• Thiết lập Amazon EventBridge Schedule Rule tự động chạy định kỳ `rate(30 minutes)`. | Tự động hóa hoàn toàn khâu Data Ingestion pipeline không cần can thiệp thủ công. |
| **5** | 31/08 - 06/09 | • Phát triển hàm Lambda `weather-api-handler` để truy vấn DynamoDB.<br>• Giải quyết bài toán ép kiểu `Decimal` sang JSON thông qua custom class `DecimalEncoder`. | Tạo thành công Lambda API Backend trả về dữ liệu chuẩn JSON 200 OK. |
| **6** | 07/09 - 13/09 | • Tạo Amazon API Gateway (HTTP API) `weather-dashboard-api`.<br>• Cấu hình Route `GET /weather` tích hợp với Lambda API Handler.<br>• Cấu hình Cross-Origin Resource Sharing (CORS `*`). | Công bố công khai API Invoke URL an toàn, sẵn sàng kết nối Frontend web. |
| **7** | 14/09 - 20/09 | • Thiết kế và lập trình giao diện Web Dashboard (`index.html`) bằng HTML5/CSS3/Vanilla JS.<br>• Tích hợp biểu đồ SVG dự báo thời tiết 5 ngày.<br>• Bật S3 Static Website Hosting & gắn Bucket Policy Public Read. | Web Dashboard chính thức chạy live trực tiếp từ S3 Bucket URL. |
| **8** | 21/09 - 27/09 | • Đánh giá & kiểm thử toàn bộ hệ thống (End-to-End Test).<br>• Đóng gói mã nguồn & đẩy lên GitHub repository `aws_final`.<br>• Xây dựng website tài liệu báo cáo FCJ Workshop song ngữ Việt/Anh. | Hoàn thành 100% dự án, bàn giao mã nguồn open-source & nộp báo cáo kết quả thực tập. |
