---
title : "Báo Cáo Thực Tập FCAJ & AWS Serverless Workshop"
date : "`r Sys.Date()`" 
weight : 1 
chapter : false
---

#### Chào mừng đến với Báo cáo Thực tập FCAJ!
Trang web này tổng hợp toàn bộ báo cáo kết quả thực tập **First Cloud Journey (FCAJ)** và tài liệu hướng dẫn thực hành xây dựng hệ thống **Serverless Weather Dashboard & Real-time Telemetry Pipeline** trên nền tảng **Amazon Web Services (AWS)**.

---

###  Thông tin tổng quan Báo cáo (7 Mục FCAJ)

| Mục | Tên nội dung | Mô tả chi tiết |
| :---: | :--- | :--- |
| **1.1** | [Thông tin Sinh viên & Đơn vị thực tập](1-student-info/) | Họ tên, Trường, Công ty thực tập AWS Việt Nam & Vị trí Kỹ sư Đám mây. |
| **1.2** | [Nhật ký Công việc 8 tuần](2-worklog/) | Chi tiết công việc, lộ trình học tập & đóng góp từ 03/08/2026 đến 27/09/2026. |
| **1.3** | [Đề xuất Dự án & Kiến trúc](3-proposal/) | Tổng quan bài toán Weather Dashboard & sơ đồ kiến trúc Serverless 6 dịch vụ. |
| **1.4** | [Sự kiện & Hoạt động](4-events/) | Các sự kiện Tech Talk, Workshop FCJ Community & làm việc nhóm. |
| **1.5** | [Hướng dẫn Thực hành Workshop](5-workshop/) | **Trọng tâm (29 Screenshots)**: Hướng dẫn Step-by-Step triển khai hệ thống AWS. |
| **1.6** | [Tự Đánh Giá & Bài Học](6-self-evaluation/) | Đánh giá kỹ năng cứng/mềm đạt được & định hướng phát triển Cloud. |
| **1.7** | [Góp ý & Đánh giá](7-feedback/) | Đánh giá chương trình FCAJ & lời cảm ơn gửi tới Ban tổ chức và Mentor. |

---

### ️ Các dịch vụ AWS được sử dụng trong dự án

- **Amazon DynamoDB**: Cơ sở dữ liệu NoSQL lưu trữ dữ liệu thời tiết với cơ chế tự động xóa dữ liệu cũ **TTL**.
- **AWS Lambda**: Hàm xử lý logic Serverless (Python 3.12) thu thập thời tiết và phản hồi API.
- **Amazon EventBridge**: Bộ lập lịch tự động kích hoạt Lambda thu thập dữ liệu định kỳ **30 phút/lần**.
- **Amazon API Gateway**: HTTP API cung cấp endpoint RESTful secure cho ứng dụng Web.
- **Amazon S3**: Lưu trữ và hosting giao diện Web tĩnh (**Static Website Hosting**).
- **AWS IAM**: Quản lý truy cập an toàn, phân quyền tối thiểu (Least Privilege).

---

{{% notice note %}}
**Mã nguồn công khai:** Toàn bộ mã nguồn backend Python, cấu hình IAM, template Infrastructure as Code (IaC) và frontend HTML/JS đã được đẩy công khai tại Repository: [GitHub - ZeusEdom/aws_final](https://github.com/ZeusEdom/aws_final).
{{% /notice %}}