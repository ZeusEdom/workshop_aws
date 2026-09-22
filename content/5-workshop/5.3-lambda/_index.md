---
title : "Develop AWS Lambda Functions"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

AWS Lambda runs your code without requiring server provisioning or management. In this architecture, we deploy two functions: `weather-fetcher` and `weather-api-handler`.

---

### Part A: `weather-fetcher` Lambda Function (Data Ingestion)

#### Step 1: Create Function
1. Open **AWS Management Console** -> Search for **Lambda**.
2. Click **Create function** -> Select **Author from scratch**.
3. Configure settings:
   - **Function name:** `weather-fetcher`
   - **Runtime:** `Python 3.12`
   - **Execution role:** Select *Use an existing role* -> Choose `weather-fetcher-role`.
4. Click **Create function**.

![Create Weather Fetcher Lambda](/images/5-workshop/5.3-lambda/01-create-fetcher.png?featherlight=false&width=90pc)

#### Step 2: Implement Python Code
In the Code Source panel (`lambda_function.py`), paste the Python script to fetch OpenWeatherMap API data and parse float numbers into DynamoDB-compatible `Decimal` format:

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

![Fetcher Source Code](/images/5-workshop/5.3-lambda/02-fetcher-code.png?featherlight=false&width=90pc)

#### Step 3: Configure Environment Variables
Switch to **Configuration** tab -> **Environment variables** -> Click **Edit** and define:
- `OWM_API_KEY`: `e6bf3b2aa25e42d37925a5eb03e80390`
- `TABLE_NAME`: `WeatherData`
- `CITIES`: `Hanoi, Ho Chi Minh City, Da Nang, Hue, Nha Trang, Da Lat, Haiphong, Can Tho, Tokyo, London`

![Environment Variables](/images/5-workshop/5.3-lambda/03-env-vars.png?featherlight=false&width=90pc)

#### Step 4: Adjust Runtime Settings & Timeout
1. Under **Configuration** tab -> **General configuration**, click **Edit** -> Increase Timeout to `15 seconds`.
2. Under **Runtime settings**, change Handler entry point to `lambda_function.handler`.

![Handler & Timeout Config](/images/5-workshop/5.3-lambda/04-handler-config.png?featherlight=false&width=90pc)

#### Step 5: Execute Test Event
Click **Test** to execute the function. The response returns `200 OK` fetching telemetry for all 10 cities.

![Test Fetcher Lambda](/images/5-workshop/5.3-lambda/05-fetcher-test.png?featherlight=false&width=90pc)

Verify records in **DynamoDB Console -> Explore Items** to confirm item insertion.

![DynamoDB Items Verification](/images/5-workshop/5.3-lambda/06-dynamodb-items.png?featherlight=false&width=90pc)

---

### Part B: `weather-api-handler` Lambda Function (REST API Endpoint)

Create the second Lambda function named `weather-api-handler` to query DynamoDB items and return formatted JSON responses:

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

Run a **Test** invocation for `weather-api-handler`, confirming `200 OK` response with serialized JSON array payload.

![Test API Handler Lambda](/images/5-workshop/5.3-lambda/07-api-handler-test.png?featherlight=false&width=90pc)
