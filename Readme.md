# Serverless Image Processing with AWS S3 and Lambda

A fully serverless image resizing pipeline leveraging **Amazon S3**, **Lambda**, **SQS**, and **CloudWatch**.  
The system resizes user-uploaded images to standardized dimensions for a social media application, following best practices for security, scalability, and cost-efficiency.

---

## 🚀 Table of Contents

- [1. Executive Summary](#1-executive-summary)
- [2. Project Overview](#2-project-overview)
- [3. Solution Architecture](#3-solution-architecture)
- [4. Implementation Details](#4-implementation-details)
- [5. Workflow and Data Processing](#5-workflow-and-data-processing)
- [6. Error Handling and Logging](#6-error-handling-and-logging)
- [7. Testing and Validation](#7-testing-and-validation)
- [8. Performance and Cost Analysis](#8-performance-and-cost-analysis)
- [9. Security and Compliance](#9-security-and-compliance)
- [10. Limitations and Future Enhancements](#10-limitations-and-future-enhancements)
- [11. Conclusion](#11-conclusion)

---

## 1. Executive Summary

This project implements an automated, serverless image processing pipeline using AWS services. It ensures secure, scalable, and efficient resizing of images uploaded by users, following best practices in access control, monitoring, and resource optimization.

---

## 2. Project Overview

The solution automatically processes images as they are uploaded to an S3 bucket. It consists of:
- Event-driven architecture with S3 notifications
- S3 buckets for source and destination images
- Lambda function for image resizing (Python + Pillow)
- SQS queue as an event buffer
- CloudWatch for logging and monitoring

---

## 3. Solution Architecture

### 3.1 System Components

- **Source S3 Bucket:** Receives uploaded images, triggers SQS notification.
- **SQS Queue:** Buffers events between S3 and Lambda.
- **Lambda Function:** Resizes images using Python/Pillow.
- **Destination S3 Bucket:** Stores processed images with proper access controls.
- **CloudWatch:** Logs and monitors the pipeline.

---

## 4. Implementation Details

### 4.1 S3 Bucket Configuration

- Source bucket allows public users to upload images and Lambda to read images.
- Destination bucket allows Lambda to write images and public users to download them.

### 4.2 SQS Queue Setup

- Configured to receive notifications from the source bucket.
- Acts as a buffer between S3 and Lambda.

### 4.3 Lambda Function Deployment

- Runtime: Python 3.9
- Library: Pillow
- Reads from source S3 bucket and writes resized images to destination bucket.
- Uses environment variables for configuration.

### 4.4 IAM Policies

Lambda execution role includes:
- Permission to read from source bucket.
- Permission to write to destination bucket.
- Permission to receive and delete messages from SQS queue.
- CloudWatch logging permissions.

### 4.5 Event Notifications and Triggers

- S3 event notifications are configured to publish `PutObject` events to SQS.
- The Lambda function polls the SQS queue and processes messages.

---

## 5. Workflow and Data Processing

1. A user uploads an image to the source S3 bucket.
2. The S3 event triggers a message to the SQS queue.
3. Lambda retrieves the message and downloads the image.
4. The image is resized and uploaded to the destination S3 bucket.
5. Logs are stored in CloudWatch.

---

## 6. Error Handling and Logging

- Lambda errors are logged in CloudWatch.
- The SQS visibility timeout ensures messages are retried if Lambda fails.

---

## 7. Testing and Validation

- Tests were conducted to confirm:
  - Correct image processing
  - Access control enforcement
  - Logging accuracy
- The system successfully processed various image formats and logged all operations.

---

## 8. Performance and Cost Analysis

- **Average processing time per image:** 500-800 milliseconds.
- **Estimated monthly cost for processing 1,000 images:** Below $1.00.
- **Lambda memory usage:** Optimized at 1024 MB.

---

## 9. Security and Compliance

- All S3 buckets use **server-side encryption (SSE)**.
- **HTTPS** is enforced for all access.
- IAM roles follow the **principle of least privilege**.

---

## 10. Limitations and Future Enhancements

### Limitations:
- No support for **SVG** and **WebP** formats.
- No duplicate image detection.

### Future Enhancements:
- Implement a **dead-letter queue** for failed messages.
- Add a **CDN layer** for optimized content delivery.

---

## 11. Conclusion

The image resizing pipeline using AWS serverless services is a **functional**, **secure**, and **scalable** solution.  
It ensures:
- Proper access control
- Efficient processing
- Compliance with security best practices.

---

## 📌 License

This project is licensed under [MIT License](LICENSE).

---

## 🙌 Acknowledgments

Thanks to the **Manara SAA Program** and all contributors!
