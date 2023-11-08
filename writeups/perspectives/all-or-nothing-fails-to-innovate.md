# Experimentation

## All or nothing fails to innovate

Introducing new technologies is hard.  Very hard.  No matter how promising the technology is, the amount of unknowns is daunting.  Who knows what the hidden risks of [Ballerina](http://ballerinalang.org/) will be in practice? The price of technical innovation is too damn high.  mu.semte.ch makes experimentation easy, and cheap.

[![The price of innovation is too damn high -- made at imgflip.com](https://i.imgflip.com/1k6pqj.jpg)](https://imgflip.com/i/1k6pqj)

### Monetheism is not your future

Hind-sight 20/20.  As easy as it is to see what turned out to be the best way, it is hard to figure out what will turn out ok in the future.  Should you guess on building your stack on the über-efficient [Elixir](http://elixir-lang.org/) language?  Will you find the devs?  How about [Haskell](https://www.haskell.org/)?  How about just taking the safe bet that you’ll find enough devs tackling the problem in [Java](https://java.com)?  It is no easy choice.

It is hard to find good developers.  In any language.  But it seems that some rock-star developers have found the holey grail in one or another strange language.  It happened with [Ruby](https://www.ruby-lang.org) as [Ruby on Rails](http://rubyonrails.org/) became big.  Some great thinkers swear by the simplicity of Haskell, others by the speed of Elixir.  We can spend decades arguing our choice is the best.  There is a catch though, we do tend to conclude that all options have their merits.  Most options have their own reason to exist.  The best solution simply depends on your problem.

When we look at specific tasks, the problem of choosing a programming language shifts from a religious war of _“the best programming language ever”_ to _“the best language for the problem”_.  A much simpler question, with a much more reasonable answer.  Need to tackle many long-running requests?  Better pick Elixir.  Want to see how purely functional programming would work in practice?  Use Haskell.  Do you think it’d be easier if the backend and the frontend used one language?  Javascript is your friend.  In the holy war of programming languages, monotheism is dead.

As much as I’d personally like for us to agree that [Common Lisp is sent to us from the gods](https://www.youtube.com/watch?v=5-OjTPj7K54), I’ve learned that most others don’t agree.  It has its place, but betting on it for the sake of your company is an HR suicide mission.  mu.semte.ch uses microservices, all wrapped in their [Docker](https://www.docker.com/) container, meaning they can all choose their programming language.  For an innovation-driven company, the hard choice is figuring out a minimal set of hard constraints on the implementation, so a wide array of unknown choices can be made in the future, without causing extra friction.

It is not an easy choice to pick the right programming language, so why make it?  Making the choice stifles innovation.  It is not to be made.

### The lack of priests

Clearly, there are downsides of having loads of programming languages in your active code-base.  A single language brings benefits.  There must be a balancing act to be made.

As everyone can introduce new programming languages, how do you keep an oversight over the space?  There is an inherent risk accompanied in using a microservice in a language you don’t know.  Or, god forbid, the lone Haskell developer leaving your company.  Fear not, these are not the risks you are looking for.

*I don’t understand a microservice in a strange language.* A common worry would be that developers have problems fixing an application in a strange programming language.  This can be true for complex applications, but small microservices are much easier to understand.  mu.semte.ch augments this with templates, offering basic constructs with similar naming in a variety of programming languages.  This makes it easier to dive into a piece of code you’ve never seen, and understand what is going on.  Worst case, it’s a microservice.  You take ownership and rewrite it in your own language of choice.

*How do I adapt a microservice in a strange language?* Each microservice should be configurable.  It turns out that basic configuration can happen through environment variables.  Meaning it’s exactly the same, regardless of the underlying programming language.  More complex configuration may need to happen in the common method for the specific language.  In Ruby, YAML is common to be used.  In Lisp, S-expressions can be used.  Regardless, the syntax of the configuration is contained in examples in the sources or the README, you just follow that syntax.

*What if my rock-star Ballerina dev leaves?* It is supposed to be microservice.  The hard part is figuring out what the service should entail exactly.  For the large majority of the services, it holds that they can be rewritten in a matter of days, given knowledge on the requests that will come in.  Fact of the matter is, that a motivated dev is way faster than a bored one.  It’s better to keep using the microservice of your rock-star dev, and replace it when substantial changes need to be made.

There are other risks on using many programming languages.  There must be.  But they are greatly overshadowed by the benefits.  Namely the motivation and efficiency of the developers turning your company strategies into an automated version of reality.

### An all-inclusive playground

All or nothing fails to innovate.  We should start embracing the new kids on the block.  Languages, or frameworks, with a limited track-record could be of great benefit.  I will give Ballerina a swirl, a language released just a few days ago.  I’ll test run it to appreciate its benefits, and discover its shortcomings.  For a virtually non-existent cost, I will have replaced an existent microservice in a real-world application.

Diversity is what makes all ecosystems thrive.  It is time to apply it to your company.

*This part has been adapted from Aad Versteden's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/02/23/all-or-nothing-fails-to-innovate/)*

## mu.semte.ch as the ideal playground
![](http://mu.semte.ch/wp-content/uploads/2017/01/juggling_with_languages.png-200x300.png)The [mu.semte.ch](http://mu.semte.ch/) framework is the ideal playground for the developer that wants to try a new language. Using the standards set out by the framework you can, in literally no time, get a (tiny) microservice up and running in your next favourite language. We are ever expanding the range of languages covered by the templates.

### What are these templates?
Within [mu.semte.ch](http://mu.semte.ch/) a template is everything a microservice needs except for your domain specific logic. Take the javascript template ([https://github.com/mu-semtech/mu-javascript-template](https://github.com/mu-semtech/mu-javascript-template)) for instance: to start a new microservice you first create a directory and a Dockerfile. In that Dockerfile you specify the FROM as the template
```Dockerfile
    FROM semtech/mu-javascript-template
```

and then you add a app.js file where you enter something like
```js
    import { app, query } from 'mu';
    
    app.get('/', function( req, res ) {
      res.send('Hello mu-javascript-template');
    } );
```

With this you have your first javascript microservice that will respond to the ‘/’ route with a simple hello world string (I know not entirely true, you will also need to configure the dispatcher but for that look at our docs on [mu.semte.ch](http://mu.semte.ch/)). After this you can go wild with javascript and you don’t have to worry about the boring stuff that make the webservice tick, allow you to write/read to the database, etc.

### Other languages
If we compare this with the [ruby template](https://github.com/mu-semtech/mu-ruby-template#building-your-first-microservice-with-sinatra/) then we find that for ruby your FROM statement is slightly different (you guess what changes) and instead of a app.js file you now create a web.rb file and again the simplest form looks almost like the example above.

Suppose you want to go for python? Well exactly the same.

Doing this we ensure that you have a similar workflow where you know what to expect with every language that you try through a [mu.semte.ch](http://mu.semte.ch/) template. Allowing you to focus on learning and playing with your newest toy.

### Just playing?
But wait there is more! You do not have to restrict yourself to just playing. Since the ideas behind a microservice are that they have a very limited set of responsibilities, they can very easily be replaced . If there is a necessity for it you can include your new pet language in a production system with little to no risk at all! This shows one of the biggest strengths of the platform. By standardising on HTTP, JSONAPI and graph databases we ensure freedom to use any tool or language and as such also the tool or language that is the best for the job!

### Future
The set of templates is, for now, rather limited but check us out regularly because we hope to expand and add many many languages to our portfolio. Of course if you want to implement a template feel free to contact us. We will provide you with a simple set of guidelines that will help you develop that template.

*This part has been adapted from Jonathan Langens' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/05/04/mu-semte-ch-as-the-ideal-playground/)*
