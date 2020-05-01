# MongoDB API

Azure Cosmos DB's API for MongoDB supports the MongoDB wire protocol. This API allows existing MongoDB client SDKs, drivers, and tools to interact with the data transparently, as if they are running against an actual MongoDB database.

```jsx
db.Items.find({},{productName:1,_id:0})
```

The operations team has been using an app that uses an existing MongoDB database to process purchase orders. The database captures data throughout the order process, including failed and partial orders, fulfillment data, and shipping status. MongoDB was chosen because each supplier wanted their orders in different formats. In addition, each supplier returns different meta-data. The volume of these suppliers has increased exponentially as your company has rolled out drop shipping. The operations team wants to keep access to this data, and continue to use their current purchase order system with as few code changes and as little downtime as possible.

## **Problem analysis**

The operations team has semi-structured data that needs the flexibility to store many different supplier order formats. Based on your research, you have determined that both Core (SQL) and MongoDB are good options. However, your operations team wants to reduce downtime during their app migration, so your best bet would be to find a way to allow your operations team to continue to use the MongoDB wire protocol.

## **Recommended API: MongoDB**

To allow the operations team to continue to use their existing app that uses MongoDB queries, your best option is to use the MongoDB API. Choosing this API means that MongoDB tools like `mongodump` and `mongorestore` are available to natively move the data into Azure Cosmos DB.

[Why not any of the other APIs?](MongoDB%20API/Why%20not%20any%20of%20the%20other%20APIs.csv)