# practical-software-design

I write this document because I feel a little bit frustrated. I have seen too many software development projects giving too much emphasis on a process/methodology (agile, scrum, devops, you name it!) while too little on a very important fundamental -- software design. I am not saying a methodology is bad. It is great, but you will not achieve that greatness if your design is bad. A few decades ago, waterfall was perhaps the only methodology we heard of, and apparently many softwares and systems were built successfully with waterfall. My argument is that even with the methodology most of us, if not all, currently classify as obsolete could deliver many successes. Why? I believe a methodology alone does not dictate the outcome of a development, but other factors do too, and software design is one of those. In this article, I write about some practical tricks/rules/principles (whatever you call them) I always use when I work on a development project from scratch, and they work!

Before we begin, please note that I code in Python these days, so examples are therefore in Python, but the same ideas can be applied to other modern OO languages.

### Program to an interface

This principle is arguably the most important, but ... what is an interface? It is not the "interface" in Java (not directly), but that interface in Java is part of the interface we are talking about. An interface is an entity with clear contracts of how others would use that entity. In OO context, a contract is a method signature, and an entity can be an interface and an abstract class in Java, a trait and an abstract class in Scala, an abc.ABCMeta class in Python, or even a concrete class in any OO language.

A concrete class as an interface! Why? Well, some developers beleive that every concrete class should extend an interface while others believe that a simple concrete class does not necessarily need additional code, and methods in that class themselves are already good enough contracts. It is subjective and totally up to you, and it is not a big deal at the end of the day. I prefer the write-less-do-more way, so I am with the latter school of thought.

Below is an example code adapted from the Head First Design Patterns book. As Python lacks the concept of access modifers, I annotate public access with @public, private access with @private, and module-private access (like package-private access in Java) with nothing.

```python
# @file duck.py

@public
class Duck(abc.ABC):
  @public
  def fly(self):
    pass
    
  @public
  def quack(self):
    pass
    
  @public
  def swim(self):
    pass
    
  @public
  def display(self):
    pass
    
class DefaultDuck(Duck):
  ...
  
class MyDuck(Duck):
  ...
  
class YourDuck(Duck):
  ...
  
@public
void create(type):
  """A simple factory function."""
  if type == "default":
    return DefaultDuck()
  elif type == "my":
    return MyDuck()
  elif type == "your":
    return YourDuck()
```

You can see that the outside world (i.e. the client that uses this module) only needs to know about class Duck (considered an interface) and function create(), which are public to the outside world. Let's see an example client code.

```python
# @file client.py

void simulate(ducks):
  for duck in ducks:
    ...
```

The client simulates a list of ducks in function simulate(). By programming to an interface, function simulate() never need to know or care about the type of ducks, and the function itself can be reused again and again. If you work in a team, you can ask other developers to develop different types of ducks, i.e. DefaultDuck, MyDuck and YourDuck, simultaneously. At the end of the day, these classes just have to provide the contracts that Duck provides. Test-wise, you can test anything that depends on an interface even before an actual implementation of that interface is available. For instance, you can test function simulate() with a mock Duck before you implement DefaultDuck, MyDuck and YourDuck -- assuming these ducks are complex sutff! These are some of the benefits of program to an interface, the fundamental principle of good software design.

### Favour Strategy design pattern and Dependency Injection at creation time

To be written





