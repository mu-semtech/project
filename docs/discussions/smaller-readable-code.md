# Smaller & readable code (How mu.semte.ch can help you beat the 10% odds)
There exists a saying in computer science that developers will end up with spending 90% of their time reading through code and 10% writing code. This reduces our effectiveness.

For every line we write we will end up reading 10 lines first. So a lot of languages like Clojure/Perl/Ruby use this as an argument to state that writing less code equals having less to read. Hence faster development. But are there more ways we can help?

![](http://mu.semte.ch/wp-content/uploads/2017/08/emacs1percent.png)

## Consistent abstractions
[mu.semte.ch](http://mu.semte.ch/) helps you build consistent abstractions. We base ourselves on standards like JSONAPI, SPARQL and HTTP. This means more assumptions can be made and less code needs to be read and (because of standards) the assumptions hold for a long time.

Functionality should be reusable. Isolating functionality by the boundaries of a microservice makes it reusable and easy to understand.

## Once upon a time
Think back at the time when you were a junior dev. You were set out to fix your first bug on a real project. You sit down, a hundred files staring at you. Good luck.  
Which file to choose? With a good system you may be able to launch a debugger and find a starting point, if you received the time to set up development environment. In a perfect code base you go through a few level of abstractions learning each of them to find the issue at point. Reality often shows that even perfect code is hard to get.

Can we make it slightly simpler?

In [mu.semte.ch](http://mu.semte.ch/) there is simple code and well known standards. Spaghetti is not an option. A microservice is typically 100 lines of code.

You are again a junior dev, set out to fix your fist bug. You open the dispatcher, see the relevant microservice, 100 lines of code, all included.

## Conclusion
We are not saying that we don’t have bugs, we are not saying that nothing else can be wrong but just about all cases this is the way we work. And yes maybe these 100 lines are important and maybe they take you longer to read. However the chance of you quickly finding the bug greatly increases.

Nice first day of work!

*This document has been adapted from Jonathan Langens' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/08/31/how-mu-semte-ch-can-help-you-beat-the-10-odds/)*
