# Default Values for classes

You can't set a default value for classes in the `__init__` function. For instance:

{% code title="Wrong way" %}
```python
class A:
    def __init__(self,data=[]):
        self.data = data
a = A()
a.data.append(1)
b = A()
b.data == [1]
```
{% endcode %}

The above returns `True`.  

The appropriate pattern is to set the default to `None` and adjust in the `__init__` function.

{% code title="Right Way" %}
```python
class A:
    def __init__(self,data=None):
        if data is None:
            data = []
        self.data = data
a = A()
a.data.append(1)
b = A()
b.data == [1]

```
{% endcode %}

This now returns `False`.

You could also use a ternary function.

{% code title="Ternary Way" %}
```python
class A:
    def __init__(self,data=None):
       self.data = data if data is not None else []
a = A()
a.data.append(1)
b = A()
b.data == [1]
```
{% endcode %}

