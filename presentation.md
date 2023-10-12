---
title: "Programming in the post AGI world"
author: ""
date: ""
paging: ""
theme: "glamour.json"
---
######
######

                     Morloc and programming in the post AGI world

                                     Oct 14, 2023

                                   Zebulun Arendsee

<!-- Good afternoon, today I will present my work on the development of the morloc    -->
<!-- programming language and its relevance to the programming in the post AGI world. -->
<!--                                                                                  -->
<!-- But first, I should introduce myself.                                            -->

---
                                My official background

 * Molecular Biology, B.Sc
 * Bioinformatics and Computational, PhD
 * USDA postdoc
 * Regeneron scientist


<!--
I am a scientific programmer

As an undergrad I studied molecular biology and worked in a lab studying multicellularity in bacteria.

In grad school I transitioned to bioinformatics and computational biology where I researched the evolutionary origins of novel proteins.

As a postdoc I worked at the USDA on phylogenetic patterns in influenza.

Currently, I work on whole genome sequencing and the design of genetic medicine at Regeneron.

This is my official background.

Unofficially, for the last 5 years I've been working on the morloc programming language. 
-->

---
#
##
 1. The systemic flaw of bioinformatics that motivated the creation of morloc
##
 2. The philosophy and design of morloc
##
 3. Programming languages after AGI and the relation to Morloc


---
##
**Bioinformatics is data science focused on biological data**

<!-- Bioinformatics is data science focused on biological data                        -->
<!--                                                                                  -->
<!-- A bioinformatics analysis                                                        -->
<!--                                                                                  -->
<!--                                                                                  -->
<!-- I will introduce you to how data is stored and then to the kind of tools used to -->
<!-- manipulate the data.                                                             -->
<!--                                                                                  -->
<!-- Bioinformatics data is stored in flat textual files                              -->

---

FASTA format

```
~~~pygmentize -x -l lexer.py:FastaLexer
>NC_000004.12:3074681-3243960 HTT [organism=Homo sapiens] [GeneID=3064]
GCTGCCGGGACGGGTCCAAGATGGACGGCCGCTCAGGTTCTGCTTTTACCTGCGGCCCAGAGCCCCATTC
ATTGCCCCGGTGCTGAGCGGCGCCGCGAGTCGGCCCGAGGCCTCCGGGGACTGCCGTGCCGGGCGGGAGA
~~~
```

---

FASTA format

```
~~~pygmentize -x -l lexer.py:FastaLexer
>1C.2.1|KT362191|A/swine/Italy/14-30549/2014|H1N1|H1|ITA|2014-03-12
ATGAAAGCAAAATTATTTGTATTATTCTGTGCATTTACTGCACTGAAAGCTGACACCATTTGTGTAGGCTATCATGCTAA
CAATTCCACAGACACTGTCGACACGATACTGGAAAAGAATGTTACTGTTACCCATTCAGTTAATTTACTAGAAAGCAGCC
~~~
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

```
|---------------------------- gene0 -----------------------------------|

|---------------------------- rna0 ------------------------------------|

|-- exon1 --|     |------ exon2 -------|     [--------- exon3 ---------|
```

<!--
This is hierarchical data, but to infer the parent/child relationships, you have
to parse the attribute strings to extract ID and Parent relationships and then
build the hierarchical models in memory.
-->

---
Gene Feature Format (GFF)  
```
NW_003302555.1  RefSeq  gene  47  2523  .  -  .  gene0;gene_name=ABCD
NW_003302555.1  RefSeq  mRNA  47  2523  .  -  .  rna0;gene_id=gene0
NW_003302555.1  RefSeq  exon  2444  2523  .  -  .  rna_id=rna0
NW_003302555.1  RefSeq  exon  2124  2347  .  -  .  rna_id=rna0
NW_003302555.1  RefSeq  exon  1803  2035  .  -  .  rna_id=rna0
```

```
|---------------------------- gene0 -----------------------------------|

|---------------------------- rna0 ------------------------------------|

|-- exon1 --|     |------ exon2 -------|     [--------- exon3 ---------|
```

<!--
But the ID and Parent fields are not standardized
-->
---

Bioinformatics data is transformed by command line tools

---

Bioinformatics data is transformed by command line tools

```
gatk HaplotypeCaller \
    -R reference.fasta \
    -I input.bam \
    -O output.vcf \
    -ERC GVCF \
    -L intervals.bed \
    -ip 100 \
    -stand-call-conf 30 \
    -A QualByDepth \
    -G StandardAnnotation
```
---

Bioinformatics data is transformed by command line tools

```
java -jar trimmomatic-0.39.jar PE -threads 4 \
    raw_R1.fastq.gz raw_R2.fastq.gz \
    trimmed_R1.fastq.gz unpaired_R1.fastq.gz \
    trimmed_R2.fastq.gz unpaired_R2.fastq.gz \
    ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 \
    LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
```

---

Bioinformatics data is transformed by command line tools

```
mafft --thread 4 input.fasta > mafft_aligned.fasta

clustalw2 -INFILE=input.fasta -OUTFILE=clustalw_aligned.fasta

muscle -in input.fasta -out muscle_aligned.fasta -maxiters 2
```

---

Bioinformatics data is transformed by command line tools

```
blastp -query protein.fasta -db proteome -out result.xml -evalue 1e-10 \
    -max_target_seqs 1000 -num_threads 8 -outfmt "6 qseqid sseqid pident \
    length qstart qend sstart send evalue bitscore" -num_alignments 10

diamond blastp -d proteome.dmnd -q protein.fasta -o result.tsv -e 1e-10 -p 8 \
    --outfmt 6 qseqid sseqid pident length qstart qend sstart send evalue \
    bitscore -k 10

hmmscan --domtblout result.domtblout --cut_ga -E 1e-10 -o /dev/null \
    custom.hmm protein.fasta
```

---

After the arguments are parsed the tool must:

 1. Check arguments for consistency (occasionally)

 1. Parse input files to an internal data structure

 1. Validate the data

 1. Apply the data and parsed args to the appropriate internal function

 1. Print log messages

 1. Catch and log errors

 1. Format results and write them to the system 

 1. Clean up any temporary files

 1. Return an exit code

---

A bioinformatics workflow is a series of calls to command line tools

---

```
~~~pygmentize -l bash
cat $H1 |
    smof grep -P '[Ss]wine' |
    perl -pe 's;>.*\|(A/[^|]*).*(1[ABC][^|]*|Other-[^|]*).*;>$1|$2;' |
    smof clean -xu | smof uniq -f |
    flutile trim ha1 --subtype=H1 --conversion=dna2aa | smof clean -xu > 800-trimmed.faa
~~~
```
---
```
~~~pygmentize -l bash
cat *fna |
    grep '>' |
    grep -v Consensus |
    awk 'BEGIN {FS="|"; OFS="\t"} {print $0, $12, $10, $11, $8, "<" $0 ">", $9}' |
    sed 's/>//' |
    sed 's/<>.*lab-[^ ]*/Previously-Tested/' |
    sed 's/<>\([^|]*\)[^	]*/\1/' |
    sed 's/		/	?	/g' > metadata.tsv
~~~
```
---
```
~~~pygmentize -l bash
maketree_iqtree (){
    # iqtree converts slashes to underscores for some reason
    # so I replace '/' with 'SLASH' and sed back to '/' after tree building
    sed 's;|;BAR;g' $1 | sed 's;/;SLASH;g' > .x

    # iqtree dumps a lot of junk into the working directory and writes STDOUT
    # to the console (where STDERR should be written). The tree file we want is
    iqtree -s .x -nt AUTO > /dev/null

    sed 's;BAR;|;g' $1.treefile | sed 's;SLASH;/;g'
}
~~~
```
---

```
>EPI_ISL_304421
TGGCTGGTTAAAAAAGGAAATTCATACCCAAAGCTCAACCAAACCTACATTAATGATAAAGGGAAAGAAGTCCTCGTGCT
CATCAAGATACAGCAAGAAGTTCAAGCCGGAAATAGCAACAAGACCCAAAGTGAGGGATCAAGAAGGGAGAATGAACTAT
ACAAAATTGAGACTGGCCACAGGATTGAGGAATGTCCCGTCTATTCAATCTAGAGGCCTATTCGGGGCCATTGCCGGCTT
CATTGAAGGGGGGTGGACAGGGATGGTAGATGGATGGTACGGTTATCACCATCAAAATGAGCAGGGGTCAGGATATGCAG
CCGATCTGAAGAGCACACAAAATAAUSTRIAHAATGAAGGCAATACTAGTAGTTCTGCTGTATACATTTACAACCGCAAA
TGCAGACACATTATGTATAGGTTATCATGCGAACAATTCAACAGACACTGTAGACACAGTACTAGAAAAGAATGTAACAG
TAACACACTCTGTTAATCTTCTGGAAGACAAGCATAACGGAAAACTATGCAAACTAAGAGGGGTAGCCCCATTGCATTTG
ATGGATGGTACGGTTATCACCATCAAAATGAGCAGGGGTCAGGATATGCAGCCGATCTGAAGAGCACACAAAAT
```

---

```
>EPI_ISL_304421
tggctggttaaaaaaggaaattcatacccaaagctcaaccaaacctacattaatgataaagggaaagaagtcctcgtgct
catcaagatacagcaagaagttcaagccggaaatagcaacaagacccaaagtgagggatcaagaagggagaatgaactat
acaaaattgagactggccacaggattgaggaatgtcccgtctattcaatctagaggcctattcggggccattgccggctt
cattgaaggggggtggacagggatggtagatggatggtacggttatcaccatcaaaatgagcaggggtcaggatatgcag
ccgatctgaagagcacacaaaataAUSTRIAhaatgaaggcaatactagtagttctgctgtatacatttacaaccgcaaa
tgcagacacattatgtataggttatcatgcgaacaattcaacagacactgtagacacagtactagaaaagaatgtaacag
taacacactctgttaatcttctggaagacaagcataacggaaaactatgcaaactaagaggggtagccccattgcatttg
atggatggtacggttatcaccatcaaaatgagcaggggtcaggatatgcagccgatctgaagagcacacaaaat
```

---

My guess:

The bioinformatician may have meant to do this

```
sed 's/\(>.*|\)AUT\(|.*\)/\1Austria\2/'
```

But instead did:

```
sed 's/\(>.*|\)AUT\(|.*\)/Austria/'
```

This removed the entry separator and joined two sequences around 'Austria"

---
**There are no compound data structures**

Rather than `[(ann, Sequence)]`, you would have a fasta file:

```
~~~pygmentize -x -l lexer.py:FastaLexer
>{annotation string}
{sequence string}
~~~
```

---
**There are no generics**

 * No generic annotation can be defined, rather raw untyped string is passed

 * General functions like `filter` and `map` are not possible

 * Instead specialized tools are needed for each format

---
 **There are no higher-order functions**

Even if you have `map` and `filter` commands for your specific format, you
cannot easily give them general functions to map or filter over the entries  

---
 **CLI tools with synonymous roles cannot be easily substituted**

 * Every tools parses arguments in a different way
 * Different input and output formats are supported
 * They may have different (and often undocumented) side effects

---
**Every command is a system call**

---
**Every tool is a monolith**

Tools are responsible for too many concerns

What should be simple algorithm implementation become software engineering projects

These projects require maintenance

Most bioinformatics tools are broken

---

**The root problem**

We have applications instead of libraries, and data files instead of data
structures, because *there is no one language everyone uses*

Writing single-language libraries limits the userbase 

Unifying the community around one language would improve things, but:
 1. unification is unlikely
 2. no one language is best for everything
 3. mono-culture leads to stagnation

---
**There are alternatives to bash**

 * Scripting languages with wrappers around the large external tools
 * Make-like workflow managers with plugins wrapping tools (and many config files)

---

So what is the solution?

---
**Surprise! Morloc!**

---
**Surprise! Morloc!**

`morloc`

 * allows function composition across languages
 * unifies many languages under one type system

<!-- This way, I can have my language and eat yours too -->
---

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

<!-- This morloc script defines a module "foo" that exports the single function
     `sumOfSquares`.

     It imports the functions `map`, `mul`, `add` and `fold` from a module named
     `base`

     Notice that `morloc` here is importing basic arithmetic operators. Morloc has no
     functions defined internally.

     `sumOfSquares` is implemented as a composition of a sum and map applied to
     warbler applied to mul. Where sum is defined with the imported fold operator and
     warbler is a basic combindators that applies the second argument twice to the
     first argument, in this case, it is used to square an input number.

     There is nothing too surprising here. Morloc features higher-order functions,
     generics, type inference, and parameterized types. Basically, it is a simple
     subset of Haskell. What is unique about morloc, is deeper down in the libraries,
     so lets look into `base`.                                                        -->

---

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module base (map, mul, fold)

-- load language-specific implementations
source cpp from "base.hpp" ("map", "mul", "fold")
source py from "base.py" ("map", "mul", "fold")

map :: (a -> b) -> List a -> List b
map Cpp :: (a -> b) -> "std::vector<$1>" a -> "std::vector<$1>" b
map Py :: (a -> b) -> "list" a -> "list" b

mul :: Real -> Real -> Real
mul Cpp :: "double" -> "double" -> "double"
mul Py :: "float" -> "float" -> "float"

fold :: (b -> a -> b) -> b -> List a -> b
fold Cpp :: (b -> a -> b) -> b -> "std::vector<$1>" a -> "std::vector<$1>" b
fold Py :: (b -> a -> b) -> b -> "list" a -> "list" b
~~~
```

<!-- `map`, `mul` and `fold` are all general functions that should have
implementations in most languages. So they are included in the `base` morloc
library (which roughly corresponds to Haskell's prelude).

First we source these functions.

Then we map the general type signature for each function to each of the
language-specific signatures. -->

---


```
~~~pygmentize -x -l lexer.py:MorlocLexer
module base (map, mul, fold)

source Cpp from "base.hpp" ("map", "mul", "fold")
source Py from "base.py" ("map", "mul", "fold")

type Cpp (List a) = "vector<$1>" a
type Py (List a) = "list" a

type Cpp Real = "double"
type Py Real = "float"

map :: (a -> b) -> List a -> List b
mul :: Real -> Real -> Real
fold :: (b -> a -> b) -> b -> List a -> b
~~~
```

---

base.hpp
```
~~~pygmentize -l C++
#ifndef __MORLOC_CPPBASE_BASE_HPP__
#define __MORLOC_CPPBASE_BASE_HPP__

#include <vector>
#include <functional>

template <class A, class B>
B fold(std::function<B(B,A)> f, B y, std::vector<A> xs){
    for(std::size_t i=0; i < xs.size(); i++){
        y = f(y, xs[i]);
    }
    return y;
}

#endif
~~~
```

---

base.py
```
~~~pygmentize -l python
def fold(f, b, xs):
  for x in xs:
    b = f(b, x)
  return b
~~~
```

<!-- Nothing in these files in morloc-specific. No morloc libraries need to
imported. The programmer doesn't need to know anything about morloc. For the
most part, they simply must write functions that correspond to the expected type
signature.

I know I am skimming over a lot of details about the type system, polyglot
typechecking and compiling, but I need to move on.
-->

---
##
##

                       Morloc is focused on building libraries

                      Libraries are functions + type signatures

---
**Why type signatures are nice**

A signature describes a family of functions

---
**Why type signatures are nice**

A signature describes a family of functions

Some families are empty:

``` haskell
~~~pygmentize -l haskell
absurd :: () -> a    -- from nothing, anything 
~~~
```

---
**Why type signatures are nice**

A signature describes a family of functions

Some families have a single non-pathological member:

``` haskell
~~~pygmentize -l haskell
identity :: a -> a
snd :: (a, b) -> b
~~~
```

---
**Why type signatures are nice**

A signature describes a family of functions

Some families contain many different functions:

``` haskell
~~~pygmentize -l haskell
? :: List a -> List a
~~~
```

This could be `sort`, `reverse`, `tail` etc
```

---
**Why type signatures are nice**

A signature describes a family of functions

Some families may describe a type of algorithm:

``` haskell
~~~pygmentize -l haskell 
-- make a multiple sequence alignment from a list of sequences
alignment :: List (List Char) -> Matrix Char
~~~
```

---
**Why type signatures are nice**

A signature describes a family of functions

Some families may describe a type of algorithm:

``` haskell
~~~pygmentize -l haskell
-- make a phylogenetic tree from a sequence alignment
treeFromAlignment :: Matrix Char -> Tree Index
~~~
```

---
**Why type signatures are nice**

A signature describes a family of functions

Some families may describe a type of algorithm:

```haskell
~~~pygmentize -l haskell
-- make a phylogenetic tree from a distance matrix
treeFromDistanceMatrix :: Matrix Real -> Tree Index
~~~
```

---
**Why type signatures are nice**

A signature describes a family of functions

Some families may describe a type of algorithm:

``` haskell
~~~pygmentize -l haskell 
-- make a multiple sequence alignment using a guide tree
alignWithGuide :: Tree Index -> List (List Char) -> Matrix Char
~~~
```


---
**Why type signatures are nice**

 Type signatures are:
 * machine-verified documentation
 * a means to organize and query function space
 * a conceptual framework for exploring algorithms <!-- for distilling what is most fundamental about them -->

---
# Morloc Summary

 * allows functions to be composed across languages
 * supports the construction of polyglot libraries
 * organizes libraries (and composition) under a common type system

<!-- Morrloc is a foundation for designing libraries where functions come from many languages -->

---
## Programming in the post AGI world

```
~~~graph-easy --as=boxart
[ AI ] ----> [ AI ] ----> [ Human ] ----> [ Human ]
[ Human ] ----> [ AI ]
~~~
```

<!-- Next I will talk about programming in the post AGI world and the relevance of    -->
<!-- morloc. Everything I say, of course, is speculative. Hopefully, my thoughts here -->
<!-- will serve as a starting point for good discussions. To that end, I've laid out  -->
<!-- the rest of the talk as an argument with clear premises and conclusions. Later,  -->
<!-- we can argue about whether the premises are true and the conclusions justified.  -->


---
## Programming in the post AGI world

```
~~~graph-easy --as=boxart
[ AI ] ----> [ AI ] ----> [ Human ] ----> [ Human ]
[ Human ] ----> [ AI ]
~~~
```

P0: Everyone is evil

---
P1: Narrow intelligence outperforms general intelligence for narrow problems

                                (stupid is fast)
----------
C1: **So AI will benefit from writing functions**

<!-- A general AI might be able to decompress a file with its native intelligence,
just like a human can, but it would be faster to automate the repetitive work
with a dedicated function -->

---
P2: Writing efficient functions may be expensive
#
----------
C2: **Caching is needed**

C3: **A searchable library (database) of functions is needed**

<!-- You don't want to generate a highly optimized algorithm and then just throw it out -->

---
P3: Functions may need to be shared

P4: Shared function need to be provably safe
#
----------
C4: **Inferring types and effects from the function itself must be possible**


<!-- What are these shared functions? They certainly could be functions in binary
shared libraries. Next I will argue that they should be higher programming
languages. -->


---
P5: Functions need to be composable

P6: Composition requires knowledge of function types and effects

P0: Function metadata may not be trustworthy (everyone is evil)
#
----------
C4: **Inferring types and effects from the function itself must be possible**

<!-- Do the functions mutate their inputs? Write to the console? Raise errors?
These effects need to be properly propagated. Of course, the creator of the
function could package the function with a description of its types and effects,
but if we are sharing functions, if we do not trust our past selves, or if we
worry the database may be insecure, then we cannot trust the metadata. You may
not trust the function creator (even if it was your past self)

If you don't trust the other, then you can't trust their documentation 
Documentation is the cached results of someone else's static analysis -->

<!-- Let's talk about these functions. What are they? Binary shared libraries? Or  -->
<!-- something higher? I'll argue next that they should be declarative, high-level -->
<!-- descriptions of computation rather than binaries.                             -->

---

(by P1): A narrow AI may validate a function faster than a general AI

P7: Validating binary is unnecessarily complicated

----------
C5: **AI will use compilers**

<!-- Future AIs may design far more elaborate compilers that we have today, but I do 
not think that the AGI themselves will serve as the compilers. There is too much
potential for optimization.                                                      -->

---

P7: Specialized languages may be easier to validate than binary

----------
**AI may use declarative programming languages**

**Functions may be shared source code and validated, composed and compiled locally**

<!-- it is useful for the AI to be able to pass data into a function and then
read the results, but it is even more useful if they can compose functions
together for further processing without needing to generate a specialized
function for the task -->

---

(by P0) Humans should not trust AI generated binaries 

Px. Humans may need to evaluate the code to understand exactly what it is doing 

----------

**AI will need to generate code that is provably benign**

**AI will need to generate readable code in a readable language**

----------

**AI will use a declarative language**


---
## How is this related to morloc?

```
~~~graph-easy --as=boxart
[ AI ] ----> [ AI ] ----> [ Human ] ----> [ Human ]
[ Human ] ----> [ AI ]
~~~
```

All entities can share and build programs safely given a language + compiler

---

Morloc is useful because it allows humans to design libraries and reason about
their compositions.

If an AI is restricted to generating code given only the libraries the human has
provided, and perhaps a set limit to the size of the abstract syntax tree, then
they can safely generate code for us.

---
## Conclusions

 1. Bioinformatics is going to kill everyone
 2. Programming languages + compilers will be a necessary means of communicating processes far into the future
 3. Morloc provides a conceptual framework for thinking about functions and building libraries

---

## Questions?

#
#
#
#
#

Name: Zebulun Arendsee 

Morloc Github: `https://github.com/morloc-project/morloc`

Email: `zbwrnz@gmail.com`
