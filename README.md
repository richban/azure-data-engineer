# How to Become a Microsoft Certified Azure Data Engineer Associate

If you want to become a Microsoft Azure Data engineer, then you will have to pass two exams:

- [Exam DP-200: Implementing an Azure Data Solution](https://docs.microsoft.com/en-us/learn/certifications/exams/dp-200)
- [Exam DP-201: Designing an Azure Data Solution](https://docs.microsoft.com/en-us/learn/certifications/exams/dp-201)

I am not going to talk about the topics covered in these exams, but I'll give you some overview, guidelines you will need to know, and what to expect.

# Scope - Skills Measured

<details>
    <summary>**Implementing an Azure Data Solution (DP-200)**</summary>

      ### Implement data storage solutions

      **Implement non-relational data stores**

      - implement a solution that uses Cosmos DB, Data Lake Storage Gen2, or Blob storage
      - implement data distribution and partitions
      - implement a consistency model in Cosmos DB
      - provision a non-relational data store
      - provide access to data to meet security requirements
      - implement for high availability, disaster recovery, and global distribution

      **Implement relational data stores**

      - configure elastic pools
      - configure geo-replication
      - provide access to data to meet security requirements
      - implement for high availability, disaster recovery, and global distribution
      - implement data distribution and partitions for Azure Synapse Analytics
      - implement PolyBase

      **Manage data security**

      - implement data masking
      - encrypt data at rest and in motion

      ### Manage and develop data processing

      **Develop batch processing solutions**

      - develop batch processing solutions by using Data Factory and Azure Databricks
      - ingest data by using PolyBase
      - implement the integration runtime for Data Factory
      - create linked services and datasets
      - create pipelines and activities
      - create and schedule triggers
      - implement Azure Databricks clusters, notebooks, jobs, and autoscaling
      - ingest data into Azure Databricks

      **Develop streaming solutions**

      - configure input and output
      - select the appropriate windowing functions
      - implement event processing by using Stream Analytics
      - ingest and query streaming data with Azure Data Explorer

      ### Monitor and optimize data solutions

      **Monitor data storage**

      - monitor relational and non-relational data sources
      - implement Blob storage monitoring
      - implement Data Lake Storage monitoring
      - implement SQL Database monitoring
      - implement Azure Synapse Analytics monitoring
      - implement Cosmos DB monitoring
      - implement Azure Data Explorer monitoring
      - configure Azure Monitor alerts
      - implement auditing by using Azure Log Analytics

      **Monitor data processing**

      - monitor Data Factory pipelines
      - monitor Azure Databricks
      - monotiro Stream Analytics
      - configure Azure Monitor alerts
      - implement auditing by using Azure Log Analytics

      **Optimize Azure data solutions**

      - troubleshoot data partitioning bottlenecks
      - optimize Data Lake Storage
      - optimize Stream Analytics
      - optimize Azure Synapse Analytics
      - optimize SQL Database
      - manage the data lifecycle

</details>

<details>
    <summary>Designing an Azure Data Solution (DP-201)**</summary>
      ### **Design Azure data storage solutions**

      **Recommend an Azure data storage solution based on requirements**

      - choose the correct data storage solution to meet the technical and business
        requirements
      - choose the partition distribution type

      **Design non-relational cloud data stores**

      - design data distribution and partitions
      - design for scale, including multi-region, latency, and throughput
      - design a solution that uses Cosmos DB, Data Lake Storage Gen2, or Blob storage
      - select the appropriate Cosmos DB API
      - design a disaster recovery strategy
      - design for high availability

      **Design relational cloud data stores**

      - design data distribution and partitions
      - design for scale, including multi-region, latency, and throughput
      - design a solution that uses SQL Database and SQL Data Warehouse
      - design a disaster recovery strategy
      - design for high availability

      ### **Design data processing solutions**

      **Design batch processing solutions**

      - design batch processing solutions by using Data Factory and Azure Databricks
      - identify the optimal data ingestion method for a batch processing solution
      - identify where processing should take place, such as at the source, at the destination, or
        in transit

      **Design real-time processing solutions**

      - design for real-time processing by using Stream Analytics and Azure Databricks
      - design and provision compute resources

      ### **Design for data security and compliance**

      **Design security for source data access**

      - plan for secure endpoints (private/public)
      - choose the appropriate authentication mechanism, such as access keys, shared access

        signatures (SAS), and Azure Active Directory (Azure AD)

      **Design security for data policies and standards**

      - design data encryption for data at rest and in transit
      - design for data auditing and data masking
      - design for data privacy and data classification
      - design a data retention policy
      - plan an archiving strategy
      - plan to purge data based on business requirements


</details>

- Preparing for the Exams

As you can see, from the scope of the exam, it's really broad and the stack keeps growing. I recommend the following steps to fill in your knowledge:

- Official Microsoft Learning Path [https://docs.microsoft.com/en-us/learn/certifications/azure-data-engineer](https://docs.microsoft.com/en-us/learn/certifications/azure-data-engineer) - It's a great start if you're a beginner and you get a chance to get hands-on experience, however, it just barely scratches the surface.
- Pluralsight [https://www.pluralsight.com/](https://www.pluralsight.com/) offers video-based courses, and they're really great.
- Taking a practice exam - even if you're skilled with Azure data services, I recommend you to look at the exam questions.
- 40~50 hours is a reasonable preparation time.

# The Exams

- **DP-200** is all about implementation and configurations. (how would you configure the db to...). It is the most difficult since it requires to "memorize" a lot of configuration and implementation details.
- **DP-201** focuses on design, planning, and concepts. (which services you would use for a Lambda architecture...)
- Multiple choices questions - no hands-on configuration, no coding (some questions ask you to fill in a code snippet)
- Drag and drop questions (which 4 actions should you perform in sequence)
- Each exam has overall 46-49 questions, including 1-5 Use Case Studies.
- The time available for the test is 180 min. for DP-201 and 210 min. for DP-200. However, you can easily do it in less than 120 min.
- You can take the exams in the comfort of your home or office while being monitored by an offsite proctor.
- Mac users, use a mouse! The OnVue app is not friendly with the trackpad.

- **Example Question**

  ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-29_at_9.05.39_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-29_at_9.05.39_PM.png)

# Notes

Feel free to check out my notes I made during my study preparation.

[Introduction](Azure%20for%20the%20Data%20Engineer/Introduction.md)

[Store data in Azure](Azure%20for%20the%20Data%20Engineer/Store%20data%20in%20Azure.md)

[Work with Relational Data in Azure](Azure%20for%20the%20Data%20Engineer/Work%20with%20Relational%20Data%20in%20Azure.md)

[NoSQL data in Azure Cosmos DB](Azure%20for%20the%20Data%20Engineer/NoSQL%20data%20in%20Azure%20Cosmos%20DB.md)

[Large-Scale Data Processing with Azure Data Lake Storage Gen2](Azure%20for%20the%20Data%20Engineer/Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S.md)

[Implement a Data Streaming Solution with Azure Streaming Analytics](Azure%20for%20the%20Data%20Engineer/Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str.md)

[Implement a Data Warehouse with Azure Synapse Analytics](Azure%20for%20the%20Data%20Engineer/Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal.md)

# Exam Questions

Here is a list of questions that you can encounter during the DP-200 exam:

- **DP-200 exam questions**

  - **SQL Joins**

    ![Azure%20for%20the%20Data%20Engineer/1jAt5tID0Kc9B-8AGbeBivw.png](Azure%20for%20the%20Data%20Engineer/1jAt5tID0Kc9B-8AGbeBivw.png)

  - **IoT solution uses Azure IoT Hub to connect and manage IoT Devices - running in Docker. Deploy Azure Stream Analytics - minimize latency and bandwidth.**
    1. Create an Azure Blob Storage Container
    2. Create a Stream Analytics job with edge hosting.
    3. Configure the Azure Blob Storage container as save location for the job definition.
    4. Set up an IoT Edge environment on the IoT devices and add a Stream Analytics module.
    5. Configure routes in IoT Edge.
  - **Azure IoT edge**

    - Minimize latency and bandwidth usage between Stream Analytics and IoT Devices
    - Analyzes data on devices instead of in the cloud.

      ![Azure%20for%20the%20Data%20Engineer/runtime.png](Azure%20for%20the%20Data%20Engineer/runtime.png)

    - If you want to reduce bandwidth costs and avoid transferring terabytes of raw data, you can clean and aggregate the data locally then only send the insights to the cloud for analysis.
    - Azure Blob Storage required to create jobs and sync the job definition with the IoT devices.
    - Stream Analytics is running on the Azue IoT Edge directly on the devices.
    - IoT Routes will upstream events from the Stream Analytics job to the IoT Hub.
    - Azure Functions can later on proces the events from IoT Hub.
    - Streaming Units SUs are not consumed withing an IoT Edge Solution.

  - **Give existing user administrative right to a database in Azure SQL Database (T-SQL)**

    ```sql
    ALTER ROLE db_owner ADD MEMBER Sam
    ```

  - **Azure DW - optimize query performance - query returns 200 000 records. Joining on StoreId - there are 50 000 stores.**

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_4.39.53_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_4.39.53_PM.png)

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_4.42.07_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_4.42.07_PM.png)

    - Hash distribution shards data across nodes by placing all data that use the same has key on the same compute node.
    - Relicated distribution is good for reads when the table is small.
    - Outer join would return more rows than necessary.

  - **Azure Data pipeline - run-data is deleted after 45 days. How to keep run-data for > 45 days?**
    - Configure Diagnostics logs to send data to a blob storage account.
    - run-data by default only for 45 days.
  - **Azure locks**
    - Allow to control resources to prevenet unexpected changes.
  - **Azure SQL DW and Azure Data Lake Storage Gen2 - configure PolyBase to load data from Data lake to DW**

    1. Create a scoped credential with the Azure Storage account key.
    2. Create and external data source with the HADOOP type.
    3. Create and external file format.
    4. Create and external table.
    5. Load the data into the DW

  - **Azure SQL DW - top 10 longest running queries**

    ```sql
    SELECT TOP 10 *
    FROM sys.dm_pdw_exec_requests
    ORDER BY total_elapsed_time DESC
    ```

    - `sys.dm_pdw_exec_requests` shows all queries to the DW

  - **Azure Synapse Analytics SQL pool - archive oldest partition (before 2018) - archive table does not exists - add a new paritition to the Sales table**

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.06.52_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.06.52_PM.png)

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.09.38_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.09.38_PM.png)

    - Switch - data in the first paritition os the Sales table is switched to the first parition of the Sales History table.
    - MERGE RANGE removes a boundary value, thus removes a partition.
    - SPLIT RANGE create a new boundary, thus a new parition.

  - **Create and alert if SU consumption is > 80%**
    - Metric: SU % util
    - Operator Greater than
    - Agg: Max
  - **List all hash distributed tables. Include the table name and the column dame of the distribution columns.**
    - `sys.tables`
    - `sys.columns`
    - `sys.pdw_column_distribution_properties`
    - `sys.pdw_distributions` - info about the dist on the applliance
    - `sys.pdw_table_distribution_props` - dist info for tables
  - **Auto-failover group for DB recovery**

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.18.43_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.18.43_PM.png)

  - **Copy data from on-premise SQL Server to Data Lake Storage over public internet - enduser performance.**

    - ExpressRoute - creates link between on-premises datacenter and Azure

  - **Active Directory (AD) Connect on-premmises**
    - Sync user accounts between on-premises AD and Azure AD
  - **ERP system with on-premise SQL Server configured with SSIS extracts data from ERP to and on-premise SQL DW. Integrate SSIS packages with Azure Data Factory by configuring IR as a proxy for Azure-SSIS IR. (Already created Azure Blob Storage).**

    1. Create an Azure-SSIS IR in Azure Data Factory
    2. Install the self-hosted IR in the on-premises SSIS
    3. Register the self-hoster IR with auth key
    4. Create a linked service in Azure Data Factory with Azure Blob Storage - will move the on-premises data into a stagging area in Azure Blob Storage. Than the Azure SSIS IR will move the data from the staging Blob Storage to the Destination.
    5. Set-up tge self-hosted IR as a proxy for your Azure-SSIS IR.

    [https://docs.microsoft.com/en-us/azure/data-factory/tutorial-deploy-ssis-packages-azure](https://docs.microsoft.com/en-us/azure/data-factory/tutorial-deploy-ssis-packages-azure)

    [https://docs.microsoft.com/en-us/azure/data-factory/self-hosted-integration-runtime-proxy-ssis](https://docs.microsoft.com/en-us/azure/data-factory/self-hosted-integration-runtime-proxy-ssis)

  - **Monitor HDInsight Cluster**
    - Apache Ambari
  - **Azure Log Analytics**
    - allows to write queries to retrieve event data from event logs
  - **Azure Monitor - how much data was uploaded in each time frame?**

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.38.30_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-10_at_5.38.30_PM.png)

  - `sp_wait_for_database_copy_sync`
    - call in the primary database.
    - active geo-replication replicates data async
    - waits until changes are replicateed and ack by the active secondary databse
  - **IoT solution - sensory data written to a Cosmos DB. The insertion rate must be maximized. Configure the default consistency level and partition key property.**
    - Eventual Consistency - does not provide guarantee on reads but provides the highest output.
    - Partitioning Serial Number - helps to optimize reporting grouped by individual parts
  - **Stream Analytics - count the number of weather reports that are received per time zone minute. Which windows function to use?**
    - Tumbling
  - **Supported programming languages in HDInsight**
    - R, SQL, Python, Java, Scala
  - **Azure SQL Database - default firewall options. How the db can be accessed?**
    - Azure portal
    - SQL Server Management - no, default firewall rules does not allow access
    - No access from other Azure services by default
  - **Data ingestion solution from text files in Azure Data Lake Gen 1 account to Azure Data Warehouse. Should you use Databricks?**
    - No
  - **Load Data from Azure Blob storage container to Azure SQL DW. What steps to perform?**
    - create master key
    - create external file format
    - create database scoped credential
    - create external data source
    - create external table
    - create table
  - **Azure data platform end-to-end**

    [Azure data platform end-to-end - Azure Example Scenarios](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/dataplate2e/data-platform-end-to-end)

  - **Hybrid ETL with existing on-premises SSIS and Azure Data Factory**

    [Hybrid ETL with Azure Data Factory - Azure Architecture Center](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/data/hybrid-etl-with-adf)

  - **Modern Data Warehouse Architecture**

    [Modern Data Warehouse Architecture - Azure Solution Ideas](https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/modern-data-warehouse)

  - **Connect an on-premises network to Azure using ExpressRoute with VPN failover**

    [Connect an on-premises network to Azure using ExpressRoute - Azure Reference Architectures](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/expressroute-vpn-failover)

  - **Data Ingestions from csv in Azure Data Lake Storage account to Azure DW**
    - create external file format and external data source
    - create external table that uses the external data source
    - Load the data
  - **Azure Blob Storage ⇒ Azure DW Complete the T-SQL**

    ![Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-29_at_3.38.05_PM.png](Azure%20for%20the%20Data%20Engineer/Screen_Shot_2020-04-29_at_3.38.05_PM.png)

  - **You Azure Synapse Analytics DB. You need to list of all hash distributed tables. Which 3 catalog views do you need to join in a query?**
    - `sys.tables`
    - `sys.columns`
    - `sys.pdw_column_distribution_properties`
  - **Give existing user admin rights to the db**

    `ALTER ROLE db_owner ADD MEMBER Sam`

  - **Data is stored on file servers and SQL Server on-premises. You anticipate that it will take a long time to copy the data from your company to Data Lake over the public internet. Ensure optimal performance. What should you do?**
    - use `ExpressRoute`
    - creates a dedicated link between on-premise and Azure
  - **Load data from Azure Blob to Azure DW. What steps are required?**
    - Create Master Key
    - Create external file format
    - create database scoped credential
    - create external data source
    - create external table
    - create table
  - **Shared Access signatures?**
    - delegates access in a storage account with granular control over how the client access data.
    - you can define a SAS token with expiration
  - **Shared Key Auth**
    - gives full administrative access to storage accounts
    - they do not expire automatically
  - **You have to split the date partition further. What steps are required?**
    - disable the columnstore
    - use the alter table with split cause
    - rebuild the columnstore
    - partition can be split only when it is empty
  - **Monitor the execution of queries in Azure DW using DMVs. Which DMV should you use?**
    - `sys.dm_pdw_exec_requests` (pwd - parallel data warehouse)
    - `sys.dm_exec_requests` only for Sql Server and Azure Sql db
  - **Azure Monitor trigger an alert or log connection incidents. What action?**

    - IT Service Management Connector (ITSMC)

  - **Automation Runbook.**
    - run a workflow, for example to shutdown a service
  - **Load data into Azure Synapse SQL Pool from azure storage account as text files. What should you use for optimal speed?**
    - PolyBase TSQL cmd (runs in parallel)
    - Use copy activity in azure data factory
  - **Avro serialization format**
    - format the relies on schemas
  - **Watermark approach**

    [Incrementally copy a table using Azure portal - Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/tutorial-incremental-copy-portal)

# Wall of Fame

![Azure%20for%20the%20Data%20Engineer/Microsoft_Certified_Professional_Certificate_0.png](Azure%20for%20the%20Data%20Engineer/Microsoft_Certified_Professional_Certificate_0.png)
