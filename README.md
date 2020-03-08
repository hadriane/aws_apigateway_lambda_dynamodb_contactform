# Building A Basic Web Contact Form Using AWS Lambda, AWS API Gateway, DynamoDB and Ajax

### Purpose

The purpose of this exercise is to understand basic behaviour of:
- AWS API Gateway
- AWS Lambda
- AWS DynamoDB
- Using the AWS services above and Ajax to create a basic web contact form

### Prerequisites

- AWS account
- Apache web server

### Overview of Steps
1. Setup DynamoDB
  a. Create a DynamoDB table to capture the web contact from information from the frontend
2. Setup Lambda Python 3 function
  a. Create a IAM role to allow Lambda function to read/write to DynamoDB 
  b. Create a Lambda function to create records in DynamoDB
  c. Attach the IAM role to the Lambda function

Upload this script to your home directory and run it

```
sudo ./bash_centos_hardening.sh
```

### Output of script

The output from running this script will be a log file placed in the directory you ran the script

```
bash_centos_hardening-16-02-2019_05-31-41.log
```
