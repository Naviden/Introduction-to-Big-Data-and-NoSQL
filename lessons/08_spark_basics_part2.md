
# Practicing Spark Basics in Databricks (Part 2)

In this lesson, we’ll continue exploring Apache Spark in Databricks by diving into more advanced Spark operations such as **joins**, **aggregations**, and **window functions**. We will also compare working with **Spark DataFrames** and **RDDs**.

# Case Study: Urban Essentials

**Urban Essentials** is a mid-sized e-commerce company specializing in the sale of everyday products such as household items, personal care, and electronics. The company focuses on providing a seamless online shopping experience, offering a wide range of affordable and high-quality products to customers across various cities.

## Business Model

- **Target Audience**: Urban professionals and families seeking convenience and quality in their daily purchases.
- **Revenue Streams**: Primarily through product sales, with additional revenue from promotions and a subscription service for frequent shoppers.
- **Technology**: Leverages big data analytics and AI to personalize the shopping experience, offering product recommendations based on customer behavior and purchase history.

## About the Data

The datasets provided for this lesson are **synthetic** and have been generated to simulate the operations of Urban Essentials. They include:

1. **Customer Dataset**: Contains information about 1,000 unique customers, including their names, emails, ages, and cities of residence.
2. **Order Dataset**: Includes 2,000 orders associated with these customers, detailing order dates and amounts.

## Purpose of the Case Study

This is a **simple case study** designed to help learners practice advanced data manipulation and analysis techniques using Apache Spark. Through this case study, we will apply functions such as:

- **Joins**: Combine multiple datasets.
- **Aggregations**: Summarize data by grouping and calculating statistics.
- **Window Functions**: Perform operations across a specified window of data.

---


### Joins in Spark

Joins are essential when working with multiple datasets. Spark supports different types of joins, including inner, left, right, and full outer joins.

#### Example: Inner Join
```
# Load two datasets
df_customers = spark.read.csv("/databricks-datasets/samples/customers/customers.csv", header=True, inferSchema=True)
df_orders = spark.read.csv("/databricks-datasets/samples/orders/orders.csv", header=True, inferSchema=True)
```
```
# Perform an inner join on customer_id
joined_df = df_customers.join(df_orders, df_customers["customer_id"] == df_orders["customer_id"], "inner")
joined_df.show()
```

You can use other types of joins such as `left`, `right`, or `full` by replacing `"inner"` in the `join` method.

---

### Aggregations in Spark

Aggregations allow you to compute summaries of your data, such as sums, averages, or counts.

#### Example: Grouping and Aggregating
```
# Group by customer_id and calculate the total order amount
agg_df = df_orders.groupBy("customer_id").agg({"order_amount": "sum"})
agg_df.show()
```

In this example, we grouped the orders by `customer_id` and calculated the total order amount for each customer.

---

### Window Functions in Spark

Window functions enable you to perform calculations across a set of rows related to the current row. They’re often used for running totals, rankings, or sliding averages.

#### Example: Window Function for Running Total
```
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number, col

# Define a window specification
window_spec = Window.partitionBy("customer_id").orderBy("order_date")

# Add a running total column
df_with_rank = df_orders.withColumn("rank", row_number().over(window_spec))
df_with_rank.show()
```

This example calculates the ranking of each order per customer based on the order date.

---