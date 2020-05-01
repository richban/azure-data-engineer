# Cassandra API

Azure Cosmos DB's support for the Cassandra API makes it possible to query data by using the Cassandra Query Language (CQL), and your data will appear to be a partitioned row store.

```sql
CREATE TABLE Catalog.Items(id text, productName text, description text, supplier text, quantity int, unitCost float, retailPrice float, categories map<text,text>, primary key (id));
```

Your web analytics team is using a third-party web analytics application that uses a Cassandra database, and the team has experience writing Cassandra Query Language (CQL) queries to produce their own reports.

## **Problem analysis**

Your web analytics application is based on Cassandra, and your web analytics team has valuable experience with CQL.

## **Recommended API: Cassandra**

Based on the existing design of your third-party web analytics application, and the subject expertise that your web analytics team already has with CQL, your easiest path for migration would be to continue to use the Cassandra API for the immediate future.

[Why not any of the other APIs?](Cassandra%20API/Why%20not%20any%20of%20the%20other%20APIs.csv)

---