# Implement a Data Warehouse with Azure Synapse Analytics

**Azure Synapse Analytics** provides a unified environment by combining the enterprise data warehouse of SQL, the Big Data analytics capabilities of Spark, and data integration technologies to ease the movement of data between both, and from external data sources. Using Azure Synapse Analytics, you are able to ingest, prepare, manage, and serve data for immediate BI and machine learning needs more easily.

![Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/3-sql-dw-architecture.png](Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/3-sql-dw-architecture.png)

## **Workload Management**

Azure Synapse Analytics provides the capability to prioritize the query workloads that take place on the server using Workload Management. This is managed by three related areas:

### **Workload Groups**

Workload Groups enable you to define the resources to isolate and reserve resources for its use. It brings about the following benefits:

- Reserve resources for a group of requests
- Limit the amount of resources a group of requests can consume
- Shared resources accessed based on importance level
- Set Query timeout value. Get DBAs out of the business of killing runaway queries

A workload group is defined using T-SQL as follows:

```sql
CREATE WORKLOAD GROUP group_name  
WITH  
(       
    MIN_PERCENTAGE_RESOURCE = value   
  , CAP_PERCENTAGE_RESOURCE = value 
  , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]  
  [ [ , ] IMPORTANCE = {LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH} ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ]  
 )[ ; ]
```

### **Workload Classification**

Using T-SQL, you can create a workload classifier to map queries to a specific classifier. A classifier can define the level of importance of the request, so that it can be mapped to a specific workload group that has an allocation of specific resources during the execution of the query.

```sql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
(
         WORKLOAD_GROUP = 'name'
     ,   MEMBERNAME     = 'security_account' 
 [ [ , ] IMPORTANCE     = {LOW|BELOW_NORMAL|NORMAL|ABOVE_NORMAL|HIGH} ] )
 [ [ , ] WLM_LABEL      = 'label' ]  
 [ [ , ] WLM_CONTEXT    = 'name' ]  
 [ [ , ] START_TIME     = 'start_time' ]  
 [ [ , ] END_TIME       = 'end_time' ]  
)[ ; ]
```

```
WORKLOAD_GROUP: maps to an existing resource class
  IMPORTANCE: specifies relative importance of request
  MEMBERNAME: database user, role, AAD login or AAD group
```

### **Workload Importance**

Workload importance is defined in the CREATE WORKLOAD CLASSIFIER command and allows higher priority queries to receive resources ahead of lower priority queries that are in the queue. By default, queries are released from the queue on a first-in, first-out basis as resources become available, but workload importance overrides this.

## **Result-set Cache**

In scenarios where the same results are requested on a regular basis, result-set caching can be used to improve the performance of the queries that retrieve these results. When result-set caching is enabled, the results of the query are cached in the SQL pool storage. This enables interactive response times for repetitive queries against tables with infrequent data changes. The result-set cache persists even if SQL pool is paused and resumed later. Although the query cache is invalidated and refreshed when the underlying table data or query code changes. To ensure that the cache is fresh, the result cache is evicted on a regular basis on a time-aware least recently used algorithm (TLRU). You can set result-set caching on at the database level or at a session level using the following code:

```sql
-- Turn on/off result-set caching for a database
-- Must be run on the MASTER database
ALTER DATABASE {database_name}  
SET RESULT_SET_CACHING { ON | OFF }

-- Turn on/off result-set caching for a client session  
-- Run on target Azure Synapse Analytics
SET RESULT_SET_CACHING {ON | OFF}
```

![Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/4-mpp-engine.png](Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/4-mpp-engine.png)

Azure Synapse Analytics supports these sharding patterns:

- Hash
- Round Robin
- Replicate

You can use the following strategies to determine which pattern is most suitable for your scenario:

[Sharding scenarios](Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/Sharding%20scenarios.csv)

Here are some tips that can help you choose a strategy:

- Start with Round Robin, but aspire to a Hash distribution strategy to take advantage of a massively parallel architecture.
- Make sure that common hash keys have the same data format.
- Do not distribute on *varchar* format.
- Dimension tables with a common hash key to a fact table with many join operations can be hash distributed.
- Use *sys.dm_pdw_request_steps* to analyze data movements behind queries, monitor the time broadcast and shuffle operations take. This is helpful to review your distribution strategy.

## **Hash-distributed tables**

A hash distributed table can deliver the highest query performance for joins and aggregations on large tables.

```sql
CREATE TABLE [dbo].[EquityTimeSeriesData](
    [Date] [varchar](30) ,
    [BookId] [decimal](38, 0) ,
    [P&L] [decimal](31, 7) ,
    [VaRLower] [decimal](31, 7) 
) 
WITH
(
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([P&L])
)
;
```

## **Round-robin distributed tables**

A round-robin table is the most straightforward table to create and delivers fast performance when used as a staging table for loads.

A round-robin distributed table distributes data evenly across the nodes but without any further optimization. A distribution is first chosen at random, and then buffers of rows are assigned to distributions sequentially. It is quick to load data into a round-robin table, but query performance can often be better with hash distributed tables. Joins on round-robin tables require reshuffling data, and this takes additional time.

```sql
CREATE TABLE [dbo].[Dates](
    [Date] [datetime2](3) ,
    [DateKey] [decimal](38, 0) ,
    ..
    ..
    [WeekDay] [nvarchar](100) ,
    [Day Of Month] [decimal](38, 0) 
)

WITH
(
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = ROUND_ROBIN
)
;
```

## **Replicated Tables**

A replicated table provides the fastest query performance for small tables.

A table that is replicated caches a full copy on each compute node. Consequently, replicating a table removes the need to transfer data among compute nodes before a join or aggregation. Replicated tables are best utilized with small tables. Extra storage is required, and there are additional overheads that are incurred when writing data which make large tables impractical.

```sql
CREATE TABLE [dbo].[BusinessHierarchies](
    [BookId] [nvarchar](250)  ,
    [Division] [nvarchar](100) ,
    [Cluster] [nvarchar](100) ,
    [Desk] [nvarchar](100) ,
    [Book] [nvarchar](100) ,
    [Volcker] [nvarchar](100) ,
    [Region] [nvarchar](100) 
) 
WITH
(
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = REPLICATE
)
;
```

**Azure Synapse Analytics** supports many loading methods. These methods include **non-PolyBase** options such as **BCP** and the **SQL Bulk Copy API**. The fastest and most scalable way to load data is through **PolyBase**. **PolyBase** is a technology that accesses external data stored in **Azure** **Blob** storage, **Hadoop**, or **Azure** **Data Lake** **Store** via the **Transact-SQL** language.

![Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/2-load-azure-dw-via-polybase.png](Implement%20a%20Data%20Warehouse%20with%20Azure%20Synapse%20Anal/2-load-azure-dw-via-polybase.png)

## **Create an import database**

The first step in using PolyBase is to create a database-scoped credential that secures the credentials to the blob storage. Create a master key first, and then use this key to encrypt the database-scoped credential named **AzureStorageCredential**.

```sql
CREATE MASTER KEY;

CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'DemoDwStorage',
    SECRET = 'THE-VALUE-OF-THE-ACCESS-KEY' -- put key1's value here
;
```

## **Create an external data source connection**

Use the database-scoped credential to create an external data source named **AzureStorage**. Note the location URL point to the container named **data-files** that you created in the blob storage. The type **Hadoop** is used for both Hadoop-based and Azure Blob storage-based access.

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://data-files@demodwstorage.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

## **Define the import file format**

Define the external file format named **TextFile**. This name indicates to PolyBase that the format of the text file is **DelimitedText** and the field terminator is a comma.

```sql
CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);
```

## **Create a temporary table**

Create an external table named `dbo.temp` with the column definition for your table.

```sql
-- Create a temp table to hold the imported data
CREATE EXTERNAL TABLE dbo.Temp (
    [Date] datetime2(3) NULL,
    [DateKey] decimal(38, 0) NULL,
    [MonthKey] decimal(38, 0) NULL,
    [Month] nvarchar(100) NULL,
    [Quarter] nvarchar(100) NULL,
    [Year] decimal(38, 0) NULL,
    [Year-Quarter] nvarchar(100) NULL,
    [Year-Month] nvarchar(100) NULL,
    [Year-MonthKey] nvarchar(100) NULL,
    [WeekDayKey] decimal(38, 0) NULL,
    [WeekDay] nvarchar(100) NULL,
    [Day Of Month] decimal(38, 0) NULL
)
WITH (
    LOCATION='/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);
```

## **Create a destination table**

Create a physical table in the SQL Data Warehouse database. In the following example, you create a table named `dbo.StageDate`.

```sql
-- Load the data from Azure Blob storage to SQL Data Warehouse
CREATE TABLE [dbo].[StageDate]
WITH (   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[Temp];
```

## **Add statistics onto columns to improve query performance**

As an optional step, create statistics on columns that feature in queries to improve the query performance against the table.

```sql
-- Create statistics on the new data
CREATE STATISTICS [DateKey] on [StageDate] ([DateKey]);
CREATE STATISTICS [Quarter] on [StageDate] ([Quarter]);
CREATE STATISTICS [Month] on [StageDate] ([Month]);
```

## Table Partitions

- **Clustered Columnstore**
    - Update-able primary storage method
    - great for read only
- **Clustered Index**
    - An index that is physically stored in the same order as the data being indexed
- **Heap**
    - Data is not in any particular order
    - use when data has no natural order

## Sizing Partitions

- to many partitions hurt performance
- 10 or a few 100 partitions
- clustered columnstore - consider how many rows belong to each partition
- Before partition already divides each table into 60 distributed databases