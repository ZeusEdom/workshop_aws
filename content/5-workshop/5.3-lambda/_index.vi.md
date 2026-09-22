---
title : "Phát triển AWS Lambda Functions"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

AWS Lambda cho phép bạn chạy mã nguồn mà không cần cấp phát hoặc quản lý máy chủ. Trong hệ thống này, ta triển khai 2 hàm Lambda: `weather-fetcher` và `weather-api-handler`.

---

### Phần A: Hàm Lambda `weather-fetcher` (Thu thập dữ liệu)

#### Bước 1: Khởi tạo Function
1. Truy cập **AWS Management Console** -> Tìm kiếm **Lambda**.
2. Chọn **Create function** -> **Author from scratch**.
3. Điền thông tin:
   - **Function name:** `weather-fetcher`
   - **Runtime:** `Python 3.12`
   - **Execution role:** Chọn *Use an existing role* -> Chọn `weather-fetcher-role`.
4. Nhấn **Create function**.

![Create Weather Fetcher Lambda](/workshop_aws/images/5-workshop/5.3-lambda/01-create-fetcher.png)

#### Bước 2: Cập nhật Mã nguồn Python
Tại ô Code Source (`lambda_function.py`), dán mã nguồn thu thập dữ liệu thời tiết và chuyển đổi kiểu `Decimal` để tương thích với DynamoDB:

```python
import json
import os
import time
import urllib.request
from decimal import Decimal
import boto3

dynamodb = boto3.resource('dynamodb')
TABLE_NAME = os.environ.get('TABLE_NAME', 'WeatherData')
table = dynamodb.Table(TABLE_NAME)
OWM_API_KEY = os.environ.get('OWM_API_KEY', '')
CITIES = os.environ.get('CITIES', 'Hanoi,Ho Chi Minh City,Da Nang').split(',')

def fetch_city_weather(city):
    url = f"https://api.openweathermap.org/data/2.5/weather?q={urllib.parse.quote(city)}&appid={OWM_API_KEY}&units=metric"
    req = urllib.request.Request(url, headers={'User-Agent': 'Mozilla/5.0'})
    with urllib.request.urlopen(req) as resp:
        data = json.loads(resp.read().decode('utf-8'), parse_float=Decimal)
        current_time = int(time.time())
        item = {
            'city': city,
            'timestamp': current_time,
            'ttl': current_time + (7 * 86400),
            'temp': data['main']['temp'],
            'humidity': data['main']['humidity'],
            'weather': data['weather'][0]['main'],
            'description': data['weather'][0]['description']
        }
        table.put_item(Item=item)
        return item

def handler(event, context):
    results = []
    for c in CITIES:
        try:
            res = fetch_city_weather(c.strip())
            results.append(res)
        except Exception as e:
            print(f"Error fetching {c}: {e}")
    return {'statusCode': 200, 'body': json.dumps({'message': 'Fetch success', 'count': len(results)})}
```

![Fetcher Source Code](/workshop_aws/images/5-workshop/5.3-lambda/02-fetcher-code.png)

#### Bước 3: Cấu hình Biến môi trường (Environment Variables)
Chuyển sang tab **Configuration** -> **Environment variables** -> Nhấn **Edit** và thêm:
- `OWM_API_KEY`: `e6bf3b2aa25e42d37925a5eb03e80390`
- `TABLE_NAME`: `WeatherData`
- `CITIES`: `Hanoi, Ho Chi Minh City, Da Nang, Hue, Nha Trang, Da Lat, Haiphong, Can Tho, Tokyo, London`

![Environment Variables](/workshop_aws/images/5-workshop/5.3-lambda/03-env-vars.png)

#### Bước 4: Điều chỉnh Runtime Settings & Timeout
1. Tại tab **Configuration** -> **General configuration**, chọn **Edit** -> Đổi Timeout lên `15 seconds`.
2. Tại mục **Runtime settings**, đổi Handler thành `lambda_function.handler`.

![Handler & Timeout Config](/workshop_aws/images/5-workshop/5.3-lambda/04-handler-config.png)

#### Bước 5: Chạy Test kiểm thử
Nhấn nút **Test** để thực thi hàm. Kết quả trả về `200 OK` thu thập đủ 10 thành phố.

![Test Fetcher Lambda](/workshop_aws/images/5-workshop/5.3-lambda/05-fetcher-test.png)

Kiểm tra trực tiếp tại **DynamoDB Console -> Explore Items**, dữ liệu 10 thành phố đã được ghi thành công!

![DynamoDB Items Verification](/workshop_aws/images/5-workshop/5.3-lambda/06-dynamodb-items.png)

---

### Phần B: Hàm Lambda `weather-api-handler` (Cung cấp API)

Tạo hàm Lambda thứ hai mang tên `weather-api-handler` để đọc dữ liệu từ DynamoDB và trả về định dạng JSON cho ứng dụng Web:

```python
import json
import os
from decimal import Decimal
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ.get('TABLE_NAME', 'WeatherData'))

class DecimalEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, Decimal):
            return float(o) if o % 1 else int(o)
        return super().default(o)

def handler(event, context):
    try:
        resp = table.scan()
        items = resp.get('Items', [])
        return {
            'statusCode': 200,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Content-Type': 'application/json'
            },
            'body': json.dumps(items, cls=DecimalEncoder)
        }
    except Exception as e:
        return {'statusCode': 500, 'body': json.dumps({'error': str(e)})}
```

Chạy **Test** kiểm thử hàm `weather-api-handler`, phản hồi `200 OK` chứa danh sách mảng JSON dữ liệu các thành phố.

![Test API Handler Lambda](/workshop_aws/images/5-workshop/5.3-lambda/07-api-handler-test.png)
