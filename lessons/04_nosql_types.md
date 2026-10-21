# No-SQL Database Types (Key-Value and Column-Family Focus)

In this lesson, we will dive deeper into two key types of No-SQL databases: **key-value stores** and **column-family stores**. We’ll explore how they work, demonstrate their use with sample data, and provide guidance on selecting the right No-SQL database for specific use cases. At the end we will also have a short look at the other two families: **document stores** and **graph databases**.

---

## Key-Value Stores: In-Depth Exploration

Key-value databases are the simplest form of No-SQL databases, storing data as a collection of key-value pairs. Each key is unique, and it’s used to quickly retrieve the associated value.

### Characteristics:
- **Simple Structure**: A straightforward, fast retrieval mechanism.
- **Scalable**: Easily scales horizontally, allowing for the distribution of data across nodes.
- **Use Cases**: Ideal for caching, session management, and real-time analytics.

### Practical Demonstration (Key-Value Example using Redis):
#### Sample Data:
```
Key: user123
Value: { "name": "John Doe", "age": 35, "email": "john@example.com" }
```
- You can store this data in Redis and retrieve it by querying the key **user123**.

```
# Storing data in Redis (the whole JSON is one string, so we wrap it in single quotes)
SET user:123 '{"name": "John Doe", "age": 35, "email": "john@example.com"}'

# Retrieving data in Redis
GET user:123
```

With `SET`, Redis does not know that the value is JSON: it is just a string. If you want to read or update a single field (for example only the email), a **hash** is a better choice:

```
# Store the same user as a hash: one key, many fields
HSET user:123 name "John Doe" age 35 email "john@example.com"

# Read one field or all of them
HGET user:123 email
HGETALL user:123
```

> Keys like `user:123` follow a common Redis convention: `object-type:id`. Redis does not care about the colon, but it makes keys easier to read and to search.

This approach is highly effective for fast lookups and session management. For example, websites often store user session data in Redis to provide quick access to user profiles.

You can find the same data in relational form and in key-value form in [`data/misc/nosql_relational_vs_key_value.xlsx`](../data/misc/nosql_relational_vs_key_value.xlsx).

---

## Column-Family Stores: In-Depth Exploration

Column-family (or wide-column) databases store data in rows identified by a key. Each row can have its own set of columns, and the rows are distributed across the nodes of the cluster based on their key.

### Characteristics:
- **Row Key + Flexible Columns**: Every row has a key, and different rows can have different columns.
- **Partitioning**: The key decides on which node a row is stored, so data spreads over many machines automatically.
- **High Write Performance**: Excellent for handling large volumes of data, especially when there are many writes.
- **Use Cases**: Best suited for time-series data, event logging, or applications requiring fast access to large datasets by key.

> Do not confuse **column-family** stores with **columnar** storage (like Parquet). Columnar formats store each column separately on disk to speed up analytics. Column-family stores like Cassandra keep the data of a row together. See [Lesson 3](03_sql_vs_nosql.md) for more details.

### Practical Demonstration (Column-Family Example using Apache Cassandra):
#### Sample Data:
```
Column Family: User_Info
------------------------------------
Key: user123
Name: John Doe
Age: 35
Email: john@example.com
```
- In Cassandra, you can insert and query data using CQL (Cassandra Query Language). CQL looks like SQL, but there are no joins, and you are expected to query by key.

```sql
-- Creating a keyspace and a table (column family) in Cassandra
CREATE KEYSPACE user_data WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};

USE user_data;

CREATE TABLE user_info (user_id text PRIMARY KEY, name text, age int, email text);

-- Inserting data
INSERT INTO user_info (user_id, name, age, email) VALUES ('user123', 'John Doe', 35, 'john@example.com');

-- Querying data
SELECT * FROM user_info WHERE user_id = 'user123';
```

### A more typical example: time-series data
The real strength of a column-family store shows with time-series. Imagine thousands of sensors sending a temperature every second:

```sql
CREATE TABLE sensor_readings (
    sensor_id   text,
    reading_ts  timestamp,
    temperature double,
    PRIMARY KEY ((sensor_id), reading_ts)
) WITH CLUSTERING ORDER BY (reading_ts DESC);

INSERT INTO sensor_readings (sensor_id, reading_ts, temperature)
VALUES ('sensor-42', '2024-05-10 19:42:27', 21.5);

-- Last 10 readings of one sensor
SELECT * FROM sensor_readings WHERE sensor_id = 'sensor-42' LIMIT 10;
```

- `sensor_id` is the **partition key**: all the readings of one sensor are stored together, on the same nodes.
- `reading_ts` is the **clustering column**: inside the partition, readings are kept sorted by time.

So "the last 10 readings of sensor-42" is a very fast query. On the other hand, a query like "all sensors with temperature above 30" is not what Cassandra is designed for, because it would have to scan every partition. In Cassandra you design the tables **starting from the queries** you need, not from the entities as in a relational database.

---

## How to Choose the Right No-SQL Database

When choosing between different types of No-SQL databases, it’s important to understand the specific needs of your application.

### Considerations:
1. **Data Model**: 
   - **Key-Value**: Best for simple lookups, where the application primarily requires access to specific keys.
   - **Column-Family**: Suitable for large datasets where each key has many values (for example all the readings of a sensor), and where you know the queries in advance.

2. **Read/Write Patterns**: 
   - **Key-Value**: Optimized for fast reads and writes of individual data points.
   - **Column-Family**: Very high write throughput, and efficient reads of a range of rows inside one partition (for example the last hour of data of one sensor).

3. **Scalability and Availability**: 
   - Both key-value and column-family databases scale horizontally. Cassandra is designed to keep working when nodes fail, because every row is copied on several nodes. Redis is usually used as a single node or a small cluster, with the data kept in memory.

4. **Use Case**: 
   - **Key-Value (e.g., Redis)**: Great for caching, session storage, and real-time analytics.
   - **Column-Family (e.g., Cassandra)**: Ideal for time-series data, event logging, and applications requiring high write throughput.

---

## The Other Two Families: Document Stores and Graph Databases

### Document Stores
A **document store** saves each record as a document, usually in a JSON-like format. Documents can contain nested objects and lists, and two documents in the same collection do not need to have the same fields. Unlike a key-value store, the database understands what is inside the document, so you can query and index any field.

- **Examples**: MongoDB, Couchbase, Amazon DocumentDB.
- **Use Cases**: Product catalogs, content management, user profiles, mobile apps.

```javascript
// MongoDB: insert a document into the "users" collection
db.users.insertOne({
  _id: "user123",
  name: "John Doe",
  age: 35,
  email: "john@example.com",
  addresses: [ { city: "Milan", type: "home" } ]
})

// Find all users older than 30 who live in Milan
db.users.find({ age: { $gt: 30 }, "addresses.city": "Milan" })
```

### Graph Databases
A **graph database** stores **nodes** (entities) and **edges** (relationships between them). Relationships are stored directly, so following them is fast even when there are many "hops", which would need many expensive joins in a relational database.

- **Examples**: Neo4j, Amazon Neptune, ArangoDB.
- **Use Cases**: Social networks, recommendation engines, fraud detection, knowledge graphs.

```cypher
// Neo4j (Cypher): two people and a friendship
CREATE (a:Person {name: "Alice"})-[:FRIEND_OF]->(b:Person {name: "Bob"})

// Friends of Alice's friends that Alice does not know yet
MATCH (a:Person {name: "Alice"})-[:FRIEND_OF]->()-[:FRIEND_OF]->(fof)
WHERE NOT (a)-[:FRIEND_OF]->(fof) AND fof <> a
RETURN fof.name
```

### Summary

| Type | Data model | Typical query | Examples |
|------|------------|---------------|----------|
| Key-Value | key -> value | get the value of a key | Redis, DynamoDB, Memcached |
| Column-Family | row key -> flexible columns | get the rows of a partition | Cassandra, HBase, Bigtable |
| Document | JSON-like documents | find documents by any field | MongoDB, Couchbase |
| Graph | nodes and edges | follow relationships | Neo4j, Neptune |

---

## Practice

1. Write the Redis commands to store a product with id `987`, name `Laptop`, price `899.90` and stock `12` as a hash. Then write the command that reduces the stock by 1 (hint: look at `HINCRBY`).
2. Design a Cassandra table to store the messages of a chat application, so that you can quickly read the last 50 messages of a conversation. What is the partition key? What is the clustering column?
3. For each of these, which of the four NoSQL families would you choose? A movie recommendation system based on "people who liked this also liked...", a blog with posts and comments, a cache for the home page of a news website, the clicks of millions of users on a website.
