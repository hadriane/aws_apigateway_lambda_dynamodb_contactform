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
    1. Create a IAM policy to allow list/list_indexes/read/write/update/delete_items to a specific DynamoDB table
    2. Create a IAM role and attach the created policy to the role 
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

### Setup API Gateway
1. 

### Setup IAM role
1. Create a IAM policy to read/write to a specific DynamoDB table
    1. Go to IAM landing page
    2. Click on **Policies**
    3. Click **Create Policy**
    4. Click on the **JSON** tab
    5. Enter the below JSON policy after modifying the **arn** to the DynamoDB table created earlier
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DynamodbContactformTable",
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:BatchGetItem",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem"
            ],
            "Resource": [
            "arn:aws:dynamodb:ap-southeast-X:XXXXXXXXXXXX:table/CONTACT_FORM",
	    "arn:aws:dynamodb:ap-southeast-X:XXXXXXXXXXXX:table/CONTACT_FORM/index/*"
	    ]
        }
    ]
}
```
    6. Click **Review Policy**
    7. Give the policy a **Name**
    8. CLick **Create Policy**
    
2. Create a IAM role and attach the created policy to the role
    1. Go to IAM landing page
    2. Click **Create Role**
    3. Select **Lambda**
    4. Click **Next: Permissions**
    5. Search and select **DynamodbContactformTable**
    6. Click **Next: Tags**
    7. Enter **Key** and **Value**
        1. Key = Name
        2. Value = DynamodbContactformTable
    8. Click **Next: Review**
    9. Enter the **Role Name** Ex: LambdaDynamodbContactformTable
    10. Click **Create Role**

### Setup Lambda Python 3 function
1. Create a Lambda function and attach an IAM role to it
    1. Go to Lambda landing page
    2. Click **Create Function**
    3. Select **Author From Scratch**
    4. Give it a **Function name** Ex: UpadteDynamodbContactformTable
    5. Select **Runtime** Python 3.8
    6. Expand **Choose or Create an Execution Role** and select **Use an Existing Role**'
    7. Select DynamodbContactformTable from **Existing role** drop-down
    8. Click **Create Function**
 
 2. Enter the Python code
     1. Select the API gateway we created earlier
     2. In the **Function code** section, in the editor box, paste the following code
```python
import boto3, json, os
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('CANDIDATE-ANSWERS')

def lambda_handler(event, context):
    
    ###---TEST 3---###
    event['FIRST_NAME'] = event['FIRST_NAME'].capitalize()
    event['LAST_NAME'] = event['LAST_NAME'].capitalize()
    table.put_item(Item=event)
    return res
```
 
