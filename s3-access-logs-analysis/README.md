# S3 Access Logs Analysis
This script collects all the S3 access logs in the specified bucket, and checks if any of the traffic to and from the 
bucket does not use SSL.

## Table of contents
* [Architecture](#architecture)
* [Setup](#setup)

## Architecture

![Architecture](assets/arch.png?raw=true "Architecture")

## Setup

### Envrioment Variables
Configure AWS SDK with the account to collect (profile entry in AWS shared credentials file).
In addition, the following environment variables needs to be set:

* `S3_ACCESS_LOGS_BUCKET` - S3 bucket that contains all the S3 access logs (*required*)
* `OUTPUT_BUCKET_NAME` - S3 bucket where the query result will be saved to (*required*)
* `ATHENA_DATABASE_NAME` - The name of the Athena DB to be created. Default is `s3_access_logs`
* `ATHENA_TABLE_NAME` - The name of the Athena Table to be created. Default is `s3_access_logs_table`
* `ATHENA_VIEW_NAME` - The name of the Athena Table to be created. Default is `s3_access_logs_view`

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
