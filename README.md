# Python intermediate tutorial for corporate exams
![image](https://user-images.githubusercontent.com/61017530/199868413-3ec92fe0-0981-4560-a1c8-db1658d3f14e.png)

# Table of Contents
1. [Operators](#o)
    1. [Comparison Operators](#co)
    2. [Arithmetic Operators](#ao)
    3. [Logical Operators](#lo)
    4. [Bool Method](#bool)
    5. [is and ==](#is==)
2. [LEGB](#LEGB)
3. [Private Methods](#pm)
4. [Copy and Deepcopy](#cdc)

# Operators <a name="o"></a>

## Comparison operators <a name="co"></a>
Operator **\>** is **NOT** limited to comparisons between int/float. 
```python
5.5 > True  # True (between float and boolean)
1 > True  # False (between int and boolean)
'ab'>'aa'  # True (between str and str)
[10,5]<[6,20,30]  # True, 6>20
[3,100,[5,6]]<[3,4,6]  # False, 100>4
[1,2,[5,6]]<[1,2,6]  # typeError: '<' not supported between instances of 'list' and 'int'
5 > 3 == 3  # True 5>3 and 3==3
```

## Arithmetic operators <a name="ao"></a>
Operator **+** can concatenate tupes.
```python
(1, 2)+(3, 4)  # (1,2,3,4)
```

Operator **\*** can multiply tupes, but if there is only one item in the tuple, \ * operation will convert it to the item type unless you explicitly add a comma.
```python
(1, 2)*3  # (1,2,1,2,1,2)
(1)*3  # int -> 3
(1,)*3 # (1, 1, 1)

("abc")*3  # "abcabcabc"
("abc", )*3  # ("abc", "abc", "abc")
```

## Logical operators <a name="lo"></a>
Operation **AND** and **OR** can be used between tuples and lists respectively. **AND** takes the last tuple/list as the return, while **OR** returns the first tuple/list. 
```python
a = [1, 2, 3] and [4, 5, 6] and [7, 8, 9]
b = [1, 2, 3] or [4, 5, 6] or [7, 8, 9]
c = [1, 2, 3] and [4, 5, 6] or [7, 8, 9]
print(a)  # [7, 8, 9]
print(b)  # [1, 2, 3]
print(c)  # [4, 5, 6]
```

## bool method <a name="bool"></a>
```python
bool(0)  # False
bool(bool(0.0001))  # True
bool("0")  # True
bool([1,2,3,4])  # True
bool((1,2,3,4))  # True
bool({1:2})  # True
```

## is and == <a name="is=="></a>
**is** checks if refer to the same object, whereas **==** compares the value or equality of two objects invoking the \_\_eq\_\_ method.
```python
class Person:
    def __init__(self, age):
        self.age = age

    def __eq__(self, other):
        return self.age == other.age

class Animal:
    def __init__(self, age):
        self.age = age

    def __eq__(self, other):
        return self.age == other.age

John = Person(25)
Peter = Person(25)
Bird = Animal(25)
print(John is Peter) # False
print(John == Peter) # True
print(John == Bird)  # True
```

# LEGB <a name="LEGB"></a>
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

## Example 1
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

## Example 2
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

## Example 3
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

## Example 4
If you want to extend the searching scope, go for *nonlocal*
```python
count = 0
def outer():
    count = 10
    def inner():
        nonlocal count
        count = 20
        print(count)  # count = 20
        def inner2():
            nonlocal count
            count = 30
            print(count)  # count = 30
        inner2()  # count = 30
        print(count)  # count = 30
    inner()  # count = 30
    print(count)  # count = 30
outer()  # count = 0
print(count)  # count = 0
```
>20 30 30 30 0

## Example 5
You must declare the variable first when using *nonlocal*.
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

## Example 6
```python
name = "global variable"
def f():
    def inner():
        nonlocal name
        name = "local variable"

    name = "global variable"
    print(name)
    inner()
    print(name)
f()
```
>global variable
>
>local variable

# Private methods <a name="pm"></a>
The single underscore (\_) is for indication only and does **NOT** prevent the method from being overrided. Instead, double underscores (\_\_) should be used.

```python
class Parent:
    def __init__(self, iterable):
        self.container = []
        self._item_print(iterable)
        self.__append(iterable)

    def _item_print(self, iterable):
        print("Parent private method.")

    def __append(self, iterable):
        for item in iterable:
            self.container.append(item)

class Children(Parent):
    def _item_print(self, iterable):  # _item_print is overrided
        print("Children private method.")

    def __append(self, iterable):  # __append is NOT overrided
        for item in iterable:
            print("It is an override method.")
            continue

p = Parent(("a", "b"))
print(p.container)
c = Children(("c", "d"))
print(c.container)  # still implement parent's method
```
>Parent private method.
>
>['a', 'b']
>
>Children private method.
>
>['c', 'd']


# Copy and deepcopy <a name="cdc"></a>
For primitive types, copy equals to deepcopy. As for reference types, copy inserts references while deepcopy constructs a new compound object. 

## Example 1
```python 
import copy

listA = [0, [0, 0]]
listB = copy.copy(listA)
listC = copy.deepcopy(listA)
listA[0] = 1
listA[1][0] = 2
print(listA, listB, listC)
print(id(listA[1]), id(listB[1]), id(listC))
```

>[1, [2, 0]] [0, [2, 0]] [0, [0, 0]]
>
>2247372890504 2247372890504 2247372861320

## Example 2
**=** gives the same id, and **list** equals to copy method.
```python
l1 = [[1, 2], [3, 4], [5, 6]]
l2 = list(l1)
for i in range(len(l1)):
    print(id(l1[i]), id(l2[i]))
l3 = l1
l1.append(7)
print(l1, l2, l3)
```

>2247391114888 2247391114888
>
>2247391114760 2247391114760
>
>2247391114824 2247391114824
>
>[[1, 2], [3, 4], [5, 6], 7] [[1, 2], [3, 4], [5, 6]] [[1, 2], [3, 4], [5, 6], 7]

## Example 3
Using **\*** on list of reference types equals to copy method, and all item generated have the same id.
```python
l1 = ["FlyingSPA"]
l2 = [l1]*3
for item in l2:
    print(id(item))
l2[1][0] = "Ramen"
print(l2)
```

>2247391211016
>
>2247391211016
>
>2247391211016
>
>[['Ramen'], ['Ramen'], ['Ramen']]
