---
description: Practical debugging tips
---

# Debugging

### Make sure you have saved the file.

### Check that your function only does one thing.

Once I was debugging something for hours only to realize my function was recursive

{% embed url="https://gist.github.com/crawftv/f1222f13c4be41e98a22673074e0fc0e" caption="Trying to be to clever" %}

### Use the Languages Debugger \(it's pdb in python\)

Call the debugger by using `import pdb;pdb.set_trace()`.

### Dealing with debugging loops

#### Option 1: Try except

If you are  in a loop, use the pattern below. Or else you wil call the debugger your fist time through the loop, rather than just when it breaks. You will now be able to observe the state of the program when you program breaks.

```python
try:
    execute_code
except:
    import pdb;pdb.set_trace()
```

#### Option 2: Take advantage of continue inside a loop

with the  `continue` debugger command and the `import pdb; pdb.set_trace()` code at the top of  a loop, the debugger will execute all code below and return you to the top of the loop. 

### Are you resetting something in your code?

One error I get when I'm merging two pieces of code is that I was working on something where I created a data structure, isolated a piece of code and recreate the data structure. Then when i merge the code I reset the data inside of the function.

```python

```

