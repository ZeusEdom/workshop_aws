---
title : "Kiểm thử & Kết quả Live"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 5.7 </b> "
---

Sau khi hoàn tất toàn bộ các bước triển khai từ DynamoDB, IAM, Lambda, EventBridge, API Gateway đến S3 Hosting, chúng ta tiến hành kiểm thử toàn bộ luồng hoạt động End-to-End của ứng dụng.

---

### 1. Truy cập Giao diện Web Dashboard Trực tuyến

Mở trình duyệt web bất kỳ và truy cập đường dẫn S3 Website Endpoint:
 **[http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com](http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com)**

Giao diện ứng dụng tải thành công với đầy đủ các thành phần dữ liệu thực tế:
1. **Hero Weather Card:** Hiển thị thời tiết hiện tại của Hà Nội (Nhiệt độ, Độ ẩm, Tình trạng thời tiết) được đọc trực tiếp từ **DynamoDB**.
2. **Stat Cards:** Các chỉ số tổng quan đo đạc từ hệ thống Serverless.
3. **SVG Interactive Forecast Chart:** Biểu đồ đường mịn dự báo diễn biến thời tiết 5 ngày tới với bộ chọn thành phố động.
4. **Bảng Nhật ký Telemetry Live (DynamoDB):** Bảng hiển thị danh sách các bản ghi thời tiết mới nhất của 10 thành phố lớn thu thập tự động từ AWS Lambda.
5. **Bảng Dự báo Lịch trình:** Bảng dữ liệu chi tiết các mốc thời gian trong 5 ngày tới.

![Live Weather Dashboard Demonstration](/workshop_aws/images/5-workshop/5.7-verification/01-live-dashboard.png)

---

### 2. Tổng kết đường dẫn Tài nguyên Dự án

| Tài nguyên | Đường dẫn Public / URL chính thức |
| :--- | :--- |
| **S3 Live Web Dashboard** | `http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com` |
| **API Gateway Endpoint** | `https://0ehzg82gn4.execute-api.ap-southeast-1.amazonaws.com/weather` |
| **Mã nguồn GitHub** | `https://github.com/ZeusEdom/aws_final` |
| **AWS Region** | `ap-southeast-1` (Singapore) |
