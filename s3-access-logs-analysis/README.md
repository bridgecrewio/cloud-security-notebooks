# S3 Access Logs Analysis
This script collects all the S3 access logs in the specified bucket, and checks if any of the traffic to and from the 
bucket does not use SSL.

## Table of contents
* [Architecture](#architecture)
* [Setup](#setup)

## Architecture

![Alt text](assets/arch.png?raw=true "Architecture")

## Setup

### Envrioment Variables
Configure AWS SDK with the account to collect (profile entry in AWS shared credentials file).
In addition, the following environment variables needs to be set:

* `S3_ACCESS_LOGS_BUCKET` - S3 bucket that contains all the S3 access logs (required)

### Workspace
* Install required Python packages with pipenv:

   `pipenv update`

* Launch a Jupyter session to use the notebook:

   `jupyter notebook`

### Description

The following steps are carried throughout the process:

* A matching Athena database, table and view are created and populated by the access logs from the bucket selected
* Query Athena view for un-secure traffic
* Display the un-secure traffic and the identity which initiated it, if such exist.
