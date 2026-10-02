# AWS Serverless Image Resizer

## Project Overview

The **AWS Serverless Image Resizer** is a cloud-based image processing project that automatically resizes images whenever they are uploaded to an Amazon S3 bucket.

The project uses **Amazon S3, AWS Lambda, IAM, Amazon CloudWatch, Python, and Pillow** to create a serverless image-processing workflow without using any servers.

When a user uploads an image to the original S3 bucket, an S3 `ObjectCreated` event triggers the Lambda function. The Lambda function downloads the image, resizes it to a maximum of **800 × 800 pixels**, and stores the resized image in a separate S3 bucket.

---

## Objective

The main objectives of this project are:

- Automatically process uploaded images.
- Resize images using AWS Lambda.
- Store original and resized images separately.
- Use S3 events to trigger Lambda automatically.
- Apply IAM permissions securely.
- Monitor Lambda execution using CloudWatch.
- Build a serverless application without managing EC2 servers.

---

## AWS Services Used

| Service | Purpose |
|---|---|
| **Amazon S3** | Stores original and resized images |
| **AWS Lambda** | Processes and resizes images |
| **AWS IAM** | Manages permissions and access |
| **Amazon CloudWatch** | Stores Lambda logs and monitors execution |
| **Python 3.13** | Programming language used for Lambda |
| **Pillow** | Python image-processing library |

---

## Architecture

![Serverless Image Resizer Architecture](screenshots/architecture-diagram.png)

### Architecture Flow

```text
User
  |
  v
S3 Original Bucket
  |
  | ObjectCreated Event
  v
AWS Lambda
  |
  | Resize Image
  | Maximum 800 x 800
  v
S3 Resized Bucket
  |
  v
Resized Image
```

CloudWatch is used for Lambda logs and monitoring, while IAM controls the required permissions.

---

## S3 Buckets

### Original Image Bucket

```text
aws-image-resizer-original-saanchi-2026
```

This bucket stores the images uploaded by the user.

### Resized Image Bucket

```text
aws-image-resizer-resized-saanchi-2026
```

The Lambda function stores processed images in the:

```text
resized/
```

folder.

### Lambda Deployment Bucket

```text
aws-image-resizer-deploy-saanchi-2026
```

This bucket was used to store the Lambda deployment ZIP package.

---

## Lambda Function

### Function Name

```text
serverless-image-resizer
```

### Runtime

```text
Python 3.13
```

### Handler

```text
lambda_function.lambda_handler
```

### Architecture

```text
x86_64
```

### Memory

```text
128 MB
```

### Timeout

```text
30 seconds
```

---

## How the Project Works

### Step 1 — Upload Image

The user uploads an image to the original S3 bucket.

Example:

```text
Screenshot (32).png
```

### Step 2 — S3 Event

Amazon S3 detects the new object and generates an `ObjectCreated` event.

The event triggers the Lambda function.

### Step 3 — Lambda Processing

The Lambda function:

1. Reads the S3 event.
2. Identifies the source bucket and image.
3. Downloads the image from S3.
4. Opens the image using Pillow.
5. Resizes the image to a maximum of 800 × 800 pixels.
6. Creates the resized image in memory.
7. Uploads the processed image to the resized S3 bucket.

### Step 4 — Store Resized Image

The resized image is stored using:

```text
resized/<original-file-name>
```

Example:

```text
resized/Screenshot (32).png
```

### Step 5 — Monitoring

AWS Lambda execution logs are available through Amazon CloudWatch.

---

## Image Resizing Logic

The project uses Pillow's thumbnail functionality:

```python
max_width = 800
max_height = 800

image.thumbnail((max_width, max_height))
```

This keeps the image within the maximum dimensions while maintaining its aspect ratio.

For the test image, the resulting dimensions were:

```text
696 × 397 pixels
```

---

## IAM Permissions

IAM is used to control access between AWS services.

The Lambda function requires permissions to:

- Read objects from the original S3 bucket.
- Write objects to the resized S3 bucket.
- Execute the Lambda function.

The project also uses IAM permissions for AWS resource management during development.

---

## Testing

The project was tested using:

```text
Screenshot (32).png
```

The image was uploaded to:

```text
aws-image-resizer-original-saanchi-2026
```

The Lambda function successfully processed the image.

### Lambda Test Result

```json
{
  "statusCode": 200,
  "body": "\"Image resized successfully\""
}
```

### Output

The resized image was created at:

```text
resized/Screenshot (32).png
```

Output image type:

```text
PNG
```

Output size:

```text
209.2 KB
```

Verified dimensions:

```text
696 × 397 pixels
```

---

## Project Screenshots

The `screenshots` folder contains screenshots demonstrating:

1. Lambda function configuration
2. S3 resized image
3. Successful Lambda test
4. Architecture diagram
5. CloudWatch logs
6. S3 buckets and project workflow

---

## Project Structure

```text
AWS-Serverless-Image-Resizer/
│
├── lambda-package/
│   └── lambda_function.py
│
├── screenshots/
│   ├── architecture-diagram.png
│   ├── lambda-function.png
│   ├── s3-resized-image.png
│   ├── lambda-success.png
│   └── cloudwatch-logs.png
│
├── lambda-function.zip
├── lambda-s3-policy.json
├── lambda-trust-policy.json
└── README.md
```

---

## Key Features

- Serverless image processing
- Automatic S3 event triggering
- Python-based Lambda function
- Automatic image resizing
- Separate original and processed image storage
- IAM-based access control
- CloudWatch monitoring
- No EC2 server required
- Scalable AWS architecture

---

## Key Learnings

Through this project, I learned:

- How Amazon S3 buckets and objects work.
- How S3 events can trigger Lambda functions.
- How to create and configure AWS Lambda functions.
- How to package Python dependencies for Lambda.
- How to use Pillow for image processing.
- How IAM permissions control AWS resources.
- How to monitor Lambda using CloudWatch.
- How to test and troubleshoot Lambda deployment errors.
- How to build a serverless AWS workflow.

---

## Future Improvements

Possible future enhancements include:

- Add a web-based frontend for image uploads.
- Support multiple image formats.
- Allow users to select custom resize dimensions.
- Add image compression.
- Add thumbnails of different sizes.
- Add CloudFront for faster content delivery.
- Add API Gateway for application-based uploads.

---

## Conclusion

The AWS Serverless Image Resizer successfully automates image processing using AWS serverless services.

The completed workflow is:

```text
Image Upload
     ↓
Amazon S3
     ↓
S3 ObjectCreated Event
     ↓
AWS Lambda
     ↓
Image Resize
     ↓
Resized Amazon S3 Bucket
     ↓
CloudWatch Monitoring
```

This project demonstrates practical knowledge of **AWS S3, Lambda, IAM, CloudWatch, Python, and serverless architecture**.

---

## Author

**Saanchi Padekar**

**Project:** AWS Serverless Image Resizer  
**Technology:** AWS | Python | Boto3 | Pillow | Serverless Architecture