# Pythonista

An extension of [python](/python), focusing on built in functions.

This page focuses on the [Built-in Functions](https://docs.python.org/3/library/functions.html)

## How can I get the absolute value of a number `num`?

```python
result = abs(num)
```

### Paste Example

```python
num_1 = -7
abs_num_1 = abs(num_1)
print(f"abs_num_1: {abs_num_1}")
```


## How can I check if every element in an array `values` is True?

```python
result = all(values)
```

### Paste Example

```python
values_1 = [True, True, True]
is_all = all(values_1)
print(f"is_all: {is_all}")

values_2 = [True, False, True]
is_all = all(values_2)
print(f"is_all: {is_all}")
```

## How can I check if any element in an array `values` is True?

```python
result = any(values)
```

### Paste Example

```python
values_1 = [True, True, True]
is_all = any(values_1)
print(f"is_all: {is_all}")

values_2 = [False, False, False]
is_all = any(values_2)
print(f"is_all: {is_all}")
```

## How can I get a string containing a printable representation of an object?

```python
my_string = ascii({'hello':'world'})
```

### Paste Example

```python
obj_as_string = ascii({'hello':'world'})
print(f"obj_as_string: {obj_as_string}")
```

## How can I convert an integer number to a binary string?

```python
my_binary_str = bin(3)
```

### Paste Example

```python
my_binary_str = bin(3)
print(f"my_binary_str: {my_binary_str}")
```

## How can I drop into a debugger at a specific point in my code?

```python
breakpoint()
```

### Paste Example

```python
print("Hello")
breakpoint()
print("World")
```

## How can I get an array of bytes (that's mutable)?

```python
encoded_str = str.encode("Hello World")
my_bytearray = bytearray(encoded_str)
```

```python
encoded_str = str.encode("Hello World")
my_bytearray = bytearray(encoded_str)
print(f"my_bytearray: {my_bytearray}")
# for idx, value in enumerate(my_bytearray):
#     print(f"At {idx}: {value}")
print("It's mutable:")
print(f"Before: {my_bytearray[3]}")
my_bytearray[3] = 109
print(f"After: {my_bytearray[3]}")
```

## What is a bytearray?

The bytearray class is a mutable sequence of integers in the range 0 <= x < 256

https://docs.python.org/3/library/functions.html#func-bytearray

## What is the difference between byte and bytearray?

The primary difference is that a bytes object is immutable, meaning that once created, you cannot modify its elements. By contrast, a bytearray object allows you to modify its elements

## How can I get an array of bytes (that's immutable)?

```python
encoded_str = str.encode("Hello World")
my_bytes = bytes(encoded_str)
print(f"my_bytes: {my_bytearray}")
# for idx, value in enumerate(my_bytes):
#     print(f"At {idx}: {value}")
print("It's immutable:")
print(f"Before: {my_bytes[3]}")
my_bytes[3] = 109
print(f"After: {my_bytes[3]}")
```

## How can I get the character whose unicode point is the integer i?

```python
chr(i)
```

### Paste Example

```python
my_chr = chr(98)
print(f"my_chr: {my_chr}")
```

## How can I convert value `n`, a string or number, to a complex number?

```python
complex(n)
```

### Paste Example

```python
s = "1+2j"
my_complex = complex(s)
print(f"my_complex: {my_complex}")

n = 15.667
my_complex = complex(n)
print(f"my_complex: {my_complex}")
```

## How can I delete an attribute from my Python object?

```python
my_obj = SomeObject()
delattr(my_obj, 'some_attr')
```

### Paste Example

```python
class Meal():
    def __init__(self, peas, carrots):
        self.peas = peas
        self.carrots = carrots
    
    def __str__(self):
        return str(self.__class__) + ": " + str(self.__dict__)

my_meal = Meal(True, True) 
print(f"my_meal: {my_meal}")

print("Goodbye Peas")

delattr(my_meal, 'peas')
print(f"my_meal: {my_meal}")
```
