---
title : "Hands-on Workshop Guide"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 1.5 </b> "
---

#### Building a Serverless Weather Dashboard on AWS

This section provides a step-by-step console guide for deploying the entire **Serverless Weather Dashboard** architecture, featuring **29 real console screenshots** captured during the internship.

---

###  7 Step-by-Step Lab Modules

1. [**5.1 Provisioning Amazon DynamoDB & Configuring TTL**](5.1-dynamodb/)
   - Create Table `WeatherData` with Partition Key `city` & Sort Key `timestamp`.
   - Enable Time-To-Live (TTL) for automatic historical data expiration.
2. [**5.2 Configuring AWS IAM Security Roles & Policies**](5.2-iam/)
   - Create IAM Execution Role for Lambda functions.
   - Attach custom inline DynamoDB read/write policies adhering to least privilege.
3. [**5.3 Developing AWS Lambda Serverless Functions**](5.3-lambda/)
   - Develop Python 3.12 `weather-fetcher` Lambda to retrieve OpenWeatherMap telemetry.
   - Build `weather-api-handler` Lambda to query DynamoDB with `DecimalEncoder` JSON serialization.
4. [**5.4 Setting up Automated EventBridge Schedules**](5.4-eventbridge/)
   - Create an EventBridge Schedule Rule executing every `rate(30 minutes)`.
   - Attach `weather-fetcher` Lambda as the target.
5. [**5.5 Exposing RESTful Interfaces via Amazon API Gateway**](5.5-apigateway/)
   - Provision HTTP API `weather-dashboard-api` and route `GET /weather`.
   - Configure Cross-Origin Resource Sharing (CORS `*`) and auto-deploy stage `$default`.
6. [**5.6 Hosting Web Frontend on Amazon S3**](5.6-s3/)
   - Create S3 bucket `weather-dashboard-toih` & enable Static Website Hosting.
   - Configure Public Read Bucket Policy (`s3:GetObject`) and upload `index.html`.
7. [**5.7 End-to-End System Verification & Live Demo**](5.7-verification/)
   - Access the live S3 Website Endpoint to verify dynamic weather visualization.
