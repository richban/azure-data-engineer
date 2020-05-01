# Azure Table API

The project architect has asked you to determine the advantages of moving data from an existing Azure Table Storage data set. The project architect found an existing 10 TB database that one of the project teams uses to store data from a legacy Internet of Things (IoT) application, and the data is seldom updated. Moving your database from Azure Table Storage into Azure Cosmos DB with a low throughput could have considerable cost savings, since Table Storage is charged on the size of data rather than how often it is accessed.

## **Problem analysis**

The IoT data consists of key-value pairs with no relationship information, and the existing dataset is currently deployed in Azure Table Storage.

## **Recommended API: Azure Table**

The best practice is to use Core (SQL) for new projects, as it has more features than the Azure Table API. However, to reduce downtime during your migration to Azure Cosmos DB, you might want to consider using the Table API for now, and switch to Core (SQL) sometime in the near future.

[Why not any of the other APIs?](Azure%20Table%20API/Why%20not%20any%20of%20the%20other%20APIs.csv)