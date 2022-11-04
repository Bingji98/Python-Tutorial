# Python tutorial for corporate exam
![image](https://user-images.githubusercontent.com/61017530/199868413-3ec92fe0-0981-4560-a1c8-db1658d3f14e.png)

# Table of Contents
1. [LEGB](#LEGB)


## LEGB <a name="LEGB"></a>
The LEGB rule is a kind of name lookup procedure, which determines the order in which Python looks up names. LEGB stand for **Local, Enclosing, Global, and Built-in** scopes. **Enclosing (or nonlocal)** scope is a special scope that only exists for nested functions.

It is worth noting that the variable explicitly declared as the global variable is not necessarily declared at the module level.

```python
def global_test():
    global x
    x = "global x"
    print(x)
global_test()
```

> global x
