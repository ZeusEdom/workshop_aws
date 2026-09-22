---
title : "Create DynamoDB Table & TTL"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.

---

### Step 1: Create DynamoDB Table `WeatherData`

1. Navigate to the **AWS Management Console** -> Search for **DynamoDB**.
2. On the DynamoDB Dashboard, click **Create table**.
3. Configure the table parameters:
   - **Table name:** `WeatherData`
   - **Partition key:** `city` (String)
   - **Sort key:** `timestamp` (Number)
4. Under **Table capacity settings**, select **On-demand** (auto-scales based on incoming traffic, pay only per request).

![Create DynamoDB Table](/images/5-workshop/5.1-dynamodb/01-create-table.png?featherlight=false&width=90pc)

5. Click **Create table** and wait a few seconds until the table status changes to **Active**.

![DynamoDB Table Active](/images/5-workshop/5.1-dynamodb/02-table-active.png?featherlight=false&width=90pc)

---

### Step 2: Configure Time-To-Live (TTL) for Auto Data Expiration

**Time-To-Live (TTL)** allows you to define a per-item timestamp to determine when an item is no longer needed. DynamoDB automatically deletes expired items without consuming write throughput.

1. Select the `WeatherData` table -> Click the **Additional settings** tab.
2. Locate the **Time-to-Live (TTL)** panel -> Click **Enable**.
3. In the **TTL attribute name** field, type `ttl`.
4. Click **Save changes** to finalize.

![Enable TTL Attribute](/images/5-workshop/5.1-dynamodb/03-enable-ttl.png?featherlight=false&width=90pc)

{{% notice note %}}
**How TTL works:** In the Lambda Fetcher function, the `ttl` attribute is computed as:
`ttl = current_epoch_timestamp + (7 * 86400)` (7 days into the future). DynamoDB automatically purges expired records in the background at no extra charge.
{{% /notice %}}
