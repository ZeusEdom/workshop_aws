---
title : "Configure IAM Roles & Policies"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

AWS Identity and Access Management (IAM) provides fine-grained access control across all AWS resources, enforcing the principle of least privilege.

---

### Step 1: Create IAM Execution Role `weather-fetcher-role`

1. Open **AWS Management Console** -> Search for **IAM**.
2. In the left navigation pane, select **Roles** -> Click **Create role**.
3. Select Trusted entity type: **AWS service** -> Choose Use case **Lambda** -> Click **Next**.

![Create IAM Role](/images/5-workshop/5.2-iam/01-create-role.png?featherlight=false&width=90pc)

4. On the **Add permissions** step, search for and select `AWSLambdaBasicExecutionRole` (grants permission to write logs to CloudWatch Logs).
5. Name the Role: `weather-fetcher-role` -> Click **Create role**.

---

### Step 2: Attach Custom Inline Policies for DynamoDB Access

To grant the Lambda function strict access to read and write records in the `WeatherData` table, attach custom inline policies restricted to the specific table ARN:

1. Open the created `weather-fetcher-role` -> Click **Add permissions** -> **Create inline policy**.

![Attach Policy to Role](/images/5-workshop/5.2-iam/02-attach-policy.png?featherlight=false&width=90pc)

2. Switch to the **JSON** editor and paste the Write Policy (`dynamodb:PutItem`):

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

3. Add the Read Policy (`dynamodb:Query`, `dynamodb:GetItem`):

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

4. Verify the finalized IAM Role permissions summary.

![IAM Role Summary](/images/5-workshop/5.2-iam/03-role-created.png?featherlight=false&width=90pc)
