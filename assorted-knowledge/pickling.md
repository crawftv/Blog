---
description: Saving data structures
---

# Pickling

> _“Pickling”_ is the process whereby a Python object hierarchy is converted into a byte stream

Many times large data projects can crash a Jupyter kernel. Pickling a data structure \(DataFrame , array, list, etc.\) consistently is a good practice to prevent losing a lot of work. Pickling data is also a good way to transfer data if you want to start working in a new workbook.

Pickle plays a part in my data analysis process. In one notebook I scrape, combine and clean data. Once I am comfortable with DataFrame and am ready to start analysis, I pickle the DataFrame and load it into a new notebook.

### What should I pickle something rather than save it as a .csv?

More complex data structures in a dataframe can lose their form. A problem I run into a lot is that a row of `list` types becomes a `string` type of the form `"["item_1",...]"` . 

## Pickling a thing

Below is my pattern for using pickle to save a data structure. 

{% embed url="https://gist.github.com/crawftv/bb578843cd2ffc0827e4d0d06a28a8b2" caption="python pickle pattern" %}

## Unpickling that thing

This is the pattern for unpickling the that you pickled above. 

{% embed url="https://gist.github.com/crawftv/b97accd3e34a840d6e73ff012b6e6c72" caption="python unpickle pattern" %}





