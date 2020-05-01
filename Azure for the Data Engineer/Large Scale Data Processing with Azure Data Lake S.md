# Large-Scale Data Processing with Azure Data Lake Storage Gen2

A data lake is a repository of data that is stored in its natural format, usually as blobs or files. Azure Data Lake Storage is a comprehensive, scalable, and cost-effective data lake solution for big data analytics built into Azure.

A benefit of Data Lake Storage Gen2 is that you can treat the data as if it's stored in a Hadoop Distributed File System.

- A modern data warehouse.
- Advanced analytics against big data.
- A real-time analytical solution.

- **Azure Blob storage vs. Azure Data Lake Storage**
    - If you want to store data without performing analysis on the data, set the Hierarchical Namespace option to Disabled to set up the storage account as an Azure Blob storage account.
    - If you are performing analytics on the data, set up the storage account as an Azure Data Lake Storage Gen2 account by setting the Hierarchical Namespace option to Enabled.

- **There are four stages for processing big data solutions that are common to all architectures:**
    - **Ingestion** - For example, for batch movement of data, Azure Data Factory may be the most appropriate technology to use. For real-time ingestion of data, Apache Kafka for HDInsight or Stream Analytics may be an appropriate technology to use.
    - **Store** - The store phase identifies where the ingested data should be placed. In this case, we're using Azure Data Lake Storage Gen2.
    - **Prep and train** - The common technologies that are used in this phase are Azure Databricks, Azure HDInsight or Azure Machine Learning Services.
    - **Model and serve** - Finally, the model and serve phase involves the technologies that will present the data to users. These can include visualization tools such as Power BI, or other data stores such as Azure SQL Data Warehouse, Azure Cosmos DB, Azure SQL Database, or Azure Analysis Services.
- **Creating a modern data warehouse**

    ![Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S/6-modern-data-warehouse.jpg](Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S/6-modern-data-warehouse.jpg)

    - The architecture uses **Azure Data Lake Storage** at the center of the solution for a modern data warehouse.
    - Integration Services is replaced by **Azure Data Factory** to ingest data into the **Data Lake** from a business application.
    - This is the source for the predictive model that is built into **Azure Databricks**.
    - **PolyBase** is used to transfer the historical data into a big data relational format that is held in **Azure SQL Data Warehouse**, which also stores the results of the trained model from Databricks.
    - **Azure Analysis Services** provides the caching capability for **SQL Data Warehouse** to service many users and to present the data through Power BI reports.
- **Advanced analytics for big data**

    ![Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S/6-advanced-analytics.jpg](Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S/6-advanced-analytics.jpg)

    - **Azure Data Factory** transfers terabytes of web logs from a web server to the **Azure Data Lake** on an hourly basis.
    - This data is provided as features to the predictive model in **Azure Databricks**, which is then trained and scored.
    - The results are distributed globally by using **Azure Cosmos DB**, which the real-time app (the AdventureWorks website) will use to provide recommendations to customers as they add products to their online baskets.
    - **PolyBase** is used against the **Data Lake** to transfer descriptive data to the SQL Data Warehouse for reporting purposes.
    - **Azure Analysis Services** provides the caching capability for **SQL Data Warehouse** to service many users and to display the data through **Power BI** reports.
- **Real-time analytical solutions**

    ![Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S/6-real-time-analytics.jpg](Large%20Scale%20Data%20Processing%20with%20Azure%20Data%20Lake%20S/6-real-time-analytics.jpg)

    - **Azure Data Factory** ingests the summary files that are generated when the HGV engine is turned off.
    - **Apache Kafka** provides the real-time ingestion engine for the telemetry data.
    - Both data streams are stored in **Azure Data Lake Store** for use in the future, but they are also passed on to other technologies to meet business needs.
    - Both streaming and batch data are provided to the predictive model in **Azure Databricks,** and the results are published to **Azure Cosmos DB** to be used by the third-party garages.
    - **PolyBase** transfers data from the **Data Lake Store** into **SQL Data Warehouse** where **Azure Analysis Services** creates the HGV reports by using **Power BI.**