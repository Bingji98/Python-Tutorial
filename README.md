# Python intermediate tutorial for corporate exam
![image](https://user-images.githubusercontent.com/61017530/199868413-3ec92fe0-0981-4560-a1c8-db1658d3f14e.png)

# Table of Contents
1. [LEGB](#LEGB)


## LEGB <a name="LEGB"></a>
The LEGB rule is a kind of name lookup procedure, which determines the order in which Python looks up names. LEGB stand for **Local, Enclosing, Global, and Built-in** scopes. **Enclosing (or nonlocal)** scope is a special scope that only exists for nested functions.

Blow is an example of enclosing scope:

*note: when you really want to change the enclosing variable, use the **nonlocal** key word*

```python
x = "global x"
def outer():
    x = "outer x"

    def inner1():
        x = "inner x"
        print(x)

    inner1()
    print(x)

outer()
print(x)
```
>inner x
>
>outer x
>
>global x

It is worth noting that the variable explicitly declared as the global variable is not necessarily declared at the module level. However, this way is *not recommended*.

```python
def global_test():
    global x
    x = "global x"
    print(x)
global_test()
```
> global x

Until now everything goes well right? Keep in mind, the company will always find a way to torture you. You will see why I said that after going through the following examples.

### Example 1
```python
name = "global variable"

def func1():
    print(name)

def func2():
    name = "local variable"
    func1()

func2()
```
>global variable

### Example 2
```python
name = "global variable"

def func2():
    name = "local variable"
    def func1():
        print(name)
    func1()

func2()
```
>local variable

### Example 3
```python
name = "global variable"

def func2():
    def func1():
        print(name)
    func1()
    name = "local variable"
 
func2()
```
>NameError: free variable 'name' referenced before assignment in enclosing scope

### Example 4
If you want to extend the searching scope, go for *nonlocal*
```python
count = 0
def outer():
    count = 10
    def inner():
        nonlocal count
        count = 20
        print(count) # count = 20
        def inner2():
            nonlocal count
            count = 30
            print(count) # count = 30
        inner2() # count = 30
        print(count) # count = 30
    inner() # count = 30
    print(count) # count = 30
outer() # count = 0
print(count) # count = 0
```
>20 30 30 30 0

### Example 5
```python
def outer():
    count = 10
    def inner():
        count = 20
        nonlocal count
        print(count)
    inner()
    print(count)
outer()
```
>SyntaxError: name 'count' is assigned to before nonlocal declaration
