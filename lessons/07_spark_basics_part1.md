
# Practicing Spark Basics in Databricks (Part 1)

In this lesson, we’ll explore how to get started with **Apache Spark** in **Databricks** and practice some of the fundamental Spark operations, such as filtering, transformations, and actions.

---

## Introduction to Databricks

**Databricks** is a unified analytics platform built on top of Apache Spark. It provides an interactive workspace for data engineers, data scientists, and analysts to collaborate on large-scale data processing projects.

### Key Features of Databricks:
1. **Integrated with Spark**: Databricks is built on Spark, making it a powerful tool for running distributed data processing jobs with minimal setup.
2. **Notebooks**: Databricks provides interactive notebooks that allow you to write code, visualize data, and document your analysis in one place.
3. **Collaborative**: Multiple users can collaborate on the same project, making it ideal for team-based data projects.
4. **Scalable**: Databricks is designed to scale out workloads on cloud-based clusters, allowing users to handle large datasets efficiently.


---

## Basic Operations in Spark

Apache Spark is a distributed computing system designed for large-scale data processing. While Spark itself is written in **Scala**, it offers APIs for several languages, including **Python** through **PySpark**. PySpark provides a Pythonic interface to Spark, making it accessible to Python developers.

Below are some basic operations you can perform using PySpark:

---

### 1. **Loading Data into Spark**
The first step in any Spark application is loading data into a **DataFrame**. In PySpark, you can easily load data from various formats such as **CSV**, **Parquet**, or **Delta**.

```python
# Load a CSV file into a DataFrame
df = spark.read.csv("/path/to/data.csv", header=True, inferSchema=True)
```

- header=True: Indicates that the first row of the file contains column names.
- inferSchema=True: Automatically infers the data types of the columns.
Spark DataFrames are similar to Pandas DataFrames but are distributed across a cluster for scalability.

### 2. Filtering Data

You can filter rows in a DataFrame using the filter or where methods. This allows you to subset the data based on specific conditions.
```python
# Filter rows where 'SOME_COLUMN' is greater than 30
filtered_df = df.filter(df["SOME_COLUMN"] > 30)
filtered_df.show()
```
•	filter and where are equivalent and can be used interchangeably.
•	show(): Displays the first few rows of the DataFrame.

### 3. Transformations in Spark

Transformations in Spark are lazy operations, meaning they define the computation steps but do not execute them immediately. These steps are executed only when an action is triggered.

Common Transformations:

- select: Choose specific columns.
- filter: Subset rows.
- groupBy: Group rows based on column values.
- withColumn: Add or modify a column.

Example: Selecting specific columns and applying a filter.
```python
# Select specific columns and filter the data
transformed_df = df.select("name", "SOME_COLUMN").filter(df["SOME_COLUMN"] > 30)
transformed_df.show()
```
 ### 4. Actions in Spark

Actions in Spark trigger the execution of the transformations. They return results to the driver program or write the results to storage.

Common Actions:
- show(): Displays rows in the DataFrame.
- count(): Returns the number of rows.
- collect(): Retrieves all rows as a list (be cautious with large datasets).

Example:

```python
# Count the number of rows after filtering
row_count = transformed_df.count()
print(f"Number of rows: {row_count}")
```


- count() computes the total number of rows that match the specified conditions.
- Actions like show() or count() force Spark to execute the computation steps defined by the transformations.

## Understanding Lazy Operations in Spark

One of the most important concepts in Spark is **lazy evaluation**. This design principle is crucial for optimizing the execution of large-scale data processing tasks.

---

### What Are Lazy Operations?

In Spark, **Transformations** (like `filter`, `select`, and `groupBy`) are **lazy operations**. This means that when you define a transformation, Spark doesn’t immediately execute it. Instead, it builds a **logical execution plan** (or **DAG** - Directed Acyclic Graph) of all the transformations you’ve defined.

![image](https://www.researchgate.net/publication/329134581/figure/fig1/AS:864405822640128@1583101816382/Example-of-a-Spark-job-DAG.png)
[(Image source)](https://www.researchgate.net/publication/329134581_A_Machine_Learning_Approach_for_Predicting_Execution_Time_of_Spark_Jobs)

**Key Point**: Transformations only define *what* Spark needs to do, not *how* or *when* it will do it.

---

### Why Is Lazy Evaluation Useful?

1. **Optimization**:
   - Spark waits until an **Action** (e.g., `count()`, `show()`, `collect()`) is called before executing the transformations.
   - During this waiting period, Spark optimizes the entire execution plan to minimize data shuffling, avoid unnecessary computations, and execute tasks efficiently.

2. **Reduced Overhead**:
   - If multiple transformations are chained together, Spark combines them into a single stage whenever possible. This avoids intermediate steps and reduces the number of passes over the data.

3. **Fault Tolerance**:
   - Lazy evaluation helps Spark recover from failures. Since the logical execution plan is defined beforehand, Spark can recompute only the necessary steps without re-running the entire workflow.

---

### How Does It Work?

Here’s a simple example to illustrate lazy operations:

```python
# Define a transformation: filtering rows where 'age' > 30
filtered_df = df.filter(df["age"] > 30)

# Define another transformation: selecting specific columns
selected_df = filtered_df.select("name", "age")

# At this point, no data is processed yet.
```
When Data Is Actually Processed:
Data is processed only when an action is called, such as:
```python
# Trigger an action
selected_df.show()
```
At this point:
	•	Spark combines the transformations (filter + select) into a single optimized execution plan.
	•	The data is processed according to this plan.


#### TLDR

Lazy operations in Spark allow you to define data transformations without immediate execution. This enables Spark to optimize the entire workflow for efficiency, ensuring that computations are executed only when necessary.

---


## Working with Spark DataFrames and RDDs

### Spark DataFrames

DataFrames are the most commonly used data structure in Spark and offer a higher-level API for working with structured data. They provide better performance optimizations and integrations with Spark SQL.

#### Key Features:
- Schema-aware: DataFrames have a schema, similar to a table in a relational database.
- Optimized: DataFrames are optimized using Spark’s **Catalyst** engine for query optimization.

#### Example:
```
# Basic DataFrame operation
df_customers.select("customer_id", "name").show()
```

---

### Spark RDDs (Resilient Distributed Datasets)

RDDs are the underlying distributed data structure in Spark and provide more control over data manipulation but are less optimized than DataFrames.

#### Key Features:
- Immutable and distributed: RDDs are split into partitions across a cluster.
- Low-level API: Provides fine-grained control but requires more manual optimization.

#### Example:
```
# Create an RDD from a list
rdd = spark.sparkContext.parallelize([("John", 35), ("Alice", 29), ("Bob", 42)])

# Perform a map operation on the RDD
rdd_mapped = rdd.map(lambda x: (x[0], x[1] + 10))  # Add 10 to each age
rdd_mapped.collect()
```

---

## When to Use DataFrames vs. RDDs

### DataFrames:
- Use DataFrames when working with structured data, especially when you want to leverage Spark’s optimizations and query engine.
  
### RDDs:
- Use RDDs for unstructured data or when you need more control over data processing and transformations. They’re also useful for certain low-level operations where DataFrames may not provide the necessary flexibility.
