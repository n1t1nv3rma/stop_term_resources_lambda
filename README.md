# Steps
Use below steps to setup an automated shutdown and/or termination of the unwanted AWS EC2 and Amazon RDS instances based upon no. of **days** and **tag**, using Amazon EventBridge and AWS Lambda. This can be really helpful in non-prod environments (like Demo/Test) to reduce ongoing operational cost drastically. 

# Option-1:
## Use AWS CloudFormation
Simply create all resources (IAM roles/policies, Lambda function and run schedule using Amazon EventBridge) using the provided CFN template: *[Demo1-Shutdown-Resources-Lambda.yml](https://github.com/n1t1nv3rma/stop_term_resources_lambda/blob/main/Demo1-Shutdown-Resources-Lambda.yml)*

# Option-2: 
Create all resources manually from AWS console, as below:

## Create IAM Role for Lambda

Create a lambda IAM Role with below policies and setup the role Trust with AWS Lambda service. Call it for ex: "AMS-DemoAdmin-Shutdown-Resources-Lambda-Role"

1. AWS Managed standard IAM policy : *ReadOnlyAccess*
2. Customer managed IAM policy: Using the permissions given in the *AMS-DemoAdmin-Shutdown-Resources-Lambda-Role.json* file. Call it for ex: "AMS-DemoAdmin-Shutdown-Resources-Lambda-Policy"

## Create Lambda function using Python 3.x

1. Create new function, ex: AMS-DemoAdmin-Shutdown-Resources (**Note**: If you have changed the name of this function, update the CW log group name mentioned in *AMS-DemoAdmin-Shutdown-Resources-Lambda-Role.json* IAM policy accordingly). Use the Permission/Role as "AMS-DemoAdmin-Shutdown-Resources-Lambda-Role".
2. Once function is created, deploy the Lambda code as per file *AMS-DemoAdmin-Shutdown-Resources.py*

## Setup regular run
1. Follow steps [outlined here](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html) to run above created Lambda function on daily basis or desired interval, using Amazon EventBridge

# Testing
1. Perform "Test" run with any/default JSON input. **Note**: Update the code by changing the **Days_** variables to a very high value (ex: 99999) to test w/o actual action.
2. Ensure the Timeout value for Lambda function is changed to a reasonable value (say 15 mins) from the default 3 seconds.
