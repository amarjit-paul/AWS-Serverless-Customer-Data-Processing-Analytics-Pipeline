# AWS-Serverless-Customer-Data-Processing-Analytics-Pipeline
This project demonstrates a Serverless Data Processing Pipeline built on AWS that automatically processes customer data uploaded to Amazon S3, cleans the data using AWS Lambda, stores the transformed dataset, and enables SQL-based analytics using Amazon Athena.
The objective was to simulate a real-world data engineering workflow where customer data is automatically prepared for business intelligence and analysis.

# Architecture

Customer CSV File
        │
        ▼
Amazon S3 (Raw Bucket)
        │
        ▼
S3 Event Trigger
        │
        ▼
AWS Lambda
(Data Cleaning & Transformation)
        │
        ▼
Amazon S3 (Processed Bucket)
        │
        ▼
Amazon Athena
(SQL Analytics)

# Data source - 
Dataset: Customer Shopping Trends Dataset
Source: Kaggle
Link: https://www.kaggle.com/datasets/rahulkumar37841/customer-purchase-behaviour-dataset

# AWS Services Used
Service	                 Purpose
Amazon S3	             Stores raw and processed customer data
AWS Lambda	           Performs automated data cleaning and transformation
IAM	                   Provides secure permissions to Lambda
Amazon CloudWatch	     Monitors Lambda execution and logs
Amazon Athena	         Queries processed data using SQL

# Project Workflow
Step 1: Data Upload

Customer data in CSV format is uploaded to the Raw S3 Bucket.

bucket-raw-data-01

Example file:

customer.csv
Step 2: Automatic Event Trigger

Whenever a file is uploaded:

S3 Object Created Event

automatically triggers the Lambda function.

No manual intervention is required.

Step 3: Data Cleaning Using AWS Lambda

The Lambda function performs:

Duplicate removal using Customer ID

Missing Subscription Status handling

Unknown

Missing Previous Purchases handling

0

Missing Frequency of Purchases handling

Not Specified

Adds Processing Timestamp

Processed_Date

Step 4: Store Processed Data

The cleaned file is automatically saved into:

bucket-processed-data-01

Output file:

cleaned_customer.csv

Step 5: Query Data Using Amazon Athena

Athena allows SQL queries directly on files stored in Amazon S3.

Example:

SELECT *
FROM customers
LIMIT 10;

# Dataset Information
Dataset Name

Customer Shopping Trends Dataset

Source
Kaggle

Dataset Features
Column
Customer ID
Age
Gender
Subscription Status
Previous Purchases
Frequency of Purchases
Location
Additional Generated Field
Column
Processed_Date

# Lambda Function Logic
Input
Customer ID,Age,Gender,Subscription Status,...

Processing
Read CSV file
Remove duplicates
Handle missing values
Add processing timestamp

Output
Customer ID,Age,Gender,Subscription Status,...
Processed_Date

# IAM Security Implementation

Created a dedicated Lambda execution role:

CustomerAnalyticsLambdaRole

Permissions:

CloudWatch Logging
AWSLambdaBasicExecutionRole
S3 Access
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject"
  ]
}

Following AWS Least Privilege principles.

# Sample Athena Queries

1. Total Customers
SELECT COUNT(*)
FROM customers;

2. Customers by Gender
SELECT gender,
COUNT(*)
FROM customers
GROUP BY gender;

3. Subscription Distribution
SELECT subscription_status,
COUNT(*)
FROM customers
GROUP BY subscription_status;

4. Weekly Purchasers
SELECT *
FROM customers
WHERE frequency_of_purchases='Weekly';

5. Subscribers by Age Group
SELECT
CASE
WHEN age BETWEEN 18 AND 25 THEN '18-25'
WHEN age BETWEEN 26 AND 35 THEN '26-35'
WHEN age BETWEEN 36 AND 45 THEN '36-45'
WHEN age BETWEEN 46 AND 55 THEN '46-55'
ELSE '56+'
END AS age_group,
COUNT(*) AS subscribers
FROM customers
WHERE subscription_status='Yes'
GROUP BY 1
ORDER BY subscribers DESC;

# Screenshots

<img width="940" height="426" alt="image" src="https://github.com/user-attachments/assets/f038f58a-ff96-4edf-8248-8d89e472f99d" />

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/ffcd8459-beb5-4325-8e1a-be794adaef68" />

<img width="940" height="432" alt="image" src="https://github.com/user-attachments/assets/ccd97102-0f5a-43f1-aed4-b72a57604264" />

<img width="940" height="436" alt="image" src="https://github.com/user-attachments/assets/bf7e4364-3859-45be-894c-b24e6d7d20b7" />

<img width="940" height="436" alt="image" src="https://github.com/user-attachments/assets/8c26953b-e5f5-4610-82ad-4dc7da0f40dc" />

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/efd01d58-e05e-4202-8562-b17d39f475dc" />

<img width="940" height="430" alt="image" src="https://github.com/user-attachments/assets/0fb2a6db-e83e-4584-9750-1267feddb19a" />

<img width="940" height="432" alt="image" src="https://github.com/user-attachments/assets/d6e4eeb5-7dbf-48b1-85ec-bbf9c64b9dcd" />

<img width="940" height="435" alt="image" src="https://github.com/user-attachments/assets/6ebb688a-f2da-45a8-9048-a9233661bea1" />

<img width="940" height="436" alt="image" src="https://github.com/user-attachments/assets/0f984a7c-b7e4-43ad-b7a8-7bfd301e2d00" />

<img width="940" height="430" alt="image" src="https://github.com/user-attachments/assets/3c43fb67-3c8a-44b8-bba3-6d00e837f1c6" />

<img width="940" height="646" alt="image" src="https://github.com/user-attachments/assets/908613a3-e30c-4600-930d-891a9f59a700" />

<img width="940" height="301" alt="image" src="https://github.com/user-attachments/assets/97fb4268-f645-4833-a42e-c2934fdce880" />

<img width="940" height="460" alt="image" src="https://github.com/user-attachments/assets/5dc898b3-d08a-42cf-b6fb-65fdbd374363" />

<img width="940" height="435" alt="image" src="https://github.com/user-attachments/assets/42cc04ca-9fff-45d3-a750-13b7ce25641f" />

<img width="940" height="444" alt="image" src="https://github.com/user-attachments/assets/56fd258b-d8c9-4bc0-8757-8ec2881f27e3" />

<img width="940" height="398" alt="image" src="https://github.com/user-attachments/assets/3e628d00-9330-453d-b8dc-c3d353898066" />

<img width="940" height="446" alt="image" src="https://github.com/user-attachments/assets/ba49e5b8-9b20-43e2-90cf-096016b76141" />

<img width="940" height="446" alt="image" src="https://github.com/user-attachments/assets/af71a7d3-cfa9-4107-995e-69a1acc1beee" />

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/ffe85a74-59d2-4a1e-b0aa-259df53d2cd3" />
