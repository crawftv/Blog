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

Make sure to put this outside a while loop. Or else you will call this debugger recursively.

If you are  in a loop, use the pattern below. Or else you wil call the debugger your fist time through the loop, rather than just when it breaks. You will now be able to observe the state of the program when you program breaks.

```python
try:
    execute_code
except:
    import pdb;pdb.set_trace()
```



