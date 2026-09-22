---
title : "Verification & Live Demo"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 5.7 </b> "
---

After completing all deployment phases across DynamoDB, IAM, Lambda, EventBridge, API Gateway, and S3 Hosting, we conduct end-to-end pipeline validation.

---

### 1. Accessing the Live Web Dashboard

Open any standard web browser and navigate to the public S3 Website Endpoint:
 **[http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com](http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com)**

The Web Dashboard loads seamlessly, presenting dynamic live components:
1. **Hero Weather Card:** Displays real-time weather metrics for Hanoi retrieved directly from **Amazon DynamoDB**.
2. **Stat Summary Cards:** Key telemetry parameters ingested via serverless Lambda functions.
3. **SVG Interactive Forecast Chart:** Smooth trendlines illustrating 5-day weather forecasts with dynamic city switching.
4. **Live Telemetry Log (DynamoDB Table):** Real-time log table displaying continuous telemetry records fetched across 10 major cities.
5. **Detailed Schedule Table:** Tabular view of upcoming 5-day forecast data.

![Live Weather Dashboard Demonstration](/workshop_aws/images/5-workshop/5.7-verification/01-live-dashboard.png)

---

### 2. Summary of Deployed Project Endpoints

| Resource | Official Public URL / Reference |
| :--- | :--- |
| **S3 Live Web Dashboard** | `http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com` |
| **API Gateway Endpoint** | `https://0ehzg82gn4.execute-api.ap-southeast-1.amazonaws.com/weather` |
| **GitHub Repository** | `https://github.com/ZeusEdom/aws_final` |
| **AWS Region** | `ap-southeast-1` (Singapore) |
