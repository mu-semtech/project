# Reactive programming
We are experimenting with reactive programming.  Why?  Orchestration!

The traditional mu.semte.ch architecture provides user-facing microservices.  The frontend orchestrates the microservices as it is best suited to communicate their effects to the end-user.  But what about backend microservices?  How do we let those communicate?  Can we indicate to the user where things were left off?  Yes.  Yes we can.  With reactive programming.

<img alt="A drawing of a scientist experimenting with RDF" src="https://mu.semte.ch/wp-content/uploads/2017/01/Reactive_programming-1-200x300.png" align="left" style="margin: 20px;">

With reactive programming, our services respond to a certain state being available in the database.  As the state changes, the service is informed, and it can react accordingly.  An email service could detect that an email is currently in the Outbox, mail it to the right user, and move it to the Sent box.  We can keep the database as the only sync-point and have services start tasks based on other services’s work.  Backend services can communicate without direct dependencies, using triples to describe their state in the triplestore. The end-user can be informed on the process by visualizing this state in the frontend.

## An email example
Backend services write contents to the triplestore, which is discovered by other microservices hooking into this content.

Let’s assume an email system.  As the user creates an email, it is in Draft status.  Once the email should be sent, we move that email into the Outbox.  As this email gets connected to the Outbox, the email sending service picks it up.  It sends the email, and moves it to the Sent mailbox.  Each of these states is trivially easy to express in the semantic model.

## Embracing failure
It may be that a microservice drops out.  Perhaps we ran out of emails to send under our current plan, perhaps the server hosting our server decided to go on a holiday.  Our approach ensures that, once the service gets back up, it picks up the work from where it left off.

Assume we prepare 5 emails to be sent at once.  We place each of the emails in the Outbox, and the sending service starts mailing.  As it has sent the third email, we kill the service.  Two emails are left unsent.  They are still present in the Outbox.  When we restart our mailer-service, it checks what emails are still in the Outbox.  The two mails that match are sent.  As new emails arrive, the mailer picks them up and sends them.

Failures can happen.  It is important to ensure the failure of a single microservice doesn’t bring down the whole application.  A big win for reactive microservices.

## Rich combinations
Reactive programming can make the construction of an application a lot simpler.  As we inform the user about state changes, an understanding of the system as a whole can be supplied.

Let us consider a complex mailing and tagging system.  As we draft an email, we see it in the Draft box.  When we send it, our service moves it to the Outbox.  Our user interface reflects this change, and shows the email in the Outbox now.  The mailer service picks up our email, sends it, and moves it to the Sent box.  This too, can be reflected in the user interface.  This whole picture makes it simple for the user to understand what is going on in the application.

We can easily push the boundaries for these rich microservices further with more complex systems.  For instance, we may extend our mail client with automatic tagging of emails.  As we classify a set of emails, a Neural Network service indicates that it has started training on the new examples.  Our user interface shows this state.  Once the network has been trained, the updated parameters are written to the store.  Our classification interface hooks into this to launch a new auto-classification of emails we haven’t checked yet.  The microservices are fully decoupled, and the user can easily get a grip on the fairly complex set of operations going on in the backend.

Rich applications are easier to construct when clear boundaries exist and are being communicated about.

## What's next?
We have ran advanced experiments and have PoCs of the necessary tooling for reactive programming on the mu.semte.ch stack.

The [delta-service](https://github.com/mu-semtech/mu-delta-service) calculates triples that would be changed by the execution of an update query.  It can inform other services about these changes so they can update as necessary.  We are building some example microservices leveraging this approach, whilst applying the concept to end-user PoC applications.

*This document has been adapted from Aad Verstend's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/02/09/reactive-programming/)*
