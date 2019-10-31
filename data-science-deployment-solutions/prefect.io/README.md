---
description: A library to put your data science into production.
---

# Prefect.io

## Why Prefect and not Apache Airflow?

Honestly, I main chose it because the docs looked better and someone in my class made a contribution to project. Another more important reason is that small tasks seem easier with Prefect. I have a relative simple ETL job to run daily. I was able get the core operation running relatively quickly and the scheduler was also very easy to get started.

## Tasks

The Prefect library uses the decorator `@task` a lot. The decorator turns your function into a class and gives it some prefect methods. So far I haven't had to access any of those. 

