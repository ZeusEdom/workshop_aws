---
title : "Đề xuất Dự án & Kiến trúc"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 1.3 </b> "
---

#### Tên dự án: Serverless Weather Dashboard & Real-time Telemetry Pipeline

---

### 1. Đặt vấn đề & Mục tiêu dự án
Các ứng dụng giám sát thời tiết truyền thống thường đối mặt với hai thách thức lớn:
1. **Chi phí duy trì máy chủ cao:** Duy trì máy chủ Web/DB chạy liên tục (EC2) gây lãng phí tài nguyên khi lưu lượng truy cập không ổn định.
2. **Quản lý dữ liệu lịch sử:** Lưu trữ dữ liệu cảm biến/thời tiết theo thời gian phát sinh lượng lớn bản ghi cũ không cần thiết, làm giảm hiệu năng truy vấn.

**Giải pháp của dự án:**
Xây dựng hệ thống **Serverless Weather Dashboard** hoàn toàn tự động trên nền tảng AWS:
- **Tự động thu thập:** Đọc dữ liệu OpenWeatherMap API mỗi 30 phút mà không cần quản lý máy chủ.
- **Tự động dọn dẹp:** Tự động xóa dữ liệu thời tiết cũ quá 7 ngày thông qua **DynamoDB Time-To-Live (TTL)**.
- **Tối ưu chi phí & Hiệu năng:** Sử dụng mô hình Pay-as-you-go (chỉ trả tiền khi hàm chạy) và hosting giao diện tĩnh trên S3.

---

### 2. Sơ đồ Kiến trúc Hệ thống (Architecture Diagram)

Hệ thống bao gồm 6 dịch vụ AWS cốt lõi phối hợp chặt chẽ theo luồng dữ liệu tự động:

```
+------------------+         +-----------------------+         +------------------+
| OpenWeatherMap   | <------ | AWS Lambda            | ------> | Amazon DynamoDB  |
| External API     |         | (weather-fetcher)     | (Put)   | (WeatherData)    |
+------------------+         +-----------------------+         +------------------+
                                         ^                              |
                                         | (Trigger 30m)                | (Query)
                                 +---------------+                      v
                                 | EventBridge   |             +------------------+
                                 | Cron Schedule |             | AWS Lambda       |
                                 +---------------+             | (api-handler)    |
                                                               +------------------+
                                                                        ^
                                                                        | (Integration)
                                                               +------------------+
                                                               | API Gateway      |
                                                               | (GET /weather)   |
                                                               +------------------+
                                                                        ^
                                                                        | (REST API Fetch)
                                                               +------------------+
                                                               | Amazon S3        |
                                                               | Static Dashboard |
                                                               +------------------+
```

---

### 3. Danh sách các Dịch vụ AWS triển khai

1. **Amazon DynamoDB (Table: `WeatherData`):**
   - Partition Key: `city` (String) | Sort Key: `timestamp` (Number)
   - Capacity mode: On-demand (pay per request)
   - Attribute TTL: `ttl` (tự động xóa dữ liệu sau 7 ngày)
2. **AWS Lambda (`weather-fetcher`):**
   - Runtime: Python 3.12 | Timeout: 15 seconds
   - Nhiệm vụ: Gọi API OpenWeatherMap thu thập dữ liệu 10 thành phố (Hà Nội, TP.HCM, Đà Nẵng, Huế, Nha Trang, Đà Lạt, Hải Phòng, Cần Thơ, Tokyo, London) và ghi vào DynamoDB.
3. **Amazon EventBridge (`weather-fetch-schedule`):**
   - Rule type: Schedule pattern `rate(30 minutes)`
   - Target: `weather-fetcher` Lambda
4. **AWS Lambda (`weather-api-handler`):**
   - Runtime: Python 3.12 | Timeout: 10 seconds
   - Nhiệm vụ: Truy vấn DynamoDB lấy bản ghi mới nhất của các thành phố, xử lý ép kiểu `DecimalEncoder` và trả về JSON 200 OK.
5. **Amazon API Gateway (`weather-dashboard-api`):**
   - Protocol: HTTP API
   - Route: `GET /weather` -> Lambda integration
   - CORS: `Access-Control-Allow-Origin: *`
6. **Amazon S3 (`weather-dashboard-toih`):**
   - Static Website Hosting: Enabled (`index.html`)
   - Bucket Policy: Public Read (`s3:GetObject`)
