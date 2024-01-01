# AWS-Big-Data-Pipeline-orchestrated-with-Airflow
A robust data pipeline leveraging Amazon EMR and PySpark, orchestrated seamlessly with Apache Airflow for efficient batch processing

## Architecture Diagram
![image](https://github.com/jashshah-dev/AWS-Big-Data-Pipeline-orchestrated-with-Airflow/assets/132673402/7d79d182-d653-452a-a6f2-0ecd5698ae72)

## Code Explanation

 # Dataset to S3 with AWS CLI

## Overview

This simple script downloads the Iris dataset from the UCI Machine Learning Repository using the `wget` command and uploads it to an Amazon S3 bucket using the AWS Command Line Interface (AWS CLI). The dataset is retrieved in CSV format and stored as `hello_world.csv` in the specified S3 bucket.

## How to Use

### Prerequisites

1. **AWS CLI Installation:**
   - Ensure that the AWS CLI is installed on your local machine. If not, you can download it from [here](https://aws.amazon.com/cli/).

2. **AWS Credentials:**
   - Make sure that you have AWS credentials configured on your machine. You can set up your AWS credentials using the `aws configure` command.

### Running the Script

1. This will be ran in first step of Airflow as an EMR step

##  Airflow DAG for EMR Automation

## Overview

This Airflow DAG automates the process of creating an EMR (Elastic MapReduce) cluster on AWS, running Spark jobs for data ingestion and transformation, and terminating the cluster upon completion. The DAG is designed to perform ETL (Extract, Transform, Load) operations on data using Spark on an EMR cluster.

## Prerequisites

1. **AWS Credentials:**
   - Ensure that you have AWS credentials configured on your local machine or the environment where Airflow is running.

2. **AWS CLI Installation:**
   - Make sure that the AWS CLI is installed on your Airflow environment. If not, you can download it from [here](https://aws.amazon.com/cli/).

3. **Boto3 Installation:**
   - Install the Boto3 library using `pip install boto3`.

4. **Airflow DAG Configuration:**
   - Configure the `boto3` client with your AWS credentials and desired region.

## DAG Structure

The DAG consists of the following steps:

1. **Create EMR Cluster (`create_emr_cluster`):**
   - Creates an EMR cluster with specified configurations.
   - The cluster details, including the `JobFlowId`, are stored in XCom for later use.

2. **Ingest Layer (`ingest_layer`):**
   - Adds a Spark job step to the created EMR cluster for data ingestion.
   - The Spark job runs the `ingest.py` script stored in the S3 bucket.

3. **Poll Step (`poll_step_layer`):**
   - Waits for the completion of the data ingestion step before proceeding to the next step.
   - Polls the EMR step status at regular intervals.

4. **Transform Layer (`transform_layer`):**
   - Adds a Spark job step to the EMR cluster for data transformation.
   - The Spark job runs the `transform.py` script stored in the S3 bucket.

5. **Poll Step 2 (`poll_step_layer2`):**
   - Waits for the completion of the data transformation step.
   - Polls the EMR step status at regular intervals.

6. **Terminate EMR Cluster (`terminate_emr_cluster`):**
   - Terminates the EMR cluster to save costs once the ETL process is complete.

## How to Use

1. **Configure AWS Credentials:**
   - Ensure that your AWS CLI is configured with the necessary credentials.

2. **Update DAG Configuration:**
   - Modify the `create_emr_cluster` and `terminate_emr_cluster` tasks to suit your EMR cluster requirements.

3. **Upload Scripts to S3:**
   - Upload the `ingest.py` and `transform.py` scripts to the S3 bucket specified in the DAG.

4. **Run the DAG:**
   - Trigger the DAG in the Airflow UI or use the Airflow CLI command to start the DAG run.

5. **Monitor Progress:**
   - Check the Airflow UI for the progress of each task. Logs and details are available in the UI.

6. **Verify Results:**
   - Verify the data ingestion and transformation steps by inspecting the EMR cluster logs and output.

## Notes

- Ensure that the S3 paths, script names, and EMR configurations in the DAG match your setup.



