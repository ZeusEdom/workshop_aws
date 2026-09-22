---
title : "Hosting Giao diện S3"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

Amazon Simple Storage Service (Amazon S3) cung cấp giải pháp lưu trữ đối tượng có khả năng mở rộng cao, cho phép triển khai các trang web tĩnh (Static Website Hosting) với chi phí tối ưu.

---

### Bước 1: Khởi tạo S3 Bucket `weather-dashboard-toih`

1. Truy cập **AWS Management Console** -> Tìm kiếm **S3**.
2. Chọn **Create bucket**.
3. Điền tên Bucket: `weather-dashboard-toih` | Region: `ap-southeast-1`.
4. Mục **Block Public Access settings for this bucket**, bỏ chọn *Block all public access* (nhấn xác nhận đồng ý mở quyền truy cập công khai cho trang web).

![Create S3 Bucket](/images/5-workshop/5.6-s3/01-create-bucket.png?featherlight=false&width=90pc)

5. Nhấn **Create bucket**.

---

### Bước 2: Kích hoạt Static Website Hosting

1. Chọn Bucket `weather-dashboard-toih` vừa tạo -> Chọn tab **Properties**.
2. Cuộn xuống mục **Static website hosting** -> Nhấn **Edit**.
3. Chọn **Enable** -> Điền **Index document**: `index.html`.
4. Nhấn **Save changes**.

![Enable Static Website Hosting](/images/5-workshop/5.6-s3/02-static-hosting.png?featherlight=false&width=90pc)

---

### Bước 3: Gắn S3 Bucket Policy cho phép Public Read

Chuyển sang tab **Permissions** -> Mục **Bucket policy** -> Nhấn **Edit** và dán đoạn JSON Policy cho phép người dùng đọc tập tin (`s3:GetObject`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::weather-dashboard-toih/*"
    }
  ]
}
```

![Bucket Policy Configuration](/images/5-workshop/5.6-s3/03-bucket-policy.png?featherlight=false&width=90pc)

---

### Bước 4: Tải giao diện Web (`index.html`) lên Bucket

1. Chuyển sang tab **Objects** -> Nhấn **Upload**.
2. Chọn tập tin `index.html` (đã cập nhật đúng `API_URL` của API Gateway vừa tạo).
3. Nhấn **Upload** để hoàn tất tải file lên S3.

![Upload Index HTML](/images/5-workshop/5.6-s3/04-upload-index.png?featherlight=false&width=90pc)

---

### Bước 5: Lấy S3 Website Endpoint URL

Quay lại tab **Properties** -> Cuộn xuống cuối cùng tại mục **Static website hosting**, copy đường dẫn **Bucket website endpoint**:
`http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com`

![S3 Website Endpoint URL](/images/5-workshop/5.6-s3/05-website-url.png?featherlight=false&width=90pc)
