# OOPs in Python

------------------------------------------------------------------------

# 1. What is OOP?

**OOP (Object-Oriented Programming)** is a way of writing programs by
organizing code around **objects**.

An object contains:

-   **Data** → attributes / variables
-   **Behavior** → methods / functions

A `Student` object, for example, can have `name` and `age` as data and
`introduce()` as behavior.

------------------------------------------------------------------------

# 2. Class

A **class** is a blueprint or template used to create objects.

``` python
class Student:
    pass
```

`Student` is the class. It is a blueprint; it is not one particular
student.

------------------------------------------------------------------------

# 3. Object

An **object** is an instance of a class.

``` python
class Student:
    pass

student1 = Student()
student2 = Student()

print(type(student1))
print(type(student2))
```

### Output

``` text
<class '__main__.Student'>
<class '__main__.Student'>
```

### Remember

``` text
Class  = blueprint
Object = actual instance created from the blueprint
```

------------------------------------------------------------------------

# 4. Constructor --- `__init__()`

`__init__()` is a special method automatically called when an object is
initialized. It is commonly used to initialize object data.

``` python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age

student1 = Student("Adi", 21)

print(student1.name)
print(student1.age)
```

### Output

``` text
Adi
21
```

When we write:

``` python
student1 = Student("Adi", 21)
```

Python initializes the object and calls `__init__()` with those values.

------------------------------------------------------------------------

# 5. What is `self`?

`self` refers to the **current object**.

``` python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")

student1 = Student("Adi")
student2 = Student("Rahul")

student1.introduce()
student2.introduce()
```

### Output

``` text
My name is Adi
My name is Rahul
```

When `student1.introduce()` runs, `self` refers to `student1`. When
`student2.introduce()` runs, `self` refers to `student2`.

### Important

``` text
name       → parameter received by the method
self.name  → attribute stored inside the current object
```

------------------------------------------------------------------------

# 6. Instance Variables

An instance variable is data stored separately inside each object.

``` python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age

student1 = Student("Adi", 21)
student2 = Student("Rahul", 22)

student1.age = 25

print(student1.name, student1.age)
print(student2.name, student2.age)
```

### Output

``` text
Adi 25
Rahul 22
```

Each object has its own instance data.

------------------------------------------------------------------------

# 7. Instance Method

A method is a function defined inside a class. An instance method
normally takes `self` and works with object data.

``` python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        print(f"Hi, I am {self.name} and I am {self.age} years old.")

student1 = Student("Adi", 21)
student1.introduce()
```

### Output

``` text
Hi, I am Adi and I am 21 years old.
```

------------------------------------------------------------------------

# 8. Why `self.name = name`?

This is one of the most important ideas.

``` python
def __init__(self, name):
    self.name = name
```

The two `name`s have different meanings:

``` text
name       → parameter / temporary input
self.name  → attribute stored in the object
```

You do **not** have to store every constructor parameter.

``` python
class Calculator:

    def __init__(self, number):
        self.result = number * 2
```

Here `number` is only needed temporarily. We do not need `self.number`
unless the object needs to remember it later.

### Rule

> Store a value with `self` only when the object needs that value after
> the current method finishes.

------------------------------------------------------------------------

# 9. Encapsulation

**Encapsulation** means keeping related data and behavior together
inside an object and controlling how the data is accessed.

``` python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        self._balance += amount

    def get_balance(self):
        return self._balance

account = BankAccount(1000)
account.deposit(500)

print(account.get_balance())
```

### Output

``` text
1500
```

`_balance` is a Python naming convention indicating that the attribute
is intended for internal use.

### Remember

> Encapsulation = organize/protect data and related behavior.

------------------------------------------------------------------------

# 10. Name Mangling

Python supports double-underscore attributes.

``` python
class BankAccount:

    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance

account = BankAccount(1000)

print(account.get_balance())
```

### Output

``` text
1000
```

`__balance` uses Python's name-mangling mechanism. It is not absolute
security, but helps prevent accidental direct access and name conflicts.

------------------------------------------------------------------------

# 11. Abstraction

**Abstraction** means hiding unnecessary implementation details and
exposing only what the user needs.

``` python
class CoffeeMachine:

    def make_coffee(self):
        self._boil_water()
        self._add_coffee()
        print("Coffee is ready")

    def _boil_water(self):
        print("Boiling water")

    def _add_coffee(self):
        print("Adding coffee")

machine = CoffeeMachine()
machine.make_coffee()
```

### Output

``` text
Boiling water
Adding coffee
Coffee is ready
```

The user only needs:

``` python
machine.make_coffee()
```

They do not need to know every internal step.

### Remember

> Abstraction = hide unnecessary complexity.

------------------------------------------------------------------------

# 12. Inheritance

Inheritance allows a child class to reuse and extend functionality from
a parent class.

``` python
class Animal:

    def eat(self):
        print("Animal is eating")

class Dog(Animal):
    pass

dog = Dog()
dog.eat()
```

### Output

``` text
Animal is eating
```

`Dog` inherits `eat()` from `Animal`.

### Remember

``` text
Parent → common functionality
Child  → inherits and can add/change functionality
```

------------------------------------------------------------------------

# 13. Method Overriding

A child class can provide its own version of a parent method.

``` python
class Animal:

    def speak(self):
        print("Animal makes a sound")

class Dog(Animal):

    def speak(self):
        print("Dog barks")

animal = Animal()
dog = Dog()

animal.speak()
dog.speak()
```

### Output

``` text
Animal makes a sound
Dog barks
```

The `Dog` version overrides the inherited version.

------------------------------------------------------------------------

# 14. Polymorphism

**Polymorphism** means the same interface/method call can produce
different behavior depending on the object.

``` python
class Dog:

    def speak(self):
        print("Dog barks")

class Cat:

    def speak(self):
        print("Cat meows")

animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

### Output

``` text
Dog barks
Cat meows
```

The same:

``` python
animal.speak()
```

behaves differently for different objects.

### Memory trick

``` text
Poly = many
Morph = forms
Polymorphism = one interface, many forms/behaviors
```

------------------------------------------------------------------------

# 15. Composition

**Composition** means one object contains or uses another object.

``` python
class Engine:

    def start(self):
        print("Engine started")

class Car:

    def __init__(self):
        self.engine = Engine()

    def start_car(self):
        self.engine.start()
        print("Car started")

car = Car()
car.start_car()
```

### Output

``` text
Engine started
Car started
```

The `Car` object has an `Engine` object.

``` text
Car
 |
 └── Engine
```

### Remember

> Composition usually represents a **HAS-A** relationship.

``` text
Car HAS-A Engine
Calculator HAS-A DataExtractor
Calculator HAS-A DataAnalyzer
```

------------------------------------------------------------------------

# 16. `@staticmethod`

A static method belongs to a class but does not need `self` or `cls`.

``` python
class Calculator:

    @staticmethod
    def add(a, b):
        return a + b

print(Calculator.add(10, 20))
```

### Output

``` text
30
```

Use a static method when the operation logically belongs to the class
but does not need object or class state.

------------------------------------------------------------------------

# 17. `@classmethod`

A class method receives `cls`, which refers to the class itself.

``` python
class Student:

    school = "ABC School"

    @classmethod
    def get_school(cls):
        return cls.school

print(Student.get_school())
```

### Output

``` text
ABC School
```

### Remember

``` text
self → current object
cls  → current class
```

------------------------------------------------------------------------

# 18. Magic / Dunder Methods

Special methods with double underscores are often called **dunder
methods**.

Examples:

``` text
__init__
__str__
__eq__
```

------------------------------------------------------------------------

# 19. OOP Four Pillars --- Last-Minute Revision

The four commonly taught pillars are:

``` text
1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism
```

### One-line revision

**Encapsulation** → Keep data and related methods together and control
access.

**Abstraction** → Hide unnecessary implementation details.

**Inheritance** → Child class reuses/extends parent functionality.

**Polymorphism** → Same interface/method call can behave differently for
different objects.

------------------------------------------------------------------------

# 20. OOP Quick Cheat Sheet

``` text
Class
→ Blueprint

Object
→ Instance of a class

__init__
→ Initializes object

self
→ Current object

self.name = name
→ Store parameter as object attribute

Method
→ Function inside a class

Encapsulation
→ Organize/protect data and behavior

Abstraction
→ Hide unnecessary complexity

Inheritance
→ IS-A relationship

Composition
→ HAS-A relationship

Polymorphism
→ Same interface, different behavior

__str__
→ String representation

__eq__
→ Defines ==

@staticmethod
→ No self / cls needed

@classmethod
→ Receives cls
```

------------------------------------------------------------------------

# SOLID Principles

# 21. What is SOLID?

SOLID is a set of five principles for designing maintainable and
flexible object-oriented software.

``` text
S → Single Responsibility Principle
O → Open/Closed Principle
L → Liskov Substitution Principle
I → Interface Segregation Principle
D → Dependency Inversion Principle
```

------------------------------------------------------------------------

# 22. S --- Single Responsibility Principle (SRP)

### Simple definition

> A class should have one main responsibility.

Or:

> **One class → one main job.**

Bad design:

``` python
class Employee:

    def calculate_salary(self):
        pass

    def save_to_database(self):
        pass

    def generate_report(self):
        pass

    def send_email(self):
        pass
```

This class has too many responsibilities.

Better:

``` python
class SalaryCalculator:

    def calculate(self):
        pass


class EmployeeRepository:

    def save(self):
        pass


class ReportGenerator:

    def generate(self):
        pass
```

Each class has a focused responsibility.

------------------------------------------------------------------------

# 23. O --- Open/Closed Principle (OCP)

### Simple definition

> Software should be open for extension but closed for unnecessary
> modification.

Example:

``` python
class Shape:

    def area(self):
        raise NotImplementedError


class Rectangle(Shape):

    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height


class Circle(Shape):

    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius


print(Rectangle(5, 4).area())
print(Circle(2).area())
```

### Output

``` text
20
12.56
```

We can add another shape without changing the existing Rectangle and
Circle classes.

``` python
class Triangle(Shape):

    def __init__(self, base, height):
        self.base = base
        self.height = height

    def area(self):
        return 0.5 * self.base * self.height
```

### Remember

``` text
Open for extension
Closed for modification
```

------------------------------------------------------------------------

# 24. L --- Liskov Substitution Principle (LSP)

### Simple definition

> A child class should be usable wherever its parent class is expected
> without breaking the expected behavior.

``` python
class Animal:

    def speak(self):
        print("Animal sound")


class Dog(Animal):

    def speak(self):
        print("Dog barks")


class Cat(Animal):

    def speak(self):
        print("Cat meows")


animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

### Output

``` text
Dog barks
Cat meows
```

Both child objects can be used where an `Animal` is expected.

### Easy memory

> **Child should properly behave as the parent type.**

------------------------------------------------------------------------

# 25. I --- Interface Segregation Principle (ISP)

### Simple definition

> A class should not be forced to depend on methods it does not need.

Bad design:

``` python
class Machine:

    def print(self):
        pass

    def scan(self):
        pass

    def fax(self):
        pass
```

A simple printer may only need `print()`.

A better design separates responsibilities:

``` python
class Printer:

    def print(self):
        print("Printing")


class Scanner:

    def scan(self):
        print("Scanning")


printer = Printer()
scanner = Scanner()

printer.print()
scanner.scan()
```

### Output

``` text
Printing
Scanning
```

### Remember

> Don't force a class to depend on things it doesn't use.

------------------------------------------------------------------------

# 26. D --- Dependency Inversion Principle (DIP)

### Simple definition

> High-level code should not be tightly coupled to low-level
> implementation details.

Tightly coupled version:

``` python
class EmailSender:

    def send(self):
        print("Sending email")


class Notification:

    def __init__(self):
        self.sender = EmailSender()

    def notify(self):
        self.sender.send()


notification = Notification()
notification.notify()
```

`Notification` is directly tied to `EmailSender`.

A more flexible version passes the dependency from outside:

``` python
class Notification:

    def __init__(self, sender):
        self.sender = sender

    def notify(self):
        self.sender.send()


class EmailSender:

    def send(self):
        print("Sending email")


class SMSSender:

    def send(self):
        print("Sending SMS")


notification = Notification(EmailSender())
notification.notify()

notification = Notification(SMSSender())
notification.notify()
```

### Output

``` text
Sending email
Sending SMS
```

The high-level `Notification` does not need to know how sending is
implemented.

------------------------------------------------------------------------

# 27. Dependency Injection

The previous example demonstrates **dependency injection**.

Instead of:

``` python
self.sender = EmailSender()
```

we give the dependency from outside:

``` python
Notification(EmailSender())
```

or:

``` python
Notification(SMSSender())
```

### Remember

> Dependency Injection = give an object the things it needs instead of
> making it create everything itself.

------------------------------------------------------------------------

# 28. Common Review Questions

### Q1. What is OOP?

> OOP is a programming approach where we organize software around
> objects that contain data and behavior.

### Q2. What is a class?

> A class is a blueprint or template used to create objects.

### Q3. What is an object?

> An object is an instance of a class.

### Q4. What is `__init__()`?

> `__init__()` is a special method automatically called when an object
> is initialized. It is commonly used to initialize instance variables.

### Q5. What is `self`?

> `self` refers to the current object and is used to access its instance
> variables and methods.

### Q6. Why do we write `self.name = name`?

> `name` is the parameter received by the method, while `self.name`
> stores that value as an attribute of the current object so it can be
> used later.

### Q7. What are the four pillars of OOP?

> Encapsulation, Abstraction, Inheritance, and Polymorphism.

### Q8. What is encapsulation?

> Encapsulation means keeping related data and behavior together inside
> an object and controlling how the data is accessed.

### Q9. What is abstraction?

> Abstraction means hiding unnecessary implementation details and
> exposing only what is required.

### Q10. What is inheritance?

> Inheritance allows a child class to reuse and extend functionality
> from a parent class.

### Q11. What is polymorphism?

> Polymorphism allows the same method/interface to behave differently
> depending on the object.

### Q12. What is composition?

> Composition means one object contains or uses another object. It
> represents a HAS-A relationship.

### Q13. Inheritance vs composition?

> Inheritance represents an IS-A relationship, while composition
> represents a HAS-A relationship.

### Q14. What is SOLID?

> SOLID is a set of five principles for designing maintainable and
> flexible object-oriented software.

### Q15. What is SRP?

> A class should have one main responsibility.

### Q16. What is OCP?

> Software should be open for extension but closed for unnecessary
> modification.

### Q17. What is LSP?

> Objects of a child class should be usable wherever objects of the
> parent class are expected without breaking the program.

### Q18. What is ISP?

> A class should not be forced to depend on methods it doesn't need.

### Q19. What is DIP?

> High-level modules should not be tightly coupled to low-level
> implementation details; dependencies should be abstracted and can be
> injected.
