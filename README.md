# 📦 AWS Mini Project — S3 → Lambda → SNS Notification

This project demonstrates a simple serverless workflow using AWS services.  
Whenever a new file is uploaded to an S3 bucket, a Lambda function is triggered.  
The Lambda processes the event and sends an SNS notification email with file details.

---

## 🚀 Architecture

1. **Amazon S3**  
   - Acts as the event source.  
   - When a file is uploaded, it automatically triggers the Lambda function.

2. **AWS Lambda**  
   - Reads the S3 event details.  
   - Publishes a message to SNS containing the file name and timestamp.

3. **Amazon SNS**  
   - Sends a notification email to all subscribed users.

---

## 🛠 AWS Services Used

- **Amazon S3**
- **AWS Lambda (Python 3.x)**
- **Amazon SNS**
- **Amazon IAM**
- **Amazon CloudWatch Logs**

---

## 📐 Workflow Diagram


---

## 📂 Project Setup Steps

### **1. Create S3 Bucket**
- Open S3 console → Create bucket  
- Bucket name example: `demo-file-upload-trigger`

---

### **2. Create SNS Topic**
- Open SNS → Create Topic → Standard  
- Name: `demo-upload-topic`  
- Create subscription:
  - Protocol: Email
  - Endpoint: *your email*  
- Confirm subscription from your inbox

---

### **3. Create IAM Role for Lambda**
Create a role with:
- **AWSLambdaBasicExecutionRole**
- **AmazonSNSFullAccess**

Role name example: `demo-lambda-s3-sns-role`

---

### **4. Create Lambda Function**
- Runtime: **Python 3.12**
- Name: `demo-s3-to-sns`
- Attach IAM role created earlier
- Add the following code:

```python
import json
import boto3

sns = boto3.client('sns')

def lambda_handler(event, context):
    print("Received event:", event)

    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        time_uploaded = record['eventTime']

        message = (
            f"New file uploaded:\n"
            f"Bucket: {bucket}\n"
            f"File: {key}\n"
            f"Time: {time_uploaded}"
        )

        sns.publish(
            TopicArn="REPLACE_WITH_YOUR_SNS_TOPIC_ARN",
            Message=message,
            Subject="S3 File Uploaded Notification"
        )

    return {"statusCode": 200}

Subject: S3 File Uploaded Notification

New file uploaded:
Bucket: demo-file-upload-trigger
File: invoice.pdf
Time: 2025-02-01T12:00:11Z
