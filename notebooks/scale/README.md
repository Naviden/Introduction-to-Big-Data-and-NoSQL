# Spark at Scale: Why Not Just pandas?

Two notebooks that answer the same questions on the same data, one year of New York taxi trips (41 million rows), first with pandas and then with Spark.

| Order | Notebook | Platform | What it shows |
|-------|----------|----------|---------------|
| 1 | [`pandas_limits.ipynb`](pandas_limits.ipynb) | Google Colab [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naviden/Introduction-to-Big-Data-and-NoSQL/blob/main/notebooks/scale/pandas_limits.ipynb) | One month is easy, one year does not fit in memory |
| 2 | [`spark_at_scale.ipynb`](spark_at_scale.ipynb) | Databricks | The whole year in seconds, reading only the columns a query needs, one billion rows, and when pandas is still the better choice |

To import the Spark notebook in Databricks (**Workspace > Import > URL**), use:

```
https://raw.githubusercontent.com/Naviden/Introduction-to-Big-Data-and-NoSQL/main/notebooks/scale/spark_at_scale.ipynb
```

Both notebooks download the data by themselves from the [NYC Taxi and Limousine Commission](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) (12 Parquet files, about 650 MB). The Spark notebook stores them in a Databricks volume called `taxi_data`, which it creates if needed.
