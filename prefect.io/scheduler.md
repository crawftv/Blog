---
description: This scheduler really helped me deploy my papermill notebook apps.
---

# Scheduler

The code I use for scheduling a notebook is pretty much straight from the docs.

```python
from prefect import Flow, task
from prefect.schedules import CronSchedule
import papermill as pm

#wrap your function with the @task decorator
@task
def etl():
    pm.execute_notebook(
    ...
    )

with Flow('ETL',schedule =CronSchedule('* * * * *') ) as flow:
 52         etl = etl()    

```

