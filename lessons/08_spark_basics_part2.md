# Practicing Spark Basics in Databricks (Part 2)

In this lesson, we’ll continue exploring Apache Spark in Databricks by diving into more advanced Spark operations such as **joins**, **aggregations**, and **window functions**. We will also compare working with **Spark DataFrames** and **RDDs**.

# Case Study: Urban Essentials

**Urban Essentials** is a mid-sized e-commerce company specializing in the sale of everyday products such as household items, personal care, and electronics. The company focuses on providing a seamless online shopping experience, offering a wide range of affordable and high-quality products to customers across various cities.

## Business Model

- **Target Audience**: Urban professionals and families seeking convenience and quality in their daily purchases.
- **Revenue Streams**: Primarily through product sales, with additional revenue from promotions and a subscription service for frequent shoppers.
- **Technology**: Leverages big data analytics and AI to personalize the shopping experience, offering product recommendations based on customer behavior and purchase history.

## About the Data

The datasets provided for this lesson are **synthetic** and have been generated to simulate the operations of Urban Essentials. You can find them in [`data/urban_essentials/`](../data/urban_essentials/):

1. **Customer Dataset** (`customers.csv`): 1,000 unique customers with `customer_id, name, email, age, city`.
2. **Order Dataset** (`orders.csv`): 2,000 orders with `order_id, customer_id, order_date, order_amount`.

## Purpose of the Case Study

This is a **simple case study** designed to help learners practice advanced data manipulation and analysis techniques using Apache Spark. Through this case study, we will apply functions such as:

- **Joins**: Combine multiple datasets.
- **Aggregations**: Summarize data by grouping and calculating statistics.
- **Window Functions**: Perform operations across a specified window of data.

---

### Loading the Data

As in [Part 1](07_spark_basics_part1.md), you can either upload the files to a volume and use `spark.read.csv`, or read them directly from GitHub:

```python
import pandas as pd

DATA_URL = "https://raw.githubusercontent.com/Naviden/Introduction-to-Big-Data-and-NoSQL/main/data"

df_customers = spark.createDataFrame(pd.read_csv(f"{DATA_URL}/urban_essentials/customers.csv"))
df_orders = spark.createDataFrame(pd.read_csv(f"{DATA_URL}/urban_essentials/orders.csv"))
```

---

### Joins in Spark

Joins are essential when working with multiple datasets. Spark supports different types of joins, including inner, left, right, and full outer joins.

#### Example: Inner Join
```python
# Perform an inner join on customer_id
joined_df = df_customers.join(df_orders, on="customer_id", how="inner")
joined_df.show(5)
```

You can use other types of joins such as `left`, `right`, or `full` by changing the `how` parameter.

> **Why `on="customer_id"`?** You may also see joins written as `df_customers["customer_id"] == df_orders["customer_id"]`. That works too, but the result then contains **two** columns called `customer_id`, and any later `select("customer_id")` fails with an "ambiguous reference" error. When the column has the same name in both DataFrames, passing its name with `on=` keeps only one copy.

---

### Aggregations in Spark

Aggregations allow you to compute summaries of your data, such as sums, averages, or counts.

#### Example: Grouping and Aggregating
```python
from pyspark.sql import functions as F

# Group by customer_id and calculate the total order amount
agg_df = df_orders.groupBy("customer_id").agg(
    F.sum("order_amount").alias("total_amount"),
    F.count("*").alias("n_orders"),
)
agg_df.show(5)
```

In this example, we grouped the orders by `customer_id` and calculated the total order amount and the number of orders for each customer. Using `F.sum(...).alias(...)` gives the new column a readable name (otherwise it would be called `sum(order_amount)`).

---

### Window Functions in Spark

Window functions enable you to perform calculations across a set of rows related to the current row. They’re often used for running totals, rankings, or sliding averages. Unlike `groupBy`, a window function does **not** reduce the number of rows: every order stays in the result, with a new column added.

#### Example 1: Ranking the orders of each customer
```python
from pyspark.sql.window import Window
from pyspark.sql import functions as F

# Define a window specification: one window per customer, ordered by date
window_spec = Window.partitionBy("customer_id").orderBy("order_date")

# Number the orders of each customer: 1 = first order, 2 = second order, ...
df_with_rank = df_orders.withColumn("order_number", F.row_number().over(window_spec))
df_with_rank.show(5)
```

This example numbers the orders of each customer based on the order date.

#### Example 2: Running total
```python
# Same window, but only from the first order up to the current one
running_window = window_spec.rowsBetween(Window.unboundedPreceding, Window.currentRow)

df_running = df_orders.withColumn("running_total", F.sum("order_amount").over(running_window))
df_running.orderBy("customer_id", "order_date").show(10)
```

For each order, `running_total` is the total amount the customer has spent **up to and including** that order.

---

## Working with Spark DataFrames and RDDs

### Spark DataFrames

DataFrames are the most commonly used data structure in Spark and offer a higher-level API for working with structured data. They provide better performance optimizations and integrations with Spark SQL.

#### Key Features:
- Schema-aware: DataFrames have a schema, similar to a table in a relational database.
- Optimized: DataFrames are optimized using Spark’s **Catalyst** engine for query optimization.

#### Example:
```python
# Basic DataFrame operation
df_customers.select("customer_id", "name").show()
```

---

### Spark RDDs (Resilient Distributed Datasets)

> [!NOTE]
> **Why this course does not practise RDDs.** RDDs were Spark's first API, and DataFrames are built on top of them, so it is good to know what they are. But today they are rarely used directly:
> - Spark can **optimize** DataFrame and SQL code (with the Catalyst engine), while RDD code runs exactly as you write it, which is usually slower, especially from Python.
> - New Spark features, like Structured Streaming and the main machine learning API, are built on DataFrames.
> - On **Databricks Free Edition**, the platform of this course, the RDD API is not available at all.
>
> So this section only shows what an RDD looks like; all the exercises use DataFrames.

RDDs are the underlying distributed data structure in Spark and provide more control over data manipulation but are less optimized than DataFrames.

#### Key Features:
- Immutable and distributed: RDDs are split into partitions across a cluster.
- Low-level API: Provides fine-grained control but requires more manual optimization.

#### Example:
```python
# Create an RDD from a list
rdd = spark.sparkContext.parallelize([("John", 35), ("Alice", 29), ("Bob", 42)])

# Perform a map operation on the RDD
rdd_mapped = rdd.map(lambda x: (x[0], x[1] + 10))  # Add 10 to each age
rdd_mapped.collect()
```

> To try this example, run it locally (`pip install pyspark`) or in Google Colab: on Databricks Free Edition `spark.sparkContext` is not available.

---

## When to Use DataFrames vs. RDDs

### DataFrames:
- Use DataFrames when working with structured data, especially when you want to leverage Spark’s optimizations and query engine.

### RDDs:
- Use RDDs for unstructured data or when you need more control over data processing and transformations. They’re also useful for certain low-level operations where DataFrames may not provide the necessary flexibility.

---

## Practice

Using the Urban Essentials data:

1. Find the 5 cities with the highest total order amount (hint: join, then `groupBy("city")`).
2. Some customers never placed an order. Use a `left` join to find how many.
3. For each customer, find the date of their largest order (hint: a window ordered by `order_amount` descending and `row_number() == 1`).
4. Compute, for each order, the difference between its amount and the average order amount of the same customer (hint: `F.avg(...).over(Window.partitionBy("customer_id"))`).

For a larger exercise with several tables, missing values and duplicates, continue with the [Retail Analytics case study](../case-studies/retail/README.md).
