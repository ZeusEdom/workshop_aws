---
title : "8-Week Internship Worklog"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

#### Internship Period: August 3, 2026 – September 27, 2026

The table below summarizes the weekly roadmap, tasks accomplished, and key milestones during the 8-week internship at AWS Vietnam:

| Week | Date Range | Primary Tasks & Responsibilities | Accomplishments & Deliverables |
| :---: | :---: | :--- | :--- |
| **1** | Aug 03 - Aug 09 | • Attended FCAJ program onboarding sessions.<br>• Explored AWS Cloud fundamentals and basic Serverless services (IAM, S3, DynamoDB, Lambda). | Completed orientation, mastered security best practices and Serverless Cloud Architecture concepts. |
| **2** | Aug 10 - Aug 16 | • Researched NoSQL database design with Amazon DynamoDB.<br>• Designed Data Schema (Partition Key `city`, Sort Key `timestamp`).<br>• Configured secure IAM execution roles. | Finalized DynamoDB Schema and created IAM Roles adhering to Least Privilege principles. |
| **3** | Aug 17 - Aug 23 | • Researched OpenWeatherMap RESTful API specifications.<br>• Developed Python 3.12 automation code with AWS SDK `boto3`.<br>• Built AWS Lambda `weather-fetcher` for 10 target cities. | Tested `weather-fetcher` Lambda successfully, writing `Decimal` format telemetry data to DynamoDB. |
| **4** | Aug 24 - Aug 30 | • Enabled Time-To-Live (TTL) on DynamoDB to automatically purge records older than 7 days.<br>• Configured Amazon EventBridge Schedule Rule to run every `rate(30 minutes)`. | Fully automated the Data Ingestion pipeline without requiring manual intervention. |
| **5** | Aug 31 - Sep 06 | • Developed `weather-api-handler` Lambda to query latest telemetry from DynamoDB.<br>• Resolved Python `Decimal` serialization issues using custom `DecimalEncoder`. | Successfully launched Lambda API Backend returning clean 200 OK JSON payloads. |
| **6** | Sep 07 - Sep 13 | • Provisioned Amazon API Gateway HTTP API `weather-dashboard-api`.<br>• Configured `GET /weather` Route integrated with API Handler Lambda.<br>• Enabled Cross-Origin Resource Sharing (CORS `*`). | Published secure public API Invoke URL ready for web frontend integration. |
| **7** | Sep 14 - Sep 20 | • Designed interactive Web Dashboard UI (`index.html`) using HTML5/CSS3/JS.<br>• Integrated dynamic 5-day SVG forecast chart.<br>• Configured S3 Static Website Hosting & Public Read Bucket Policy. | Live Web Dashboard accessible via S3 Static Website Hosting URL. |
| **8** | Sep 21 - Sep 27 | • Conducted End-to-End system verification & performance testing.<br>• Packaged codebase and pushed to public GitHub repo `aws_final`.<br>• Authored bilingual FCJ Workshop documentation site. | 100% project completion, published open-source repo & submitted final internship report. |
