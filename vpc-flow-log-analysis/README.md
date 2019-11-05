# VPC Flow Log Analysis
A script to collect all active elastic network interface by validating used ports
in VPC flow logs of an AWS account.
## Table of contents
* [Rationale](#rationale)
* [Setup](#setup)
* [Execution Flow](#execution-flow)
* [Contact](#contact)


## Rationale
An AWS account contains a VPC. Within it, EC2/ECS instances are attached with Elastic
Network Interfaces, where the network interfaces are attached to security groups.

Security groups rules dictates the allowed and forbidden traffic through network interfaces.
These rules are formulated using standard CIDR blocks.

Given an AWS account, we would like to identify used network interfaces (allowed traffic)
by classifying used and unused ports in VPC flow logs, and aggregating the number of bytes 
transferred in each network interface.

## Architecture

![Alt text](assets/arch.png?raw=true "Architecture")

## Setup

### Envrioment Variables
Configure AWS SDK with the account to collect (profile entry in AWS shared credentials file).
In addition, the following environment variables needs to be set:

* `ACCOUNT_ID`- AWS Account to collect (required)
* `ASSUME_RULE_PROFILE` - Assume role ARN to use (optional)
* `AWS_ROLE_SESSION_NAME` - Boto3 session name (optional)
* `AWS_SHARED_CREDENTIALS_FILE` - AWS Credetails file path (optional, defaults to `~/.aws/credentials` )
* `AWS_PROFILE` - Name of the AWS profile to use, as appears in the credentials file. if not set, the default profile is used
* `AWS_DEFAULT_REGION` - Default AWS region to use (required)
* `S3_FLOW_LOG_BUCKET` - VPC flow logs S3 bucket name (required)
* `S3_FLOW_LOG_PATH` - VPC flow logs S3 object path in the bucket (required)

if `ASSUME_RULE_PROFILE` environment variable is set, the profile name provided at the `AWS_PROFILE` 
environment variable is used as a source profile to assume `ASSUME_RULE_PROFILE` on the selected account. 
Otherwise, the profile configured at the `AWS_PROFILE` is used.
Make sure the source profile has full CRUD Athena permissions on the account
Make sure you assume role has the following permissions:
* Describe network interfaces of EC2 instances
* Describe VPC's security groups


### VPC Flow Logs Acquisition

Collect VPC flow logs of the account to be analysed, and persist it to an S3 bucket prior to execution.
Follow the instructions at the official AWS docs [official AWS docs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-s3.html)

In the notebook, configure Athena's database,table and view name to be created, 
and the Athena S3 output bucket name. (Found in the "Athena Configuration" cell)

### Workspace
* Install required Python packages with pipenv:

   `pipenv update`

* Launch a Jupyter session to use the notebook:

   `jupyter notebook`


## Execution Flow

![Alt text](assets/vpc_flow_diag.png?raw=true "Flow Chart")

### Description

The following steps are carried throughout the process:

* Initiate an AWS session (With a selected profile or with assume role via STS client)
* ENIs data are collected, and enriched with their security groups' configuration data
* Select only public facing ENIs
* Corresponding Athena database, table and view are created to be later populated by the flow logs
* Populate Athena table with VPC flow logs
* Query flow log Athena table for un-rejected traffic
* Identify used ENIs by identifying used ports
* Summarize results




## Contact
Created by [Bridgecrew](https://www.bridgecrew.io)
