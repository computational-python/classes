# Classes

---

layout: false


## Variables and Functions

Data: Variables
~~~python
str1 = "Hello World!"
str2 = "Python Programming"
lst = ['physics', 'chemistry', 1997, 2000]
~~~

Actions: Functions
~~~python
def printinfo(name, age):
    print("Name" , name)
    print("Age", age)
~~~

---

## Objects and Classes

### Procedure Oriented Programming

* functions and variables used to make program
* suitable for small/medium programs

### Object Oriented Programming

* objects are used to make a program
* objects collect data functions and variable into one single unit
* suitable for large programs

**Everything in Python is an *object*, and almost everything has attributes
(data) and methods (functions for manipulating data)**

---


## Class - a blueprint for creation of an object

~~~python
class Person:
    def __init__(self, given_name, surname):
        self.given_name = given_name
        self.surname = surname

    def __str__(self):
        return f"Person: {self.given_name} {self.surname}"

~~~

* Class definition in Python

~~~
class Name:
    ...
~~~

* Most important special method is a *initializer*

~~~
def __init__(self, <list of parameters>):
    ...
~~~

---

*  Ordinary class instance methods are defined as

~~~
def name_of_method(self, <list of paramters>):
    ...
~~~

* self is passed to all class methods and has the following meaning
* In the constructor init refers to newly created object
* In ordinary instance methods it refers to the object for which this method is called

*Note: the `__init__` method is often called a constructor, in analogy with
other OO languages, but the object has already been constructed entering the
function and is refered to with the `self` variable. Initializer would be a
more correct description*

---

## Using objects in a program

* creating an **instance** of the class in a program

~~~python
>>> p = Person("Adam", "Smith")
~~~

* accessing and modifying instance attributes

~~~python
>>> print(p.given_name)
Adam
>>> p.given_name = 'John'
~~~

* calling instance methods

~~~python
p.display_person()
~~~

---

## Instance attributes vs Class attributes

~~~python
class Person:
    """
    Person class with class and instance attributes

    class attribute:
        number - counts number of Person instances created
    instance attributes:
        given_name - person's given name
        surname - person's surname
    """
    number = 0

    def __init__(self, given_name, surname):
        self.given_name = given_name
        self.surname = surname
        Person.number += 1

    def __str__(self):
        return f"Person: {self.given_name} {self.surname}"

~~~

* class attributes are shared by instances
* instance attributes are unique to each instance

---

## Special methods and overloading

* initializer: creates `p` and  calls `p.__init__()`
~~~
p = ClassName()
~~~

* official string representation: calls `p.__repr__()`
~~~
repr(p)
~~~

* informal string representation: calls `p.__str__()`
~~~
str(p) 
~~~

* getting attribute: calls `p.__getattribute__('attr')`
~~~
p.attr
~~~

---

* setting attribute: calls `p.__setattribute__('attr', value)`
~~~
p.attr  = value
~~~

* getting list of attributes: calls `p.__dir__()`
~~~
dir(p)
~~~

* overloading binary operators:
~~~
p + q: calls p.__add__(q)
p - q: calls p.__sub__(q)
p * q: calls p.__mul__(q)
p / q: calls p.__truediv__(q)
~~~

---

## Class inheritance

* Making a derived class from a base class:

~~~
class DerivedClassName(BaseClass): #inheriting from one base class
    ...
~~~

~~~
class DerivedClassName(BaseClass1, BaseClass2): #inheriting from multiple base classes
    ...
~~~

* Private attributes are not inherited

* Relationship between derived and base classes

~~~
issubclass(DerivedClass, BaseClass) #True or False
~~~

* Relationship between instance and class

~~~
isinstance(Object, Class) # True or False
~~~

---

### Simple example of parent (base) and child (derived) classes

~~~python
>>> class Parent:
...    def __init__(self):
...        print("Base constructor")
...
...    def base_method(self):
...        print('Calling base method')

>>> class Child(Parent):
...     def __init__(self):
...         print("Derived constructor")
...
...     def derived_method(self):
...         print('Calling derived method')

~~~
~~~
>>> c = Child()
Derived constructor
>>> c.base_method()
Calling base method
>>> c.derived_method()
Calling derived method
>>>
~~~

---

### When to use inheritance?

* to avoid replication of code (same attributes and methods in different
  classes)

* to take advantages of existing Python classes

* when similar if statements are repeated throughout code

~~~
import moduleName
class DerivedClass(moduleName.BaseClass):
    ...
    # override/add any functions here.
~~~

---

### Example employee class

~~~python
>>> class Person:
...     def __init__(self, given_name, surname):
...         self.given_name = given_name
...         self.surname = surname
... 
...     def get_person(self):
...         return "Person : " + self.given_name + " " + self.surname

>>> class Employee(Person):
...    def __init__(self, given_name, surname, salary):
...        super().__init__(given_name, surname)
...        self.salary = salary
...
...    def get_employee(self):
...        return self.get_person() + ", salary: " + str(self.salary)

~~~

~~~
>>> e = Employee('John', 'Doe', 29500)
>>> print(e.get_employee())
Person : John Doe, salary: 29500

~~~

---

### Replacing repeated if-statements with inheritance

~~~python
>>> def action(animal, name):
... 
...     print(name, 'is a', animal)
... 
...     if animal == 'Cat':
...         print(name, "says 'Meow'!")
...     elif animal == 'Dog':
...         print(name, "says 'Bow-wow!'")
...     else:
...         print("Don't know what", name, "sounds like")
... 
... 
...     if animal == 'Cat':
...         print(name, "chases mouse")
...     elif animal == 'Dog':
...         print(name, "chases cat")

...     else:
...         print("Don't know what", name, "chases")

~~~

Consider adding more animals and more behaviours - 
this pattern becomes difficult to maintain in larger codes

~~~
>>> action('Cat', 'Felix')
Felix is a Cat
Felix says 'Meow'!
Felix chases mouse

~~~

---
~~~python
>>> class Animal:
...     def __init__(self, name):
...         self.name = name
... 
...     def action(self):
...         print(self.name, 'is a', self.__class__.__name__)
...         self.make_sound()
...         self.chase()
... 
...     def make_sound(self):
...         print("Don't know what", self.name, "sounds like")
... 
...     def chase(self):
...         print("Don't know what", self.name, "chases")
... 
>>> class Dog(Animal):
...     def make_sound(self):
...         print(self.name, "says 'Bow-wow!'")
... 
...     def chase(self):
...         print(self.name, "chases cat")
... 
>>> class Cat(Animal):
...     def make_sound(self):
...         print(self.name, "says 'Meow'!")
... 
...     def chase(self):
...         print(self.name, "chases mouse")

~~~
~~~
>>> Cat('Felix').action()
Felix is a Cat
Felix says 'Meow'!
Felix chases mouse

~~~


---

## Class diagrams

Are often used to illustrate dependencies between classes

<img src="{{ base }}/img/classes.png">

* each class is represented by a box
* each box is divided name, attributes, methods
* arrows indicate inheritance relationships

---

## Static methods

Static methods are ordinary functions, living in the class namespace. They
might as well be defined outside the class, except that they are now called 
with a class name prefix. They do not depend on an instance, and do not have a
`self` parameter.

Static methods are defined with the `@staticmethod` decorator

~~~
>>> class A:
...    def instance_method(self):
...        print("In instance_method")
...
...    @staticmethod
...    def static_method():
...        print("In static method")

~~~

~~~
>>> A().instance_method()
In instance_method
>>> A.static_method()
In static method

~~~

---

## Class methods

Class methods are often used as alternative constructors

~~~
class Dog:

    def __init__(self, breed, name):
        self.breed = breed

    @classmethod
    def snoopy(cls):
        return cls('Beagle', 'Snoopy')

dog = Dog.snoopy()
print(dog.breed, dog.name)

~~~
