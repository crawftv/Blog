---
description: Advancing beyond Googling everything.
---

# Think like an intermediate programmer.

## The Setup

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

### The Main Error

```text
 Collecting falcon-jinja2==0.0.3
 ...
 ModuleNotFoundError: No module named 'falcon'
```

When I installed in my dev, I had the same error but with `jinja2`. The solution was to install `jinja2` first and reinstall `falcon-jinja2`.   
From this I reasoned that I needed to have `falcon` and `jinja2` installed and built before installing `falcon-jinja2`. 

## The Problem

#### I need to install falcon and jinja2 before installing some things in the requirements.txt

## Solutions

The first thing I did was scour the Heroku documentation. I also made a few unhelpful google searches.

#### Release Phase

Eventually I found [a page on the "Release Phase"](https://devcenter.heroku.com/articles/release-phase). The first line of the pretty much guaranteed a solution to my problem.

> Release phase _**enables you to run certain tasks**_ before a new [release](https://devcenter.heroku.com/articles/releases) of your app is deployed.

Perfect \(The emphasis in the quote above is mine\). I quickly wrote a bash script of a few `pip install` commands, added that script to the Profile, and pushed it,  awaiting success.

```text
/bin/sh: 1: ./install.sh: Permission denied
```

Yikes. I quickly realized two things. I did not know what a release phase meant _and_ I did not know how to fix it. Since this quick solution did not work, I decided to find a totally new solution. I glanced over most of the documentation when I saw that first sentence. I should have paid more attention to the examples, which used code for database migrations. The solution I tried was for a totally different problem.

#### The Buildpack

After clicking nearly every Python related thing on Heroku website, I found this crucial sentence in my project's settings tab. 

> Buildpacks are scripts that are run when your app is deployed.

I decided that I was going to make my own Buildpack. Heroku provides a great set of [Documentation on this as well](https://devcenter.heroku.com/articles/buildpack-api).   
I forked the Python Buildpack repo. The docs pointed me to the `bin/compile` folder. Unfortunately for me, the repo is shell code and I can't do much more than one liners. 

#### Reassessing the Problem

What do I know?

* I know a solution, but I don't know where to put it.
* I know the program is logging things to a console. 

To find where I needed to add my code, I searched for the line I need and landed at a file called `bin/compile/steps/pip-install` . This looked promising. The file was suspiciously long for something as simple as running a pip install, about 70 lines. But I knew the relevant code was going to look like the standard `pip install ...` So I searched and found this:

```text
/app/.heroku/python/bin/pip install -r "$BUILD_DIR/requirements.txt" --exists-action=w --src=/app/.heroku/src --disable-pip-version-check --no-cache-dir 2>&1 | tee "$WARNINGS_LOG" | cleanup | indent

```

A very long line with a lot going on. At the same time it was still just a doing a simple thing.  Right above it, I copy-pasted the line, changing `"$BUILD_DIR/requirements.txt"`

