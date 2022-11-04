# Python tutorial for corporate exam
![image](https://user-images.githubusercontent.com/61017530/199868413-3ec92fe0-0981-4560-a1c8-db1658d3f14e.png)

# Table of Contents
1. [LEGB](#LEGB)


## LEGB <a name="LEGB"></a>
The LEGB rule is a kind of name lookup procedure, which determines the order in which Python looks up names. LEGB stand for **Local, Enclosing, Global, and Built-in** scopes. **Enclosing (or nonlocal)** scope is a special scope that only exists for nested functions.

Blow is an example of enclosing scope:

*note: when you really want to change enclosing variable, use the **nonlocal** key word*

```python
def outer():
    x = "outer x"

    def inner1():
        x = "inner x"
        print(x)

    def inner2():
        print(x)

    inner1()
    inner2()
    print(x)

outer()
```

>inner x
>
>outer x
>
>outer x


It is worth noting that the variable explicitly declared as the global variable is not necessarily declared at the module level. However, this way is *not recommended*.

```python
def global_test():
    global x
    x = "global x"
    print(x)
global_test()
```

> global x

