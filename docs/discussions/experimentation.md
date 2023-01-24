# Experimentation (All or nothing fails to innovate)

Introducing new technologies is hard.  Very hard.  No matter how promising the technology is, the amount of unknowns is daunting.  Who knows what the hidden risks of [Ballerina](http://ballerinalang.org/) will be in practice? The price of technical innovation is too damn high.  mu.semte.ch makes experimentation easy, and cheap.

[![The price of innovation is too damn high -- made at imgflip.com](https://i.imgflip.com/1k6pqj.jpg)](https://imgflip.com/i/1k6pqj)

## Monetheism is not your future

Hind-sight 20/20.  As easy as it is to see what turned out to be the best way, it is hard to figure out what will turn out ok in the future.  Should you guess on building your stack on the über-efficient [Elixir](http://elixir-lang.org/) language?  Will you find the devs?  How about [Haskell](https://www.haskell.org/)?  How about just taking the safe bet that you’ll find enough devs tackling the problem in [Java](https://java.com)?  It is no easy choice.

It is hard to find good developers.  In any language.  But it seems that some rock-star developers have found the holey grail in one or another strange language.  It happened with [Ruby](https://www.ruby-lang.org) as [Ruby on Rails](http://rubyonrails.org/) became big.  Some great thinkers swear by the simplicity of Haskell, others by the speed of Elixir.  We can spend decades arguing our choice is the best.  There is a catch though, we do tend to conclude that all options have their merits.  Most options have their own reason to exist.  The best solution simply depends on your problem.

When we look at specific tasks, the problem of choosing a programming language shifts from a religious war of _“the best programming language ever”_ to _“the best language for the problem”_.  A much simpler question, with a much more reasonable answer.  Need to tackle many long-running requests?  Better pick Elixir.  Want to see how purely functional programming would work in practice?  Use Haskell.  Do you think it’d be easier if the backend and the frontend used one language?  Javascript is your friend.  In the holy war of programming languages, monotheism is dead.

As much as I’d personally like for us to agree that [Common Lisp is sent to us from the gods](https://www.youtube.com/watch?v=5-OjTPj7K54), I’ve learned that most others don’t agree.  It has its place, but betting on it for the sake of your company is an HR suicide mission.  mu.semte.ch uses microservices, all wrapped in their [Docker](https://www.docker.com/) container, meaning they can all choose their programming language.  For an innovation-driven company, the hard choice is figuring out a minimal set of hard constraints on the implementation, so a wide array of unknown choices can be made in the future, without causing extra friction.

It is not an easy choice to pick the right programming language, so why make it?  Making the choice stifles innovation.  It is not to be made.

## The lack of priests

Clearly, there are downsides of having loads of programming languages in your active code-base.  A single language brings benefits.  There must be a balancing act to be made.

As everyone can introduce new programming languages, how do you keep an oversight over the space?  There is an inherent risk accompanied in using a microservice in a language you don’t know.  Or, god forbid, the lone Haskell developer leaving your company.  Fear not, these are not the risks you are looking for.

*I don’t understand a microservice in a strange language.* A common worry would be that developers have problems fixing an application in a strange programming language.  This can be true for complex applications, but small microservices are much easier to understand.  mu.semte.ch augments this with templates, offering basic constructs with similar naming in a variety of programming languages.  This makes it easier to dive into a piece of code you’ve never seen, and understand what is going on.  Worst case, it’s a microservice.  You take ownership and rewrite it in your own language of choice.

*How do I adapt a microservice in a strange language?* Each microservice should be configurable.  It turns out that basic configuration can happen through environment variables.  Meaning it’s exactly the same, regardless of the underlying programming language.  More complex configuration may need to happen in the common method for the specific language.  In Ruby, YAML is common to be used.  In Lisp, S-expressions can be used.  Regardless, the syntax of the configuration is contained in examples in the sources or the README, you just follow that syntax.

*What if my rock-star Ballerina dev leaves?* It is supposed to be microservice.  The hard part is figuring out what the service should entail exactly.  For the large majority of the services, it holds that they can be rewritten in a matter of days, given knowledge on the requests that will come in.  Fact of the matter is, that a motivated dev is way faster than a bored one.  It’s better to keep using the microservice of your rock-star dev, and replace it when substantial changes need to be made.

There are other risks on using many programming languages.  There must be.  But they are greatly overshadowed by the benefits.  Namely the motivation and efficiency of the developers turning your company strategies into an automated version of reality.

## An all-inclusive playground

All or nothing fails to innovate.  We should start embracing the new kids on the block.  Languages, or frameworks, with a limited track-record could be of great benefit.  I will give Ballerina a swirl, a language released just a few days ago.  I’ll test run it to appreciate its benefits, and discover its shortcomings.  For a virtually non-existent cost, I will have replaced an existent microservice in a real-world application.

Diversity is what makes all ecosystems thrive.  It is time to apply it to your company.

*This document has been adapted from Aad Versteden's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/02/23/all-or-nothing-fails-to-innovate/)*
