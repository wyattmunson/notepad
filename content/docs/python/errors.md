---
title: "Errors in Python"
description: ""
summary: "Errors types and error handing in Python"
date: 2024-10-29T22:18:43-07:00
lastmod: 2024-10-29T22:18:43-07:00
weight: 32
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Basic Error Handling

Python uses try/except blocks to define code error handling. The `try` block defines the code that should run assuming there is no error. The `except` block defines the code that should fire if an error is encountered.

```python
try:
    undefined_function()
except:
    print('Error hit.')
```

```bash { title="Output" }
python main.py
↪ Error hit.
```

### Catch Specific Error Types

Catch specific [error types](#error-types) in the try/except block and handle errors differently. Additionally include an optional general `except` as the last exception of the try/except block.

```python
try:
    foo = 5 / 0
except ZeroDivisionError
    print('Divide by zero error encountered')
except:
    print('Some error occurred.')
```

```bash { title="Output" }
python main.py
↪ Divide by zero error encountered
```

## Raising an Error

Use the `raise` keyword to raise an error. This can be a general or specific [error type](#error-types).

```python
def check_transmit(self):
  if not hasattr(self, "transmit"):
      print("Error message for you to see.")
      print("ERROR: Call setPipes() to define transmit address")
      raise ValueError("Transmit not set. See more information above.")
```

## Error Types

Python includes many built-in error types that are thrown depending on the condition encountered in the code.

### Common Error Types

List of common error types. See [full error type list](#all-error-types) below.

| Error Type | Description                                                                                                                                                                                        |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ValueError | Raised when a correct argument type but an incorrect value is supplied to a function. For example, if you try to pass a string to a function that expects an integer, a ValueError will be raised. |
| TypeError  | Raised when an argument is of the wrong type. For example, if you try to pass a string to a function that expects an integer, a TypeError will be raised.                                          |
| IndexError | Raised when an index is out of range. For example, if you try to access the 10th element of a list that only has 5 elements, an IndexError will be raised.                                         |
| KeyError   | Raised when a key is not found in a dictionary. For example, if you try to access the key "foo" in a dictionary that does not have a key "foo", a KeyError will be raised.                         |
| NameError  | Raised when a name is not defined. For example, if you try to use the variable "foo" but the variable "foo" has not been defined, a NameError will be raised.                                      |

### All Error Types

Full list of built-in error types.

| Error Type            | Error Description                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| AssertionError        | Raised when the assert statement fails.                                                                                    |
| AttributeError        | Raised on the attribute assignment or reference fails.                                                                     |
| EOFError              | Raised when the input() function hits the end-of-file condition.                                                           |
| FloatingPointError    | Raised when a floating point operation fails.                                                                              |
| GeneratorExit         | Raised when a generator's close() method is called.                                                                        |
| ImportError           | Raised when the imported module is not found.                                                                              |
| IndexError            | Raised when the index of a sequence is out of range.                                                                       |
| KeyError              | Raised when a key is not found in a dictionary.                                                                            |
| KeyboardInterrupt     | Raised when the user hits the interrupt key (Ctrl+c or delete).                                                            |
| MemoryError           | Raised when an operation runs out of memory.                                                                               |
| NameError             | Raised when a variable is not found in the local or global scope.                                                          |
| NotImplementedError   | Raised by abstract methods.                                                                                                |
| OSError               | Raised when a system operation causes a system-related error.                                                              |
| OverflowError         | Raised when the result of an arithmetic operation is too large to be represented.                                          |
| ReferenceError        | Raised when a weak reference proxy is used to access a garbage collected referent.                                         |
| RuntimeError          | Raised when an error does not fall under any other category.                                                               |
| StopIteration         | Raised by the next() function to indicate that there is no further item to be returned by the iterator.                    |
| SyntaxError           | Raised by the parser when a syntax error is encountered.                                                                   |
| IndentationError      | Raised when there is an incorrect indentation.                                                                             |
| TabError              | Raised when the indentation consists of inconsistent tabs and spaces.                                                      |
| SystemError           | Raised when the interpreter detects internal error.                                                                        |
| SystemExit            | Raised by the sys.exit() function.                                                                                         |
| TypeError             | Raised when a function or operation is applied to an object of an incorrect type.                                          |
| UnboundLocalError     | Raised when a reference is made to a local variable in a function or method, but no value has been bound to that variable. |
| UnicodeError          | Raised when a Unicode-related encoding or decoding error occurs.                                                           |
| UnicodeEncodeError    | Raised when a Unicode-related error occurs during encoding.                                                                |
| UnicodeDecodeError    | Raised when a Unicode-related error occurs during decoding.                                                                |
| UnicodeTranslateError | Raised when a Unicode-related error occurs during translation.                                                             |
| ValueError            | Raised when a function gets an argument of correct type but improper value.                                                |
| ZeroDivisionError     | Raised when the second operand of a division or module operation is zero.                                                  |

## Set Variables in Try Catch

A variable can be set in a try/except assuming the variable is also defined in the except or an error is raised.

```python
try:
	cfg = {"spi": spi, "csn": undefined_var, "ce": 0}
except ValueError as e:
	print("Upstream error: ", e)
	raise ValueError("Last error rendered to user")

# spi can be defined in "try"
# spi can be defined in "except" or raise Error
print(spi)
```

> A variable can be set in a try/except assuming the variable is also defined in the except or an error is raised.
>
> ```python
> try:
> 	cfg = {"spi": spi, "csn": undefined_var, "ce": 0}
> except ValueError as e:
> 	print("Upstream error: ", e)
> 	raise ValueError("Last error rendered to user")
>
> # spi can be defined in "try"
> # spi can be defined in "except" or raise Error
> print(spi)
> ```

Here is a test

> Text in a callout

Regular paragraph

> Multiline 1 \
> Multiline 1 \
> Multiline 1
>
> - Bullet point 1
> - Bullet point 2

Regular paragraph
