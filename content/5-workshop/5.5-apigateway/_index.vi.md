---
title : "Thiết lập API Gateway"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

Amazon API Gateway giúp các nhà phát triển dễ dàng tạo, xuất bản, duy trì, giám sát và bảo mật các giao diện lập trình ứng dụng (API) ở mọi quy mô.

---

### Bước 1: Khởi tạo HTTP API

1. Truy cập **AWS Management Console** -> Tìm kiếm **API Gateway**.
2. Chọn loại API **HTTP API** -> Nhấn **Build**.
3. Điền tên API: `weather-dashboard-api`.

![Create HTTP API](/workshop_aws/images/5-workshop/5.5-apigateway/01-create-api.png)

---

### Bước 2: Tạo Route `GET /weather`

1. Tại menu bên trái, chọn **Routes** -> Nhấn **Create**.
2. Chọn Method: `GET` | Route path: `/weather`.

![Create GET Weather Route](/workshop_aws/images/5-workshop/5.5-apigateway/02-create-route.png)

---

### Bước 3: Tích hợp Route với Lambda `weather-api-handler`

1. Chọn Route `GET /weather` -> Nhấn **Attach integration**.
2. Chọn Integration type: **Lambda function**.
3. Chọn Lambda function: `weather-api-handler` -> Nhấn **Attach**.

![Attach Lambda Integration](/workshop_aws/images/5-workshop/5.5-apigateway/03-attach-integration.png)

---

### Bước 4: Kiểm tra Stage `$default` Auto-Deploy

API Gateway tự động cấu hình Stage `$default` với chế độ Auto-deploy được bật, giúp mọi thay đổi về Route/Integration lập tức có hiệu lực.

![Deploy Stage Config](/workshop_aws/images/5-workshop/5.5-apigateway/04-deploy-stage.png)

---

### Bước 5: Cấu hình CORS (Cross-Origin Resource Sharing)

Để trình duyệt Web từ domain S3 có thể gọi API mà không bị chặn bởi chính sách Security CORS:

1. Chọn menu **CORS** bên trái.
2. Nhấp **Configure** và điền:
   - **Access-Control-Allow-Origin:** `*`
   - **Access-Control-Allow-Headers:** `*`
   - **Access-Control-Allow-Methods:** `GET, OPTIONS`
3. Nhấn **Save**.

![CORS Setup](/workshop_aws/images/5-workshop/5.5-apigateway/05-cors-setup.png)

---

### Bước 6: Lấy API Invoke URL & Test Trình duyệt

1. Tại mục **Stages**, copy đường dẫn **Invoke URL** của API:
   `https://0ehzg82gn4.execute-api.ap-southeast-1.amazonaws.com`

![API Invoke URL](/workshop_aws/images/5-workshop/5.5-apigateway/06-invoke-url.png)

2. Mở trình duyệt web và truy cập endpoint:
   `https://0ehzg82gn4.execute-api.ap-southeast-1.amazonaws.com/weather`

Kết quả trả về danh sách dữ liệu thời tiết mảng JSON chuẩn `200 OK`.

![Browser JSON Test](/workshop_aws/images/5-workshop/5.5-apigateway/07-browser-test.png)
