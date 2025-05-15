# 🧾 Serverless Invoice Generator using AWS

A serverless invoice generation pipeline built with AWS services that converts invoice data into a PDF, emails it to the customer, uploads it to S3, sends a notification via SNS, and logs metrics to CloudWatch.

---

##  Features

➤ Accepts invoice data via HTTP (Postman / API Gateway)  
➤ Converts HTML invoice to PDF using `html2pdf.app`  
➤ Uploads the PDF to an S3 bucket  
➤ Emails the invoice as a PDF attachment via SES  
➤ Sends a notification through SNS  
➤ Monitors success/error metrics using CloudWatch  
➤ Raises CloudWatch Alarms on errors  

---

##  Architecture

**Postman** ➝ **API Gateway** ➝ **Lambda**  
                                                        ↳ HTML ➝ PDF ➝ S3  
                                                        ↳ SES (email)  
                                                        ↳ SNS (notification)  
                                                        ↳ CloudWatch Logs + Alarms

---

##  Tech Stack

- **AWS Lambda** – Serverless compute for invoice processing  
- **AWS API Gateway** – Entry point for HTTP requests  
- **AWS S3** – Stores generated invoice PDFs  
- **AWS SES** – Sends emails with invoice attachments  
- **AWS SNS** – Sends invoice creation alerts  
- **AWS CloudWatch** – Monitors Lambda success/error metrics  
- **html2pdf.app** – Converts HTML invoices to PDF  

---

## 📂 Folder Structure

