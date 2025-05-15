
# Serverless Invoice Generator using AWS

A serverless invoice generation pipeline built with AWS services that converts invoice data into a PDF, emails it to the customer, uploads it to S3, sends a notification via SNS, and logs metrics to CloudWatch.

---

## Features

- Accepts invoice data via HTTP (Postman / API Gateway)
- Converts HTML invoice to PDF using `html2pdf.app`
- Uploads the PDF to an S3 bucket
- Emails the invoice as a PDF attachment via SES
- Sends a notification through SNS
- Monitors success/error metrics using CloudWatch
- Raises CloudWatch Alarms on errors

---

##  Architecture

**Postman** → **API Gateway** → **Lambda**
- Converts raw data → HTML → PDF
- Uploads to S3
- Sends email via SES
- Sends alert via SNS
- Pushes metrics to CloudWatch (Success / Error)
- CloudWatch Alarm triggers if errors occur

---

##  Tech Stack

- **AWS Lambda** – Invoice processing logic
- **AWS API Gateway** – Accepts invoice HTTP requests
- **AWS S3** – Stores the invoice PDFs
- **AWS SES** – Sends invoice emails
- **AWS SNS** – Sends invoice alerts
- **AWS CloudWatch** – Custom metrics & alarms
- **html2pdf.app** – Converts HTML to PDF

---

##  Sample Input (Postman)

```json
POST /generate-invoice
{
  "customer_name": "John Doe",
  "invoice_id": "INV1234",
  "date": "2025-05-15",
  "items": [
    {"description": "Product A", "quantity": 2, "price": 50},
    {"description": "Product B", "quantity": 1, "price": 100}
  ]
}
```

---

##  CloudWatch Metrics & Alarm

- `InvoiceCreatedSuccess` – Custom metric for success count
- `InvoiceCreationError` – Custom metric for failure count
- Alarm triggers if error metric ≥ 1 in 5-minute window

---

##  Output

- Invoice is sent to customer via email (SES)
- PDF invoice is uploaded to S3
- SNS notification is sent
- CloudWatch metrics logged

---

##  Environment Setup

- Verify SES email (sandbox or production)
- Create SNS topic & subscribe
- Create S3 bucket
- Attach IAM permissions to Lambda:
  - `AmazonS3FullAccess`
  - `AmazonSESFullAccess`
  - `AmazonSNSFullAccess`
  - `CloudWatchFullAccess`

---

##  Future Improvements

- Add front-end dashboard
- Add database (DynamoDB) to store invoices
- Retry logic for email/PDF failures
- Payment integration

---

## 👨‍💻 Author

**Daksh Sawhney**  
DevOps Engineer | Cloud Enthusiast  
📫 dakshsawhneyy@gmail.com  
🔗 [Blog](https://dakshsawhneyy.hashnode.dev/) | [LinkedIn](https://www.linkedin.com/in/dakshsawhneyy/)

---
