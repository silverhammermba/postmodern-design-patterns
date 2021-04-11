## Personal thoughts on design patterns.

### Mel

There is an appealing romanticism to [The Story of
Mel](http://www.cs.utah.edu/~elb/folklore/mel.html). On the one hand this is a
completely valid form of programming: if the problem is small enough, or the
programmer's brain is big enough, they can fit the whole problem in their head at once
and find a perfect solution that takes advantage of holistic optimizations that
a compiler would never find. But there are downsides obviously. Even Mel
couldn't modify his own program for a new requirement, and the task was even
more daunting for another programmer.

It's about more than writing working code, or coming up with a clever solution.
This seems similar to people who do something only because the compiler
complained, or because autocomplete suggested it, or they saw it highly upvoted
on stack overflow. What is missing from all of these examples, including Mel's,
is infusing reasoning into the program. Does the program have a structure that
communicates how it works? This has several benefits

* If the structure matches the functionality, it helps unfamiliar programmers
  learn the functionality, because the as you read the structure you learn its
  purpose. A program missing explicit structure is hard to learn. A program with
  unnecessary or perpendicular(?) structure is also hard to learn.
* It should make it easier to modify things if the logical structure matches how
  the problem works?

### Wisdom

Could also compare GoF book to ancient wisdom. Yes it's wisdom, but it's also
ancient. Don't accept it uncritically. It's not that design patterns are
considered harmful. It's that we need to make sure we're keeping them relevant
to modern times. Hence "postmodern".

## Planning out final sections?

Start off with the good bits about simplifying things and reasoning behind
patterns

Definitely need a section about how it is outdated

Need something like a "design pattern manifesto" to remember the purpose of
design patters and not miss the forest for the trees, like how people bitch
about agile when their scrum sucks.

A section about approach, how you need to iterate, start simple. Don't shoot for
ideal right away or you will fail and end up with a bloated design that looks
cool on paper but which is actually unhelpful and difficult to
understand.

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

Very outdated concept of what is reasonable to assume from the language (pg 72)
> ...fairly esoteric capabilities like type-safe casts.

WTF acquaintance vs. aggregation (pg 22)

causes of redesign and why they are overkill (pg 24):

1. creating an object by specifying a class explicitly: commits you to a
   particular implementation. i disagree, if you only need one implementation,
   you can change that one implementation, can even change a concrete class into
   an interface (as long as you aren't using Hungarian notation). also this can
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
   any patterns for this. this is just encapsulation??? TODO: revisit this once
   you read about abstract factory, bridge, memento, proxy.
5. algorithmic dependencies. "algorithms that are likely to be replaced should
   be isolated", again this is just encapsulation? when would it not be
   sufficient to change the implementation of the existing algorithm? TODO
   revisit after builder, iterator, strategy, template method, visitor
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

Similar mistake is made with embellishments, where we use our existing glyph
type to represent UI-only concepts like buttons, menus, page boarders, and
scrollbars. This makes sense only if "glyph" means "a graphical element", but up
to now glyphs represented the document content/hierarchy! Their software design
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
the bridge is the interface? The bridge is only useful if we have some
nontrivial amount of code which needs to live in the window object e.g. our
application needs a "draw text" method, but each window system only provides a
"draw character" method, so we're going to end up with many similar
implementations of "draw text". TODO How easy is to transition to a bridge
if/when we need it?

### User operations

The way they introduce this concept is problematic, similar to the glyphs. They
identify that there is immediately a fundamental distinction between kinds of
operations: those that support undo and redo and those that do not. But then
they never introduce a fundamental distinction between these in their design.
They unify everything under Command, and each individual command decides its own
reversibility.

This design is extremely flexible, but not in a sensible way. It allows for
crazy shit like saving the file being reversible, or basic editing operations
_not_ being reversible. It makes the undo implementation more complicated,
because we need to figure out how to handle situations like lots of irreversible
operations interspersed between reversible ones. Logically you want separate
queues, like saving the file 100 times shouldn't push any reversible operations
out of the undo queue. But the design is missing all of this.

A more helpful design would be to have two unrelated classes:
DocumentEditOperaiton and EditorOperation. The former is used by anything that
can edit the actual document (and thus must be reversible) and the latter is
used by anything that only interacts with Lexi itself, such as saving the file
or changing preferences (which should not be reversible). Both classes can
conform to a Command interface if you want, but that is actually a minor point
in the design, since largely the application will probably want to keep the two
very separate. For example a DocumentEditOperation must have a document to work
on whereas an EditorOperation does not. Thus these two types of operations have
fundamentally different dependencies, which our software design should reflect.

## Traversal

Iterator is a great pattern, but most good languages have it built-in these
days. You should conform to your language's idiomatic iterator pattern rather
than reinventing the wheel.

Visitor is also pretty useful, but I'm curious if there are better alternatives
in stuff like Swift.
