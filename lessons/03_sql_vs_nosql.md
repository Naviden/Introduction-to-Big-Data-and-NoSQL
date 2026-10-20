# SQL vs. No-SQL (Focus on Key-Value and Column-Family No-SQL Databases)

In this lesson, we’ll explore the differences between **SQL** (relational) and **No-SQL** (non-relational) databases, with a focus on two types of No-SQL databases: **key-value stores** and **column-family stores**. Understanding when and why to use each can be critical for building scalable applications.

---

## Introduction to Relational Databases (SQL)

**SQL (Structured Query Language)** databases are designed for structured data and rely on predefined schemas. Data is organized into **tables** with rows and columns, and relationships between data points are strictly defined.

### Characteristics of SQL Databases:
- **Schema-Based**: Data must follow a predefined schema with a clear structure (tables with rows and columns).
- **ACID Compliance**: SQL databases ensure **Atomicity, Consistency, Isolation, and Durability**, making them ideal for applications requiring complex transactions.
  
  - *Atomicity*: Ensures that a transaction is all-or-nothing; either all operations succeed, or none are applied.  
  - *Consistency*: Guarantees that a transaction transforms the database from one valid state to another, maintaining data integrity.
  - *Isolation*: Ensures that concurrent transactions do not interfere with each other, producing the same result as if they were executed sequentially.
  - *Durability*: Ensures that once a transaction is committed, the changes are permanent, even in the event of a system failure.
  
- **Relational**: Data is often linked using keys (primary and foreign keys) to maintain relationships between tables.

#### Example:
- **MySQL** and **PostgreSQL** are popular SQL databases. They’re often used in applications requiring high data consistency, such as financial systems or e-commerce platforms where data integrity is critical.

---

## No-SQL Databases: A Flexible Approach

**No-SQL** databases offer more flexibility in storing and retrieving data, making them a good fit for semi-structured data and for data whose structure changes often. They do not rely on fixed schemas and are usually designed to scale horizontally, making them suitable for handling massive datasets.

No-SQL databases are classified into several types. In this lesson, we’ll focus on:

### 1. **Key-Value Stores**
Key-value databases store data as a collection of **key-value pairs**. Each key is unique, and the value associated with the key can be any type of data (e.g., a string, a number, or a more complex object).

#### Characteristics:
- **Simple Structure**: Data is stored as key-value pairs, making retrieval very fast.
- **Scalability**: Scales horizontally by distributing key-value pairs across different nodes.
- **Best For**: Caching, session management, and scenarios where fast access to simple data is needed.

#### Example:
- **Redis**: A popular in-memory key-value store, often used for caching and real-time analytics.
  
#### How It Works:
```
Key: user123
Value: { "name": "Alice", "age": 29, "email": "alice@example.com" }
```
In practice, retrieving data from a key-value store is quick and efficient because you simply query the key to get the corresponding value.

---

### 2. **Column-Family Stores (Wide-Column Stores)**
Column-family databases organize data in **rows identified by a key**, where each row can have a different set of columns. Related columns are grouped together in a **column family**. You can think of it as a two-level map: `row key -> (column name -> value)`.

#### Characteristics:
- **Flexible Rows**: Two rows in the same table do not need to have the same columns, and a single row can hold thousands of columns (this is why they are also called **wide-column** stores).
- **Partitioned by Key**: Rows are spread across many nodes based on their key, so reading or writing a row by its key is fast even with billions of rows.
- **High Write Throughput**: Optimized for very fast writes across distributed systems.
- **Best For**: Applications that write a lot of data and read it back by key, like time-series data or event logging.

#### Example:
- **Apache Cassandra**: A widely-used column-family store, known for its scalability and fault tolerance across distributed systems. Other examples are Apache HBase and Google Bigtable.

#### Remember: *column-family is not the same as columnar.*
The name is confusing. A **columnar** format (like Parquet) or a columnar database (like ClickHouse) physically stores all the values of one column together, which makes analytical queries such as "average price of all products" very fast. A **column-family** store like Cassandra is still built around rows: the data of one row key is stored together. The word "column" refers to the flexible columns inside each row, not to how the data sits on disk.

#### How It Works:
```
Column Family: User_Info
------------------------------------
Key: user123
Name: Alice
Age: 29
Email: alice@example.com
```
In column-family stores, the row key (`user123`) decides on which node the row lives, and the columns inside the row can differ from one user to another. Queries are fast when they look up data by key.

---

## Real-World Use Cases

### Key-Value Stores (Redis):
- **Caching**: Redis is often used to cache frequently accessed data, like user sessions, reducing the load on a database.
  - **Example**: In an e-commerce website, Redis can cache product information to improve load times during high traffic.

### Column-Family Stores (Cassandra):
- **Time-Series Data**: Cassandra is used to handle large volumes of time-series data, such as IoT sensor data or event logging.
  - **Example**: In a telecommunications company, Cassandra can store call records or network events for real-time analysis.

---

## SQL vs. No-SQL: Key Differences

| Feature               | SQL (Relational)                     | No-SQL (Non-Relational)                 |
|-----------------------|--------------------------------------|-----------------------------------------|
| **Data Structure**     | Structured, Schema-Based            | Unstructured/Semi-Structured, Flexible  |
| **Scaling**            | Mostly vertical (bigger server); horizontal scaling is possible but harder | Horizontal (multiple servers/nodes)     |
| **Consistency Model**  | ACID transactions                   | Often BASE (eventual consistency); some offer ACID for single records or transactions |
| **Best For**           | Complex Queries, Transactions       | Scalability, Large Data, Flexibility    |
| **Examples**           | MySQL, PostgreSQL                   | Redis, Cassandra, MongoDB               |

### Other Types of NoSQL Databases

In addition to key-value and columnar databases, here are a few more types of NoSQL databases:

- **Document Stores**: These databases store data as JSON-like documents. Examples include MongoDB and CouchDB. They are suitable for applications requiring flexible and hierarchical data structures.
- **Graph Databases**: Designed to handle data with complex relationships, graph databases store entities as nodes and relationships as edges. Neo4j and Amazon Neptune are popular examples.

You will see examples of both in the [next lesson](04_nosql_types.md).

---

## CAP Theorem and BASE

When data is spread over many servers (nodes), the network between them can fail. The **CAP theorem** says that a distributed database can guarantee at most two of these three properties at the same time:

- **Consistency (C)**: every read gets the most recent write (all nodes see the same data).
- **Availability (A)**: every request gets an answer, even if some nodes are down.
- **Partition tolerance (P)**: the system keeps working even if the network splits the nodes into groups that cannot talk to each other.

In a real distributed system network problems will happen, so **P is not optional**. The real choice is what to do during a network partition:

- **CP systems** refuse some requests to stay consistent (for example HBase, MongoDB with default settings).
- **AP systems** keep answering, even if some answers may be a little out of date (for example Cassandra, DynamoDB with default settings).

Note that the "C" in CAP is not the same as the "C" in ACID. In ACID, consistency means that a transaction respects the rules of the database (constraints, foreign keys, ...).

### BASE
Many NoSQL databases choose availability and describe their behavior with **BASE**, which is the "relaxed" counterpart of ACID:

- **Basically Available**: the system answers as long as possible, even during failures.
- **Soft state**: the state of the data can change over time, even without new writes, while nodes synchronize.
- **Eventually consistent**: if no new writes arrive, all nodes will eventually hold the same data.

**Example**: you change your profile picture on a social network. For a few seconds a friend in another country may still see the old picture. This is eventual consistency, and for a profile picture it is perfectly acceptable. For a bank transfer it would not be, which is why banks use ACID databases.

Many modern systems let you tune this. In Cassandra, for example, you can choose for each query how many replicas must answer (`ONE`, `QUORUM`, `ALL`), trading speed for consistency.

---

## Practice

1. For each case, choose SQL or NoSQL (and which NoSQL type) and explain why:
   - a bank's account balances
   - the shopping cart of an online shop with millions of visitors
   - temperature readings sent every second by 100,000 sensors
2. Explain with your own words why "column-family" and "columnar" are not the same thing.
3. A ticket booking website must never sell the same seat twice. Would you prefer a CP or an AP system? What about a "likes" counter on a video?

