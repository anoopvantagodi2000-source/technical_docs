# Object Oriented Programming (OOP)

## What is Object Oriented Programming?
- Object Oriented programming is paradigm that organizes code around objects.
- Objects combines Attributes(data) and Behavior(methods) as single unit.
- Attributes represents State Of a object.
- Method represents behavior/action of object.

## Why do we need Object Oriented Programming?
- It help us to break larger application to smaller,manageable components.
- It promotes code reusability.
- It groups related data and method together.

## The Pillars of Object Oriented Programming.

- Inheritance 
- Encapsulation
- Polymorphism
- Abstraction

## What is Class
 - Class is blueprint used to create an objects the class contains predefined attributes and methods for an object which they need to have, these attributes and methods can be accessed by . operators.

## What is Object?
- Object is an instance of the class these objects have there own data and methods which are defined in the classes.

- In below example we created class of Person within this class we have constructor which takes parameters name,age and initializes the attributes and the we have method which displays information about person.
```python
class Person:
    # constructor
    def __init__(self,name,age):
        self.name = name
        self.age = age
    def display_info(self):
        print(f"Hi I'm {self.name} and my age is {self.age}")
        
# Creating an object
user = Person("anoop",18)
user.display_info()
```

## Encapsulation 
- It is principal of Object-Oriented Programming that refers to bundling of attributes and methods which act on those attributes into single unit as class.
- It restricts direct access of attributes and allow only controlled modification by its method. This helps us for data hiding.
- In Python we can achieve encapsulation by using private attributes (prefixing _)
```python
class Person:
    def __init__(self,name,age):
        self.__name = name
        self.__age = age
        
    
    def get_name(self):
        return self.__name
    def get_age(self):
        return self.__age
    
    
    def display_info(self):
        print(f"Hi I'm {name} I'm {age} old")
    
# creating the object of class Person
user = Person("anoop", 28)

user.age # This line throws an error

user.get_age()
```
- In above example we created class Person having private attributes name,age then we created method display_info which displays information of person then we created an object then we tried to access private attribute from object using "." operator due to which Python throws an error because we can't access the private variables outside the class.

## Inheritance
- Inheritance is another principle of Object-Oriented Programming (OOP) that allows a class (child class) to reuse the properties and methods from another class (parent class). It supports hierarchical classification and promotes reusability of code.
```python
class Animal:
    def __init__(self,name,animal_type):
        self.__name = name
        self.__animal_type = animal_type
    def display_info(self):
        print(f"Hi I'M {self.__name} and I'M {self.__animal_type}")

# creating a child class  
class Dog(Animal):
    def __init__(self, name, animal_type):
        super().__init__(name, animal_type)


class Cat(Animal):
    def __init__(self, name, animal_type):
        super().__init__(name, animal_type)


def main():
    dog = Dog("John","dog")
    dog.display_info()
    cat = Cat("Sweet","cat")
    cat.display_info()

main()

```
- Above example, we created Parent class with private attributes (name,type) and method display info. we created two children classes Cat and Dog , atlast we created objects of cat and dog , and we called method display_info, notice here the method which we are not defined in particular classes but  the object was able to access the method which was defined in parent class, this was possible due to inheritance.

## Abstraction 
- Abstraction is another principle of object-oriented programming (OOP), that allows us to hide the internal implementation details while showing only the necessary functionality. The main goal of abstraction is to enable our focus on "what to do" rather than "how to do it".
- Abstraction Example
Let us take a look at a simple example showing how abstraction work in Python.
```python
# importing the ABC class and abstractmethod decorator from abc module  
from abc import ABC, abstractmethod  
  
# creating a class  
class Three_Dimensional_Shapes(ABC):  
  @abstractmethod  
  def calc_volume(self):  
    pass  
  
# creating another class to implement abstraction  
class Cube(Three_Dimensional_Shapes):  
  def __init__(self, side_length):  
    self.side_length = side_length  
  
  def calc_volume(self):  
    return self.side_length ** 3  
  
# creating an object of Cube class  
side_length = 6  
cube = Cube(side_length)  
  
# printing the results  
print("Cube's Side:", side_length)  
print("Cube's Volume:", cube.calc_volume())  
```
-In the above example, we have imported the ABC class and abstractmethod decorator from the abc module. We have then defined a class that inherits the properties of the ABC class and defined an abstract method inside it. We have then defined another class that inherits the properties from the base class and overridden the method to return the preferred output.

## Polymorphism

- Polymorphism is another significant principle of Object-Oriented Programming (OOP) that simply means more than one form. Polymorphism allows methods to have the same name; however, their behaviour is different on the basis of the context of the object. We can achieve Polymorphism through method overriding or overloading.

```python
class Vertebrate:  
    def __init__(self, name):  
        self.name = name  
  
    def move(self):  
        print(f"{self.name} moves in a generic way.")  
  
# creating a child class  
class Mammal(Vertebrate):  
    def move(self):   # method overriding  
        print(f"{self.name} walks.")  
  
# creating a child class  
class Bird(Vertebrate):  
    def move(self):   # method overriding  
        print(f"{self.name} flies.")  
  
# creating object as a list  
animals = [Vertebrate("Generic Vertebrate"), Mammal("Dog"), Bird("Eagle")]  
  
for animal in animals: 
    animal.move()
```

### Conclusion
Object-Oriented Programming encourages modularity, scalability, and reusability, making complex applications easier to manage. Being an OOP-friendly language, Python enables efficient organization of code through object interactions. This approach improves code maintainability by structuring it in a logical and hierarchical way. OOP is widely used in software development, game design, GUI applications, and data modeling, ensuring flexibility and efficiency.