# Datasets

All the files are small enough to be read directly from GitHub by the notebooks.

| Folder | File | Rows | Used in | Description |
|--------|------|------|---------|-------------|
| `retail/` | `customers.csv` | 250 | [Retail case study](../case-studies/retail/README.md), [Lesson 7](../lessons/07_spark_basics_part1.md) | Customers of a small retail chain |
| `retail/` | `products.csv` | 60 | Retail case study | Product catalog |
| `retail/` | `stores.csv` | 12 | Retail case study | Stores |
| `retail/` | `transactions.csv` | 3,512 | Retail case study | Sales, with a JSON column, missing values, duplicates and outliers |
| `retail/` | `returns.csv` | 94 | Retail case study | Returned products and reasons |
| `urban_essentials/` | `customers.csv` | 1,000 | [Lesson 8](../lessons/08_spark_basics_part2.md) | Customers of the Urban Essentials case study |
| `urban_essentials/` | `orders.csv` | 2,000 | Lesson 8 | Orders of those customers |
| `airbnb/` | `AB_NYC_2019.csv` | 48,895 | [Airbnb pipeline project](../notebooks/spark/airbnb_pipeline_project.ipynb) | Airbnb listings in New York City, 2019 |
| `misc/` | `divina_commedia_first_pages.txt` | 10 lines | [MapReduce word count](../notebooks/mapreduce/mapreduce_word_count.ipynb) | The first lines of Dante's *Divina Commedia* (public domain) |
| `misc/` | `salary_data.csv` | 5 | [Spark functions explained](../notebooks/spark/salary_analysis_functions.ipynb) | A tiny dataset to explain Spark functions |
| `misc/` | `session_info.csv` | 100 | [Session data practice](../notebooks/spark/session_data_practice.ipynb) | User sessions; separated by `;`, some addresses span two lines |
| `misc/` | `1000_users.xlsx` | 1,000 | | Users (name, surname, email, address), same structure as the data generated in the Parquet notebook |
| `misc/` | `nosql_relational_vs_key_value.xlsx` | 30 | [Lesson 4](../lessons/04_nosql_types.md) | The same user sessions in relational form and in key-value form |

All the datasets are **synthetic**, except the Airbnb one and the *Divina Commedia* text.

## Airbnb data

`airbnb/AB_NYC_2019.csv` comes from [Inside Airbnb](https://insideairbnb.com/), which publishes it under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/). This version was published on Kaggle as [New York City Airbnb Open Data](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data).
