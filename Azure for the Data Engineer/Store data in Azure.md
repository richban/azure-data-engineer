# Store data in Azure

## Data Storage Approach in Azure

- **Classify your data**
    - Structured data  - db
    - semi-structured data - json, xml, csv
    - unstructured data - video, png, pdf
- **Operational needs**
    - Will you be doing simple lookups using an ID?
    - Do you need to query the database for one or more fields?
    - How many create, update, and delete operations do you expect?
    - Do you need to run complex analytical queries?
    - How quickly do these operations need to complete?
- **Group multiple operations in a transaction**
    - **Atomicity** means a transaction must execute exactly once and must be atomic; either all of the work is done, or none of it is. Operations within a transaction usually share a common intent and are interdependent.
    - **Consistency** ensures that the data is consistent both before and after the transaction.
    - **Isolation** ensures that one transaction is not impacted by another transaction.
    - **Durability** means that the changes made due to the transaction are permanently saved in the system. Committed data is saved by the system so that even in the event of a failure and system restart, the data is available in its correct state.
    - OLTP vs. OLAP
- **Product catalog data**
    - Azure Cosmos DB
    - Data classification: Semi-structured because of the need to extend or modify the schema for new products
    - Latency & throughput: High throughput and low latency
- **Photos and videos**
    - Azure Blob storage
    - Data classification: Unstructured
    - Latency & throughput: Retrievals by ID need to support low latency and high throughput. Creates and updates can have higher latency than read operations
- **Business Data**
    - Azure SQL Database
    - Operations: Read-only, complex analytical queries across multiple databases
    - Latency & throughput: Some latency in the results is expected based on the complex nature of the queries.
    - Transactional support: Required

## Azure Storage Account

- **Create an Account**

## Connect an app to Azure Storage

- Azure data services
    - **Blobs**: A massively scalable object store for text and binary data. Can include support for Azure Data Lake Storage Gen2.
    - **Files**: Managed file shares for cloud or on-premises deployments.
    - **Queues**: A messaging store for reliable messaging between application components.
    - **Table Storage**: A NoSQL store for schema-less storage of structured data. Table Storage is not covered in this module.

### Secure Azure Storage Account

### Store application data with Azure Blob storage

- **Azure Table Storage**
    - store large amount of data
    - is a NoSQL store
    - ideal for storing structured, non-relational data
    - good for web scale apps
    - not good for complex joins, foreign keys or stored procedures
    - quick query data
    - **Concepts**
        - URL
        - Account
        - Table - collection of entities
        - Entity - a set of props - like row or object
        - Props
    - **Props**
        - partition key - dev is responsible for inserting updating and updating
        - row key - same as above
        - timestamp - automatic
    - **Limitations**
        - entity max 1MB

- **Blob**
    - use for unstructured data - docs, pngs etc.
- **Queue**
    - storing messages
    - process async
- **Files**
    - file sharing
    - replace on-premises file servers
    - lift and shift scenarios
- **Storage Security**
    - Management security (roles)
    - data access security (storage account key, sas - shared access signatures)
    - encryption for data in transit (https,
    - encryption for data at rest
    - audit/monitor access
    - CORS for browser clients
- **Azure Cosmos DB**
    - globally distributed - regions (US, West) - replication
    - multi-homing - is aware of the nearest region and can send request to that region
    - Time-to-live (**TTL**) -  expiration
    - **Data Consistency** - global replication - adjustable
        - eventual
        - strong  - slow
        - 5 lvls - Strong, Bounded Staleness, Session, Consistent Prefix, Eventual

            ![Store%20data%20in%20Azure/Screen_Shot_2020-03-24_at_3.46.35_PM.png](Store%20data%20in%20Azure/Screen_Shot_2020-03-24_at_3.46.35_PM.png)

    - **Partitioning**
        - Data partinionig
            - partition key that has a wide range of values - evenly distributed
            - 10 GB of storage