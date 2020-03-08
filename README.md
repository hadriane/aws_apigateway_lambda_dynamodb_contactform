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
- Postman

### Overview of Steps
1. Setup DynamoDB
    1. Create a DynamoDB table to capture the web contact from information from the frontend
2. Setup Lambda Python 3 function
    1. Create a IAM role to allow Lambda function to read/write to DynamoDB 
    2. Create a Lambda function to create records in DynamoDB
    3. Attach the IAM role to the Lambda function
3. Setup an API Gateway
    1. Create a resource for the API gateway
    2. Create a POST method for the API gateway
    3. Attach the Lambda function the the POST method
    4. Enable CORS on the POST method
    5. Publish the API
4. Use postmand to test
5. Setup a frontend web contact form
    1. Create a web contact form in Apache

### Create DynamoDB table
1. Login to AWS
2. Go to DynamoDB landing page
3. Click [Create Table]

### Output of script

The output from running this script will be a log file placed in the directory you ran the script

```
bash_centos_hardening-16-02-2019_05-31-41.log
```
