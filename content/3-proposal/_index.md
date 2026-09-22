---
title : "Project Proposal & Architecture"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 1.3 </b> "
---

#### Project Name: Serverless Weather Dashboard & Real-time Telemetry Pipeline

---

### 1. Problem Statement & Project Objectives
Traditional weather telemetry monitoring applications typically encounter two major operational challenges:
1. **High Infrastructure Maintenance Costs:** Operating dedicated 24/7 Virtual Machines (EC2) or database servers results in resource underutilization during low-traffic periods.
2. **Historical Data Overhead:** Accumulating continuous sensor data creates large volumes of stale data, degrading query responsiveness.

**Proposed Solution:**
Architecting a fully automated **Serverless Weather Dashboard** on Amazon Web Services:
- **Automated Ingestion:** Periodically fetching data from OpenWeatherMap API every 30 minutes without server maintenance.
- **Automated Data Lifecycle:** Purging stale weather records older than 7 days using **DynamoDB Time-To-Live (TTL)**.
- **Cost-Efficiency & High Scalability:** Utilizing pay-as-you-go serverless execution and hosting static web UI assets on Amazon S3.

---

### 2. System Architecture Diagram

The pipeline integrates 6 core AWS services operating in an automated data flow:

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

### 3. Deployed AWS Services Breakdown

1. **Amazon DynamoDB (Table: `WeatherData`):**
   - Partition Key: `city` (String) | Sort Key: `timestamp` (Number)
   - Capacity Mode: On-demand
   - Attribute TTL: `ttl` (automatically purges records after 7 days)
2. **AWS Lambda (`weather-fetcher`):**
   - Runtime: Python 3.12 | Timeout: 15 seconds
   - Task: Invokes OpenWeatherMap API for 10 major cities (Hanoi, Ho Chi Minh City, Da Nang, Hue, Nha Trang, Da Lat, Haiphong, Can Tho, Tokyo, London) and persists JSON payloads into DynamoDB.
3. **Amazon EventBridge (`weather-fetch-schedule`):**
   - Rule Pattern: `rate(30 minutes)` schedule
   - Target: `weather-fetcher` Lambda function
4. **AWS Lambda (`weather-api-handler`):**
   - Runtime: Python 3.12 | Timeout: 10 seconds
   - Task: Queries DynamoDB for latest telemetry data per city, formats numbers via `DecimalEncoder`, and returns clean JSON HTTP responses.
5. **Amazon API Gateway (`weather-dashboard-api`):**
   - Protocol: HTTP API
   - Route: `GET /weather` -> Lambda integration
   - CORS Configuration: `Access-Control-Allow-Origin: *`
6. **Amazon S3 (`weather-dashboard-toih`):**
   - Static Website Hosting: Enabled (`index.html`)
   - Bucket Policy: Public Read (`s3:GetObject`)
