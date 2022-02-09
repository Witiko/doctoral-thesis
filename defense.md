Good afternoon,

my name is Vít Novotný and this is the defense of my doctoral thesis.
First, I will summarize the key points of my thesis.
Then, I will react to the reader's reports and answer any follow-up questions from the audience.

* * *

- Previous presentations:
    - <https://www.overleaf.com/project/5ec0171153b27f0001599ced>
    - <https://www.overleaf.com/project/60e70b1d3797bcefd6389079>

# Introduction {#introduction}

When Ptolemy the 1st Soter, the Ancient Macedonian general of Alexander the
Great, found Euclid's seminal work, *the Elements*, too difficult to study,
he asked the matematician, whether there was an easier way to master geometry.
According to historical accounts, Euclid famously responded: \`\`Sire, there
is no royal road to mathematics.''

Two centuries later, David Hilbert has successfully axiomatized the Euclidean
geometry, but *the Elements* are arguably as current and as relevant as they
were when Euclid first wrote them.  This is because in mathematics, the new
does not invalidate the old and mathematicians rely on their discipline's rich
published heritage. However, since the advent of the world wide web, math
literature has more than doubled in volume, which makes it increasingly
difficult to find math results without specialized *math-aware search engines*.

Thankfully, such search engines do exist and they are actively deployed in
*digital mathematical libraries*. First, optical character recognition is used to
extract both text and math formulae from scanned documents. Then, specialized
representations are used to enable fast and accurate retrieval using advanced
math queries. However, user studies [@libbrecht2006methods] [@miller2013three]
have shown that as much as staggering 95\% of queries in digital mathematical
libraries are simple text queries that do not make proper use of the math-aware
search engines. This indicates that the users struggle to understand why the
advanced math queries produced the results that they did.

* * *

- Mathematicians dream of a digital library of all peer-reviewed math literature. [@sojka2010towardsb]

> \`\`In light of mathematicians' reliance on their
> discipline's rich published heritage and the key role
> of mathematics in enabling other scientific disciplines,
> the Digital Mathematics Library strives to make the
> entirety of past mathematics scholarship available
> online [...]'' -- DML project vision, Cornell Library, 2002

- Math literature is scarcely digitized and doubles in volume every 16 years. [@trott2016overview] [@bouche2008some]

> \`\`The amount of resources available to digitize mathematics is
> significantly less than the resources available to digitize cats.'' @watt2016semantic

- Math information retrieval systems retrieve math literature from digital libraries.
- OCR techniques and math representations ensure fast and accurate retrieval.
- 95\% of queries are stated in simple text, [@libbrecht2006methods] [@miller2013three] which exposes interpretability gaps.

> \`\`We find math search to be used modestly, but this is not surprising given
> that users don’t expect it and we have ruled out being confrontational to
> promote it.'' @miller2013three

## Research Questions {#research-questions}

Therefore, in my thesis, I focus on representations and techniques that can
make the results of math-aware search engines more interpretable to the users.
Namely, I focus on *token embeddings*, which explain the semantic
relationships between text words and math symbols and which can be used with
*semantic matching techniques* to produce interpretable  search results.
I also focus on *learning to rank* techniques and voting algorithms, which
can combine the results of several math-aware search engines to produce more
accurate results without compromising their interpretability.

In my thesis, I aim to answer the following three research questions:

1. Semantic matching techniques that use token embeddings do not only help explain
   the search results, but they also enable multimodal search, where a user can
   search for a math formula using a simple text query, or vise versa. Therefore,
   it seem natural to ask, whether the semantic matching techniques not only improve
   the interpretability of results, but also their accuracy.

2. User studies [@taylor2013situation] have shown that the speed of search
   engines is crucial to the user experience. Since semantic matching
   techniques add complexity to a search engine, I figured that it would be
   good to know whether they can still be reasonably fast.

3. Learning to rank techniques and voting algorithms have been shown
   [@cormack2009reciprocal] to consistently achieve better accuracy than
   individual text search engines. However, it was unclear whether this was
   also true of math-aware search engines.

* * *

- I focus on representations and techniques that make retrieval results interpretable:
    - *Token embeddings* explain semantic relationships between text and math symbols.
    - *Learning to rank* combines systems to improve accuracy without loss of interpretability.
- I pose three research questions:
    1. Can token embeddings *improve the accuracy* of math information retrieval systems?
    2. Can token embeddings produce *reasonably fast* math information retrieval systems?
    3. Can learning-to-rank consistently *improve accuracy* compared to individual systems?

## Thesis Structure {#thesis-structure}

In my thesis, I first describe math information retrieval and information
retrieval in general, including existing digital mathematical libraries,
math representations, retrieval models, different kinds of token embeddings,
semantic matching techniques, learning-to-rank techniques, and voting
algorithms. Next, I describe past competitions of math-aware search engines and
the search engines that achieved state-of-the-art accuracy in the competitions.

Then, in the following two chapters, I describe the methods and the results of
my research on representations that enable fast and accurate math-aware search.
Here, I also answer all of my research questions.
In the final chapter before the conclusion, I draw from the work of my
undergraduate and graduate students to I showcase some tangible qualitative
benefits of interpretability.

I conclude by summarizing my results and outlining venues for future
work.

* * *

- I describe the *background* of information retrieval and math information retrieval.
- I describe the math information retrieval *competitions* and *state-of-the-art* systems.
- In following two chapters, I answer my research questions about *accuracy* and *speed*.
- In the final chapter before the conclusion, I show tangible benefits of *interpretability*.
- In the *conclusion*, I summarize my results and outline venues for *future work*.

# Background {#background}

* * *

## Digital Mathematical Libraries {#digital-mathematical-libraries}

Math information can take many forms. Traditionally, it has been journal
articles that are stored in conventional *digital libraries* such as
Ar$\chi$iv, the European Digital Mathematical Library, or the ACM Digital
Library.

More recently, it has also been *community question answering forums*, such as
the Mathematics StackExchange, Math Overflow, or the Data Science
StackExchange, and *online encyclopedias*, such as the Wikipedia, Wolfram
MathWorld, or the ProofWiki.

* * *

/figures/digital-mathematical-libraries-01.png
/figures/digital-mathematical-libraries-02.png

## Math Representations {#math-representations}

To use the math information in a search engine, we need to represent it
somehow.

Written natural languages have:

1. rules of orthography and typography, which govern how the language is
   presented to the reader,
2. syntax, which governs how words can be combined into phrases and sentences,
   and
3. semantics, which give the words, phrases, and sentences their meaning.

Similarly, we can represent math formulae using their *presentation* aspects,
*structural* aspects, and *semantic* aspects.

← In the picture, you can see two different representations of the math formula
$x!! - y^2 = 0$:

- In the left picture, we represent the presentation aspects of the formula
  using the *Presentation MathML* XML language. This representation is the
  closest to the written form of a formula and the easiest to obtain.

- In the right picture, we represent the structural aspects of the formula
  using the *Content MathML* XML language. Notice how the two exclamation marks
  in Presentation MathML become the double factorial operator in Content
  MathML. In general, superficially different math formulae such as $a / b$
  and $\frac{a}{b}$ will be equivalent in Content MathML. This is especially
  useful in search engines, where we may want to ignore superficial differences
  between math formulae. Importantly, a structural representation of a formula
  can be automatically obtained from the written form.

- However, two math formulae that seem equivalent may actually be quite
  different depending on variable bindings, definitions of the operands,
  and the underlying mathematical theory. Using an XML language such as
  *OpenMath*, we can represent the semantic aspects of a formula by binding
  each symbol in a formula to its definition in an ontology. This can be
  useful in *computer algebra systems* and *proof assistants*, where we need
  to know the exact meaning of a math formula.

  However, although semantic representations allow us to prove whether two math
  formulae are equivalent or not, which could be useful for math-aware search,
  this equivalence is generally undecidable according to Gödel's first
  incompleteness theorem. What's more, even if it were decidable, this decision
  would not be practically fast unless $P = NP$, in which case we could prove
  the equivalence as fast as we could verify it. Furthermore, a semantic
  representation of a formula cannot be automatically obtained from the written
  form and requires costly human annotations. Therefore, semantic math
  representations have seen limited use in math-aware search engines.

* * *

/figures/pmml-and-cmml.tex

## Math Information Retrieval {#math-information-retrieval}

* * *

## Objectives and Evaluation {#objectives-and-evaluation}

* * *

# State of the Art {#state-of-the-art}

* * *

## Competitions {#competitions}

* * *

## Systems {#systems}

foo

* * *

bar

* * *

baz
