---
author: Zebulun Arendsee
date: YYYY-MM-dd
paging: Slide %d / %d
---

Q&A

---

Q. Why functional?

A. What else is there? OOP does not easily generalize across languages since
it couples data and functions. The data stored in an object itself can be
shared between languages in a functional fashion at the interfaces. Why not
imperative? Functional languages can emulate imperative languages as shown in
Haskell.

---

Q. Already AIs have function dispatches, how is this different from Morloc?

A. A couple reasons. These function dispatches are usually calls through APIs
to monolithic programs on other servers. They require serialization steps
(slow), network connections (unreliable), and may require the movement of data
off-premise (unsafe). APIs can form a kind of typesystem, but they are not
very expressive. Also, API calls are true black boxes. You cannot statically
analyze the source code in most cases because it will not be available. In the
morloc ecosystem, each node in a block of code that you can actually
analyze. There are some caveates here, the code may have dependencies that
cannot easily be analyzed or the code may itself make API calls to black
boxes..

---

Q. If morloc allows any language to be imported as a function, and these
functions are black boxes, how does this confer any safety?

A.

---

Q. Why must an AI use functions, they could alternatively use small
models.

A. A specialized model such as this really is just a function. The distinction
between an AI and a function is perhaps a bit artificial. Though this notion
can be wrapped into a language. Really, it is just a function that uses a
large constant (the architecture, matrices and such). This isn't so different
from a thesaurus function that looks up synonyms in a massive table.

---

Q. Yes many different styles of language are needed, but must they be
different languages, why not specialized subsets of one language? 
A. Doesn't really matter. Is being a specialized subset, with entirely
unique grammar and syntax, not still a novel language just because it shares a
common compiler?

A. "The lingua-franca of bioinformatics is UNIX" (quote from Nextflow) - this
sums up perfectly everything that is wrong with bioinformatics.

---

Q. How can you guarantee that functions are substitutable?

A. 

---

Q. What about hidden state? What if an object requires hidden state, such as
temporary data structures or mutable counters and such?

A. This is fine. An object can store hidden state. It simply cannot be passed
across language bounds. If it must be hidden, then you can always implement
the entire algorithm in the one language and expose it as a single function to
morloc. This would be the coarse grain solution. Alternatively, you could
expose the hidden state.

---

Q. What about UIs, games, agent models and such?

A. There are very roughly two kinds of computation. One kind is a
question/answer kind of computation where every program has an input and
terminates with an output. The other kind of computation is theoretically
non-terminating, such as a game or a UI. I am focusing here entirely on the
first kind of program.


---

Q. Typechecking with AI

A. 

    ``` haskell
    data Character = Character
      { name :: String -- 
      , class :: String -- 
      , race :: String
      , weapon :: Weapon
      }

    data Weapon = Weapon
      { name :: String -- A D&D style name for a weapon, should be consistent with other fields
      , dmg :: (Int, Int) -- Pair of the number of dice to roll for damage and the
                          -- die denomination.
      , element :: Maybe String -- optional damage element, accept normal and exotic elements
      }
    ```


---

Q. What about Curry-Howard correspondence?

A.

    Curry-Howard correspondence:
        a proof is a program
        the proven formula is the type

    Once a type signature is written for a function, often writing the function is
    just busy work.

    For some signatures map to a single non-pathological implementation:


    ```
    snd :: (a, b) -> b
    ```

    In richer type systems, there are more signatures that map to single
    implementations:


    ```
    sort :: xs:[a] -> ys:[a] where
        length xs == length ys
        ys[i-1] <= ys[i] for i in [2..length xs]
    ```

    But still many non-trivial functions have many nuances:


    Here, one signature describes a broad family of algorithms. An entire wing of
    the library.

    ```
    -- most general alignment algorithm family
    _ :: [[a]] -> Matrix (a + Gap)

    -- tree from sequences
    _ :: [[a]] -> Tree node edge Int

    -- tree from a alignment
    _ :: Matrix a -> Tree node edge Int

    -- tree from a distance matrix
    _ :: Matrix Real -> Tree node edge Int

    -- distance matrix from alignment
    _ :: Matrix a -> Matrix Real

    -- distance matrix directly from sequences
    _ :: [[a]] -> Matrix Real

    -- alignment that uses a guide tree
    _ :: [[a]] -> Tree () () Int -> Matrix (a + Gap)
    ```


```
~~~graph-easy --as=boxart
[ bio ] --> [ The Morloc Language ] --> [ Morloc as a query language ] --> [ The Infinite Library ]
[ safety ] --> [ deterministic compilers ] -> [ The Morloc Language ]
[ performance ] , [ safety ] , [ communication ] , [ abstraction ] - support -> [ AI need PL ] --> [ The Infinite Library ]
[ trust ] , [ clarity ] , [ abstraction ] --> [ Human need PL ] --> [ The Infinite Library ]
~~~
```

---

Q. Expound on your pathway
A. Here is more verbage from a letter I never sent

    I am currently a bioinformatics scientist at Regeneron where I maintain some of
    the company's central antibody and whole genome sequencing pipelines and write
    code that generates recipes for new oligonucleotide therapeutics. In the past, I
    worked as a post-doc at the USDA where, among other things, I collaborated with
    the CDC and WHO in assessing zoonotic risks of swine flu strains as an early
    step in the development of annual flu vaccines. As a graduate student, I worked
    on comparative genetics and contributed to many open source tools. So I have a
    fair understanding of programming practices in academia, government, and
    industry.

    And what I've seen is a real shit show. From WHO's vaccine pipeline to the
    billion dollar Regeneron antibody pipeline, the codebases are brittle scripts
    cobbling together idiosyncratic third-party applications and buggy in-house
    tools. Every time I've delved into an in-house codebase (including my own)
    I've found logical bugs that could lead to misinterpretation of the
    data. Bioinformatics programmers and those working with them are generally
    resigned to this fact. Sensible downstream biologists don't trust anything
    computers tell them until they've verified it experimentally.

    The root problem is the reliance on the interactive UNIX shell for doing
    bioinformatics analysis. In this universe, every function is a standalone
    program. These programs take many parameters that alter their functional types
    or behaviors. Each program is responsible for parsing input files (often in
    multiple possible formats) and writing output files. Given these extra
    responsibilities, these programs are hard to develop and hard to maintain. In
    practice, Most bioinformatics tools are unusable. Building larger pipelines from
    these applications requires heavy use of scripting languages to rename and
    organize files and resolve conflicts between the output of one application and
    the input of the next. I could, and probably will, write a paper on everything
    that is wrong here.

    The general problem with code reliability is well known in my community. But
    proposed solutions involve laying new frameworks or conventions over the
    existing mess. I believe the better solution is to ditch the entire
    file-oriented model and only write functions. Then build complexity through
    function composition. With this approach, we would strip all the monolithic
    bioinformatics applications down to their constituent functions and expose those
    as a library. The tricky part is that there are many languages involved. So we
    need a way to safely compose functions across languages, typecheck the resulting
    compositions, and compile a high-performance executable. My language is designed
    to do exactly this.

Q. Have you started a company?
A. Yes. I started company, I formed a team 3 times and wrote grants for SBIR
   funding. I don't really have the right disposition, though.

   Steve Blank says "talk customers, form hypotheses, make things people
   want". I say fuck them all, I know what the world should be and I know what
   people should want, I will head in that direction and if people follow great,
   otherwise I'll die alone.

   Less poetically, I'm willing to spend 80 hour weeks working on Morloc, but
   never had much patience for the day-to-day entrepreneur work. I guess I just
   needed to find the right co-founder.
   
Q. What do you want?
A. 
