---
description: What to do without a Case/Switch feature.
---

# Using a class attribute without knowing it's existence.

## Wrong Ways

My evolution to finding this pattern.   
Lets say you have a class and you want to see if it has an attribute. The first way I thought to do this was:

{% code-tabs %}
{% code-tabs-item title="Very wrong way" %}
```python
class A:
    ...
a = A()
if 'attribute' is in dir(a):
    #do something
else:
    #do something else
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then I found out about the `hasattr` function.

{% code-tabs %}
{% code-tabs-item title="less wrong way" %}
```python
class A:
    ...
a = A()
if hasattr(a,'attribute'):
    #do something
else:
    #do something else
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Right Way - "Easier to Ask for Forgiveness instead of Permission"

The Python community generally prefers "EAFP" rather than "LBYL" \(Look Before Your Leap\).

What this means in practice is to use a try and except block.

{% code-tabs %}
{% code-tabs-item title="Right Way" %}
```python
class A:
    ...
a = A()
try:
    #do something
except:
    #do something else

```
{% endcode-tabs-item %}
{% endcode-tabs %}







### Sources and References

1. [https://web.archive.org/web/20070929122422/http://mail.python.org/pipermail/python-list/2003-May/205182.html](https://web.archive.org/web/20070929122422/http://mail.python.org/pipermail/python-list/2003-May/205182.html)
2. h[tps://web.archive.org/web/20180411011411/http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html\#eafp-vs-lbyl](https://web.archive.org/web/20180411011411/http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html#eafp-vs-lbyl)
3. [https://stackoverflow.com/questions/610883/how-to-know-if-an-object-has-an-attribute-in-python](https://stackoverflow.com/questions/610883/how-to-know-if-an-object-has-an-attribute-in-python)

  


