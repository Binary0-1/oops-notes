	SOLID Principles.
## SRP: single responsibility principle
A class should do only one thing and one thing completely

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        
class PaymentService:
    
    def calculate_pay(self,employee)-> float:
        return employee.salary * 0.8

class EmployeeRepository:
    
    def save(self,employee):
        print(f"Saving {employee.name} to database")
        
class EmployeeEmailService:
    
    def send_email(self, employee,message):
        print(f"Sending email to {employee.name}: {message}")
    
```

## O — Open/Closed Principle (OCP)

A class should be open for extension but closed for modification.

#### Bad Example
```python
class PaymentService:
    def process(self, payment_type: str, amount: float):
        if payment_type == "credit_card":
            print(f"Processing credit card payment of {amount}")
        elif payment_type == "paypal":
            print(f"Processing paypal payment of {amount}")
        elif payment_type == "crypto":        # every new payment type
            print(f"Processing crypto payment of {amount}")  # touches this class
```

### FIxed 
```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process(self, amount: float) -> str:
        pass

class CreditCardProcessor(PaymentProcessor):
    def process(self, amount: float) -> str:
        return f"Processing credit card payment of {amount}"

class PaypalProcessor(PaymentProcessor):
    def process(self, amount: float) -> str:
        return f"Processing paypal payment of {amount}"

# New payment method? Just ADD a new class, touch nothing else
class CryptoProcessor(PaymentProcessor):
    def process(self, amount: float) -> str:
        return f"Processing crypto payment of {amount}"

# Usage
def checkout(processor: PaymentProcessor, amount: float):
    print(processor.process(amount))

checkout(CreditCardProcessor(), 100)
checkout(CryptoProcessor(), 200)
```

How to check if your code passes the Open closed principle

"If requirements change, do I modify existing classes or add new ones?" If modify — you're violating OCP. Restructure with abstraction.

## Liskov Substitution Principle (LSP)

If S is a subclass of L then anywhere L is used shoudl be replaceable by S without Breaking the flow
seems very obvious but examples of places where it can happen are:

```python
class Bird(ABC):
    @abstractmethod
    def fly(self) -> str:
        pass

class Eagle(Bird):
    def fly(self) -> str:
        return "Eagle soaring high"

class Penguin(Bird):
    def fly(self) -> str:
        raise Exception("Penguins can't fly!")  # ← LSP violation
```

Here when the fly function is called on Penguin then it raises an exception as penguin cannot fly but it inherits the bird class and should be able to fly

To fix This we usually make use of Inheritance Chaining 

Fix:
```python
class Bird(ABC):
    @abstractmethod
    def eat(self) -> str:   # all birds eat, safe abstraction
        pass

class FlyingBird(Bird):
    @abstractmethod
    def fly(self) -> str:   # only flying birds fly
        pass

class Eagle(FlyingBird):
    def eat(self) -> str:
        return "Eagle eating fish"
    def fly(self) -> str:
        return "Eagle soaring high"

class Penguin(Bird):        # Penguin is a Bird, not a FlyingBird
    def eat(self) -> str:
        return "Penguin eating fish"

def make_bird_fly(bird: FlyingBird):  # now only accepts FlyingBird
    return bird.fly()

make_bird_fly(Eagle())    # works
make_bird_fly(Penguin())  # TypeError at call site, not a surprise crash inside
```

Here when we see the Type Error before execution and can be flagged before .

The LSP mental checklist:

Can every subclass be substituted for its parent without surprising behavior or exceptions?" If subclass throws `NotImplemented` or `Exception` on an inherited method — your hierarchy is wrong.

## I — Interface Segregation Principle (ISP)

A class should not be forced to implement methods it doesn't need.

In plain terms — **don't put too many abstract methods in one base class. Split it into smaller, focused interfaces.**

Bad Example:
```python
class Worker(ABC):
    @abstractmethod
    def work(self) -> str:
        pass

    @abstractmethod
    def eat(self) -> str:
        pass

    @abstractmethod
    def sleep(self) -> str:
        pass

class Human(Worker):
    def work(self) -> str:
        return "Human working"
    def eat(self) -> str:
        return "Human eating"
    def sleep(self) -> str:
        return "Human sleeping"

class Robot(Worker):
    def work(self) -> str:
        return "Robot working"
    def eat(self) -> str:
        raise Exception("Robots don't eat!")   # ← ISP violation
    def sleep(self) -> str:
        raise Exception("Robots don't sleep!") # ← ISP violation
```

Here even though Robot is not able to eat and sleep it is implementing functions it does not need
This is a voilation.

Fix :

```python
class Printer(ABC):
    @abstractmethod
    def print_doc(self) -> str:
        pass

    @abstractmethod
    def scan(self) -> str:
        pass

    @abstractmethod
    def fax(self) -> str:
        pass

class ModernPrinter(Printer):
    def print_doc(self) -> str:
        return "Printing..."
    def scan(self) -> str:
        return "Scanning..."
    def fax(self) -> str:
        return "Faxing..."

class OldPrinter(Printer):
    def print_doc(self) -> str:
        return "Printing..."
    def scan(self) -> str:
        raise Exception("Old printer can't scan!")
    def fax(self) -> str:
        raise Exception("Old printer can't fax!")
```

One concept from this exercise worth remembering for interviews — **multiple inheritance syntax in Python:**

python

```python
class ModernPrinter(Printable, Faxable, Scannable):
```

If an interviewer asks _"how do you implement multiple interfaces in Python?"_ — this is your answer. Python has no `interface` keyword like Java, you just inherit from multiple ABCs.

## Dependency Inversion Principle (DIP)

High level modules should not depend on low level modules. Both should depend on abstractions.

Voilation :

```python
class MySQLDatabase:          # low level — specific implementation
    def save(self, data: str):
        print(f"Saving {data} to MySQL")

class UserService:            # high level — business logic
    def __init__(self):
        self.db = MySQLDatabase()   # ← directly depends on MySQL

    def create_user(self, name: str):
        self.db.save(name)

```

`UserService` is hardwired to MySQL. If you want to switch to MongoDB or Postgres — you have to open `UserService` and change it. Your business logic should never care what database you're using.

Fix:
```python
from abc import ABC, abstractmethod

class Database(ABC):                # abstraction
    @abstractmethod
    def save(self, data: str):
        pass

class MySQLDatabase(Database):      # low level depends on abstraction
    def save(self, data: str):
        print(f"Saving {data} to MySQL")

class MongoDatabase(Database):      # easy to add new implementation
    def save(self, data: str):
        print(f"Saving {data} to MongoDB")

class UserService:                  # high level depends on abstraction
    def __init__(self, db: Database):   # ← receives abstraction, not concrete class
        self.db = db

    def create_user(self, name: str):
        self.db.save(name)

# Usage — you decide which DB at runtime
mysql = MySQLDatabase()
mongo = MongoDatabase()

UserService(mysql).create_user("John")  # uses MySQL
UserService(mongo).create_user("Jane")  # uses MongoDB, zero changes to UserService
```

|Principle|One line summary|
|---|---|
|**S**|One class, one responsibility|
|**O**|Add new code, don't modify old code|
|**L**|Subclass should work anywhere parent works|
|**I**|Don't force classes to implement what they don't need|
|**D**|Depend on abstractions, not concrete classes|
