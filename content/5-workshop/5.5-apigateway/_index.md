---
title : "Setup API Gateway"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

Amazon API Gateway makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale.

---

### Step 1: Provision HTTP API

1. Open **AWS Management Console** -> Search for **API Gateway**.
2. Select **HTTP API** -> Click **Build**.
3. Set API Name: `weather-dashboard-api`.

![Create HTTP API](/images/5-workshop/5.5-apigateway/01-create-api.png?featherlight=false&width=90pc)

---

### Step 2: Create Route `GET /weather`

1. In the left menu, click **Routes** -> Click **Create**.
2. Choose Method: `GET` | Route path: `/weather`.

![Create GET Weather Route](/images/5-workshop/5.5-apigateway/02-create-route.png?featherlight=false&width=90pc)

---

### Step 3: Attach Integration to `weather-api-handler` Lambda

1. Select the `GET /weather` route -> Click **Attach integration**.
2. Integration type: **Lambda function**.
3. Choose function: `weather-api-handler` -> Click **Attach**.

![Attach Lambda Integration](/images/5-workshop/5.5-apigateway/03-attach-integration.png?featherlight=false&width=90pc)

---

### Step 4: Verify `$default` Auto-Deploy Stage

API Gateway automatically configures the `$default` stage with Auto-deploy enabled, instantly publishing route updates.

![Deploy Stage Config](/images/5-workshop/5.5-apigateway/04-deploy-stage.png?featherlight=false&width=90pc)

---

### Step 5: Configure CORS (Cross-Origin Resource Sharing)

To ensure web browsers hosting the S3 web UI can query API endpoints without cross-origin security blocks:

1. Select **CORS** from the left panel.
2. Click **Configure** and populate:
   - **Access-Control-Allow-Origin:** `*`
   - **Access-Control-Allow-Headers:** `*`
   - **Access-Control-Allow-Methods:** `GET, OPTIONS`
3. Click **Save**.

![CORS Setup](/images/5-workshop/5.5-apigateway/05-cors-setup.png?featherlight=false&width=90pc)

---

### Step 6: Retrieve Invoke URL & Browser Verification

1. Under **Stages**, copy the stage **Invoke URL**:
   `https://0ehzg82gn4.execute-api.ap-southeast-1.amazonaws.com`

![API Invoke URL](/images/5-workshop/5.5-apigateway/06-invoke-url.png?featherlight=false&width=90pc)

2. Test the API in your web browser:
   `https://0ehzg82gn4.execute-api.ap-southeast-1.amazonaws.com/weather`

The browser renders a clean `200 OK` JSON array containing current telemetry records.

![Browser JSON Test](/images/5-workshop/5.5-apigateway/07-browser-test.png?featherlight=false&width=90pc)
