# practical-software-design

I write this document because I feel a little bit frustrated. I have seen too many software development projects giving too much emphasis on processes/methodologies (agile, scrum, devops, you name it!) while too little on a very important fundamental -- software design. I am not saying a methodology is bad. It is great, but you will not achieve that greatness if your design is bad. A few decades ago, waterfall was perhaps the only methodology we heard of, and apparently many softwares and systems were built successfully with waterfall. My argument is that even with the methodology most of us, if not all, currently classify as "out of date and no more of it" could deliver many successes. Why? I believe the choice of methodology alone does not dictate the outcome of a development, but other factors too, and software design is one of those. In this article, I am telling you my practical tricks/rules/principles (whatever you call them) I always use when I have to work on something from scratch, and they do work!

Before we begin, please note that I code in Python these days, so examples are therefore in Python, but the same ideas can be applied to other modern OO languages.

### Program to an interface

This principle is arguably the most important, but ... what is an interface? It is not the "interface" in Java (not directly), but that interface in Java is part of the interface we are talking about. An interface of a software entity is the specification of how others would be able to work with that entity. In this OOP context, a specification is a method signature, and an entity can be an interface and an abstract class in Java, a trait and an abstract class in Scala, an abc.ABCMeta class in Python, or even a concrete class in any OO language.

A concrete class as an entity with interface! Why? Some developers beleive that something like Java interface should be used for every class while others believe that a simple class does not need additional (and unnecessary) code, and methods in that class themselves are already good enough interface. It is subjective and totally up to you, and it is not a big deal at the end of the day. I prefer the write less do more way, so I am with the latter school of thought.

It will not be complete to talk about a interface without talking about access modifiers (or access specifiers).

<< TBC >>















