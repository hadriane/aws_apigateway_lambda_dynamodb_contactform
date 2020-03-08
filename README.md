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
2. Setup IAM role
    1. Create a IAM policy to allow list/list indexes/read/write/update/delete to a specific DynamoDB table
    2. Create a IAM role and attach the create role to the policy 
3. Setup Lambda Python 3 function
    1. Create a Lambda function to create records in DynamoDB
    2. Attach the IAM role to the Lambda function
3. Setup an API Gateway
    1. Create a resource for the API gateway
    2. Create a POST method for the API gateway
    3. Attach the Lambda function the the POST method
    4. Enable CORS on the POST method
    5. Publish the API
4. Use postmand to test
5. Setup a frontend web contact form
    1. Create a web contact form in Apache

### Setup DynamoDB
1. Login to AWS
2. Go to DynamoDB landing page
3. Click **Create Table**
    1. **Table Name** - Give the table a name. Ex: CONTACT_FORM
    2. **Primary Key** - This is the column in the table that will uniquely indetify the rows in the table. Ex: FIRST_NAME
    3. **Use Default Settings** - Leave this ticked. This will default the the minimum 5 Read capacity units and 5 Write capacity units which is all we need for a POC
    4. Note the **Amazon Resource Name (ARN)** : arn:aws:dynamodb:ap-southeast-X:XXXXXXXXXXXX:table/CONTACT_FORM

### Setup IAM role
1. Create a IAM policy to read/write to a specific DynamoDB table
    1. Go to IAM landing page
    2. Click on **Policies**
    3. Click **Create Policy**
    4. Click on the **JSON** tab
    5. Enter the below JSON policy after modifying the **arn** to the DynamoDB table created earlier
    ...
    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ReadOnlyAPIActionsOnBooks",
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:BatchGetItem"
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem"
            ],
            "arn:aws:dynamodb:ap-southeast-X:XXXXXXXXXXXX:table/CONTACT_FORM"
			"arn:aws:dynamodb:ap-southeast-X:XXXXXXXXXXXX:table/CONTACT_FORM/index/*"
            }
        ]
    }
    ...
    
1. Setup IAM role for the Lambda function
    1. Click **Roles**
    2. CLick **Create Role**
    3. Search for **Lambda** and click on it from the list
    4. CLick **Next: Permissions**


The output from running this script will be a log file placed in the directory you ran the script

```
bash_centos_hardening-16-02-2019_05-31-41.log
```
