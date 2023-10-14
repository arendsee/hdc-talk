---
title: "Programming in the post AGI world"
author: Zebulun Arendsee
---

# Morloc and the place of programming in the post AGI world

 > Never trust anything that can think for itself if you can't see where it keeps its brain
 >   -- Mr. Weasley

<!-- Good afternoon, today I will present my work on the development of the morloc    -->
<!-- programming language and its relevance to the programming in the post AGI world. -->
<!--                                                                                  -->
<!-- But first, I should introduce myself.                                            -->

---
## Personal background

My official background:
 * Molecular Biology, B.Sc
 * Bioinformatics PhD
 * USDA postdoc
 * Regeneron scientist

---
## Personal background

My unofficial background
 * I began developing Morloc in 2016 
 * Founded Morloc Inc in 2018

I began developing Morloc in during winter break of 2016, midway through my PhD
program. Initially, the scope was modest enough. I just wanted a simple
language for composing functions in R. But soon I realized I 

Brenden Eich famously created Javascript in a week. But that doesn't really
impress me. Making prototypes is easy.

---
## Presentation Pipeline 

 1. 



 <!-- 1. The original motivating problem                                     -->
 <!--    * The computational structure of practical bioinformatics           -->
 <!--    * Why this structure is insane                                      -->
 <!--    * The root cause of this insanity                                   -->
 <!--    * How the Morloc programming language fixes the problem             -->
 <!-- 1. A description of Morloc                                             -->
 <!--    * Design philosophy                                                 -->
 <!--    * A quick example                                                   -->
 <!--    * Building polyglot libraries                                       -->
 <!--    * The type system                                                   -->
 <!-- 1. Programming in the post AGI world                                   -->
 <!--    * Humans will always need programming languages                     -->
 <!--    * AIs will always need programming languages                        -->
 <!--    * Programming languages are necessary for safe human/AI interaction -->
 <!--    * Libraries will be important in the post AGI world                 -->
 <!-- 1. Predictions about the nature of future programming languages        -->
 <!--    * self-maintaining software                                         -->
 <!--    * semi-intelligent programs                                         -->
 <!--    * a truly infinite library                                          -->
 <!--    * Programming as a query against an infinite library                -->

---
## The original motivating problem

Why does bioinformatics matter?

 * Bioinformatics is foundational to most papers in biology 
 * Bioinformatics is used to design vaccines and genetic medicine
 * Bioinformatics is behind all inferences made from personal genomics

---
## The computational structure of practical bioinformatics

 Bioinformatics analyses are typically designed as graphs of black box applications with
 untyped data flowing between them.

 These are called graphs.

 * Data is represented in flat files in many ad hoc formats
 * Each application builds an internal data structure from the raw input,
   performs an operation, and writes the output.

<!-- > Most errors are interface errors    -->
<!-- >  -- Margaret Hamilton (paraphrased) -->

---
### Typical formats (FASTA)

 * FASTA files store genetic sequence
 * The header (`>.*`) is an unformatted string describing the sequence
 * There is no accepted convention for how data is encoded

Here are the first few lines of the human Huntington gene (retrieved from NCBI):

```
~~~pygmentize -x -l lexer.py:FastaLexer
>NC_000004.12:3074681-3243960 HTT [organism=Homo sapiens] [GeneID=3064] [chromosome=4]
GCTGCCGGGACGGGTCCAAGATGGACGGCCGCTCAGGTTCTGCTTTTACCTGCGGCCCAGAGCCCCATTC
ATTGCCCCGGTGCTGAGCGGCGCCGCGAGTCGGCCCGAGGCCTCCGGGGACTGCCGTGCCGGGCGGGAGA
~~~
```

Here are a few (truncated) entries from an influenza study, not the data is
encoded in the header in a completely diferent fashion:

```
~~~pygmentize -x -l lexer.py:FastaLexer
>1C.2.1|KT362191|A/swine/Italy/14-30549/2014|H1N1|H1|ITA|2014-03-12
ATGAAAGCAAAATTATTTGTATTATTCTGTGCATTTACTGCACTGAAAGCTGACACCATTTGTGTAGGCTATCATGCTAA
CAATTCCACAGACACTGTCGACACGATACTGGAAAAGAATGTTACTGTTACCCATTCAGTTAATTTACTAGAAAGCAGCC
...
>1C.2.3|MN393712|A/swine/Liaoning/BX266/2015|H1N1|H1|CHN|2015-11-19
ATGGAAGCAAAACTATTTGTATTATTCTGTGCATTCACTGCACTGAAAGCTGACACCATTTGTGTAGGCTACCATGCCAA
CAATTCCACAGACACTGTCGACACTATACTAGAGAAAAATGTGACTGTTACCCATTCAGTTAATTTACTGGAAAACAGCC
...
>1C.1|KR701057|A/swine/England/024079/2013|H1N1|H1|GBR|2013-08-21
ATGGAGACAAAACTGTTTGTGTTATTCTGCACATTTACTGCATTAAAAGCTGATACCATCTGTGTAGGCTATCATGCTAA
CAACTCCACAGATACTGTTGACACAATACTGGAGAAGAATGTAACTGTCACCCATTCCGTTAACTTACTAGAAAGCAGCC
...
~~~
```

---
### Typical formats (Newick)

Trees may be stored as Newick files

```
~~~pygmentize -x -l lexer.py:NewickLexer
(( "A/swine/Belgium/Gent-208/2019":0.0863
 , "A/swine/Italy/138986/2021":0.108
 ):0.0039
 ,"A/Jiangsu/1/2011":0.0447
):0.0393;
~~~
```

To store more detailed metadata, we could overload the species labels:

```
~~~pygmentize -x -l lexer.py:NewickLexer
(( "A/swine/Belgium/Gent-208/2019|H1N1|Swine|BEL|1C.2.1|2019-01-01":0.0863
 , "A/swine/Italy/138986/2021|H1N1|Swine|ITA|1C.2.5|2021-05-07":0.108
 ):0.0039
 ,"A/Jiangsu/1/2011|H1N1|Human|CHN|1C.2.3|2011-01-04":0.0447
):0.0393;
~~~
```

---
### A bioinformatics tool

A bioinformatics tool is typically a CLI standalone program that reads one or
more files as inputs and writes one or more files to the filesystem.

It is responsible for:

 * parsing command line arguments
 * dispatching based on arguments to many different wrapped functions
 * parsing input data formats into internal representations
 * validating the input format and data
 * running the actual algorithm
 * raising errors, writing them to STDERR, and raising proper exit codes
 * writing output to one or more formats

Their fundamental flaw is that they couple too many logically separate concerns

Each tool must duplicate support for each of these features (parsing arguments,
parsing input formats, data validation, error handling, format writing).

These tools are hard to maintain

Most are broken


---
### Why this structure is insane

 1. logically equivalent tools cannot be substituted
 1. algorithm implementations become software projects
 1. each tool duplicates format readers/writers

Workflow "languages" are not expressive, they lack
  * higher order functions
  * generics
  * compound data structures,
  * type checking

 * files
 * functions (applications) are not composable
 * lack of a common language
   * and no, Python isn't the answer

 * applications couple too many distinct concerns together

Compensating for this lack of consistency requires either highly flexible
parsers on the application side or extra interface code written by the
researcher.

A bioinformatics workflow takes many of these tools and composes them together
with ad hoc code added between them for compatibility

---
### Enterprise Bioinformatics (and why it is also insane)

 1. describe all the state changes in YAML files
 1. wrap all command line tools in standardized command line tools written in Python
 1. define execution graph structure with WDL code (YAML files)
 1. run the WDL code in a special cloud environment
 1. add in lots of extra dev ops for extra job security


In general, wrapping a bad thing in more layers of indirection doesn't make the
thing good

---
### How the Morloc language solves the bioinformatics problem

 * Decompose tools into libraries of simple functions
 * Replace bespoke file formats with idiomatic data structures
 * Compose functions from the libraries to build new bioinformatics programs

 * make functions searchable
 * you can get just one
 * no need to import the gorilla (dependencies)

---
## Morloc

The main focus is on libraries.
Libraries are needed by AIs
They are needed for abstraction
Morloc places libraries at its core

 * A strongly-typed functional programming language
 * A foundation for a universal library
 * Unifies many languages under a common type system
 * Supports multi-lingual function composition

 * There are **no** built-in functions -- all functions are imported from outside languages
 * All programs are compositions of these imported functions
 * The morloc language is a language for composing library functions and building libraries from source functions.

Principles
 * encourage code reuse by bridging language gaps
 * avoid language lock-in and code duplication
 * allow multi-language teams to collaborate
 * never sacrifice power, strict supersetting
 * automate tedious work (IO, input checking, testing)

Evils
 * writing documentation is evil
 * writing tests is evil
 * checking function inputs is evil
 * writing package infrastructure is evil
 * good software design is evil 
 * handling IO is evil
 * functions that aren't searchable are evil
 * language insularity is particularly evil

And the golden rule:

 **Say what the data is and what you want to do with it. Nothing else. Ever.**

---
### Basic syntax

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module foo (sumOfSquares)

import core (map, mul, add, foldl)

-- a combinator from Raymond Smullyan's aviary
-- (see "To Mock a Mockingbird")
warbler :: (a -> a -> b) -> a -> b
warbler f x = f x x

sum = foldl add 0

sumOfSquares = sum . map (warbler mul)
~~~
```

 * Haskell-style type annotations (no type classes just yet)
 * Supports basic functional programming: higher-order functions, eta-reduction, etc

<!-- But what is `import` doing? Where does `map`, `mul`, etc come from? -->

---
### Basic syntax

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module foo (sumOfSquares)

import core (map, mul, add, foldl)

-- a combinator from Raymond Smullyan's aviary
-- (see "To Mock a Mockingbird")
warbler :: (a -> a -> b) -> a -> b
warbler f x = f x x

sum = foldl add 0

sumOfSquares = sum . map (warbler mul)
~~~
```

 * Imported functions may have multiple implementations
 * The compiler decides which to use
 * The compiler generates all required interoperability code

---
###  Building libraries

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module core (map, mul, foldl)

-- load language-specific implementations
source cpp from "core.hpp" ("map", "mul", "foldl")
source py from "core.py" ("map", "mul", "foldl")

map :: (a -> b) -> [a] -> [b]
map Cpp :: (a -> b) -> "std::vector<$1>" a -> "std::vector<$1>" b
map Py :: (a -> b) -> "list" a -> "list" b

mul :: Real -> Real -> Real
mul Cpp :: "double" -> "double" -> "double"
mul Py :: "float" -> "float" -> "float"

foldl :: (b -> a -> b) -> b -> [a] -> b
foldl Cpp :: (b -> a -> b) -> b -> "std::vector<$1>" a -> "std::vector<$1>" b
foldl Py :: (b -> a -> b) -> b -> "list" a -> "list" b
~~~
```

---
### Building libraries

Concrete signatures can be inferred with default mappings to the general type

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module core (map, snd, sum)
import conventions (List, Real, Tuple2) 

source Cpp from "core.hpp" ("map", "sum", "snd")
source Py from "core.py" ("map", "sum", "snd")

map :: (a -> b) -> [a] -> [b]
snd :: (a, b) -> b
sum :: [Real] -> Real
~~~
```

where:

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module conventions (List, Real, Tuple2)

type Cpp (List x) = "vector<$1>" a
type Py (List x) = "list" a
~~~
```

---
### What does a signature tell us?

A signature describes a family of functions:

``` haskell
_ :: () -> a    -- from nothing, anything; absurd, empty family 
_ :: a -> a     -- identity; family of size 1
_ :: [a] -> [a] -- sort, take functions, reverse; large family of list-to-list
                -- functions that do not change the container type
```

Richer type systems can make more specific families

``` haskell
-- broad family of functions
_ :: Matrix a -> Matrix a

-- narrower family where row/column dimensions are flipped
_ :: Matrix m n a -> Matrix n m a

_ :: x:(Matrix m n a) -> y:(Matrix m n a) where 
   x[i,j] == y[j,i] for all (i,j) in indices x 
```

But a type signature does not have to be so precise that limits the function
family to 1, further limiting the function family size is the responsibility of
the implementation itself.


---
### What does a signature tell us?

Functions signatures are an elegant tool for thought: they can be used to
explore and organize a family of algorithms.

alignment and tree building examples

this is why I want to go back to academia, to teach in this way

---
### Advantages

 * All the usual benefits of strongly-typed functional programming: type-driven
   design, typechecking, easy reasoning

 * All serialization/interop code is automatically generated, so the entire
   system can be replaced without altering the user-provided pipelines. For
   example, switching from the current JSON-based serialization to Google
   Protobufs would be a mostly invisible change to the user. 

 * Type-based search



---
## Programming in the post AGI world

**Programming language:** A grammar that can be uniquely evaluated into an executable program

I will argue that:

 1. Humans will always need programming languages
 1. AIs will always need programming languages
 1. Programming languages are necessary for safe human/AI interaction
 1. Libraries will be important in the post AGI world

<!-- This is a fairly strict definition. In the past I've used looser definitions to                                  -->
<!-- argue that programming will always be important, but loose definitions can be so                                 -->
<!-- inclusive that "programming will always be necessary" becomes the wishy washy                                    -->
<!-- assertion that "precise communication will always be necessary", which is a too                                  -->
<!-- obvious to be very interesting.                                                                                  -->

---
### Humans will always need programming languages

 * We need programming languages for *thinking*
 * We need programming languages for *understanding*
 * We need programming languages for *communicating*

Programming languages use small grammars to represent general computation. But
programming languages are not the first specialized grammars. Mathematics has
used grammars for millennia. But mathematical proofs are not typically written
solely in the concise symbolic grammar. Rather they mix symbolic grammar with
natural language. It is possible that programming language will follow the same
pattern. The natural language, which is currently included as unbinding
comments, may be considered by future compilers with natural language capacity.

To what degree will prompting be integrated with programming?

How intelligent do we want our compilers to be?

Elegant grammars with concise rules are useful for reasoning. In a world
with AI-based compilers that understand natural language.

``` python
# `f` is a function that is expensive to call
# `xs` is a homogenous iterable
# `xs` may not fit in memory, but each element should
# `r` folds over elements in `ys`, creating a single return value
def mapreduce (f, r, xs):
    # `f(x)` should be small, so `ys` can easily fit in memory
    ys = [f(x) for x in xs]
    return r(ys)
```

``` python
# `f` is a function that is expensive to call
# `xs` may not fit in memory, but each element should
mapreduce :: f:(a -> b) -> r:([b] -> c) -> xs:[a] -> z:c where
mapreduce f r = r . map f
```

A smart compiler might then flag the following function call: 

``` python
mapreduce id id mygiantdata
```

---
### AI will always need programming languages

 * **performance** - The strength of general intelligence is that it considers
   many paths using broad knowledge. But for problems that trace one path with
   limited knowledge, the generality of the intelligence is a hindrance. An AGI
   should always be able to make a narrower AI with better performance for a
   narrower problem. In the extreme, these narrow AIs would be conventional
   programs representable in conventional languages.

 * **validation** - If an AGI reuses code, either its own or external code, it
   must confirm that the code is correct by itself and in the new usage
   context. That is, it must do static analysis. Static analysis is hard, so for
   efficiency sake, it makes sense to create a narrower AI to perform the
   analysis. This narrower AI is a compiler. It further makes sense to use code
   written in a manner that makes the compiler's job possible and efficient. So
   a language that is designed to facilitate reasoning will be more efficient
   and require less intelligence (and thus less work) to validate.

 * **reasoning** - Finding solutions is easier with fewer dimensions, since the
   search space is smaller. Using a strict grammar, where generated programs are
   correct by construction when the production rules are followed, will be more
   efficient than finding solutions in an unconstrained space.

 * **reproducibility** - An AGI may be able to solve any problem directly using
   its natural intelligence. But an AGI will likely evolve (depending on their
   architecture) with time and their solutions will evolve accordingly. In many
   situations we want to be able to reproduce a result using simple
   steps. Generating solutions in code is one way to achieve this.

 * **abstraction** - Functions are a means of abstracting complex problems down
   to simple signatures. They reduce dimensionality and are thus a tool for more
   effective reasoning.


The small grammars of programming languages reduces the search space for an
algorithm. 

An AI needs to be able to explain how it reached a conclusion. Both to humans
and to other AIs

An AI needs to be able to share a process. They could share a natural language
description of what they did and allow the other to reconstruct the
algorithm. But it makes more sense, for algorithms that can be written
succinctly, to express them in code.

An AI needs to be able to abstract. To cache past reasonings and reuse
them. Writing callable functions is one way to do this. It is probably faster
to query a function used in the past than to rewrite it.

---
### Programming languages are necessary for safe human/AI interaction

 **Are you comfortable with an AGI generating binaries for you?**

 Suppose we give an AGI the prompt:

 "Given the personal human genome in file xxx.fasta and medical record
 xxx.json generate a medical regimen to optimize their health and lifespan."

 * **genies** - If you are talking to a genie, you need to be precise, you might get
   what you ask for not what you want.

 * **trust** - If you can't safely assume an AI is benign, can you ever trust its
   results? Possibly, one way to safeguard against a possibly adversarial AI is
   to require solutions to problems written in code that can be formally
   verified by a traditional, deterministic compiler.

 * **interpretability** - This is part of trust. If the AI creates solutions as
   code, and writes in a very strongly-typed language (say Agda or some other
   dependently typed language) the interpretation of the code can be fairly
   straightforward. If a function is not in IO monad, and your compiler verifies
   no unsafe IO functions are called, then you can be sure your function won't
   have side effects. 

 * **reproducibility** - already mentioned

 * **safety** - apart from trust, having AIs generate code adds an extra layer of
   confidence to a solution. 

 * Do we want AGI compilers? I don't think so. I think the deterministic
   compilers are an important safeguard.


Could god make a compiler so good that he could write no bad code that would pass or good code that would fail?


---
## Libraries will be important in the post AGI world

 * a library defines a finite set of functions

 * a library of functions
 * self-maintaining software
 * code generation together with static analysis and compiling

 * semi-intelligent programs
 * a truly infinite library
 * Programming as a query against an infinite library

 * Dangers - we need to kill the software as a service idea where


```
module box (compute)

import pure Bio
import readonly BioIO

foo :: PersonalGenome -> MedicalHistory -> [GuideRNA]
foo g h = %generate "Given the genome `g` and medical history `h` for a single
human patient, design a cocktail of siRNAs that will optimize their chance of
having a long healthy life." 
```

---
### Programming as a query against an infinite library


---
### Morloc post AGI

In morloc we can define the space of functions that is permitted. We can place
limits on what languages can be used. We can define wings of the library where
all composed programs must, for example, all contain proofs that they have no
side effects. For this I need to implement an effect system.

We could, for instance, require that the only IO effects be read access.

A plain may contain only a defined set of functions for interacting with the
internet (say a few for pulling data from various sources, but none for posting
data to twitter or making arbitrary searches or anything else). Then if the AI
can only write code, and if they are limited to composing functions from the
plain, then they may be safe. They may have the ability to fetch data from given
URLs or read given files.

---
## Any Questions

Morloc Github: https://github.com/morloc-project/morloc
