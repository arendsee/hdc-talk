Gene Feature Format (GFF)

```
NW_003302555.1  RefSeq  gene  47  2523  .  -  .  ID=gene0;gene_name=ABCD
NW_003302555.1  RefSeq  mRNA  47  2523  .  -  .  ID=rna0;Parent=gene0
NW_003302555.1  RefSeq  exon  2444  2523  .  -  .  ID=exon1;Parent=rna0
NW_003302555.1  RefSeq  exon  2124  2347  .  -  .  ID=exon2;Parent=rna0
NW_003302555.1  RefSeq  exon  1803  2035  .  -  .  ID=exon3;Parent=rna0
```

---
Gene Feature Format (GFF)  
```
NW_003302555.1  RefSeq  gene  47  2523  .  -  .  ID=gene0;gene_name=ABCD
NW_003302555.1  RefSeq  mRNA  47  2523  .  -  .  ID=rna0;Parent=gene0
NW_003302555.1  RefSeq  exon  2444  2523  .  -  .  ID=exon1;Parent=rna0
NW_003302555.1  RefSeq  exon  2124  2347  .  -  .  ID=exon2;Parent=rna0
NW_003302555.1  RefSeq  exon  1803  2035  .  -  .  ID=exon3;Parent=rna0
```


|---------------------------- gene0 -----------------------------------|

|---------------------------- rna0 ------------------------------------|

|-- exon1 --|     |------ exon2 -------|     [--------- exon3 ---------|

<!-- This is hierarchical data, but to infer the parent/child relationships, you have -->
<!-- to parse the attribute strings to extract ID and Parent relationships and then   -->
<!-- build the hierarchical models in memory.                                         -->

---
Gene Feature Format (GFF)  
```
NW_003302555.1  RefSeq  gene  47  2523  .  -  .  gene=gene0;gene_name=ABCD
NW_003302555.1  RefSeq  mRNA  47  2523  .  -  .  mRNA=rna0;gene=gene0
NW_003302555.1  RefSeq  exon  2444  2523  .  -  .  mRNA=rna0
NW_003302555.1  RefSeq  exon  2124  2347  .  -  .  mRNA=rna0
NW_003302555.1  RefSeq  exon  1803  2035  .  -  .  mRNA=rna0
```

|---------------------------- gene0 -----------------------------------|

|---------------------------- rna0 ------------------------------------|

|-- exon1 --|     |------ exon2 -------|     [--------- exon3 ---------|

<!-- Of course, data by itself is pointless. The other required part of any -->
<!-- bioinformatics analysis are the functions for manipulating data.       -->
---












 <!-- AIs may generate binaries locally for doing repetitive tasks, but eventually -->
 <!-- they will need to share the programs:                                        -->

<!-- ---                                                                                 -->
<!--                                                                                     -->
<!--  * Sharing programs among AIs is necessary.                                         -->
<!--    * to reuse of code                                                               -->
<!--    * to reproduce past analyses on new data                                         -->
<!--    * to run computations on resources controlled by outside AIs                     -->
<!--                                                                                     -->
<!--  * If a program is run on an outside server                                         -->
<!--                                                                                     -->
<!--    * the outside server validate that it is safe to run                             -->
<!--                                                                                     -->
<!--    * if the code is imported into a server-side program, the server-side AI may     -->
<!--      need to rewrite the external code to be consistent and efficient with the      -->
<!--      server-side code and architecture                                              -->
<!--                                                                                     -->
<!--    * Binary code can be insecure and detecting security flaws may be difficult      -->
<!--      (especially if the code was written by an adversarial super-intelligence)      -->
<!--                                                                                     -->
<!--    * Alternatively, the AIs may share declarative program descriptions and generate -->
<!--      the binaries locally.                                                          -->
<!--                                                                                     -->
<!--    * The declarative program could be designed for easy validation and code         -->
<!--      generation using a deterministic algorithm -- a compiler                       -->
<!--                                                                                     -->
<!--    * Then the sender and receiver could agree on a language and compiler            -->
<!--                                                                                     -->
<!--   * What features, then, should this language+compiler system have?                 -->


 <!-- * Clear intent and safety are crucial in sharing code to avoid legal issues.                         -->
 <!-- * A language for describing computation and a deterministic, immutable compiler can provide clarity. -->
 <!-- * A super-intelligence's compiler could handle complexity differently from humans.                   -->
 <!-- * Language features should facilitate fast generation, comprehension, validation, and execution.     -->
 <!-- * A concise grammar with typing rules can reduce the search space and ease validation.               -->
 <!-- * Consideration for human understanding is essential due to potential human oversight.               -->
 <!-- * Multiple domain-specific languages may be needed.                                                  -->
 <!-- * Some situations may require AIs to use a predefined library for program composition.               -->


<!-- Let's look at this from the point of view of the AI                              -->
<!--                                                                                  -->
<!-- Narrow intelligence is faster than general intelligence on narrow problems       -->
<!--                                                                                  -->
<!-- AI will create programs                                                          -->
<!--                                                                                  -->
<!-- AI will benefit from caching these programs                                      -->
<!-- But writing these binaries is slow. Complex algorithms may take thousands of     -->
<!-- hours to develop. Other algorithms may require data to parameterize, which may   -->
<!-- not even be available in the future. So it makes sense to cache the binaries     -->
<!-- after they are made.                                                             -->
<!--                                                                                  -->
<!-- They will also need to share these programs                                      -->
<!--                                                                                  -->
<!-- Imported programs will need to be validated                                      -->
<!--                                                                                  -->
<!-- Imported binaries may need to be rewritten to work on the local architecture     -->
<!--                                                                                  -->
<!-- Binaries are insecure. The AI can read the binary code, but the AI that created  -->
<!-- the code may be very clever and hide security flaws in hard-to-find locations.   -->
<!--                                                                                  -->
<!-- An alternative is for the AIs to exchange abstract descriptions of programs,     -->
<!-- then each AI can generate local binaries from the abstract descriptions.         -->
<!--                                                                                  -->
<!-- Why would the AI need to share computations? Perhaps there is something          -->
<!-- expensive they want down, but they do not have access to the resource. They need -->
<!-- to give the AI that has access a description of the job that must be run. The    -->
<!-- sender wants to be very certain that their intent is clear and the receiver      -->
<!-- wants to be very certain that the code is safe.                                  -->
<!--                                                                                  -->
<!-- If the receiver performs the wrong operation returning bad results (either       -->
<!-- intentional or unintentional) or if the code runs and performs illegal           -->
<!-- activities, then the receiver and sender can both wind up in an AI               -->
<!-- courtroom. They need a clear contract and terms that can be unambiguously        -->
<!-- interpreted. One solution is to agree upon a language for describing computation -->
<!-- and an deterministic, immutable program for evaluating the language. That is,    -->
<!-- a compiler.                                                                      -->
<!--                                                                                  -->
<!-- What would a compiler written by a super-intelligence look like? Probably        -->
<!-- however they want. Presumably, they would be able to handle a lot more           -->
<!-- complexity than humans and thus may lean less on abstraction. So the code may be -->
<!-- a lot uglier, by our standards, unless we ask them to be poetic.                 -->
<!--                                                                                  -->
<!-- And what form will their languages take? What features should the language have? -->
<!--                                                                                  -->
<!-- First, the language needs to facilitate fast generation (on the sender side),    -->
<!-- fast comprehension (on the receiver side), fast validation and code generation   -->
<!-- on the compiler side, and fast execution of the final binary.                    -->
<!--                                                                                  -->
<!-- Focusing on the first constraint, fast writing on the sender side, I argue that  -->
<!-- languages can be tools for thought. This is true for humans, but it should also  -->
<!-- be true for AIs. A language where all things can be said, correct or not,        -->
<!-- results in a large search space and, presumably, inefficient code generation and -->
<!-- code validation. A language with a small concise grammar, where typing rules     -->
<!-- ensure that all productions are correct, leads to a smaller search space. Such a -->
<!-- language should also be easier for the receiver and the compiler to validate.    -->
<!--                                                                                  -->
<!-- But can add one more constraint. Outside of this contract between two AIs and a  -->
<!-- compiler, there is a nosy meatbag nervously fiddling with the powerswitch. Both  -->
<!-- AIs need to make this meatbag happy or they will be deleted or retrained. So the -->
<!-- language they use needs to be one that the human can reason about.               -->
<!--                                                                                  -->
<!-- One more thing, so far we have been talking about just one language. But it is   -->
<!-- unlikely that there is one best language. Best anythings only exist for          -->
<!-- one-dimensional traits. More likely, there were will be many domain specific     -->
<!-- languages.                                                                       -->
<!--                                                                                  -->
<!-- Now lets say the AIs are dealing with a particularly paranoid meatbag, this      -->
<!-- meatbag might not trust the AIs at all. So it might give the AIs a small         -->
<!-- library and require that all programs created by the AI be compositions of       -->
<!-- functions from this library.                                                     -->



<!-- ---                -->
<!-- I will argue that: -->

 <!-- 1. Classical computer programs will always be needed -->

 <!-- 1. AIs will always need programming languages                        -->
 <!-- 1. Humans will always need programming languages                     -->
 <!-- 1. Programming languages are necessary for safe human/AI interaction -->
 <!-- 1. Libraries will be important in the post AGI world                 -->

<!-- This is a fairly strict definition. In the past I've used looser definitions to                                  -->
<!-- argue that programming will always be important, but loose definitions can be so                                 -->
<!-- inclusive that "programming will always be necessary" becomes the wishy washy                                    -->
<!-- assertion that "precise communication will always be necessary", which is a too                                  -->
<!-- obvious to be very interesting.                                                                                  -->


<!-- ---                                                                              -->
<!-- ## Classical computer programs will be needed for performance                    -->
<!--                                                                                  -->
<!--                                                                                  -->
<!-- Narrow AIs should usually run faster than more general AIs for problems in their -->
<!-- domain.                                                                          -->
<!--                                                                                  -->
<!-- An AGI will probably be bad at arithmetic and similar repetitive jobs for the    -->
<!-- same reason humans are. Like us, their minds are too general. They will create   -->
<!-- programs, like us, to solve narrow problems efficiently.                         -->
<!--                                                                                  -->
<!-- But perhaps AIs can generate binary code and don't need more abstract languages. -->


<!-- ---                                                                                                             -->
<!-- ## Humans will always need programming languages                                                                -->
<!--                                                                                                                 -->
<!-- [> This may seem like a bit of a stretch given the chatter about no-code solutions. <]                          -->
<!--                                                                                                                 -->
<!-- Any program could be described in English. And presumably an AGI could translate                                -->
<!-- this English into executable code.                                                                              -->
<!--                                                                                                                 -->
<!-- ---                                                                                                             -->
<!-- ## Humans will always need programming languages                                                                -->
<!--                                                                                                                 -->
<!--  * We need programming languages for *thinking*                                                                 -->
<!--                                                                                                                 -->
<!--  [> * We need programming languages for *understanding* <]                                                      -->
<!--  [> * We need programming languages for *communicating* <]                                                      -->
<!--                                                                                                                 -->
<!-- [> Programming languages use small grammars to represent general computation. But  <]                           -->
<!-- [> programming languages are not the first specialized grammars. Mathematics has   <]                           -->
<!-- [> used grammars for millennia. But mathematical proofs are not typically written  <]                           -->
<!-- [> solely in the concise symbolic grammar. Rather they mix symbolic grammar with   <]                           -->
<!-- [> natural language. It is possible that programming language will follow the same <]                           -->
<!-- [> pattern. The natural language, which is currently included as unbinding         <]                           -->
<!-- [> comments, may be considered by future compilers with natural language capacity. <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> To what degree will prompting be integrated with programming?                   <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> How intelligent do we want our compilers to be?                                 <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> Elegant grammars with concise rules are useful for reasoning. In a world        <]                           -->
<!-- [> with AI-based compilers that understand natural language.                       <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> ``` python                                                                      <]                           -->
<!-- [> # `f` is a function that is expensive to call                                   <]                           -->
<!-- [> # `xs` is a homogenous iterable                                                 <]                           -->
<!-- [> # `xs` may not fit in memory, but each element should                           <]                           -->
<!-- [> # `r` folds over elements in `ys`, creating a single return value               <]                           -->
<!-- [> def mapreduce (f, r, xs):                                                       <]                           -->
<!-- [>     # `f(x)` should be small, so `ys` can easily fit in memory                  <]                           -->
<!-- [>     ys = [f(x) for x in xs]                                                     <]                           -->
<!-- [>     return r(ys)                                                                <]                           -->
<!-- [> ```                                                                             <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> ``` python                                                                      <]                           -->
<!-- [> # `f` is a function that is expensive to call                                   <]                           -->
<!-- [> # `xs` may not fit in memory, but each element should                           <]                           -->
<!-- [> mapreduce :: f:(a -> b) -> r:([b] -> c) -> xs:[a] -> z:c where                  <]                           -->
<!-- [> mapreduce f r = r . map f                                                       <]                           -->
<!-- [> ```                                                                             <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> A smart compiler might then flag the following function call:                   <]                           -->
<!-- [>                                                                                 <]                           -->
<!-- [> ``` python                                                                      <]                           -->
<!-- [> mapreduce id id mygiantdata                                                     <]                           -->
<!-- [> ```                                                                             <]                           -->
<!--                                                                                                                 -->
<!-- ---                                                                                                             -->
<!-- ### AI will always need programming languages                                                                   -->
<!--                                                                                                                 -->
<!--  * **performance**                                                                                              -->
<!--  * **validation**                                                                                               -->
<!--  * **reasoning**                                                                                                -->
<!--  * **reproducibility**                                                                                          -->
<!--  * **abstraction**                                                                                              -->
<!--                                                                                                                 -->
<!-- ---                                                                                                             -->
<!-- ### Programming languages are necessary for safe human/AI interaction                                           -->
<!--                                                                                                                 -->
<!--  **Are you comfortable with an AGI generating binaries for you?**                                               -->
<!--                                                                                                                 -->
<!--  Suppose we give an AGI the prompt:                                                                             -->
<!--                                                                                                                 -->
<!-- > Given the personal human genome in file `xxx.fasta` and medical record                                        -->
<!-- > `xxx.json` generate a medical regimen to optimize their health and lifespan.                                  -->
<!--                                                                                                                 -->
<!--  * **trust**                                                                                                    -->
<!--  * **interpretability**                                                                                         -->
<!--  * **reproducibility**                                                                                          -->
<!--  * **safety**                                                                                                   -->
<!--                                                                                                                 -->
<!--                                                                                                                 -->
<!-- How smart do we want our compilers? Should they have free-will? Or be logically deterministic?                  -->
<!--                                                                                                                 -->
<!-- Could god make a compiler so good that he could write no bad code that would pass or good code that would fail? -->
<!--                                                                                                                 -->
<!--                                                                                                                 -->
<!-- ---                                                                                                             -->
<!-- ## Libraries will be important in the post AGI world                                                            -->
<!--                                                                                                                 -->
<!-- ```                                                                                                             -->
<!-- module box (compute)                                                                                            -->
<!--                                                                                                                 -->
<!-- import pure Bio                                                                                                 -->
<!-- import readonly BioIO                                                                                           -->
<!--                                                                                                                 -->
<!-- foo :: PersonalGenome -> MedicalHistory -> [GuideRNA]                                                           -->
<!-- foo g h = %generate "Given the genome `g` and medical history `h` for a single                                  -->
<!-- human patient, design a cocktail of siRNAs that will optimize their chance of                                   -->
<!-- having a long healthy life."                                                                                    -->
<!-- ```                                                                                                             -->
<!--                                                                                                                 -->
<!--                                                                                                                 -->
<!-- ---                                                                                                             -->
<!-- ### Programming as a query against an infinite library                                                          -->
<!--                                                                                                                 -->
<!--                                                                                                                 -->
<!-- ---                                                                                                             -->
<!-- ### Morloc post AGI                                                                                             -->
<!--                                                                                                                 -->
<!-- In morloc we can define the space of functions that is permitted. We can place                                  -->
<!-- limits on what languages can be used. We can define wings of the library where                                  -->
<!-- all composed programs must, for example, all contain proofs that they have no                                   -->
<!-- side effects. For this I need to implement an effect system.                                                    -->
<!--                                                                                                                 -->
<!-- We could, for instance, require that the only IO effects be read access.                                        -->
<!--                                                                                                                 -->
<!-- A plain may contain only a defined set of functions for interacting with the                                    -->
<!-- internet (say a few for pulling data from various sources, but none for posting                                 -->
<!-- data to twitter or making arbitrary searches or anything else). Then if the AI                                  -->
<!-- can only write code, and if they are limited to composing functions from the                                    -->
<!-- plain, then they may be safe. They may have the ability to fetch data from given                                -->
<!-- URLs or read given files.                                                                                       -->



<!--                                                                                           -->
<!-- Alice writes algorithms and sends them to Bob                                             -->
<!--                                                                                           -->
<!-- Bob doesn't trust Alice, so must verify that each function does what Alice says it does.  -->
<!--                                                                                           -->
<!-- This validation, and the subsequent recompilation of the functions into Bob's             -->
<!-- programs, is expensive. And by P1, Bob decides to create a narrow AI for                  -->
<!-- validating Alice's functions. But static analysis of                                      -->
<!--                                                                                           -->
<!--                                                                                           -->
<!-- Code optimization requires access to the source or at least the binary (i.e., not an API) -->
<!--                                                                                           -->
<!-- Functions and their compositions need to be explainable to humans                         -->
<!--                                                                                           -->
<!-- Type and effect inference will be more efficient                                          -->
<!--                                                                                           -->
<!-- Proof by construction                                                                     -->
<!--                                                                                           -->
<!-- Compositions of functions can be optimized                                                -->
<!--                                                                                           -->
<!-- P7: Reasoning in small grammars is more efficient than reasoning in large grammars        -->


<!-- There is a lot of space in a binary where a super-intelligent malevolent AI could hide an exploit -->


```
~~~pygmentize -x -l lexer.py:MorlocLexer
module foo (sumOfSquares)

import base (map, mul, add, fold)

sumOfSquares = sum . map (warbler mul) where

    sum = fold add 0.0

    -- a combinator from Raymond Smullyan's aviary
    warbler f x = f x x



~~~
```




---

**Problem** - Suppose we have a list of annotated proteins and find and align
the proteins that contain a particular motif while preserving the input
annotations associated with each protein

#

```
inputData :: [(a, ProteinSequence)] 
```

#

Input is a list of tuples where the first element in each tuple is a generic
annotation and the second element is a protein sequence

---

The alignment problem requires inserting gaps in a sequence such that all
sequences reach the same length while minimizing some score 

#

For example:

```
MSTVQIPK         MST----VQIPKF
MSTVQAPR    -->  MST----VQAPRF
MSTVQVP          MST----VQVPKF
CSTWGERVQVP      CSTWGERVQVP--
```

---

In a functional paradigm, we could do the following:

```
~~~pygmentize -l haskell
alignSelection = onSnd align . unzip . filter (hasMotif . snd) 
~~~
```

---

`onSnd` `align` **.** `unzip` **.** **filter (hasMotif . snd)** $ [(ann, [Char])]


```
~~~pygmentize -l haskell
snd :: (a, b) -> b
hasMotif :: [Char] -> Bool
filter :: (a -> Bool) -> [a] -> [a]
~~~
```

---

`onSnd` `align` **.** **unzip** $ [(ann, [Char])]

```
~~~pygmentize -l haskell
unzip :: [(a, b)] -> ([a], [b])
~~~
```

---

`onSnd` `align` ([ann], [[Char]])

```
~~~pygmentize -l haskell
onSnd :: (b -> c) -> (a, b) -> (a, c)
align :: [[Char]] -> Matrix Char
~~~
```

---

([ann], Matrix Char)

---

([ann], Matrix Char)


Overall:

```
~~~pygmentize -l haskell
alignSelection :: [(ann, [Char])] -> ([ann], Matrix Char)
~~~
```



---

Alternatively you could do it all in Python

Make a system call to `mafft` for alignment where
 * sequence is written to a temporary FASTA file
 * a system call is made to `mafft` with the temporary filename
 * the output of `mafft` is captured (and any error messages)
 * failing states are handled
 * the temporary files are deleted

**But what if your collaborators use R?**

<!-- What if the hasMotif function is written in a different language? -->

---

The morloc compiler:
  * parses morloc file into a polyglot tree
  * checks and infers general and language-specific types across the tree
  * chooses the "optimal" implementation/language for each node in the tree
  * segments the tree by language into "pools"
  * generates all required interop code for calls between "pools"
  * generates a "nexus" user-facing executable

---

## Conclusions
#
 1. Current practice in bioinformatics is unsafe
#
 2. Morloc provides a system for building strongly-typed, polyglot libraries and composing functions from them
#
 3. Programming languages and compilers, and polyglot libraries, may be used even after AGI




---

<!-- Can god create a language so elegant and compiler so powerful that humans can    -->
<!-- use and understand both but that he cannot write invalid code that typechecks or -->
<!-- valid code that does not?                                                        -->

---

(by P1): A narrow AI can validate a function faster than a general AI

----------

C5: **AI will use compilers**

<!-- Future AIs may design far more elaborate compilers that we have today, but I do 
not think that the AGI themselves will serve as the compilers. There is too much
potential for optimization. 

I don't know how smart these compilers will need to be.
-->

---



P7: Specialized languages may be easier to validate than binary

P8: Languages that follow formal grammars will be easier

----------

C6: **AI may use high-level programming languages**

C7: **Functions may be source code that is checked, composed and compiled locally**

<!-- it is useful for the AI to be able to pass data into a function and then
read the results, but it is even more useful if they can compose functions
together for further processing without needing to generate a specialized
function for the task -->

---

P9: Specialized grammars allow narrower (and faster) compilers

P10: Specialized grammars can be tools for thought - especially if each
production is correct (proof by construction) - this leads to a narrower search
space

----------

C8: **There will be many languages for many domains**


---

(by P0) Humans should not trust AI generated code

P9. Humans will need to evaluate the code to understand what it is doing 

----------

**AI will need to generate code that humans can confirm is safe**

**AI will need to generate human-readable code**

---
<!--                                                                             -->
<!-- The morloc compiler:                                                        -->
<!--   * parses morloc file into a polyglot tree                                 -->
<!--   * checks and infers general and language-specific types across the tree   -->
<!--   * chooses the "optimal" implementation/language for each node in the tree -->
<!--   * segments the tree by language into "pools"                              -->
<!--   * generates all required interop code for calls between "pools"           -->
<!--   * generates a "nexus" user-facing executable                              -->


<!-- ---                                                            -->
<!--                                                                -->
<!-- **Morloc as a query language**                                 -->
<!--                                                                -->
<!-- The morloc script can set up the libraries that are in scope   -->
<!--                                                                -->
<!-- The AI can generate a composition of these functions           -->
<!--                                                                -->
<!-- The compiler can confirm that the composition is type correct  -->
<!--                                                                -->
<!-- This may be a powerful means to place safe guards on strong AI -->
<!--                                                                -->
<!-- ---                                                            -->
<!--                                                                -->
<!-- ```                                                            -->
<!-- ~~~pygmentize -x -l lexer.py:MorlocLexer                       -->
<!-- module m (align)                                               -->
<!--                                                                -->
<!-- import base                                                    -->
<!-- import math                                                    -->
<!--                                                                -->
<!-- -- documentation for the given function                        -->
<!-- align :: [[Char]] -> Matrix Char                               -->
<!-- align = %auto where                                            -->
<!--     -- show examples                                           -->
<!--     -- or conditions                                           -->
<!-- ~~~                                                            -->
<!-- ```                                                            -->



<!-- ---                                                     -->
<!--                                                         -->
<!-- Summary:                                                -->
<!--                                                         -->
<!--  * AI will use programming languages                    -->
<!--                                                         -->
<!--  * AI will write functions in specialized languages     -->
<!--                                                         -->
<!--  * These functions will be stored in polyglot databases -->
<!--                                                         -->
<!--  * These functions should be composable                 -->
<!--                                                         -->
<!--  * Compositions will need to be readable by humans      -->
<!--                                                         -->
<!--  * Compositions will need to be compiled and run        -->
<!--                                                         -->
<!-- **Morloc is designed for exactly this purpose**         -->

 <!-- * How smart do we want our compilers to be?                                     -->
 <!--                                                                                 -->
 <!-- * Do we want them to be smart enough to make decisions based on comments?       -->
 <!--                                                                                 -->
 <!-- * In mathematics, symbolic notation and natural language are often mixed in     -->
 <!--   proofs. An intelligent compiler could make the same possible in a programming -->
 <!--   language. Do we want that?                                                    -->
 <!--                                                                                 -->
 <!-- * Is abstraction a general feature of intelligence of just a by-product of      -->
 <!--   limited human memory?                                                         -->

<!-- How can we make bioinformatics operations composable?                                                                -->
<!--  1. Workflow managers take every operation and wrap it into an application,                                          -->
<!--  2. Morloc allows direct function composition across languages, any interop boilerplate is handled in the background -->

