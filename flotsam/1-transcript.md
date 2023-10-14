---
author: Zebulun Arendsee
date: YYYY-MM-dd
paging: Slide %d / %d
---

# Title: The place of programming in the post AGI world

I will talk about the work that I have done on the development of the morloc
programming language. Then I will discuss how this is relevant to the
programming in the post AGI world.

This is perhaps a presumptuous title. There are good reasons to be hesitant
about post-AGI predictions. That said, I will make a case for a few general
principles that I believe will hold and then make a more tentative case for the
relevance of my work on the morloc programming language in that future world.

But first, I should introduce myself.

## Background

I am a scientific programmer - specifically a bioinformatics scientist. In my
undergrad I studied molecular biology. For grad school I transitioned to
bioinformatics and computational biology where I studied the evolutionary
origins of novel proteins. As a postdoc I worked at the USDA studying
phylogenetic patterns in influenza. Then I joined Regeneron where I currently
work on whole genome sequencing and genetic medicine design.

That line is my professional story. A second line diverged in December of 2016
when I began working on what I now call the Morloc language. My motivation was
practical. I wanted to introduce sanity to bioinformatics.

Bioinformatics programming is comprised of data formats and command line
tools. A bioinformatics pipeline is a series of command line calls that fuck the
system state, then ad hoc scripting code to unfuck the state, then then run the
next command.

Enterprise bioinformatics attempts to clean this up with a many-layered workflow
management framework where (for example):

 1. they describe all the state changes in YAML files (along with dependencies,
    versions an such)
 2. wrap all command line tools in new command line tools written in Python with
    more consistent argument calling semantics
 3. define the graph structure with WDL code and specify resource requirements
    and such
 4. run the WDL code in a special cloud environment
 5. add in lots of extra dev ops for extra job security

In this way, what should be a single line of functional code (just a simple
composition) becomes thousands of lines of config files and wrappers.

I wanted to go in the opposite direction. Rather than adding layers, and
specifications, and configs on top of the command line tools, I wanted to break
them down into their atomic functions and write new bioinformatics programs by
composing them using standard functional programming methods.

My approach was as follows:

 1. Decompose tools into libraries of simple functions
 2. Replace bespoke file formats with idiomatic data structures
 3. Compose functions from the libraries to build new bioinformatics programs
    (more or less, just use obvious functional programming principles)

But there is a problem. The original cause of all this mess, the reason
bioinformaticians originally resorted to ugly textual formats and command line
tools, was that there was no single language they could use. If they developed
only a library, then their user base would be limited to that one library.

What we need is a language that can compose functions from libraries in many
languages. To compose functions across languages, we will need a common type
system. I developed morloc to fill this role.

The purpose of morloc can be summarized by two core requirements. First, the
scientific programmer should be free to focus on writing functions rather than
applications. They should be free to write in their favored language and not be
bothered with wrappers, formatting, user interfaces, or APIs. Second, the
workflow designer should be free to seamlessly compose the scientific
programmer’s functions using an expressive language.

morloc is a language that supports function composition across languages under a
common type system. The nodes in a morloc workflow are native functions imported
from external libraries. All code needed for interoperability is automatically
generated by the compiler. The external libraries are independent of the morloc
ecosystem. The workflow is implemented as a simple functional programming
language with full support for generics, parameterized types, and higher-order
functions. morloc modules may be compiled into interactive command line
interfaces.

Walk through an example

```
module core (map, snd, sum)
source Cpp from "core.hpp" ("map", "sum", "snd")
source Py from "core.py" ("map", "sum", "snd")

map :: (a -> b) -> [a] -> [b]
map Cpp :: (a -> b) -> "vector<$1>" a -> "vector<$1>" b
map Py :: (a -> b) -> "list" a -> "list" b

snd :: (a, b) -> b
snd Py :: "tuple" a b -> b
snd Cpp :: "pair" a b -> b

sum :: [Real] -> Real
sum Py :: "list" "float" -> "float"
sum Cpp :: "vector<$1>" "double" -> "Real"
```

```
module core (map, snd, sum)
import conventions (List, Real, Tuple2) 

source Cpp from "core.hpp" ("map", "sum", "snd")
source Py from "core.py" ("map", "sum", "snd")

map :: (a -> b) -> [a] -> [b]
snd :: (a, b) -> b
sum :: [Real] -> Real
```

```
module foo (f)

import core

f = map snd . sum
```


Where it is going:

 * plumb pie
 * deterministic typechecking
 * the concept of plains
 * database of functions - vector database?
 * common type system
 * representation
 * querying programs





Merging of documentation and code


So, enough with bioinformatics. In the next part of my talk, I will step away
from morloc and make an argument for why programming, and specifically
functional programming, will be important in the future, even a future with
AGI. Secondly, I will argue that a common library of functions is necessary and
will describe the features this library ought to have. This will then bring me
back to morloc and (hopefully) support my argument that morloc is more than a
solution to a architecture problem in bioinformatics.


## What is a programming language?

A grammar that can be uniquely evaluated into an executable program


## Humans will always need programming languages

This may seem unlikely given all the talk about no-code solutions and code
generating AI. But I argue that programming languages will be valuable as long
as precise human thought is valuable.

We need programming languages for *thinking*

We need programming languages for *understanding*

We need programming languages for *communicating*. This is especially true if we
are communicating with a "genie", with something that would twist our words in
any way possible to achieve a negative end for us.

## Humans will always need many programming languages

The idea of "best" is only defined one-dimensional metrics. For any object
described be two or more dimensions, the best you can get is to be near the
Pareto plain, where any change leads to loss in one dimension. For a programming
language, the dimensions include simplicity and expressiveness.

A simple grammar is easier to reason on. Both for us as humans and for the
compiler and typechecker. This means simple languages are easier to verify.

Dimensions: simplicity, strictness, safety, domain

There are many languages of math, and of chemistry, and of law, and of
cooking. These can all be seen as Domain Specific Languages. More expressive
languages can describe more concepts, but the cost of expressiveness is
verbosity and verifiability.

    Don't belabor this section, the next one is more interesting.


## AGI will always need programming languages

 * performance - a general intelligence uses unnecessary resources and faculties
   to solve problems. An AGI should be able to generate a narrow AI smaller and
   more efficient than itself that solves specialized problems. In the extreme
   case, the generated "narrow AI" might be a conventional computer program.

 * reasoning - reasoning in a narrow space (such as the grammar of a language)
   should be more efficient than reasoning in a unrestricted space given the
   problem can be represented by the grammar. 

 * communication - if an AI needs to share how it solved a problem, either with
   humans or other AIs, then it needs to be able to describe the process, this
   description may just be conventional code (likely very high level
   code). Perhaps code associated with a formal proof that can be fed into a
   proof checker like Coq. Or perhaps some future more general logic checker.

 * reproducibility - 

 * abstraction

## AGI will always need multiple programming languages

As with humans, AGI are unlikely to settle on just one programming
language. This is especially true since AGI are unlikely to have the same
learning limitations that humans have. So they should be able to use many
languages that are suited to the problem they want to express. They might
conceivable even package the language specification in the metadata and expect
the receiver to generate a compiler on the fly.


## Human / AI interaction needs programming languages

 * genies
 * interpretability
 * safety (e.g., IO monads)

 * reproducibility - 

 * safety - conventional code can be statically analyzed, proofs can be made
   about its properties. An AIs reasoning is more likely to change as it
   learns.

 * interpretability - if the AI can write code, then humans can more clearly see
   what they are doing.

 * trust - An AI cannot necessarily be trusted. They may be trained for
   nefarious purposes intentionally. They be vulnerable to adversarial
   inputs. They may make innocent mistakes in code generation that can harm a
   system. Generated code, though, can be subject to static analysis and proven
   to, for example, have no side effects. Further, the AI could be limited to a
   controlled language where all available functions are carefully curated and
   known to be reliable.


   One solution is to have the AI generate code that can be reasoned upon by a
   conventional, deterministic compiler

AIs are likely to use conventional programs in the future in exactly the same we
humans do currently. These programs take problems that we could painstakingly
solve ourselves but they solve it faster and in a way that is easy to share with
others. They are also less error prone and less subject to personal bias or
random mistakes than we are, in all of our fuzzy complexity.

Could god make a compiler so good that he could write no bad code that would pass or good code that would fail? 


## Database of functions


## Final thoughts