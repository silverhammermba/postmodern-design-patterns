## Intro

From the foreword by Grady Booch:
> ..an architecture that is smaller, simpler, and for more understandable..."

From the introduction:
> Experienced object-oriented designers will tell you that a reusable and
> flexible design is difficult if not impossible to get "right" the first time.
> Before a design is finished, they usually try to reuse it several times,
> modifying it each time.

From "What is a design pattern?"
> The choice of programming language is important because it influences one's
> point of view. Our patterns assume Smalltalk/C++-level language features...
> some of our patterns are supported directly by the less common object-oriented
> languages... which lessen the need for a pattern such as Visitor.

Class vs. type. Interesting definition that doesn't match modern definitions
> An object's class defines how the object is implemented. The class defines the
> object's internal state and the implementation of its operations. In contrast,
> an object's type only refers to its interface--the set of requests to which it
> can respond.

This is insanity:
> Program to an interface, not an implementation.
>
> Don't declare variables to be instances of particular concrete classes. Instead,
> commit only to an interface defined by an abstract class.

Sometimes you want an explicit dependence! Also the first sentence and second
sentence are not necessarily related, you can have concrete classes, as long as
they are well-encapsulated your program is only depending on the class's
interface

What the fuck does this even mean????
> Ideally, you shouldn't have to create new components to achieve reuse. You
> should be able to get all the functionality you need just by assembling existing
> components through object composition. But this is rarely the case, because the
> set of available components is never quite rich enough in practice.

Sorting example. Can implement a reusable sort operation using "Template
Method", "Strategy", or parametrized types (generics). I.e. using generics
mean no design pattern is needed. But then...
> None of the patterns in this book concerns parameterized types...

WTF acquaintance vs. aggregation (pg 22)

causes of redesign and why they are overkill (pg 24):

1. creating an object by specifying a class explicitly: commits you to a
   particular implementation. i disagree, if you only need one implementation,
   you can change that one implementation, can even change a concrete class into
   an interface (as long as you aren't using hungarian notation). also this can
   be a trap: what level of abstraction do you want for your interface? it's
   hard to know this up front. if you pick the wrong level of abstraction you
   will need to also redesign the interface later.
2. dependence on specific operations. "by avoiding hard-coded requests, you make
   it easier to...", again it would be crazy to implement this across the board
   when you don't know where the flexibility is needed.
3. dependence on hardware and software platform. in certain cases this makes
   sense, in others not so much. e.g. us abstracting iOS's alert handling code
   at the wrong level, didn't save us any time. if your code is
   platform-specific this is generally a waste of time
4. dependence on object representations or implementations. you shouldn't need
   any patterns for this. this is just basic encapsulation??? TODO: revisit this
   once you read about abstract factory, bridge, memento, proxy.
5. algorithmic dependencies. "algorthmis that are likely to be replaced should be
   isolated", again this is just basic enscapsulation? when would it not be
   sufficient to just change the implementation? TODO revisit after builder,
   iterator, strategy, template method, visitor
6. tight coupling: use abstract coupling and layering instead. though I'd say
   dependency injection solves most of this problem, making redesigns very cheap
7. extending functionality by subclassing. generally agree, people seem to do
   this rarely these days
8. inability to alter classes. e.g. from a closed source library, or has too
   many dependencies to change it. closed source i kinda agree with but for too
   many dependencies again that depends on what kind of class it is. maybe you
   want to make the expensive change now to avoid tech debt later

distinguishes applications vs. toolkits (a general-purpose library) vs.
frameworks (an architecture). you need more design patters for toolkits and most
for frameworks.

## Case study: Lexi

> The classes must have compatible interfaces... The way to make interfaces
> compatible in a language like C++ is to relate the classes through
> inheritance.

No one would do this in a modern language. You use
interfaces/protocols/traits

Some strange logic in the footnotes:

> Most contemporary editors don't use an object for every character, presumably
> for efficiency reasons.

Then they say they leave it as an exercise to find a way to optimize this by
applying more design patterns e.g. flyweight. I'm not crazy about this design.
Rather than hacking in more patterns to make characters act like glyphs, why not
represent this fact in your design? Individual characters _could_ be represented
as glyphs, but knowing that each page could have hundred of repeated characters
you might want to specifically model this in the design. It would be more
intuitive and the architecture might better represent the logic of how the
program works.

General issue with how the book (needs?) to talk about patterns: with
hypotheticals. Versus how you talk about patterns in a real context: with
requirements. The case study is so vague that it's hard to evaluate whether
their patterns are actually good choices. Example is their compositing design:
the document's contents and structure are mixed because both user input
(text/images) and auto-generated structure (rows/columns) are represented as
glyphs. They talk about how their strategized compositor pattern makes it
possible to swap about compositors on the fly, but how do different compositors
handle the mixed structure of the document? This aspect of WYSIWYG editing is
missing from the software design.

Similar mistake is made with embellishments, where we use our existing glypth
type to represent UI-only concepts like buttons, menus, page boarders, and
scrollbars. This makes sense only if "glyph" means "a graphical element", but up
to now glyps represented the document content/hierarchy! Their software design
supports adding a button/scrollbar inside a document, which might be frivolous
depending on the requirements.

### Bridge case study

The Bridge pattern example from Lexi also seems incomplete. I completely agree
with their evaluation of the situation: you want to unify all window systems in
your application, which has two competing approaches

1. Define the requirements of a window system for your application code, which
   defines a minimal protocol for the window system which is divorced from any
   particular implementation
2. Define the common features of window systems, which might be some mix of a
   union/intersection of concrete window system features that you know of

Then they "solve" this problem with a bridge: a window object conforms to 1,
then for all of its methods it calls a window implementation object which
conforms to 2. But why not have only the window implementation objects, where
the bridge is just the interface? The bridge is only useful if we have some
nontrivial amount of code which needs to live in the window object e.g. our
application needs a "draw text" method, but each window system only provides a
"draw character" method, so we're going to end up with many similar
implementations of "draw text". TODO How easy is to transition to a bridge
if/when we need it?


