# practical-software-design

I write this document because I feel frustrated. I have seen too many software development projects giving too much emphasis on (agile, scrum, devops, you name it!) while too little on a very important fundamental -- software design. I am not saying a methodology is bad. It is great, but you will not achieve that greatness if your design is bad. A few decades ago, waterfall was perhaps the only methodology we heard of, and apparently many software was built successfully with waterfall. My argument is that even with the methodology most of us, if not all, currently classify as bad could deliver many successes. Why? I believe the answer lies in the design. In this article, I am telling you my practical tricks/rules/principles (whatever you call them) I always use when I have to work on something from scratch, and they do work!

### Program to an interface

This principle is the most important because it promotes reusability, ease test-driven development, and allows multiple developers to work together in harmony at the coding level.

Reusability is in the sense that your code that only depends on interfaces can be resused as is. See the psuedo code below for an example. Method use of class Client can be reused as is, again and again, with any Duck object.
```
interface Duck {
   method fly()
}

class DefaultDuck implements Duck {
   method fly() {
      // do something
   }
}

class Client {
    method use(Duck duck) {
        // do something with duck
    }
}
```

Once you have an interface, you can implement the inteface in any way you want as long as you provide the requried methods. You may have a very simple default implementation for each interface, and wiring all those implementations together will enable you to test your whole software (in particular your implemented algorithms) even before you have actual, specific implementations.

If it is a team development, a developer may be assigned to work on a specific implementation of an interface while he or she does not need to know anything beyond what the inteface requires.

### Use Strategy design pattern first

TODO

### Favour Dependency Injection at creation time

TODO

### Use Abstract with Dependency Injection at run time

TODO

### Use other more complex design patterns only when needed

TODO
