---
title: "Programming in the post AGI world"
author: ""
date: ""
#paging: ""
theme: "glamour.json"
---
######
######

                       Programming in the post AGI world

                                  Oct 14, 2023

                                Zebulun Arendsee

---
#
##
 1. The systemic flaw of bioinformatics that motivated the creation of morloc
##
 2. The philosophy and design of morloc
##
 3. Programming in the post-AGI world and the possible relevance of morloc


---
##
**Bioinformatics is data science focused on biological data**

---
##
**Bioinformatics is data science focused on biological data**
 #
 * It is behind the results of nearly every biology paper
 #
 * It is used to design mRNA vaccines
 #
 * It is the foundation of genetic medicine
 #
 * It may someday be used to fine tune your gene expression

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

FASTA format

```
~~~pygmentize -x -l lexer.py:FastaLexer
>1C.2.1|KT362191|A/swine/Italy/14-30549/2014|H1N1|H1|ITA|2014-03-12
ATGAAAGCAAAATTATTTGTATTATTCTGTGCATTTACTGCACTGAAAGCTGACACCATTTGTGTAGGCTATCATGCTAA
CAATTCCACAGACACTGTCGACACGATACTGGAAAAGAATGTTACTGTTACCCATTCAGTTAATTTACTAGAAAGCAGCC
>1C.2.3|MN393712|A/swine/Liaoning/BX266/2015|H1N1|H1|CHN|2015-11-19
ATGGAAGCAAAACTATTTGTATTATTCTGTGCATTCACTGCACTGAAAGCTGACACCATTTGTGTAGGCTACCATGCCAA
CAATTCCACAGACACTGTCGACACTATACTAGAGAAAAATGTGACTGTTACCCATTCAGTTAATTTACTGGAAAACAGCC
>1C.1|KR701057|A/swine/England/024079/2013|H1N1|H1|GBR|2013-08-21
ATGGAGACAAAACTGTTTGTGTTATTCTGCACATTTACTGCATTAAAAGCTGATACCATCTGTGTAGGCTATCATGCTAA
CAACTCCACAGATACTGTTGACACAATACTGGAGAAGAATGTAACTGTCACCCATTCCGTTAACTTACTAGAAAGCAGCC
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

---

Bioinformatics data is transformed by command line tools

---

Bioinformatics data is transformed by command line tools

```
$ gatk HaplotypeCaller \
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
$ mafft --thread 4 input.fasta > mafft_aligned.fasta

$ clustalw2 -INFILE=input.fasta -OUTFILE=clustalw_aligned.fasta

$ muscle -in input.fasta -out muscle_aligned.fasta -maxiters 2
```

---

Bioinformatics data is transformed by command line tools

```
$ blastp -query protein.fasta -db proteome -outfmt \
    "6 qseqid sseqid pident length qstart qend sstart send evalue bitscore"

$ diamond blastp -d proteome.dmnd -q protein.fasta --outfmt \
    6 qseqid sseqid pident length qstart qend sstart send evalue

$ hmmscan --domtblout result.domtblout --cut_ga -E 1e-10 -o /dev/null \
    custom.hmm protein.fasta
```

---

After the arguments are parsed the tool must:

 1. Check arguments for consistency

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
smof grep -P '[Ss]wine' input.fasta | # find swine
    perl -pe 's;>.*\|(A/[^|]*).*(1[ABC][^|]*|Other-[^|]*).*;>$1|$2;' | # munge headers
    smof clean -xu | # clean output files
    smof uniq -f | # find uniq files by header
    flutile trim ha1 --subtype=H1 --conversion=dna2aa | # the one important command
    smof clean -xu > trimmed.faa # last cleaning
~~~
```

---
```
~~~pygmentize -l bash
cat *.fasta |
    grep '>' |
    grep -v Consensus |
    awk 'BEGIN {FS="|"; OFS="\t"} {print $0, $12, $10, $11, $8, "<" $0 ">", $9}' |
    sed 's/>//' |
    sed 's/<>.*lab-[^ ]*/Previously-Tested/' |
    sed 's/<>\([^|]*\)[^\t]*/\1/' |
    sed 's//\t?\t/g' > metadata.tsv
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
    # to the console (where STDERR should be written).
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

The bioinformatician may have meant to do something like this

```
sed 's/\(>.*|\)AUT\(|.*\)/\1Austria\2/'
```

But accidentally did:

```
sed 's/\(>.*|\)AUT\(|.*\)/Austria/'
```

This removed the entry separator and joined two sequences around "Austria"

---
**There are no compound data structures**

In a programming language you might have a structure of type:

`List (Annotation, Sequence)`

But FASTA format records the structure as a raw string:

```
~~~pygmentize -x -l lexer.py:FastaLexer
>{annotation string}
{sequence string}
~~~
```

---
**There are no generics**

 * Annotations have to be passed as raw strings

 * Basic generic functions like "sort" are not possible

 * Instead specialized command line tools are needed for each format

---
 **Functions cannot be passed as arguments**

 * No generic `map`, `filter`, or `fold` operations

---
**Command line tools with the same role cannot be easily substituted**

 * Parse arguments in different ways
 * Parse input and write output in different formats/flavors
 * Have different side effects

---
**Alternatives to bash**

Workflow managers:
 * Require wrappers around command line tools
 * Specify workflows with Make-like rule languages (usually)
 * They manage filenames, handle dependencies, track providence, report
   progress, and cache results
 * But they suffer from the same weak abstractions as bash workflows

Scripting languages (e.g., Python or R):
 * Require wrappers around command line tools
 * Allow stronger abstractions
 * But they split the community by language

---

Overall:

#

* We have applications instead of functions

#

* We have data files instead of data structures

---

So what is the solution?

---

So what is the solution?

We could try unifying the community around one language, but:
 * no one language is best for everything
 * mono-culture leads to stagnation
 * people like their current languages (e.g., Python and R)

---

So what is the solution?

**Morloc!!!**

---

So what is the solution?

**Morloc!!!**


                              function composition

                                across languages

                           under a common type system

---

foo.loc

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module foo (sumOfSquares)

import base (map, mul, add, foldl)

sumOfSquares = sum . map (\x -> mul x x) where

    sum = foldl add 0.0
~~~
```

---

```
~~~pygmentize -x -l lexer.py:MorlocLexer
module base (map, mul, add, foldl)

-- load language-specific implementations
source cpp from "base.hpp" ("map", "mul", "add", "foldl")
source py from "base.py" ("map", "mul", "add", "reduce" as foldl)

map :: (a -> b) -> List a -> List b
map Cpp :: (a -> b) -> "std::vector<$1>" a -> "std::vector<$1>" b
map Py :: (a -> b) -> "list" a -> "list" b

mul :: Real -> Real -> Real
mul Cpp :: "double" -> "double" -> "double"
mul Py :: "float" -> "float" -> "float"
~~~
...
```

---


```
~~~pygmentize -x -l lexer.py:MorlocLexer
module base (map, mul, add, foldl)

source Cpp from "base.hpp" ("map", "mul", "add", "foldl")
source Py from "base.py" ("map", "mul", "add", "foldl")

type Cpp (List a) = "vector<$1>" a
type Py (List a) = "list" a

type Cpp Real = "double"
type Py Real = "float"

map :: (a -> b) -> List a -> List b
mul :: Real -> Real -> Real
add :: Real -> Real -> Real
foldl :: (b -> a -> b) -> b -> List a -> b
~~~
```

---
#
base.hpp
```
~~~pygmentize -l C++
#include <vector>
#include <functional>

template <class A, class B>
B foldl(std::function<B(B,A)> f, B y, std::vector<A> xs){
    for(std::size_t i=0; i < xs.size(); i++){
        y = f(y, xs[i]);
    }
    return y;
}

// insert add, mul, map
~~~
```

---
#
base.py
```
~~~pygmentize -l python
from functools import reduce

foldl = lambda f b xs: reduce(f, xs, b)

def add(x, y):
  return (x + y)

def mul(x, y):
  return (x * y)
~~~
```

---
#
```
$ morloc make -o nexus foo.loc
```

---
#
```
$ morloc make -o nexus foo.loc
$ ./nexus -h
The following commands are exported:
    sumOfSquares
        param 1: List Real
        return: Real
```

---
#
```
$ morloc make -o nexus foo.loc
$ ./nexus -h
The following commands are exported:
    sumOfSquares
        param 1: List Real
        return: Real
$ ./nexus sumOfSquares '[1,2,3]'
14
```

---
# Morloc Summary

 * allows functions to be composed across languages
 * supports the construction of polyglot libraries
 * organizes libraries and composition under a common type system

---
**Why type signatures are nice**

---
**Why type signatures are nice**

A signature describes a family of functions

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

 Type signatures are:
 * machine-verified documentation
 * a means to organize and query function space
 * a conceptual framework for exploring algorithms

---
## Programming in the post AGI world

```
~~~graph-easy --as=boxart
[ AI ] ----> [ AI ] ----> [ Human ] ----> [ Human ]
[ Human ] ----> [ AI ]
~~~
```

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

---
P2: Writing efficient functions may be expensive

----------

C2: **Caching is needed**

C3: **A searchable library (database) of functions is needed**

---
P3: Functions may need to be shared

(by P0): Function metadata may not be trustworthy

----------

C4: **AI must be able to prove that functions they import are correct**

----------

C5: **AI will need to share code**

---
P3: Functions may need to be shared

(by P0): Function metadata may not be trustworthy

----------

C4: **AI must be able to prove that functions they import are correct**

----------

C5: **AI will need to share code (not just APIs)**

#

*But what language will they use?*


---

P4: We will want AI to generate code for us

P5: We will be very free in how we describe programs

---

P4: We will want AI to generate code for us

P5: We will be very free in how we describe programs

(by P0): We should not trust AI generated code

---

P4: We will want AI to generate code for us

P5: We will be very free in how we describe programs

(by P0): We should not trust AI generated code

---------

C6: **So we will need to verify their code**

---

P4: We will want AI to generate code for us

P5: We will be very free in how we describe programs

(by P0): We should not trust AI generated code

---------

C6: **So we will need to verify their code**

P8: We are lazy

---

P4: We will want AI to generate code for us

P5: We will be very free in how we describe programs

(by P0): We should not trust AI generated code

---------

C6: **So we will need to verify their code**

P8: We are lazy

---------

C7: **We need help**




---

P9: For some languages, static analysis can confirm type correctness and effects

P10: Functions may be safely tested if they provably have no harmful side-effects

---

P9: For some languages, static analysis can confirm type correctness and effects

P10: Functions may be safely tested if they provably have no harmful side-effects

P11: Static analysis cannot always prove logical correctness

P12: Tests cannot always prove logical correctness

---

P9: For some languages, static analysis can confirm type correctness and effects

P10: Functions may be safely tested if they provably have no harmful side-effects

P11: Static analysis cannot always prove logical correctness

P12: Tests cannot always prove logical correctness

---------

C8: **Verifying AI generated functions will take work**

---

P9: For some languages, static analysis can confirm type correctness and effects

P10: Functions may be safely tested if they provably have no harmful side-effects

P11: Static analysis cannot always prove logical correctness

P12: Tests cannot always prove logical correctness

---------

C8: **Verifying AI generated functions will take work**

---------

C9: **We will want to cache verified functions in curated databases**


---
*What kind code should the AI generate?*

---

*What kind code should the AI generate?*

P13: Narrow languages may be easier to evaluate than general languages 

P14: Different people prefer different languages

---

*What kind code should the AI generate?*

P13: Narrow languages may be easier to evaluate than general languages 

P14: Different people prefer different languages

---------

C9: **AI will generate code in many languages**

C10: **The verified function database will be polyglot** 

C11: **Polyglot programs will need to be compiled**

---

*What kind code should the AI generate?*

P13: Narrow languages may be easier to evaluate than general languages 

P14: Different people prefer different languages

---------

C9: **AI will generate code in many languages**

C10: **The verified function database will be polyglot** 

C11: **Polyglot programs will need to be compiled**

---------

C12: **There may be a need for morloc!!!**
---
## Conclusions
 * Bioinformatics needs a firmer computational foundation
#
 * `morloc` composes polyglot functions under a common type system
#
 * AI will share functions with each other
#
 * Humans will describe programs to AI and then verify the generated code
#
 * AGI may make verifiable workflows by generating `morloc` scripts using trusted libraries

---
**Questions?**

#
#
#
#
#
#

Contact:
 * Name: Zebulun Arendsee
 * Morloc Github: `https://github.com/morloc-project/morloc`
 * Email: `zbwrnz@gmail.com`
