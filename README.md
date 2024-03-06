# Data-Analytics-Pipeline-using-Amazon-Data-Migration-Service

AWS launched the AWS Data Pipeline service in 2012. At that time, customers were looking for a service to help them reliably move data between different data sources using a variety of compute options. Now, there are other services that offer customers a better experience. For example, you can use AWS Glue to to run and orchestrate Apache Spark applications, AWS Step Functions to help orchestrate AWS service components, or Amazon Managed Workflows for Apache Airflow (Amazon MWAA) to help manage workflow orchestration for Apache Airflow.

This topic explains how to migrate from AWS Data Pipeline to alternative options. The option you choose depends on your current workload on AWS Data Pipeline. You can migrate typical use cases of AWS Data Pipeline to either AWS Glue, AWS Step Functions, or Amazon MWAA.
Migrating workloads to AWS Glue

AWS Glue

is a serverless data integration service that makes it easy for analytics users to discover, prepare, move, and integrate data from multiple sources. It includes tooling for authoring, running jobs, and orchestrating workflows. With AWS Glue, you can discover and connect to more than 70 diverse data sources and manage your data in a centralized data catalog. You can visually create, run, and monitor extract, transform, and load (ETL) pipelines to load data into your data lakes. Also, you can immediately search and query cataloged data using Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum.

We recommend migrating your AWS Data Pipeline workload to AWS Glue when:

    You're looking for a serverless data integration service that supports various data sources, authoring interfaces including visual editors and notebooks, and advanced data management capabilities such as data quality and sensitive data detection.

    Your workload can be migrated to AWS Glue workflows, jobs (in Python or Apache Spark) and crawlers (for example, your existing pipeline is built on top of Apache Spark).

    You require a single platform that can handle all aspects of your data pipeline, including ingestion, processing, transfer, integrity testing, and quality checks.

    Your existing pipeline was created from a pre-defined template on the AWS Data Pipeline console, such as exporting a DynamoDB table to Amazon S3, and you are looking for the same purpose template.

    Your workload does not depend on a specific Hadoop ecosystem application like Apache Hive.

    Your workload does not require orchestrating on-premises servers.

AWS charges an hourly rate, billed by the second, for crawlers (discovering data) and ETL jobs (processing and loading data). AWS Glue Studio is a built-in orchestration engine for AWS Glue resources, and is offered at no additional cost. Learn more about pricing in AWS Glue Pricing

.
Migrating workloads to AWS Step Functions

AWS Step Functions

is a serverless orchestration service that lets you build workflows for your business-critical applications. With Step Functions you use a visual editor to build workflows and integrate directly with over 11,000 actions for over 250 AWS services, such as AWS Lambda, Amazon EMR, DynamoDB and more. You can use Step Functions for orchestrating data processing pipelines, handling errors, and working with the throttling limits on the underlying AWS services. You can create workflows that process and publish machine learning models, orchestrate micro-services, as well as control AWS services, such as AWS Glue, to create extract, transform, and load (ETL) workflows. You also can create long-running, automated workflows for applications that require human interaction.

Similarly to AWS Data Pipeline, AWS Step Functions is a fully managed service provided by AWS. You will not be required to manage infrastructure, patch workers, manage OS version updates or similar.

We recommend migrating your AWS Data Pipeline workload to AWS Step Functions when:

    You're looking for a serverless, highly available workflow orchestration service.

    You're looking for a cost-effective solution that charges at a granularity of a single task execution.

    Your workloads are orchestrating tasks for multiple other AWS services, such as Amazon EMR, Lambda, AWS Glue, or DynamoDB.

    You're looking for a low-code solution that comes with a drag-and-drop visual designer for workflow creation and does not require learning new programming concepts.

    You're looking for a service that provides integrations with over 250 other AWS services covering over 11,000 actions out-of-the-box, as well as allowing integrations with custom non-AWS services and activities.

Both AWS Data Pipeline and Step Functions use JSON format to define workflows. This allows to store your workflows in source control, manage versions, control access, and automate with CI/CD. Step Functions are using a syntax called Amazon State Language which is fully based on JSON, and allows a seamless transition between the textual and visual representations of the workflow.

With Step Functions, you can choose the same version of Amazon EMR that you're currently using in AWS Data Pipeline.

For migrating activities on AWS Data Pipeline managed resources, you can use AWS SDK service integration on Step Functions to automate resource provisioning and cleaning up.

For migrating activities on on-premises servers, user-managed EC2 instances, or a user-managed EMR cluster, you can install an SSM agent to the instance. You can initiate the command through the AWS Systems Manager Run Command from Step Functions. You can also initiate the state machine from the schedule defined in Amazon EventBridge

.

AWS Step Functions has two types of workflows: Standard Workflows and Express Workflows. For Standard Workflows, you’re charged based on the number of state transitions required to run your application. For Express Workflows, you’re charged based on the number of requests for your workflow and its duration. Learn more about pricing in AWS Step Functions Pricing

.
Migrating workloads to Amazon MWAA

Amazon MWAA
(Managed Workflows for Apache Airflow) is a managed orchestration service for Apache Airflow

that makes it easier to set up and operate end-to-end data pipelines in the cloud at scale. Apache Airflow is an open-source tool used to programmatically author, schedule, and monitor sequences of processes and tasks referred to as "workflows". With Amazon MWAA, you can use Airflow and Python programming language to create workflows without having to manage the underlying infrastructure for scalability, availability, and security. Amazon MWAA automatically scales its workflow execution capacity to meet your needs, and is integrated with AWS security services to help provide you with fast and secure access to your data.

Similarly to AWS Data Pipeline, Amazon MWAA is fully managed services provided by AWS. While you need to learn several new concepts specific to these services, you are not required to manage infrastructure, patch workers, manage OS version updates or similar.

We recommend migrating your AWS Data Pipeline workloads to Amazon MWAA when:

    You're looking for a managed, highly available service to orchestrate workflows written in Python.

    You want to transition to a fully-managed, widely-adopted open-source technology, Apache Airflow, for maximum portability.

    You require a single platform that can handle all aspects of your data pipeline, including ingestion, processing, transfer, integrity testing, and quality checks.

    You're looking for a service designed for data pipeline orchestration with features such as rich UI for observability, restarts for failed workflows, backfills, and retries for tasks.

    You're looking for a service that comes with more than 800 pre-built operators and sensors, covering AWS as well as non-AWS services.

Amazon MWAA workflows are defined as Directed Acyclic Graphs (DAGs) using Python, so you can also treat them as source code. Airflow's extensible Python framework enables you to build workflows connecting with virtually any technology. It comes with a rich user interface for viewing and monitoring workflows and can be easily integrated with version control systems to automate the CI/CD process.

With Amazon MWAA, you can choose the same version of Amazon EMR that you’re currently using in AWS Data Pipeline.

AWS charges for the time your Airflow environment runs plus any additional auto scaling to provide more worker or web server capacity. Learn more about pricing in Amazon Managed Workflows for Apache Airflow Pricing

.
Mapping the concepts

The following table contains mapping of major concepts used by the services. It will help people familiar with Data Pipeline to understand the Step Functions and MWAA terminology.
Data Pipeline 	Glue 	Step Functions 	Amazon MWAA
Pipelines 	Workflows 	Workflows 	Direct acylic graphs
Pipeline definition JSON 	Workflow definition or Python-based blueprints 	Amazon State Language JSON 	Python-based
Activities 	Jobs 	States and Tasks 	Tasks
(Operators and Sensors
)
Instances 	Job runs 	Executions 	DAG runs
Attempts 	Retry attempts 	Catchers and retriers 	Retries
Pipeline schedule 	Schedule triggers 	EventBridge Scheduler tasks 	Cron
, timetables, data-aware
Pipeline expressions and functions 	Blueprint library 	Step Functions intrinsic functions and AWS Lambda 	Extensible Python framework
Samples

The following sections lists public examples that you can refer to migrate from AWS Data Pipeline to individual services. You can refer them as examples, and build your own pipeline on the individual services by updating and testing it based on your use case.
