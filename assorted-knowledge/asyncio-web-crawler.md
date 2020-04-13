---
description: Approximately 100 requests per second
---

# Asyncio Web Crawler

I spent a full day working out problems with asyncio. The current setup might be a little overdone, but I am running in the code in production on Google Cloud Functions, and testing it on Jupyter notebooks. It works without errors across different operating systems, platforms, and python versions \(as long as it is 3.6 or 3.7\).  

## What is Asyncio?

> asyncio is a library to write **concurrent** code using the **async/await** syntax.
>
> asyncio is used as a foundation for multiple Python asynchronous frameworks that provide high-performance network and web-servers, database connection libraries, distributed task queues, etc.
>
> asyncio is often a perfect fit for IO-bound and high-level **structured** network code.



## Asyncio Flow

My pattern for using the asyncio for web scraping has three parts.  


1. Make a function for a single request. 
2. Turn this function into list of tasks \(tasks is the nomenclature for asyncio library\)
3.  Execute the list of tasks.  

## Step 0. Imports

Nothing out of the ordinary except for nest\_asyncio. I had a lot of trouble with the`event_loop`. The `event_loop` can throw a lot of errors while running in the Jupyter notebook. Using nest\_asyncio solves the problems in the notebook and it works in an app with no problems. 

{% embed url="https://gist.github.com/crawftv/979dd2a32e920c1a66858603e8dd6f04" caption="Imports" %}

## Step 1. Define the function

Putting `nest_aysncio.apply()` at the top of the program, just under the the imports is how to implement the module. This line of code is the answer to many headaches while working with asyncio.  
This function is our example function for demonstrating the pattern, use your own function if you are comfortable. 

`request_song_info` takes three parameters.

1. **session:** session is the requests.Session\(\) object. The session speeds up requests by reusing cookies and TCP connections.
2. **song\_num:** an integer that is concatenated to the request url. 
3. **song\_urls:** our data structure for these requests. song\_urls is updated for every requests. Each item is a tuple of the song\_num and the information we are getting from the request.

{% embed url="https://gist.github.com/crawftv/8bdfbe8ce23903cc816f7a5639ce195e" caption="Define our web scraping function" %}

## Step 2. Gather the tasks

The doc string in the function is a good high-level description of how the function works.  
`Executor` is a subclass that uses a pool of workers to execute calls asynchronously.`ThreadPoolExecutor` is a subclass of `Executor` .  `get_event_loop` gets or creates the `event_loop`.  `Event_loop` is the core of the asyncio application and runs the tasks. `tasks` is a list comprehension which creates a list of individual function calls. `loop.run_in_executor` arranges for the function to be called in the executor. It takes the `executor`, the function we want to run, and the functions parameters in the form of `*(param1, param2, param3)`. Since it uses a list comprehension, we need list of things to parameters to iterate over. It can be a list of ints, strings, other objects.  
`aysncio.gather(*tasks)`runs the tasks concurrently and gathers the  results. Since the original function updates the data structure, we do nothing with the response. 

The parameters for the `get_data_asynchronously` are the parameters used to form the list comprehension for the task creation. \(song\_num is not passed because it is iterated over in the range.\)

{% embed url="https://gist.github.com/crawftv/ebe769a39d28c6163ab36175a62d8730" caption="Gather a list of tasks" %}

## Step 3: Execute the tasks

{% embed url="https://gist.github.com/crawftv/1f752308d60ff560b533763f8637b278" caption="Execute the list of tasks" %}

`create_task` schedules the execution of the tasks. \(in python3.6, replace `create_task` with `ensure_future`. \) Get the event loop again. `loop.run_until_complete` finally executes the tasks. 

## Tips and tricks

If your tasks come from multiple functions, you can add two lists together i.e. `tasks = [ loop.run_in_executor(...) for x in y]  + [loop.run_in_executor(...) for v in w ]`

To reuse this code, only a few changes need to be made. 

1. Change your function at Step 1.
2.  Update the parameters for `get_data_asynchronous` function. change the function inside `loop.run_in_executor(...)` 
3. Update the parameters for `execute_async_event_loop` and `asyncio.create_task(get_data_asynchronous(...))`.

