# Pydantic as a data engineering tool

## What is pydantic?

Pydantic is a parsing and validation library in python. It is used to
ensure data is the correct type and passes constraints. It does this by hooking
in to python's type annotations allowing the user to declare the expected type
for the data and let Pydantic take care of the rest.

Pydantic works best when it's taking in untrusted data, if you are grabbing data
from a database and doing minimal type casting, then Pydantic might not be for you.
If, however, you are pulling data from external APIs or parsing user data then
Pydantic is a powerful tool to have on your belt.

## quick example

in this example we are making a `TestClass` which has 3 fields: an int, a str and
either an int or bool.

When we pass data in we can see that the string gets case as an int, the next arg
gets converted to a string and finally the `True` becomes a 1.

```python
from pydantic import BaseModel

class TestClass(BaseModel):

    field1 : int
    field2 : str
    field3 : int | bool


t = TestClass(field1= "1", field2 = 2, field3 = True)

print(repr(t))
# TestClass(field1=1, field2='2', field3=1)
```

So order matters in a Pydantic BaseModel. It will attempt to cast the data given
as each type in turn until that type doesn't raise an exception. This can be changed
by passing a `smart_union` argument. Or by defining a configuration class
(which we'll discuss later).

```python
from pydantic import BaseModel

class TestClass(BaseModel, smart_union=True):

    field1 : int
    field2 : str
    field3 : int | bool


t = TestClass(field1= "1", field2 = 2, field3 = True)

print(repr(t))
# TestClass(field1=1, field2='2', field3=True)
```

Now we can see that `field3` is correctly cast as a bool. smart_union tells Pydantic
to not just stop on the first valid case it finds, but to check all of the available types.
(Though this does come with a performance hit)

So with that quick introduction out of the way, what can you use pydantic for?
