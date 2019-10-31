---
description: 'Extract, Transform, and Load'
---

# ETL

### What is ETL?

ETL is a term meaning Extract, Transform, Load. It is used when there a routine for pulling data, cleaning it, and saving/analyzing it.  
I thin the Prefect Library is great for some simple ETL. 

### Template for Prefect ETL

This code basically comes from the docs

```python
from prefect import task, Flow

@task
def extract():
    ...
    return extract_result

@task
def transform(extract_result):
    ...
    return transform_result

@task
def load(transform_result):
    ...
    
with Flow('ETL') as flow:
    e = extract()
    t = transform(e)
    l = load(t)

flow.run()
```

## Tips

### Be careful with side effects

When i was struggling with prefect I had code that took used side effects. Side effects, as I understand them are when a function changes something that was not passed to it. In python this is often treated as feature. But using a Prefect @task did not would not do the side effects.   
My code as a web scraper that would add something to a list. I did not originally pass the list to the function. So the scraper would run, but it would not update the list when the function was run as a prefect task. 

I feel like avoiding side effects is a good habit to get into anyway.

```python
import requests

#The original failing code
comments_list = []
def hn_scrape(i):
    r = requests.get('https://hacker-news.firebaseio.com/v0/item/'+str(i)+'.json').json()
    try:
        if ('deleted' in r.keys()):
            pass
        else:
            if r["type"] == 'comment':
                t = (r["by"],r["id"],r["text"],r["time"])
                comments_list.append(t)
    except:
        pass
@task
def extract():
    for i in range(0,...):
        hn_scrape(i)    
        
#Working Code
#Note that comments_list is added as a parameter to the function.
def hn_scrape(i,comments_list):
    r = requests.get('https://hacker-news.firebaseio.com/v0/item/'+str(i)+'.json').json()
    try:
        if ('deleted' in r.keys()):
            pass
        else:
            if r["type"] == 'comment':
                t = (r["by"],r["id"],r["text"],r["time"])
                comments_list.append(t)
    except:
        pass
@task
def extract():
    comments_list = []
    for i in range(0,...):
        hn_scrape(i,comments_list)

```

I also moved the creation of the empy `comment_list` to inside the task. I don't know if this is necessary but I assume it is a best practice. 

Furthermore,  
Please don't judge my use of try: except:

