# practical-software-design

I write this document because I feel a little bit frustrated. I have seen too many software development projects giving too much emphasis on a process/methodology (agile, scrum, devops, you name it!) while too little on a very important fundamental -- software design. I am not saying a methodology is bad. It is great, but you will not achieve that greatness if your design is bad. A few decades ago, waterfall was perhaps the only methodology we heard of, and apparently many softwares and systems were built successfully with waterfall. My argument is even with a methodology most of us, if not all, currently classify as obsolete could deliver many successes. Why? I believe a methodology alone does not dictate the outcome of a development project, but other factors do too, and software design is one of those. In this article, I write about some practical tricks/rules/principles (whatever you call them) I always use when I work on a development project from scratch, and they do work.

Before we begin, please note that I am coding in Python these days, so examples are therefore in Python, but the same ideas can be applied to other modern OO languages.

### Program to an interface

This principle is arguably the most important, but ... what is an interface? It is not the "interface" in Java (not directly), but that interface in Java is part of the interface we are talking about here. An interface is an entity with clear contracts of how others would use that entity. In OO context, a contract is a method signature, and an entity can be an interface and an abstract class in Java, a trait and an abstract class in Scala, an abc.ABC class in Python, or even a concrete class in any OO language.

A concrete class as an interface! Why? Well, some developers beleive that every concrete class should extend/implement an interface while others believe that a simple concrete class does not necessarily need additional code and methods in that class themselves are already contracts. It is subjective and totally up to you and not a big deal at the end of the day. I prefer the write-less-do-more way, so I am with the latter school of thought.

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

You can see that the outside world of duck (i.e. the client that uses this duck module) only needs to know about class Duck (considered an interface) and function create() (considered a factory -- more on this later), which are public to the outside world. Let's see an example client code.

```python
# @file client.py

void simulate(ducks):
  for duck in ducks:
    ...
```

The client simulates a list of ducks in function simulate(). By programming to an interface, function simulate() never need to know or care about types of ducks. The function depends on Duck, considered abstract, so underlying changes will not affect code in simulate(). This is decoupling and promotes reusabilty in the sense that simulate() can be reused easily. If you work in a team, you can ask other developers to develop different types of ducks, i.e. DefaultDuck, MyDuck and YourDuck, simultaneously. At the end of the day, these classes just have to provide the contracts that Duck provides. Test-wise, you can test anything that depends on an interface even before an actual implementation of that interface is available. For instance, you can test function simulate() with a mock Duck before DefaultDuck, MyDuck and YourDuck are implemented -- assuming these ducks are complex sutff! These are some of the benefits of "program to an interface", the fundamental principle of good software design.

### Favour Strategy design pattern and Dependency Injection at creation time

You normally have more than one class in a software project, and how these classes fit together requires some, most of the time deep, thought. To me, it is an art, and a skilled designer always surprises others with a beautiful, simple design that works. When classes have clear structural relationship, you may simply follow that relationship. Often, an issue arises when you have to deal with behaviour relationship, and a solution for that is Strategy design pattern. It is a simple design pattern based on composition, and by using it you follow other two well-known software design principles, "separate what varies from what statys the same" and "favour composition over inheritance".

Again let's look at the duck stuff adapted from the Head First Design Patterns book. Say that you have so many types of Duck to implement, some of them share the same way they fly(). If I did not change anything in the current design, I would find myself cut-and-paste the same fly() code from one class to another -- this was certainly me 20 years ago. Another solution that I might have come up with was to house common fly() code in Duck (interface), and let subclasses share that code. Sounds great no more cut-and-paste and only a single copy of fly() code! Unfortunately, I would have to introduce type-specific code somehow somewhere to help determine what fly() code each implementation of Duck would use, most likely in Duck's fly() with lots of if-then-else. This sounds very anti-abstraction. Um... abstraction... right! how about another level of abstraction (interface) of Duck based on fly() behaviours; that is, I would have something like FlyHighDuck, FlyLowDuck, FlyFastDuck, FlySlowDuck etc. that extends Duck. These interfaces house their common fly() code. Problem solved, yeah!!! It does not sound that bad actually, but what if I also have to deal with quack() and swim()?

A good solution for this type of design problem is Strategy. Copied direcly from wiki as is, Strategy design pattern defines a family of algorithms, encapsulates each algorithm, and makes the algorithms interchangable within that family. In our context, we would have another interface to encopsulate fly() behaviour and some specific implementations of fly(), something like below.

```python
# @file duck.py

class Fly(abc.ABC):

  @public
  def fly(self):
    pass

class DefaultFly(Fly):
  ...
        
class FlyHigh(Fly):
  ...

class FlyLow(Fly):
  ...

class FlyFast(Fly):
  ...

class FlySlow(Fly):
  ...

...
```

The next question is how Duck uses Fly. At this stage, it is clear that Duck depends on Fly, and preferrably you establish that dependency at creation time -- thing will be a bit more complicated when you need this dependency at run time, but more on that later. In a fancier term, you do Dependency Injection of Fly to Duck, and preferrably you do it via Duck's constructor. Let's see an example.

```python
# @file duck.py

class Fly(abc.ABC):

  @public
  def fly(self):
    pass
    
class DefaultFly(Fly):
  ...
    
class FlyHigh(Fly):
  ...

class FlyLow(Fly):
  ...

class FlyFast(Fly):
  ...

class FlySlow(Fly):
  ...

@public
class Duck(abc.ABC):
  def __init__(self, fly_strategy=None):
    self._fly_strategy = fly_strategy or DefaultFly()

  @public
  def fly(self):
    self._fly_strategy.fly()
    
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
    return MyDuck(fly_strategy=FlyFast())
  elif type == "your":
    return YourDuck(fly_strategy=FlyLow())
```

Now, I can have common fly() behaviour in class DefaultFly, and that is the default behaviour of all Duck. I can also change this default bahaviour when I create a Duck object. It looks pretty neat. A couple of things to point out are that the only two things public to the outside world are still class Duck and function create() and that the client program does not need to change anything at all.

### Creation via Abstract Factory (TO REVISE)

In the example, I use a function as a public contract for creating a Duck object. It suffices for what is needed, but not flexible enough if we want more. How about allowing the client program to specify the fly strategy of a Duck at creation time? In this case, we need to make Fly accessible (public) to the client program and provide an abstract way to create both Duck and Fly.

```python
# @file duck.py

@public
class Fly(abc.ABC):

  @public
  def fly(self):
    pass


@public
class DuckFactory(abc.ABC):
  def create_duck(self, *args, **kwargs):
    pass
  def create_fly(self, *args, **kwargs):
    pass
    
class DefaultDuckFactory(DuckFactory):
  def create_duck(self, *args, **kwargs):
    # return a Duck with Fly strategy object based on passed in args and kwargs
  def create_fly(self, *args, **kwargs):
    # return a Fly object based on passed in args and kwargs

factory = DefaultDuckFactory()
```

In the code, I provide public interface DuckFactory and a publicly availble default factory (global variable factory). Note that class DefaultDuckFactory is not public becuase it does not have to be.

### Dependency Injection at run time (TO REVISE)

Sometimes, requirements require behaviour changes at run time. For instance, the client program needs to change the fly strategy of a duck. The easiest way to do this is to introduce a contract (method) to do this in the interface (Duck).

However, in reality, you may not be able to do it due to various reasons e.g. you do not have write access to Duck, you want a (perhaps) better seperation of concerns, etc. I have two ways of handling this case. The first way is Mixin.

```python
# @file duck.py

@public
class FlyMixin(abc.ABC):
  @public
  def set_fly(self, fly):
    self._fly_strategy = fly

@public
class Duck(abc.ABC):
  def __init__(self, fly_strategy=None):
    self._fly_strategy = fly_strategy or DefaultFly()

  @public
  def fly(self):
    self._fly_strategy.fly()
    
  @public
  def quack(self):
    pass
    
  @public
  def swim(self):
    pass
    
  @public
  def display(self):
    pass

class DefaultDuck(FlyMixin, Duck):
  def __init__(self, fly_strategy=None):
    super(DefaultDuck, self).__init__(fly_strategy=fly_strategy)
```

Class DefaultDuck now has an ability to set fly strategy, and no change to factory classes is required. Using Mixin is not possible for a language that does not allow multiple inheritance e.g. Java, and here comes the second way, interface extension.

```python
# @file duck.py

@public
class Duck(abc.ABC):
  def __init__(self, fly_strategy=None):
    self._fly_strategy = fly_strategy or DefaultFly()

  @public
  def fly(self):
    self._fly_strategy.fly()
    
  @public
  def quack(self):
    pass
    
  @public
  def swim(self):
    pass
    
  @public
  def display(self):
    pass

@public
class EvolvedDuck(Duck):
  @public
  def set_fly(self, fly):
    self._fly_strategy = fly

class DefaultDuck(EvolvedDuck):
  def __init__(self, fly_strategy=None):
    super(DefaultDuck, self).__init__(fly_strategy=fly_strategy)
```

In this case, you may have to come up with a sensible name for the extending interface. For strongly typed language like Java, the client program has to convert Duck and EvolvedDuck in order to call method set_fly(), but it is not a problem with Python.

### Embrace language features (TO REVISE)

Programming languages have advanced significantly, and the trend is towards allowing programmers to be able to write less and less boilerplate code and focus more and more on ways to solve their programming problems. Embrace features of your used language. Do not stick to old ways just because you know they work. Once you are able to produce programming solutions through your fingertips instantly without even knowing where they come from, then you can silently call yourself a master!
