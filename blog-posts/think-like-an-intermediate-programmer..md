---
description: Advancing beyond Googling everything.
---

# Think like an intermediate programmer.

## The Problem

I was trying to deploy a GitHub repo to Heroku with continuous integration for a new personal website. The Heroku provides a couple comprehensive & step-by-step guides to get started. I've gone through this process more than a handful of time so I was comfortable with it. After adding the Procfile, requirements.txt, & runtime.txt and pushing to the master branch. Something went wrong.

```text
-----> Python app detected
-----> Found python-3.6.8, removing
-----> Clearing cached dependencies
-----> Installing python-3.8.2
-----> Installing pip
-----> Installing SQLite3
Sqlite3 successfully installed.
-----> Installing requirements with pip
       Collecting click==7.1.1
         Downloading click-7.1.1-py2.py3-none-any.whl (82 kB)
       Collecting Cython==0.29.16
         Downloading Cython-0.29.16-cp38-cp38-manylinux1_x86_64.whl (2.0 MB)
       Collecting falcon==2.0.0
         Downloading falcon-2.0.0-py2.py3-none-any.whl (163 kB)
       Collecting falcon-jinja2==0.0.3
         Downloading falcon_jinja2-0.0.3.tar.gz (2.6 kB)
           ERROR: Command errored out with exit status 1:
            command: /app/.heroku/python/bin/python -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-z28m1sve/falcon-jinja2/setup.py'"'"'; __file__='"'"'/tmp/pip-install-z28m1sve/falcon-jinja2/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base /tmp/pip-install-z28m1sve/falcon-jinja2/pip-egg-info
                cwd: /tmp/pip-install-z28m1sve/falcon-jinja2/
           Complete output (9 lines):
           Traceback (most recent call last):
             File "<string>", line 1, in <module>
             File "/tmp/pip-install-z28m1sve/falcon-jinja2/setup.py", line 9, in <module>
               import falcon_jinja2
             File "/tmp/pip-install-z28m1sve/falcon-jinja2/falcon_jinja2/__init__.py", line 1, in <module>
               from .template import *  # noqa
             File "/tmp/pip-install-z28m1sve/falcon-jinja2/falcon_jinja2/template.py", line 1, in <module>
               import falcon
           ModuleNotFoundError: No module named 'falcon'
           ----------------------------------------
       ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
 !     Push rejected, failed to compile Python app.
 !     Push failed
```

