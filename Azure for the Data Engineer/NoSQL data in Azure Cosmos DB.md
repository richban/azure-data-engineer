# NoSQL data in Azure Cosmos DB

- **Request unit**
    - measures throughput using something called a request unit (RU/s).
    - A single request unit, 1 RU, is equal to the approximate cost of performing a single GET request on a 1-KB document using a document's ID.
    - Creating, replacing, or deleting the same item requires additional processing by the service, and therefore requires more request units.
- **Partition Strategy**
    - you need a partitioning strategy to scale out instead of up.
    - Scaling out is also called **horizontal scaling**, and it enables you to **add more partitions** to your database as your application needs them.
    - A partition key should aim to distribute operations across the database. You want to distribute requests to avoid hot partitions.
    - **A hot partition** is a single partition that **receives many more requests** than the others, which can create a throughput bottleneck.
    - The **more values your partition key** has, the **more scalability** you have.
    - **best partition key** for a **read-heavy** workload, review the top three to five queries you plan on using. The value **most frequently included** in the **WHERE** clause is a good candidate for the partition key.
    - For **write-heavy** workloads, you'll need to **understand** the **transactional** needs of your **workload**, because the **partition key** is the scope of **multi-document transactions**.

        ![NoSQL%20data%20in%20Azure%20Cosmos%20DB/logical-partitions.png](NoSQL%20data%20in%20Azure%20Cosmos%20DB/logical-partitions.png)

## Azure Cosmos DB

Azure Cosmos DB is a globally distributed and elastically scalable database.

![NoSQL%20data%20in%20Azure%20Cosmos%20DB/azure-cosmos-db.png](NoSQL%20data%20in%20Azure%20Cosmos%20DB/azure-cosmos-db.png)

## APIs

[**Core (SQL) API**](NoSQL%20data%20in%20Azure%20Cosmos%20DB/Core%20SQL%20API.md)

[MongoDB API](NoSQL%20data%20in%20Azure%20Cosmos%20DB/MongoDB%20API.md)

[Cassandra API](NoSQL%20data%20in%20Azure%20Cosmos%20DB/Cassandra%20API.md)

[Gremlin (graph) API](NoSQL%20data%20in%20Azure%20Cosmos%20DB/Gremlin%20graph%20API.md)

[Azure Table API](NoSQL%20data%20in%20Azure%20Cosmos%20DB/Azure%20Table%20API.md)

---

- **Create and Azure Cosmos DB account + db with Azure CLI**

    ```bash
    export NAME=cosmos$RANDOM

    az cosmosdb create \
        --name $NAME \
        --kind GlobalDocumentDB \
        --resource-group learn-68be69f9-38ea-46b0-ac39-1eef26c424f3
    ```

    ```json
    {
      "capabilities": [],
      "consistencyPolicy": {
        "defaultConsistencyLevel": "Session",
        "maxIntervalInSeconds": 5,
        "maxStalenessPrefix": 100
      },
      "databaseAccountOfferType": "Standard",
      "documentEndpoint": "https://xyz.documents.azure.com:443/",
      "enableAutomaticFailover": false,
      "enableMultipleWriteLocations": false,
      "failoverPolicies": [
        {
          "failoverPriority": 0,
          "id": "xyz-southcentralus",
          "locationName": "South Central US"
        }
      ],
      ...
    }
    ```

    ```bash
    # Create the Products database in the account using the cosmosdb database create command
    az cosmosdb sql database create \
        --account-name $NAME \
        --name "Products" \
        --resource-group learn-68be69f9-38ea-46b0-ac39-1eef26c424f3

    # Finally, create the Clothing container with the cosmosdb collection creat
    az cosmosdb sql container create \
        --account-name $NAME \
        --database-name "Products" \
        --name "Clothing" \
        --partition-key-path "/productId" \
        --throughput 1000 \
        --resource-group learn-68be69f9-38ea-46b0-ac39-1eef26c424f3
    ```

- **Stored Procedure**

    ```java
    function helloWorld() {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
    ```

- **User Defined Functions**

    ```java
    function producttax(price) {
        if (price == undefined) 
            throw 'no input';

        var amount = parseFloat(price);

        if (amount < 1000) 
            return amount * 0.1;
        else if (amount < 10000) 
            return amount * 0.2;
        else
            return amount * 0.4;
    }
    ```

## **Strengths and weaknesses of graph databases**

- **Performance**
    - with relational databases, the performance of of relationship queries decreases as the number of relationships increase.
    - which is not the case for the graph databases,  the performance stays constant even as the data and complexity continues to grow.

- **Flexibility**
    - easy to add to an existing structure without affecting functionality.
    - allows the model to dictate change, rather than being forced to adapt to a tabular way of seeing your data in a standard relational database.
- **Weaknesses**
    - not as efficient at processing high volumes of transactions
    - not effective at handling queries that traverse an entire database
    - Graph databases do not create better relationships; instead they provide rapid data retrieval for connected data. This increases the need for efficient data design, because any performance gains from graph searches can be reduced by failing to model the relationships between your nodes efficiently.

---

- **Differences between Azure Storage Tables and Azure Cosmos DB Tables**
    - You are charged for the capacity of an Azure Cosmos DB Table as soon as it is created, even if that capacity isn't used. This charging structure is because Azure Cosmos DB uses a reserved-capacity model to ensure that clients can read data within 10 ms.
    - In **Azure Storage Tables**, you are only **charged for used capacity**, but read access is only guaranteed within 10 seconds.
    - Query results from **Azure Cosmos DB** are **not** **sorted** in **order of partition key** and **row key** as they are from Storage Tables.
    - Row keys in Azure Cosmos DB are limited to 255 bytes.
    - Batch operations are limited to 2 MBs.
    - Cross-Origin Resource Sharing (**CORS**) is **not** currently **supported** by **Azure Cosmos DB**.
    - Table names are **case-sensitive** in **Azure Cosmos DB**. They are **not case-sensitive in Storage tables**.

## Optimization and Performance

**Request Units (RUs). An RU is the amount of CPU, disk I/O, and memory required to read 1 KB of data in 1 second.**

- **Total requests** made per second, and the proportion of requests rejected because the provisioned throughput is exceeded
- **Total throughput**, measured in Request Units per second (RU/s), and the distribution of throughput across partitions
- **Total storage**, measured in kilobytes (KB), and the distribution of data across partitions

## **Resources**

- An **Azure Cosmos DB Account is a container for one or more databases.**
- An Azure **Cosmos DB** database is a **container** for one or more **collections**.
- **A collection contains documents**. A document is an unstructured set of key/value pairs, read and written in JSON format.

## **Partitioning**

- **Distribution** and **grouping** of your data **across** the underlying **resources**.
- **Documents** are **grouped** in a **partition** based on the value of the **partition** **key**.
- You specify the partition key when you create the collection.
- **Example**

    ```json
    {
      "OrderTime": "4:21 PM",
      "id": "e152c6b5-2d9b-f232-6f58-de17190ecfec",
      "OrderStatus": "NEW",
      "Item": {
        "id": "52841410-7500-828d-e932-66f166e6f87f",
        "Title": "8vlyf0jyvexfv3f",
        "Category": "Books",
        "UPC": "74:8585:249",
        "Website": "https://pzr.khdftcp.com",
        "ReleaseDate": "2019-01-10T16:21:41.039088-08:00",
        "Condition": "NEW",
        "Merchant": "24/7",
        "ListPricePerItem": 13.41,
        "PurchasePrice": 12.76,
        "Currency": "USD"
      },
      "Quantity": 24,
      "PaymentInstrumentType": 1,
      "PurchaseOrderNumber": "422-40277-87",
      "Customer": {
        "id": "297b2be8-f31f-cd84-4013-5a9a13a8aae6",
        "FirstName": "Clair",
        "LastName": "Weber",
        "Email": "Clair.Weber@yahoo.com",
        "StreetAddress": "594 Stoltenberg Divide",
        "ZipCode": "93267-7740",
        "State": "VT"
      },
      "ShippingDate": "2019-01-16T16:21:41.65195-08:00",
      "Data": "2FgYt+9u0FiL4Q=="
    }
    ```

- Any of these properties, or a combination of them, can be a partition key.
- For example, where you defined the partition key as a combination of the properties **Category** and **Merchant**, any documents that have matching values for **Category** and **Merchant** are grouped in the same partition.
- An **effective** **partitioning** **strategy** **distributes** **data** and access **evenly** **across** **partitions**, and across time.
- **Querying** documents from within the **same** **partition** is **less** **expensive** than querying across partitions.
- **Partition Strategies CLI**

    ```bash
    # store its endpoint in an environment variable
    export ENDPOINT=$(az cosmosdb list --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --output tsv --query [0].documentEndpoint)

    # store the access key
    export KEY=$(az cosmosdb list-keys --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f  \
            --name $COSMOS_NAME --output tsv --query primaryMasterKey)

    # Create a database called mslearn in your Azure Cosmos DB account
    az cosmosdb database create --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --name $COSMOS_NAME --db-name mslearn

    # We're going to create three collections to compare different partitioning strategies and workloads.
    az cosmosdb collection create --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --name $COSMOS_NAME --db-name mslearn --collection Small \
            --partition-key-path /id --throughput 400

    # This collection uses an order item's product category as the partition key
    az cosmosdb collection create --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --name $COSMOS_NAME --db-name mslearn --collection HotPartition \
            --partition-key-path /Item/Category --throughput 7000

    # This collection partitions the documents by the order item's unique product identifier.
    az cosmosdb collection create --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --name $COSMOS_NAME --db-name mslearn --collection Orders \
            --partition-key-path /Item/id --throughput 7000

    # check the cosmos db name 
    export COSMOS_NAME=$(az cosmosdb list --output tsv --query [0].name)

    export ENDPOINT=$(az cosmosdb list --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --output tsv --query [0].documentEndpoint)

    export KEY=$(az cosmosdb list-keys --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f  \
            --name $COSMOS_NAME --output tsv --query primaryMasterKey)

    ```

For example, let's say your configured throughput is 500 RUs per second (RU/s), and:

- You need to write 25 documents per second.
- Each write requires 20 RUs.
- You're at capacity because the total throughput is 25 x 20 RUs = 500 RU/s.

## **Partition design considerations**

Designing a partitioning strategy requires you to understand your data and its operational workloads. 

- **Estimate the scale of your data needs**
    - What's the approximate size of your documents, or range of sizes?
    - What's the required throughput in number of reads per second and writes per second?
    - What's the volume of documents being queried?
- **Understand the workload**
    - Do you have a read-heavy or write-heavy workload, or both?
    - If it's read-heavy, what are the top five queries?
    - If it's write-heavy, do you need transactions?
- **Propose some partition key options**
    - Does the key choice have a large number of possible values or large cardinality?
    - Do the values have a consistent spread across the data?
    - Are some values accessed more than others?
    - For read-heavy workloads, can the query be within a single partition?
    - For write-heavy transactional workloads, can the transaction be within a single partition?
- **Indexing models**
    - **Consistent**: The index is updated synchronously every time a new document is written to the collection. New queries on the collection use the updated index right away. Query results are consistent with the updated documents in the collection.
    - **Lazy**: The index is updated at a lower priority. The reads and writes from the collection take a higher priority. In lazy mode, writes are cheaper because the index isn't updated immediately. When the index is fully updated depends on the demands on the collection. Query results don't include the updated documents until the index is consistent with the collection.
    - **None**: No index is created. Queries are expensive on collections that aren't indexed. If you're using your Azure Cosmos DB collection to read records directly rather than querying the collection, it's possible to avoid the overhead of indexing.
- **Indexing Example**

    ```json
    {
        "indexingMode": "consistent",
        "automatic": true,
        "includedPaths": [
            {
                "path": "/Item/id/?"
            },
            {
              "path": "/Customer/email/?"
            },
            {
              "path": "/OrderTime/?"
            },
            {
              "path": "/Merchant/?"
            },
            {
              "path": "/OrderStatus/?"
            }
        ],
        "excludedPaths": [
            {
                "path": "/"
            }
        ]
    }
    ```

- **Measure RUs for no index**

    ```bash
    # cosmos name
    export COSMOS_NAME=$(az cosmosdb list --output tsv --query [0].name)

    export ENDPOINT=$(az cosmosdb list --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f \
            --output tsv --query [0].documentEndpoint)

    export KEY=$(az cosmosdb list-keys --resource-group learn-ca7938f0-c432-4434-b0ee-dfee7445a11f  \
            --name $COSMOS_NAME --output tsv --query primaryMasterKey)

    # update collection indes to none
    az cosmosdb collection update -g learn-ca7938f0-c432-4434-b0ee-dfee7445a11f -n $COSMOS_NAME -d mslearn -c Orders --indexing-policy @IndexConfig/index-none.json

    Performed 1 Write operations @ 1 operations/s, 2.5 RU/s)

    -----------------------------------------------------------------
    Performed 1 Write operations @ 0 operations/s, 2.5 RU/s)
    Total (consumed 5 RUs in 2 seconds)
    ------------------------------------------------------------------

    Performed 1 Query operations @ 1 operations/s, 2.2 RU/s)

    -----------------------------------------------------------------
    Performed 1 Query operations @ 1 operations/s, 2.2 RU/s)
    Total (consumed 2.2 RUs in 1 seconds)
    ------------------------------------------------------------------

    ```

- **Measure RUs for a partial index**

    ```bash
    az cosmosdb collection update -g learn-ca7938f0-c432-4434-b0ee-dfee7445a11f -n $COSMOS_NAME -d mslearn -c Orders --indexing-policy @IndexConfig/index-partial.json

    -----------------------------------------------------------------
    Performed 1 Write operations @ 0 operations/s, 2.5 RU/s)
    Total (consumed 5 RUs in 2 seconds)
    ------------------------------------------------------------------

    -----------------------------------------------------------------
    Performed 1 Query operations @ 0 operations/s, 1.5 RU/s)
    Total (consumed 4.6 RUs in 3 seconds)
    ------------------------------------------------------------------
    ```

## **Compare RUs across indexing strategies**

The following table summarizes the results you've gotten in previous exercises. These results apply to read, query, and insert operations for 1 KB of data that's within a single partition.

[Indexing Strategies](NoSQL%20data%20in%20Azure%20Cosmos%20DB/Indexing%20Strategies.csv)

## Distribute Data Globally

- **Scenarios for replicating data in two or more regions:**
    1. **Delivering low-latency** data access to end users no matter where they are located around the globe
    2. Adding **regional resiliency** for business continuity and **disaster recovery** (BCDR)

### **What is multi-master support?**

Azure Cosmos DB regions operating as master regions in a multi-master configuration automatically work to converge data written to all replicas and ensure global consistency and data integrity.

- **The benefits of multi-master support are:**
    - Single-digit write **latency** – Multi-master accounts have an improved write latency of <10 ms for 99% of writes, up from <15 ms for non-multi-master accounts.
    - 99.999% read-write **availability** - The write availability multi-master accounts increases to 99.999%, up from the 99.99% for non-multi-master accounts.
    - Unlimited write **scalability** and throughput – With multi-master accounts, you can write to every region, providing unlimited write scalability and throughput to support billions of devices.
    - Built-in conflict **resolution** – Multi-master accounts have three methods for resolving conflicts to ensure global data integrity and consistency.

### Failover basics

- Automatically detect when a region is available and redirect read calls to the next available region in the preferred region list.
- If none of the regions in the preferred region list is available, calls automatically fall back to the current write region.

### **Choose a consistency level**

- **Strong** **consistency**, offers a linearizability guarantee with the reads guaranteed to return the most recent version of an item.
- **Eventual** **consistency**, which guarantees that in absence of any further writes, the replicas within the group eventually converge.
- **Session** **consistency**, which is the most popular because it guarantees monotonic reads, monotonic writes, and read your own writes (RYW) guarantees.

![NoSQL%20data%20in%20Azure%20Cosmos%20DB/five-consistency-levels.png](NoSQL%20data%20in%20Azure%20Cosmos%20DB/five-consistency-levels.png)

[Consistency levels and guarantees](NoSQL%20data%20in%20Azure%20Cosmos%20DB/Consistency%20levels%20and%20guarantees.csv)

- **Implementing NoSQL Databases in Microsoft Azure**
    - **Strong consistency**
        - Strong consistency offers a [linearizability](https://aphyr.com/posts/313-strong-consistency-models) guarantee with the reads guaranteed to return the most recent version of an item.
        - Strong consistency guarantees that a write is only visible after it is committed durably by the majority quorum of replicas. A write is either synchronously committed durably by both the primary and the quorum of secondaries, or it is aborted. A read is always acknowledged by the majority read quorum, a client can never see an uncommitted or partial write and is always guaranteed to read the latest acknowledged write.
        - You can have an Azure Cosmos account with strong consistency and multiple write regions. However, the benefits such as low write latency, high write availability that are available to multiple write regions are not applicable to Cosmos accounts configured with strong consistency, because of synchronous replication across regions. For more information, see [Consistency and Data Durability in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels-tradeoffs) article.
        - The cost of a read operation (in terms of request units consumed) with strong consistency is higher than session and eventual, but the same as bounded staleness.
    - **Bounded staleness consistency**
        - Bounded staleness consistency guarantees that the reads may lag behind writes by at most *K* versions or prefixes of an item or *t* time-interval.
        - Therefore, when choosing bounded staleness, the "staleness" can be configured in two ways: number of versions *K*of the item by which the reads lag behind the writes, and the time interval *t*.
        - Bounded staleness offers total global order except within the "staleness window." The monotonic read guarantees exist within a region both inside and outside the "staleness window."
        - Bounded staleness provides a stronger consistency guarantee than session, consistent-prefix, or eventual consistency. For globally distributed applications, we recommend you use bounded staleness for scenarios where you would like to have strong consistency but also want 99.99% availability and low latency.
        - Azure Cosmos DB accounts that are configured with bounded staleness consistency can associate any number of Azure regions with their Azure Cosmos DB account.
        - The cost of a read operation (in terms of RUs consumed) with bounded staleness is higher than session and eventual consistency, but the same as strong consistency.
    - **Session consistency**
        - Unlike the global consistency models offered by strong and bounded staleness consistency levels, session consistency is scoped to a client session.
        - Session consistency is ideal for all scenarios where a device or user session is involved since it guarantees monotonic reads, monotonic writes, and read your own writes (RYW) guarantees.
        - Session consistency provides predictable consistency for a session, and maximum read throughput while offering the lowest latency writes and reads.
        - Azure Cosmos DB accounts that are configured with session consistency can associate any number of Azure regions with their Azure Cosmos DB account.
        - The cost of a read operation (in terms of RUs consumed) with session consistency level is less than strong and bounded staleness, but more than eventual consistency.
    - **Consistent prefix consistency**
        - Consistent prefix guarantees that in absence of any further writes, the replicas within the group eventually converge.
        - Consistent prefix guarantees that reads never see out of order writes. If writes were performed in the order `A, B, C`, then a client sees either `A`, `A,B`, or `A,B,C`, but never out of order like `A,C` or `B,A,C`.
        - Azure Cosmos DB accounts that are configured with consistent prefix consistency can associate any number of Azure regions with their Azure Cosmos DB account.
    - **Eventual consistency**
        - Eventual consistency guarantees that in absence of any further writes, the replicas within the group eventually converge.
        - Eventual consistency is the weakest form of consistency where a client may get the values that are older than the ones it had seen before.
        - Eventual consistency provides the weakest read consistency but offers the lowest latency for both reads and writes.
        - Azure Cosmos DB accounts that are configured with eventual consistency can associate any number of Azure regions with their Azure Cosmos DB account.
        - The cost of a read operation (in terms of RUs consumed) with the eventual consistency level is the lowest of all the Azure Cosmos DB consistency levels.
    - **Storage Account**
        - contains all azure storage data objects (table, blob, file, queue).

    ### Cosmos DB - SQL API

    - **Containers = collections**
        - unit of scalability for throughput and storage
        - container items and the throughput are distributed across partitions
        - created based on partion keys
        - can scale elastically
    - **Migration Tools**

    ### Mongo DB API

    - **Overview**
        - NoSQL
        - protocols - Cassandra, Mongo DB, Gremlin and Table
        - multi-master replication
        - elastically scale provisioned
        - RU (memory, cpu, I/Os) - throughput abstraction
        - indexes all data fields upon ingestion
        - globally distribute data
    - **Migration MongoDB to Cosmos Mongo DB**

    ### Table API

    - **Overview**
        - turnkey global distribution
        - dedicated throughput - for dbs and containeres
        - single-digit ms latencies
        - high availability
        - automatic secondary indexing
        - Specialized entity - Table
        - Query by Partition Key and RowKey
        - Query OData Filter
    - **Migration Table Storage to Cosmos DB**

    ### Gremplin API

    - graph database

    ### Cassandra

    - wide column store

    ### Azure Data Lake Storage

    - repo for big data analytic workloads
    - file system semantics
    - Hadoop compatible - HDFS