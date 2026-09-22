---
title : "Cấu hình IAM Roles & Policies"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

AWS Identity and Access Management (IAM) giúp kiểm soát quyền truy cập vào các tài nguyên AWS một cách an toàn tuân thủ nguyên tắc Quyền tối thiểu (Least Privilege).

---

### Bước 1: Khởi tạo IAM Role cho Lambda `weather-fetcher-role`

1. Truy cập **AWS Management Console** -> Tìm kiếm **IAM**.
2. Tại menu bên trái, chọn **Roles** -> Nhấn **Create role**.
3. Chọn loại Trusted Entity: **AWS service** -> Chọn Use case **Lambda** -> Nhấn **Next**.

![Create IAM Role](/images/5-workshop/5.2-iam/01-create-role.png?featherlight=false&width=90pc)

4. Ở bước **Add permissions**, tìm kiếm và chọn policy mặc định `AWSLambdaBasicExecutionRole` (cho phép Lambda ghi log ra Amazon CloudWatch Logs).
5. Đặt tên Role: `weather-fetcher-role` -> Nhấn **Create role**.

---

### Bước 2: Gắn Inline Policies truy vấn DynamoDB

Để hàm Lambda có thể đọc/ghi dữ liệu vào Table `WeatherData`, ta thêm các Inline Policies được phân quyền chính xác theo ARN của Table:

1. Mở Role `weather-fetcher-role` vừa tạo -> Chọn **Add permissions** -> **Create inline policy**.

![Attach Policy to Role](/images/5-workshop/5.2-iam/02-attach-policy.png?featherlight=false&width=90pc)

2. Chuyển sang chế độ **JSON** và nhập Policy ghi dữ liệu (`dynamodb:PutItem`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem"
      ],
      "Resource": "arn:aws:dynamodb:ap-southeast-1:215038507979:table/WeatherData"
    }
  ]
}
```

3. Tiếp tục tạo Inline Policy đọc dữ liệu (`dynamodb:Query`, `dynamodb:GetItem`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:Query",
        "dynamodb:GetItem"
      ],
      "Resource": "arn:aws:dynamodb:ap-southeast-1:215038507979:table/WeatherData"
    }
  ]
}
```

4. Kiểm tra tổng quan IAM Role sau khi gắn đủ các permissions.

![IAM Role Summary](/images/5-workshop/5.2-iam/03-role-created.png?featherlight=false&width=90pc)
