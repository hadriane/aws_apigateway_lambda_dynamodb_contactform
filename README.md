## Building A Basic Web Contact Form Using AWS Lambda, AWS API Gateway, DynamoDB and Ajax

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
3. Setup an API Gateway
    1. Create an API Gateway
    2. Create a resource for the API gateway and enable CORS
    3. Create a POST method for the API gateway
    4. Deploy the API
4. Setup Lambda Python 3 function
    1. Create a Lambda function and attach an IAM role to it
    2. Add Trigger to Lambda function 
	3. Write the Python code
5. Use Postman to test
    1. Create a new Postman test
	2. Setup the test
	3. Check the DynamoDB table CONTACT_FORM has been update
6. Setup a frontend web contact form
    1. Create a web contact form in Apache
7. Test the frontend web contact form

### 1 - Setup DynamoDB
1. Login to AWS
2. Go to DynamoDB landing page
3. Click **Create Table**
    1. **Table Name** - Give the table a name. Ex: CONTACT_FORM
    2. **Primary Key** - This is the column in the table that will uniquely indetify the rows in the table. Ex: FIRST_NAME
    3. **Use Default Settings** - Leave this ticked. This will default the the minimum 5 Read capacity units and 5 Write capacity units which is all we need for a POC
    4. Note the **Amazon Resource Name (ARN)** : arn:aws:dynamodb:ap-southeast-X:XXXXXXXXXXXX:table/CONTACT_FORM

### 2 - Setup IAM role
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
        1. **Key** = Name
        2. **Value** = DynamodbContactformTable
    8. Click **Next: Review**
    9. Enter the **Role Name** Ex: LambdaDynamodbContactformTable
    10. Click **Create Role**

### 3 - Setup API Gateway
1. Create and API Gateway 
    1. Go to API Gateway landing page
    2. Give it an **API Name** Ex: UpdateDynamodbContactformTable
    3. Click **Create API**
2. Create a resource for the API gateway and enable CORS
    1. Click **Actions** then click **Create Resource**
    2. Give it a **Resource Name** Ex: UpdateDynamodbContactformTable
    3. Tick **Enable API Gateway CORS**
    4. Click **Create Resource**
3.  Create a POST method for the API gateway
    1. Click **Actions** then click **Create Method**
    2. Select **POST** from the drop down then click the **Tick Symbol**
    3. Select Lambda Function for **Integration Type**
    4. Select UpadteDynamodbContactformTable for the **Lambda Function** drop-down
    5. Click **Save** then click **Ok**
4. Deploy the API
    1. Click **Actions**
    2. Click **Deploy API**
    3. Select [New Stage] for the **Deployment Stage**
    4. Give it a **Stage Name** Ex: DEV
    5. Click **Deploy**
    6. Highlight the **POST** method and note down the **Invoke URL** Ex: https://lkaXXXXXXX.execute-api.ap-southeast-1.amazonaws.com/DEV/updatedynamodbcontactformtable

### 4 - Setup Lambda Python 3 function
1. Create a Lambda function and attach an IAM role to it
    1. Go to Lambda landing page
    2. Click **Create Function**
    3. Select **Author From Scratch**
    4. Give it a **Function name** Ex: UpadteDynamodbContactformTable
    5. Select **Runtime** Python 3.8
    6. Expand **Choose or Create an Execution Role** and select **Use an Existing Role**'
    7. Select DynamodbContactformTable from **Existing role** drop-down
    8. Click **Create Function**
 2. Add Trigger to Lambda function
     1. Click on **Add Trigger**
     2. Select **API Gateway** from the drop-down
     3. Select UpdateDynamodbContactformTable for **API**
     4. Select DEV for **Deployment stage**
     5. Select Open for **Security** (Not a good practice however to keep things simple)
     6. Click **Add**
 3. Write the Python code
     1. Select the API gateway we created earlier
     2. In the **Function code** section, in the editor box, paste the following code
    ```python
    import boto3, json, os
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('CONTACT_FORM')
    
    def lambda_handler(event, context):
        
        #---Capital the first letter of First Name and Last Name in the JSON object
        event['FIRST_NAME'] = event['FIRST_NAME'].capitalize()
        event['LAST_NAME'] = event['LAST_NAME'].capitalize()
        #---Put the items into DynamoDB
        table.put_item(Item=event)
    ```
 
### 5 - Use Postman to test
1. Create a new Postman test
    1. Launch Postman on you PC
    2. Click **NEW**
    3. In **Building Blocks** select **Request**
    4. Give it a **Request Name** Ex: UpdateDynamodbContactformTable 
    5. Click **Create Collection** and give it a **Collection Name** Ex: AWS
    6. Click the **Tick Symbol**
    7. Click **Save to AWS**
2. Setup the test
    1. Change the method to **POST**
    2. Enter the **Invoke URL** we noted earlier from AWS API Gateway
    3. Select **Body** then select **raw**
    4. Enter the following json object
    ```json
    {
        "FIRST_NAME":"jack",
        "LAST_NAME":"johnson",
        "LOCATION":"ampang"
    }
    ```
    5. Change **Text** to **JSON**
    6. Click **Save** at the top right
3. Check the DynamoDB table CONTACT_FORM has been update

### 6 - Setup a frontend web contact form
1. Provision a Apache webserver on Centos
2. In the /var/www/html create two files
    1. index.html with the contents as in [here](https://github.com/hadriane/aws_apigateway_lambda_dynamodb_contactform/edit/master/index.html)
    2. thankyou.html with the content as in [here](https://github.com/hadriane/aws_apigateway_lambda_dynamodb_contactform/blob/master/thankyou.html)

### 7 - Test the frontend web contact form
1. Go to http://www.yourdomain.com/index.html
2. Enter **First Name**
3. Enter **Last Name**
4. Pick the **Location** from the drop-down
5. Click **Submit**
6. Check the DynamoDB table CONTACT_FORM has been update 
