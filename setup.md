# Setup

## Operating system

My main operating system is Windows. Almost all code is run on some flavor of a Linux server, so being able to code in a linux environment with the Windows Linux Subsystem has been super helpful. 

### How I use Windows

Most of my exploratory code happens in a Jupyter notebook through my windows OS.  

### How I use Linux.

I like to use the WSL strictly for the code that I am going to deploy.  I do not use Jupyter in the subsystem.  When I start making code for deployment, I like to use pipenv to get a clean environment and add only the dependencies I need. In my experience, Anaconda does not work well pipenv. Trying to use the two at the same time produces a lot of segmentation fault errors. It is problem better avoided than solved.

Access to bash scripts is an upgrade from programming solely on windows. And most tutorials I have done came from Linux or mac users. Windows users can often get left behind when it comes to learning to program. 

## Using a code formatter

I recently found the [black](https://github.com/python/black) code formatter for python and its [counterpart for Jupyter notebooks](https://github.com/csurfer/blackcellmagic). These have been a godsend.   
Code formatters as a topic are pretty controversial. "What is readable to some, is not readable to all," as well as many more unmoving concerns. In my opinion, anything that can make code consistent and good looking with zero actual cost should be every time.  
I can only devote so much time to cleaning and documenting my code before I get distracted. Not only has using black saved me time, but having good-looking is my biggest motivator for writing good documentation. Code formatting energizes my work flow. 

The commands are simple. `pip install black` & `black file_to_blacken.py`.  
And for notebooks: `pip install blackcelllmagic` . Put the command `%load_ext blackcellmagic` somewhere, I prefere the top of the notebook. To actually format an individual cell, add `%%black` at the top of that cell and shift+enter to run it. It will blacken the code, but the cell will not run. 

