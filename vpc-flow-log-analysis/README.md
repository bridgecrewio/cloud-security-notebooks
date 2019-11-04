# VPC Flow Log Analysis
A script to collect all active elastic network interface by validating used ports
in VPC flow logs of an AWS account.
## Table of contents
* [Setup](#setup)
* [Execution Flow](#execution)
* [Contact](#contact)


## Setup
Configure aws sdk with the account to collect (`~/.aws/credentials file`).
In addition, the following environment variables needs to be set:

* ACCOUNT_ID - AWS Account to collect (required)
* ASSUME_RULE_PROFILE - Assume role ARN to use (optional)
* AWS_ROLE_SESSION_NAME - Boto3 session name (optional)
* AWS_SHARED_CREDENTIALS_FILE - AWS Credetails file path (optional, defaults to `~/.aws/credentials` )
* AWS_PROFILE - Name of the AWS profile to use, as appears in the credentials file. if not set, the default profile is used
* AWS_DEFAULT_REGION - Default AWS region to use (required)
* S3_FLOW_LOG_BUCKET - VPC flow log s3 bucket name (required)
* S3_FLOW_LOG_PATH - VPC flow logs s3 object path in the bucket (required)

Install required Python packages with pipenv:

`pipenv update`

## Execution Flow

![Alt text](assets/flow_chart.png?raw=true "Title")

## Contact
Created by [Bridgecrew](https://www.bridgecrew.io)
