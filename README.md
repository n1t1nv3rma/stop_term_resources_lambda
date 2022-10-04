# Steps
Use below steps to setup auto shutdown and termination of EC2 and RDS instance based upon no. of days and tag, using CW Events and Lambda.

## Create IAM Role for Lambda

Create a lambda IAM Role (ex: AMS-DemoAdmin-Shutdown-Resources-Lambda-Role ) with below policies

1. AWS Managed "ReadOnlyAccess" policy
2. Customer managed policy (ex: AMS-DemoAdmin-Shutdown-Resources-Lambda-Policy) as per the permissions in "AMS-DemoAdmin-Shutdown-Resources-Lambda-Role.json"

## Create Lambda function using Python 3.x

1. Create new function, ex: AMS-DemoAdmin-Shutdown-Resources (**Note**: If you change this name, update the CW log group name in "AMS-DemoAdmin-Shutdown-Resources-Lambda-Role.json")
  a. Use the Permission/Role as "AMS-DemoAdmin-Shutdown-Resources-Lambda-Role"

2. Once function is created, deploy the Lambda code as per file "AMS-DemoAdmin-Shutdown-Resources.py"

3. Perform "Test" run with any/default JSON input. **Note**: Update the code by changing the **Days_** variables to very high value to test w/o actual action.

4. Ensure the Timeout value for Lambda function is changed to a reasonable value (say 15 mins) from the default 3 seconds.

## Once test is successful, setup regular run

1. Follow steps [outlined here](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html) to run above created Lambda function on daily basis or desired interval, using Amazon EventBridge

