# Introduction to Big Data and NoSQL

This repository contains the materials for an introductory **Big Data** course. The content is designed for undergraduate and graduate students and covers a range of topics related to big data concepts, NoSQL databases, and hands-on practice with Redis and Apache Spark (on Databricks).

## Overview

The course focuses on the following areas:

- Basic concepts and definitions related to Big Data, such as databases, data warehouses, data marts, data lakes and lakehouses.
- The 5 V's of Big Data (Volume, Velocity, Variety, Veracity, and Value).
- Introduction and comparison of SQL and NoSQL databases, the CAP theorem and BASE, with an emphasis on key-value and column-family NoSQL databases.
- Hands-on practice with Redis, a key-value database.
- Big Data technologies: Apache Hadoop, MapReduce, and Apache Spark.
- Practical exercises in Apache Spark using Databricks, covering filtering, transformations, joins, aggregations, window functions, and working with both DataFrames and RDDs.

## Syllabus

| # | Lesson | Hands-on | Time |
|---|--------|----------|------|
| 1 | [Basic Definitions](lessons/01_basic_definitions.md) | [University Information System case study](case-studies/university/README.md) | 1 h |
| 2 | [Big Data Definition (the 5 V's)](lessons/02_big_data_definition.md) | | 20 min |
| 3 | [SQL vs. NoSQL, CAP and BASE](lessons/03_sql_vs_nosql.md) | | 1 h |
| 4 | [NoSQL Database Types](lessons/04_nosql_types.md) | [Columnar formats: Parquet](notebooks/parquet/columnar_parquet.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naviden/Introduction-to-Big-Data-and-NoSQL/blob/main/notebooks/parquet/columnar_parquet.ipynb) | 1 h |
| 5 | [Key-Value Databases and Redis](lessons/05_key_value_databases_and_redis.md) | [Redis intro](notebooks/redis/redis_intro.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naviden/Introduction-to-Big-Data-and-NoSQL/blob/main/notebooks/redis/redis_intro.ipynb)<br>[Key-value case study](case-studies/key-value/key_value_databases_case_study.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naviden/Introduction-to-Big-Data-and-NoSQL/blob/main/case-studies/key-value/key_value_databases_case_study.ipynb)<br>[Exercise: Wikipedia data in Redis](notebooks/redis/redis_wikipedia_exercise.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Naviden/Introduction-to-Big-Data-and-NoSQL/blob/main/notebooks/redis/redis_wikipedia_exercise.ipynb) | 2 h |
| 6 | [Big Data Technologies: Hadoop, MapReduce, Spark](lessons/06_big_data_technologies.md) | [MapReduce in Python](https://github.com/Naviden/MapReduceExample/blob/main/Mapreduce%20Example.ipynb) | 1 h |
| 7 | [Spark Basics in Databricks (Part 1)](lessons/07_spark_basics_part1.md) | [Spark functions explained](notebooks/spark/salary_analysis_functions.ipynb)<br>[Practice: session data](notebooks/spark/session_data_practice.ipynb)<br>[Project: Airbnb pipeline](notebooks/spark/airbnb_pipeline_project.ipynb) | 1.5 h |
| 8 | [Spark Basics in Databricks (Part 2)](lessons/08_spark_basics_part2.md) | [Retail Analytics case study](case-studies/retail/README.md) | 1.5 h |

Times are indicative and include the hands-on part. Every lesson ends with a short **Practice** section that can be used for self-study.

## Repository Structure

```
lessons/        the lessons (Markdown)
notebooks/      hands-on notebooks: redis/, parquet/, spark/
case-studies/   longer exercises: university/, key-value/, retail/
data/           datasets used in the notebooks (see data/README.md)
extra/          additional guides
```

## Requirements

To run the code examples and exercises provided in this repository, you will need:

1. **A Databricks Free Edition account** for the Spark lessons. You can sign up for free [here](https://www.databricks.com/learn/free-edition). See [Lesson 7](lessons/07_spark_basics_part1.md) for the setup.
2. **A Google account** to open the Redis and Parquet notebooks in [Google Colab](https://colab.research.google.com/) (use the "Open in Colab" buttons above).
3. **Python**: all the examples are written in Python (PySpark for Spark).

All the notebooks also run locally. Create a virtual environment and install the requirements:

```bash
pip install -r requirements.txt
```

The Spark notebooks need Java 17 or newer when you run them outside Databricks. For Redis, see the [installation guide](extra/install_redis_py_guide.md); if no Redis server is running, the notebooks fall back to `fakeredis`.

## Getting Started

1. Clone the repository to your local machine:
   ```bash
   git clone https://github.com/Naviden/Introduction-to-Big-Data-and-NoSQL.git
   ```

2. Follow the lessons in the order of the [syllabus](#syllabus). Each lesson links to its notebooks.

3. For the Spark notebooks, import the `.ipynb` file into your Databricks workspace (**Workspace > Import**). The notebooks read the data directly from this repository, so you don't need to upload anything.

## Contributing

Contributions are welcome! If you'd like to suggest improvements or add new topics to the repository, feel free to open an issue or a pull request.

If you edit the student notebooks, install the pre-commit hook so that their outputs are removed before each commit:

```bash
pip install pre-commit
pre-commit install
```

## License

The course materials are released under the [Creative Commons Attribution 4.0 International License](LICENSE) (CC BY 4.0). The Airbnb dataset in `data/airbnb/` comes from [Inside Airbnb](https://insideairbnb.com/), see [data/README.md](data/README.md).
