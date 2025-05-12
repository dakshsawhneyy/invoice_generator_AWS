import json
import boto3
import requests
import base64
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication
from email.mime.text import MIMEText

s3 = boto3.client('s3')
ses = boto3.client('ses', region_name='us-east-1')  # your region
BUCKET = 'pdf-invoices-<your-username>'  # Enter your username

SENDER = "your_verified_email@example.com"
RECIPIENT = "recipient@example.com"  # must be verified in SES sandbox

def lambda_handler(event, context):
    try:
        data = json.loads(event['body'])
        customer_name = data['customer_name']
        invoice_id = data['invoice_id']
        items = data['items']
        date = data['date']

        # Create HTML content
        html = f"""
        <html><body>
        <h1>Invoice {invoice_id}</h1>
        <p>Date: {date}</p>
        <p>Customer: {customer_name}</p>
        <table border="1">
            <tr><th>Description</th><th>Quantity</th><th>Price</th><th>Total</th></tr>
        """

        total = 0
        for item in items:
            subtotal = item['quantity'] * item['price']
            total += subtotal
            html += f"<tr><td>{item['description']}</td><td>{item['quantity']}</td><td>{item['price']}</td><td>{subtotal}</td></tr>"

        html += f"</table><h2>Total: ${total}</h2></body></html>"

        # Convert HTML to PDF
        pdf_response = requests.post(
            'https://api.html2pdf.app/v1/generate',
            json={
                "html": html,
                "apiKey": "<htmlpdf.app-api-key>" # Enter htmlpdf.app api key from its website
            }
        )

        if pdf_response.status_code != 200:
            raise Exception("Failed to convert HTML to PDF")

        pdf_content = pdf_response.content

        # Upload to S3
        s3_key = f'invoices/{invoice_id}.pdf'
        s3.put_object(
            Bucket=BUCKET,
            Key=s3_key,
            Body=pdf_content,
            ContentType='application/pdf'
        )

        # Email the PDF using SES
        msg = MIMEMultipart()
        msg['Subject'] = f'Invoice {invoice_id}'
        msg['From'] = SENDER
        msg['To'] = RECIPIENT

        # Email body
        body = MIMEText(f"Hi {customer_name},\n\nPlease find attached your invoice {invoice_id}.", 'plain')
        msg.attach(body)

        # Attach PDF
        attachment = MIMEApplication(pdf_content)
        attachment.add_header('Content-Disposition', 'attachment', filename=f"{invoice_id}.pdf")
        msg.attach(attachment)

        # Send email
        ses.send_raw_email(
            Source=SENDER,
            Destinations=[RECIPIENT],
            RawMessage={'Data': msg.as_string()}
        )

        return {
            'statusCode': 200,
            'body': json.dumps({'message': 'Invoice created and emailed successfully'}),
            'headers': {'Content-Type': 'application/json'}
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'message': str(e)}),
            'headers': {'Content-Type': 'application/json'}
        }
