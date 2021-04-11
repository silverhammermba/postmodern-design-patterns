## Introduction

This is a commentary on the Gang of Four's [Design Patterns][book] book
(hereafter referred to as GoF). Even if you have never heard of that book, if
you are a software developer you have felt its influence. It defined what is
essentially the standard list of software design patterns. When you hear people
talking about factories, visitors, singletons, etc. you can be sure that they,
or someone they learned from, originally heard about those terms from GoF.

[book]: https://www.worldcat.org/title/design-patterns-elements-of-reusable-object-oriented-software/oclc/31171684

### Why "postmodern"?

I mainly wanted to grab your attention. GoF was first published in 1994. That
makes it ancient wisdom in terms of software development. And like all ancient
wisdom, while it is good to learn from it, it shouldn't be learned by rote. For
some reason this same list of design patterns keeps getting brought up as a best
practice. I see young developers discussing these patterns as if they are just
as modern and relevant as they were in 1994.

I would not go so far as to say "design patterns considered harmful", but we
need a refresher. We need to figure out which parts are missing, which parts to
keep, and which parts to throw away. The modern practice seems to be trusting in
the wisdom of ancients, so instead lets be postmodern software developers.

## Thinking in patterns

Almost a quarter of GoF is dedicated not to the specific patterns, but to
introducing the concept of design patterns itself. How do you think about
patterns? When should you use them? Why should you use them? This is the most
crucial aspect of working with patterns, so it's great that GoF dedicates so
much time to it. But what is written in those parts of the book may shock you.

My hunch is that most developers these days skip over that stuff, or don't have
access to GoF at all, so they instead read only about the specific patterns,
probably reprinted out of context on some random website. This is a shame
because there _is_ wisdom in those overlooked words, and there is also folly.
We need to know about both because they inform how and why we continue to use
these patterns.

### The dream of '94

The foreword by Grady Booch says it so well. The goal of design patterns is
simply

> ..an architecture that is smaller, simpler, and for more understandable...

I am inclined to say that this should be the "design pattern manifesto". No
matter how you are applying design patterns, if they aren't helping to remove
bloat, simplify your codebase, and reduce confusion, you are doing something
wrong.

So we know the goal, how do you get there? Again, their advice is rock-solid:

> Experienced object-oriented designers will tell you that a reusable and
> flexible design is difficult if not impossible to get "right" the first time.
> Before a design is finished, they usually try to reuse it several times,
> modifying it each time.

This is the #1 pitfall that I see when people try to use design patterns: they
want to rush right away to the finish line, so they throw a ton of design
patterns at the problem, thinking "more patterns" == "more flexible". But this
is not true! Applying design patterns incorrectly can actually make your code
more bloated and harder to understand. Later on, after trying to reuse their
design as requirements change and grow, they start feeling the pain of their
misapplied patterns, but their design is already so large and unwieldy that
their options are either a total rewrite or adding workarounds for their
useless patterns (hardly smaller or more understandable).

The correct approach is what I call "programmer driven design". Always design
your software with your programmers in mind. You want to make it easy for them
to understand the structure of the program, you want to make it hard for them to
misuse the software components, you want to make the architecture flexible in
areas of uncertainty and strict in areas where mistakes would be expensive. The
easiest way to do this, if you can afford it, is to start your software designs
with rapid prototyping and iteration. Start with the simplest possible thing,
only adding design patterns to alleviate friction as you try to expand your
prototype to handle more requirements. Do what the ancients say: "try to reuse
it several times" until you get it "right".

To be fair, GoF sends out some mixed messages here. Their wild dream is:

> You should be able to get all the functionality you need just by assembling
> existing components through object composition. But this is rarely the case,
> because the set of available components is never quite rich enough in
> practice.


