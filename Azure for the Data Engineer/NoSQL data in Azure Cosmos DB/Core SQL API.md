# Core (SQL) API

Core (SQL) is the default API for Azure Cosmos DB, which provides you with a view of your data that resembles a traditional NoSQL document store.

```json
{
    "id": "cc410485-e177-4cbf-95e1-708f7d5e9297",
    "productName": "Industrial Saw",
    "description": "Cuts through anything",
    "supplier": "Hammer & Nail Inc",
    "quantity": 261,
    "unitCost": "$10.47",
    "retailPrice": "$29.99",
    "categories" : [
        {"name": "hammers"},
        {"name": "hand tools"}
    ]
}
```

```sql
SELECT c.productName FROM Items c
```

You've decided to look at how the new project is going to store the catalog for your customer facing e-commerce site. The sales team is likely to need support for adding new product categories quickly. The team had issues in the past as the old system that was using a relational database was too structured. Any necessary changes to add properties to products required downtime to update the table schemas, queries, and databases.

A flat, denormalized table was being used, which also lead to many columns being empty. The database needs to store products in a way that will enable filtering data based on the category where the products are located.

For example:

- A clothing product needs to be searchable by sex, size, color, and style.
- A TV needs to be searchable on LCD or OLED, screen resolution, screen ratio, and HDR support.
- A DVD needs to have a region, the languages supported, and any extras.

## **Problem analysis**

The sales team's primary requirement is to support semi-structured data. The schema needs to be flexible in order to store an ever-increasing number of product categories. The system needs to support searching and sorting across many different properties. There is a structured relational database that can be used to import data.

## **Recommended API: Core (SQL)**

The existing app uses a traditional relational database, which means that none of the other APIs are currently being used. However, Core (SQL) won't allow for any code reuse, but your team should quickly get up-to-speed with the SQL-like query language that is supported by Core (SQL).

Supporting new product categories is an important requirement for your project, and the Core (SQL) schema is flexible and requires a schemaless data store. As a result of this architecture, bringing a new product category online is as simple as adding a document for the new product. Changes to the schema or taking the database offline are not required.

[Why not any of the other APIs?](Core%20SQL%20API/Why%20not%20any%20of%20the%20other%20APIs.csv)