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
<img src="SERVER 1A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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

✅ Phase 1: S3 Upload → Lambda → DynamoDB
- Create a DynamoDB table with:
- Partition key: item_id
- Attributes: item_name, inventory_level, last_updated.

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
<img src="server 2.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br/>
<img src="server 2a.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 Examine the code. It performs the following steps:
- Downloads the file from Amazon S3 that invokes the event
- Loops through each line in the file
- Inserts the data into the DynamoDB Inventory table
- At the top of the Code source section, choose File and then choose Save
-  Then Deploy your changes.

✅ 2. Configuring an Amazon S3 event.

- On the AWS Management Console, in the search box, enter and choose S3
- Choose Create bucket.
- For Bucket name enter inventory-<number>
- Choose Create bucket.
- Choose the name of your inventory-<number> bucket.
- Choose the Properties tab.
- In the Event notifications section, choose **Create event notification**, and then configure these settings:
- Event name: Enter Load-Inventory
- Event types: Choose All object create events.
- Destination: Choose Lambda function.
- Lambda function: Choose Load-Inventory.
- Choose Save changes.

When an object is created in the bucket, this configuration tells Amazon S3 to invoke the Load-Inventory Lambda function that you created earlier.
Your bucket is now ready to receive inventory files.

✅ 3: Testing
 - Upload an inventory file to S3.
 - Confirm Lambda #1 runs and updates DynamoDB.
 - Confirm Lambda #2 triggers if any item is out of stock and SNS sends notification.
 - Log in to the dashboard and confirm
 - inventory data displays correctly.

4️⃣ SNS sends SMS/email notifications to the operations team.

- On the AWS Management Console, in the search box, enter and choose SNS.
- In the Create topic section, for Topic name, enter NoStock.
- Choose Next step.
- On the Create topic page, keep Standard selected.
- Choose Create topic.
To receive notifications, you must subscribe to the topic. You can choose to receive notifications through several methods, such as SMS and email.
- On the NoStock topic page, in the Subscriptions section, choose Create subscription.
- On the Create subscription page, configure these settings
- Protocol: Choose **Email**.
- Endpoint: Enter your email address.
- Choose **Create subscription**.
 After you create an email subscription, you will receive a confirmation email message. 
To confirm your subscription, open the email message, and choose the Confirm subscription link.
Any message that is sent to the SNS topic will be forwarded to your email.

5️⃣ Creating a Lambda function to send notifications.

You could modify the existing Load-Inventory Lambda function to check inventory levels while the file is being loaded. However, this configuration is not a good architectural practice. Instead of overloading the Load-Inventory function with business logic, you create another Lambda function that is invoked when data is loaded into the DynamoDB table. A DynamoDB stream invokes this function.

**Follow the previous step to create another LAMBDA Function**

In the Code source editor, copy and paste the following code:
<br/>
<img src="server 3.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br/>
<img src="server 3a.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
- It loops through the incoming records.
- If the inventory count is zero, it sends a message to the NoStock SNS topic.
You now configure the function so it is called when data is added to the Inventory table in DynamoDB
- To save your changes, chooseFile and then choose Save
- Then choose Deploy.
- In the Function overview section, choose Add trigger, and configure these settings:
- Select a source: Choose DynamoDB.
- DynamoDB table: Choose Inventory.
- Choose Add.
You are now ready to test the system.

<h2>🌟 Benefits</h2>
✅ Serverless, auto-scaling, cost-efficient architecture.
✅ No server management or capacity planning.
✅ Real-time inventory tracking and notifications.
✅ Secure user access via Cognito.
✅ Modern, maintainable cloud-native design.


