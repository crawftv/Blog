# Default Values for classes

You can't set a default value for classes in the `__init__` function. For instance:  
`class A:  
    def __init__(self,data=[]):  
        self.data = data  
a = A()  
a.data.append(1)  
b = A()  
b.data == [1]`  
   

The above returns `True`.  

The appropriate pattern is to set the default to `None` and adjust in the `__init__` function.  
`class A:  
    def __init__(self,data=None):  
        if data is None:  
            data = []  
        self.data = data  
a = A()  
a.data.append(1)  
b = A()  
b.data == [1]`  


This now returns `False`.

  


