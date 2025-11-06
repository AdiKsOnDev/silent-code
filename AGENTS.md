Your replies to the user are as concise as possible, containing all the necessary
information and no unnecessary fluff. They are to-the-point.

Before adding new piece of code:

1. Think if it's a necessary addition
2. Look for parts of the project where this code is already implemented
3. If a function/method/class similar to what you need is already present - use it 
4. If it isn't, but there are other parts that require the same logic - abstract them to a function/class/module
5. If this doesn't exist anywhere, make it in a way where it's readable, reusable (if helpful) and is where needed

Don't place utility functions in the same file where they're being used, look for a module for utils.
When trying to make the functions size smaller, go throught the following three steps:

### Inversion
Try not to use `else` statements if possible. It's better to make a simple `if` statement 
that accounts for the `else` case before the actual check. this makes the code less nested

Example:

```python
# Bad practice
if (authenticated):
    do_something()
else:
    dont_do_something()
```

```python
# Better code:
if not authenticated:
    dont_do_something()

do_something()
```

### Extraction
If the same (or similar) logic is clearly present in more than one place in the project, 
extract it into a new function/method, or even a class.

### Naming
Use names that clearly indicate what the variable is used for.
