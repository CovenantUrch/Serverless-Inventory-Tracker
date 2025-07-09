<h1>📦 Serverless Inventory Tracking System on AWS</h1>

This project demonstrates how to **implement a fully serverless architecture on AWS** for an **inventory-tracking system** without using Amazon EC2, enabling automatic scaling and efficient pay-per-use operations.

---

<h1>🚀 Project Overview</h2>

Stores worldwide upload inventory files to Amazon S3, triggering a serverless workflow:
✅ **AWS Lambda** processes uploaded files and updates **Amazon DynamoDB** with inventory data.  
✅ Another Lambda function monitors DynamoDB updates and triggers **Amazon SNS** notifications when inventory is out of stock.  
✅ A **Cognito-authenticated web dashboard** displays current inventory levels by querying DynamoDB directly.

---

**Your architecture will look like the following example:**
<br/>
<img src="SERVER 1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<h2>🛠️ Services Used</h2>

- Amazon S3 – Stores uploaded inventory files.
- AWS Lambda – Executes backend logic on demand.
- Amazon DynamoDB – Stores and manages inventory data.
- Amazon SNS – Sends email/SMS notifications for low inventory.
- Amazon Cognito – Manages user authentication for dashboard access.
- AWS IAM – Manages secure permissions.

<h2>⚙️ Architecture Workflow</h2>

1️⃣ Store uploads inventory file (CSV/JSON) to S3.
2️⃣ S3 triggers Lambda #1:
Parses the file.
Inserts/updates records in DynamoDB.
3️⃣ DynamoDB Streams trigger Lambda #2:
Checks inventory levels.
Publishes to SNS if inventory is out of stock.
4️⃣ SNS sends notifications via email/SMS to stakeholders.
5️⃣ Cognito-authenticated users log into a web dashboard to view live inventory data directly from DynamoDB.

<h2>🚀 Deployment Steps</h2>

✅ 1. Creating a Lambda function to load data.
- On the AWS Management Console, in the search box, enter and choose Lambda to open the Lambda console.
- Choose Create function.
- Configure the following settings:
For Function name, enter **Load-Inventory**.
For Runtime, choose **Python 3.8**.
Expand  Change default execution role, and configure the following options:
For Execution role, choose create new role. This role gives the Lambda function permission to access Amazon S3 and DynamoDB.
- Choose Create function.
- In the Code source section, in the Environment pane, choose lambda_function.py.
- In the code editor for the lambda_function.py file, delete all the default code.
- In the Code source editor, copy and paste the following code:
<br/>
<img src="SERVER 1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
