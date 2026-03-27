
#### How to define a class in python:

```python
class Animal:
    def __init__(self, name: str):  # __init__ = constructor, self = $this`
        self.name = name            # no need to declare properties upfront`

    def speak(self) -> str:         # type hints optional but good practice`
        `return "..."`

class Dog(Animal):                  # inheritance — same as PHP extends`
    def speak(self) -> str:
        return f"Woof! I am {self.name}"`

class Cat(Animal):
    def speak(self) -> str:
        return f"Meow! I am {self.name}"

dog = Dog("Rex")
print(dog.speak())   # Woof! I am Rex`
```

To make a class abstract in python you import ABC and pass when you are creating the class.
``
`` from abc import ABC, abstractmethod // to make a function abstract you define the annotation @abstractmethod there

The parent constructor in python is not automatically called like in every other language and for each method of a class the first parameter is the this ( the object on which its called its not magically injected unlike other languages) so

```python
def call(self)->str:
	return "self is this so we can call self.someproperty to  access properties"
```

To call the parent controller we use the super().__init__() call //  

```python
def __init__(self,property):
	super().init(property)
```


### Final Code

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    def __init__(self,name, fuel: int):
        print("calling parent constructor")
        self.name = name
        self.fuel = fuel

    @abstractmethod
    def start(self) -> str:
        pass
    
    @abstractmethod
    def stop(self) -> str:
        pass
    
    def refuel(self,fuelAmount):
        self.fuel += fuelAmount
        return f"fuel has been filled in {self.name} "
    
    def __str__(self)-> str :
        return f"fuel : {self.fuel} | name: {self.name}"
        
    
    
class Car(Vehicle):
    
    def __init__(self, name, fuel, speed):
        super().__init__(name, fuel)
        self.speed = speed
        
    def __str__(self)-> str :
        return f"speed : {self.speed} | {super().__str__()}" 
        
    def start(self):
        self.fuel = self.fuel-1
        self.speed = self.speed+1
        return f"Starting...{self.name} | speed : {self.speed}"
        
    def stop(self):
        self.speed = self.speed-1
        return f"Stopping...{self.name} | speed : {self.speed}"
        
c = Car("Vokswagon",20, 100)
print(c)
```

Day 2 [[OOPs - Week 1 Python - 2]]