# Assorted Tips

### Saving a pandas dataframe

If you save a pandas dataframe to csv with data structures like lists or tuples, when you load it, the list will be a string. However you will have trouble loading it with `json.loads()` . I found the snippet below to work best.  

```python
from ast import literal_eval
literal_eval()
```

### Starting a project

When you start any type of project, It works best to make a blank repo on github first and clone it into your local machine. Using version control outside of this flow can be very difficult. 

### Installing psycopg2 on linux

Generally, I've had an issue installing psycopg2 on linux in which the Pipfile.lock does not lock.  
This line of code fixes that:

```bash
sudo apt-get install libpq-dev python-dev
# OR The two commands below
pipenv uninstall psycopg2
pipenv install psycopg2
```

### mongodb connection issue

```bash
pymongo.errors.ServerSelectionTimeoutError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1076),
[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1076),: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1076)
```





{% embed url="https://stackoverflow.com/a/45018725" %}



