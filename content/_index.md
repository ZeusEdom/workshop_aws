---
title : "FCAJ Internship Report & AWS Serverless Workshop"
date : "`r Sys.Date()`" 
weight : 1 
chapter : false
---

#### Welcome to the FCAJ Internship Final Report!
This website presents the comprehensive **First Cloud Journey (FCAJ)** internship final report and the step-by-step hands-on guide for building a **Serverless Weather Dashboard & Real-time Telemetry Pipeline** on **Amazon Web Services (AWS)**.

---

###  Overview of 7 Report Sections

| Section | Title | Summary |
| :---: | :--- | :--- |
| **1.1** | [Student & Company Information](1-student-info/) | Student details, Hanoi University of Civil Engineering, AWS Vietnam & Cloud Engineer Intern role. |
| **1.2** | [8-Week Internship Worklog](2-worklog/) | Detailed weekly milestones, learning progress, and contributions (03/08/2026 - 27/09/2026). |
| **1.3** | [Project Proposal & Architecture](3-proposal/) | Serverless Weather Dashboard problem statement & 6-service architecture diagram. |
| **1.4** | [Events & Activities](4-events/) | AWS Tech Talks, FCJ Community Workshops, and teamwork activities. |
| **1.5** | [Hands-on Workshop Guide](5-workshop/) | **Core Section (29 Screenshots)**: Step-by-step console guide for full AWS deployment. |
| **1.6** | [Self-Evaluation & Reflections](6-self-evaluation/) | Technical & soft skill growth, learnings, and future cloud direction. |
| **1.7** | [Program Feedback](7-feedback/) | Constructive feedback for FCAJ program and acknowledgments to mentors. |

---

### ️ AWS Services Utilized

- **Amazon DynamoDB**: NoSQL database for real-time telemetry storage with **TTL** auto-expiration.
- **AWS Lambda**: Serverless Python 3.12 compute functions for fetching data and serving REST requests.
- **Amazon EventBridge**: Cron scheduler executing data collection every **30 minutes**.
- **Amazon API Gateway**: HTTP API providing CORS-enabled RESTful API endpoint.
- **Amazon S3**: High-availability **Static Website Hosting** for the interactive dashboard UI.
- **AWS IAM**: Security roles adhering to the principle of least privilege.

---

{{% notice note %}}
**Public GitHub Repository:** The complete source code, IAM policies, IaC template, and frontend HTML/JS are available at: [GitHub - ZeusEdom/aws_final](https://github.com/ZeusEdom/aws_final).
{{% /notice %}}