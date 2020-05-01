# Gremlin (graph) API

Choosing Gremlin as the API provides a graph-based view over the data. Remember that at the lowest level, all data in any Azure Cosmos DB is stored in an ARS format. A graph-based view on the database means data is either a vertex (which is an individual item in the database), or an edge (which is a relationship between items in the database).

```sql
g.addV('product').property('productName', 'Industrial Saw').property('description', 'Cuts through anything').property('quantity', 261)
g.addV('product').property('productName', 'Belt Sander').property('description', 'Smoothes rough edges').property('quantity', 312)
g.addV('product').property('productName', 'Cordless Drill').property('description', 'Bores holes').property('quantity', 647)

g.V().hasLabel('product').has('productName', 'Industrial Saw').addE('boughtWith').to(g.V().hasLabel('product').has('productName', 'Belt Sander'))
g.V().hasLabel('product').has('productName', 'Industrial Saw').addE('boughtWith').to(g.V().hasLabel('product').has('productName', 'Cordless Drill'))
```

The marketing team wants to be able to offer additional product recommendations while customers are browsing products on your e-commerce site. For example, the team would like to provide suggestions like, "people who bought this product also bought that product", and "people who viewed this product also are viewing that product." The products that are recommended first should be your most popular products, therefore a method needs to be provided that will enable ranking the relationships between products.

## **Problem analysis**

The data store needs to be able to assign weight values to the relationships between products. For example, you might store a count of the number of times that a relationship occurs. With that in mind, each time that a person buys "Product A" and "Product B", the relationship link between these two products needs to be incremented. This relationship counter is meta-data that needs to be stored in a database.

![Gremlin%20graph%20API/graph-example.png](Gremlin%20graph%20API/graph-example.png)

A graph database is the perfect fit to model this kind of data. Gremlin, the graph query language, will support the marketing department's requirements.

[Why not any of the other APIs?](Gremlin%20graph%20API/Why%20not%20any%20of%20the%20other%20APIs.csv)