# Pythonista

An extension of [python](/python), focusing on built in functions.

This page focuses on the [Built-in Functions](https://docs.python.org/3/library/functions.html)

## How can I get the absolute value of a number `num`?

```python
result = abs(num)
```

### Example

```python
num_1 = -7
abs_num_1 = abs(num_1)
print(f"abs_num_1: {abs_num_1}")
```


## How can I check if every element in an array `values` is True?

```python
result = all(values)
```

### Example

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

### Example

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

### Example

```python
obj_as_string = ascii({'hello':'world'})
print(f"obj_as_string: {obj_as_string}")
```
