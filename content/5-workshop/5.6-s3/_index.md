---
title : "Host Web UI on S3"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

Amazon Simple Storage Service (Amazon S3) provides industry-leading scalability, data availability, security, and performance for hosting cost-effective static websites.

---

### Step 1: Create S3 Bucket `weather-dashboard-toih`

1. Open **AWS Management Console** -> Search for **S3**.
2. Click **Create bucket**.
3. Bucket Name: `weather-dashboard-toih` | AWS Region: `ap-southeast-1`.
4. Under **Block Public Access settings for this bucket**, uncheck *Block all public access* (confirm acknowledging public bucket availability for web hosting).

![Create S3 Bucket](/workshop_aws/images/5-workshop/5.6-s3/01-create-bucket.png)

5. Click **Create bucket**.

---

### Step 2: Enable Static Website Hosting

1. Open the created `weather-dashboard-toih` bucket -> Click the **Properties** tab.
2. Scroll to **Static website hosting** -> Click **Edit**.
3. Choose **Enable** -> Enter **Index document**: `index.html`.
4. Click **Save changes**.

![Enable Static Website Hosting](/workshop_aws/images/5-workshop/5.6-s3/02-static-hosting.png)

---

### Step 3: Attach Public Read S3 Bucket Policy

Switch to **Permissions** tab -> Under **Bucket policy**, click **Edit** and paste the public read policy (`s3:GetObject`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::weather-dashboard-toih/*"
    }
  ]
}
```

![Bucket Policy Configuration](/workshop_aws/images/5-workshop/5.6-s3/03-bucket-policy.png)

---

### Step 4: Upload Frontend Assets (`index.html`)

1. Switch to the **Objects** tab -> Click **Upload**.
2. Select the `index.html` file (pre-configured with the API Gateway Invoke URL).
3. Click **Upload** to complete asset deployment.

![Upload Index HTML](/workshop_aws/images/5-workshop/5.6-s3/04-upload-index.png)

---

### Step 5: Obtain Bucket Website Endpoint URL

Return to the **Properties** tab -> Scroll to the bottom under **Static website hosting** to copy the public **Bucket website endpoint**:
`http://weather-dashboard-toih.s3-website-ap-southeast-1.amazonaws.com`

![S3 Website Endpoint URL](/workshop_aws/images/5-workshop/5.6-s3/05-website-url.png)
