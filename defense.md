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
difficult to find math results without specialized math-aware search engines.

Thankfully, such search engines do exist and they are actively deployed in
digital mathematical libraries. First, optical character recognition is used to
extract both text and math formulae from scanned documents. Then, specialized
representations are used to enable fast and accurate retrieval using advanced
math queries. However, user studies [@libbrecht2006methods] [@miller2013three]
have shown that as much as staggering 95\% of queries in digital mathematical
libraries are simple text queries that do not make proper use of the math-aware
search engines. This indicates that the users struggle to understand why the
advanced math queries produced the results that they did.

* * *

- Mathematicians dream of a digital library of all peer-reviewed math literature. [@sojka2010towardsb]
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

Therefore, in my research, I focused on representations and techniques that can
make the results of math-aware search engines more interpretable to the users.
Namely, I focused on *token embeddings*, which explain the semantic
relationships between text words and math symbols and which can be used with
semantic matching techniques to produce interpretable  search results.
I also focused on *learning to rank* techniques and voting algorithms, which
can combine the results of several math-aware search engines to produce more
accurate results without compromising their interpretability.

In my research, I aimed to answer the following three research questions:

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

- I focused on representations and techniques that make retrieval results interpretable:
    - *Token embeddings* explain semantic relationships between text and math symbols.
    - *learning to rank* combines systems to improve accuracy without loss of interpretability.
- I posed three research questions:
    1. Can token embeddings *improve the accuracy* of math information retrieval systems?
    2. Can token embeddings produce *reasonably fast* math information retrieval systems?
    3. Can learning-to-rank consistently improve accuracy compared to individual systems?

## Thesis Structure {#thesis-structure}

* * *

# Background {#background}

* * *

## Digital Mathematical Libraries {#digital-mathematical-libraries}

* * *

## Math Representations {#math-representations}

* * *

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
