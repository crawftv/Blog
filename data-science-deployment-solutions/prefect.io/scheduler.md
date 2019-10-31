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
#For more information on scheduling see the section Cron Time in the Cron chapter
with Flow('ETL',schedule =CronSchedule('* * * * *') ) as flow:
    etl = etl()    

task.run()

```

## Running

Since I use a pipenv environment, i do `pipenv run python etl.py` in my terminal. The terminal should look like this. \(Although it wont look exactly like a code file with the numbers, YYYY-MM-DD will be replaced by actual dates, etc.\)

```text
ubuntu@ip-xxx-xxx-xxx-xxx:~/hn_app$ pipenv run python etl/etl.py

Loading .env environment variables…
[2019-06-28 05:01:25,631] INFO - prefect.Flow | Waiting for next scheduled run at YYYY-MM-DDT09:00:00+00:00
```

