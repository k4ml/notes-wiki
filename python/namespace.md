# Python-namespace

Namespace in Python is derived from physical directory structure. Any directory under `PYTHONPATH` with at least `__init__.py` file is considered as a module. Example:-

```text
$ ls lib
customer order
$ ls lib/customer
__init__.py models.py
```

Putting `lib` under `PYTHONPATH` would make module customer and order available to import from Python:-

```text
$ python
Python 2.5.2 (r252:60911, Jul 22 2009, 15:35:03) 
[GCC 4.2.4 (Ubuntu 4.2.4-1ubuntu3)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import customer.models
>>> customer.models
<module 'customer.models' from '/home/kamal/python/lib/customer/models.py'>
```

This make it impossible to create a module in separate physical location but still sharing same common namespace. `setuptools` allow us to define

