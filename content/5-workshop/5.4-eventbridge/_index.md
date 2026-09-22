---
title : "Automated EventBridge Schedule"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---

Amazon EventBridge is a serverless event bus that enables building event-driven applications and automating cron schedule triggers at scale.

---

### Step 1: Create EventBridge Schedule Rule

1. Open **AWS Management Console** -> Search for **Amazon EventBridge**.
2. In the left navigation pane, select **Rules** (or **Schedules**) -> Click **Create rule**.
3. Name the Rule: `weather-fetch-schedule` -> Select Rule type: **Schedule**.

![Create EventBridge Rule](/workshop_aws/images/5-workshop/5.4-eventbridge/01-create-rule.png)

---

### Step 2: Configure Schedule Frequency Pattern

1. Select **A schedule that runs at a regular rate**.
2. Define parameters:
   - **Value:** `30`
   - **Unit:** `Minutes` (or expression `rate(30 minutes)`).
3. Click **Next**.

![Schedule Pattern 30 Minutes](/workshop_aws/images/5-workshop/5.4-eventbridge/02-schedule-pattern.png)

---

### Step 3: Attach Target AWS Lambda Function

1. Under **Select target**, choose Target types: **AWS service**.
2. Select target service: **Lambda function**.
3. Choose function target: `weather-fetcher`.

![Attach Lambda Target](/workshop_aws/images/5-workshop/5.4-eventbridge/03-target-lambda.png)

4. Click **Create rule** to activate.

Starting now, the automated pipeline invokes the `weather-fetcher` Lambda function every **30 minutes**, ensuring continuous ingestion of updated weather telemetry.
