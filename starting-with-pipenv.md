# Starting with Pipenv

## Why pipenv instead of conda?

If you want to deploy your work, I think the best thing is to use pipenv.   
If you are trying to deploy your work to a small server, serverless, or Heroku, you probably don't want all 700+ packaages that conda comes with.

## Pre-Install and Installation

1. If you do not, run `sudo apt-get update` 
2. Run `python3 --version` to see if you have python3 installed
   1. Then run  `sudo apt-get install python3.x` where x is whatever version of python you would like. Right now it would be `python3.6` or `python3.7` 
3. After you know you have a modern version of python, run `sudo apt-get install python3-distutils` . If you do not add this library, there is a good chance pipenv will not work for you.
4. Install pip3 with `sudo apt install python3-pip` with out the `pip3` library, your system might try to rectify your new python3 packages with the python2.7 environment that ships with most operating systems. That will cause problems later on.
5. install pipenv with `sudo -H pip3 install -U pipenv` .
6. To create the virtual environment run `pipenv --three` to ensure your environment uses your python3.
7. to activate and enter your new environment run `pipenv shell` 

If your Pipfile & Pipfile.lock are included in source control \(git\), after you clone a repo, you can just start the shell and run `pipenv install` to get the same environment you were using on a different computer. 

## Installing Packages

The base command for installing packages is `pipenv install package_name` . You can have as many packages as you want like `pipenv install package1 package2 package3` . 

### Errors

Some times you get an error message that looks like the below.

```text
['ERROR: Could not find a version that satisfies the requirement annoy==1.16.0 (from -
r /tmp/pipenv-oiodvvvq-requirements/pipenv-pcovd6ay-requirement.txt (line 1)) (from versions: 1.16.0.macosx-10.11-x86_64
, 1.0, 1.0.1, 1.0.2, 1.0.3, 1.0.4, 1.0.5, 1.1.1, 1.2.1, 1.2.2, 1.3.1, 1.3.2, 1.4.1, 1.5.1, 1.5.2, 1.6.0, 1.6.1, 1.6.2, 1
.7.0, 1.8.0, 1.8.3, 1.9.0, 1.9.1, 1.9.2, 1.9.3, 1.9.4, 1.9.5, 1.10.0, 1.11.1, 1.11.4, 1.11.5, 1.12.0, 1.13.0, 1.14.0, 1.
15.0, 1.15.1, 1.15.2)', 'ERROR: No matching distribution found for annoy==1.16.0 (from -r /tmp/pipenv-oiodvvvq-requireme
nts/pipenv-pcovd6ay-requirement.txt (line 1))']
```

If this is the case you can run install the latest version provided. So from the error above, the command would be `pipenv install annoy==1.15.2`.

## Dev packages

While developing stuff you might use some packages only in development and use different packages in your app. If you plan on using Jupyter notebooks for data analysis and not in your app it is probably best to install it as a dev package. Later, when you run `pipenv install` in your app environment dev packages will not be included. If you are switching between two analysis environments you can use `pipenv install --dev` to install all the packages including development packages.

