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
he asked the mathematician, whether there was an easier way to master geometry.
According to historical accounts, Euclid famously responded: \`\`Sire, there
is no royal road to mathematics.''

Two centuries later, David Hilbert has successfully formalized the Euclidean
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

- Mathematicians dream of a digital library of all peer-reviewed math
  literature. [@sojka2010towardsb]

> \`\`In light of mathematicians' reliance on their
> discipline's rich published heritage and the key role
> of mathematics in enabling other scientific disciplines,
> the Digital Mathematics Library strives to make the
> entirety of past mathematics scholarship available
> online [...]'' -- DML project vision, Cornell Library, 2002

- Math literature is scarcely digitized and doubles in volume every 16 years.
  [@trott2016overview] [@bouche2008some]

> \`\`The amount of resources available to digitize mathematics is
> significantly less than the resources available to digitize cats.''
> @watt2016semantic

- Math information retrieval systems retrieve math literature from digital libraries.
- OCR techniques and math representations ensure fast and accurate retrieval.
- 95\% of queries are stated in simple text, [@libbrecht2006methods]
  [@miller2013three] which exposes interpretability gaps.

> \`\`We find math search to be used modestly, but this is not surprising given
> that users don’t expect it and we have ruled out being confrontational to
> promote it.'' @miller2013three

## Research Questions {#research-questions}

Therefore, in my thesis, I focus on representations and techniques that can
make the results of math-aware search engines more *interpretable* to the users.
Namely, I focus on *token embeddings*, which explain the semantic
relationships between text words and math symbols and which can be used with
*semantic matching techniques* to produce interpretable search results.
I also focus on fusion-based *learning-to-rank* techniques, which can combine
the results of several math-aware search engines to produce more accurate
results without compromising on their interpretability.

In my thesis, I aim to answer the following three research questions:

1. Semantic matching techniques that use token embeddings do not only help explain
   the search results, but they also enable multimodal search, where a user can
   search for a math formula using a simple text query, or vise versa. Therefore,
   it seem natural to ask, whether the semantic matching techniques not only improve
   the interpretability of results, but also their accuracy.

2. User studies [@taylor2013situation] have shown that the speed of search
   engines is crucial to the user experience. Since semantic matching
   techniques add complexity to a search engine, it would be
   good to know whether they can still be reasonably fast.

3. Learning to rank techniques have been shown
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

In my thesis, I first describe the background of math information retrieval and information
retrieval in general.
Next, I describe past competitions of math-aware search engines and the search
engines that achieved state-of-the-art accuracy in the competitions.

Then, in the following two chapters, I describe the methods and the results of
my research on fast and accurate math information retrieval.
Here, I also answer all of my research questions.

In the final chapter before the conclusion, I draw from the work of my
undergraduate and graduate students to showcase some tangible
benefits of interpretability.

I conclude by summarizing my results and outlining venues for future work.

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
ArXiv, the European Digital Mathematical Library, or the ACM Digital
Library. ↷

More recently, it has also been *community question answering forums*, such as
the Mathematics StackExchange, Math Overflow, or the Data Science
StackExchange, and *online encyclopedias*, such as the Wikipedia, Wolfram
MathWorld, or the ProofWiki.

* * *

 /figures/digital-mathematical-libraries-01.png
 /figures/digital-mathematical-libraries-02.png

## Math Representations {#math-representations}

To use the math information in a search engine, we need to properly
represent it.

Written natural languages have rules of orthography and typography, which
govern how the language is presented to the reader, syntax, which governs
how words can be combined into phrases and sentences, and semantics, which
give the words, phrases, and sentences their meaning.

Similarly, we can represent math formulae using their *presentation* aspects,
*structural* aspects, and *semantic* aspects. In the picture, you can see two
different representations of the math formula $x!! - y^2 = 0$:

In the left picture, we represent the presentation aspects of the formula
using the *Presentation MathML* XML language. This representation is the
closest to the written form of a formula and the easiest to obtain.

In the right picture, we represent the structural aspects of the formula
using the *Content MathML* XML language. Notice how the two exclamation marks
in Presentation MathML become the double factorial operator in Content
MathML. In general, superficially different math formulae such as $a / b$
and $\frac{a}{b}$ will be equivalent in Content MathML. This is
useful in search engines, where we may want to ignore superficial differences
between math formulae. Importantly, a structural representation of a formula
can be automatically obtained from the written form.

However, two math formulae that seem equivalent may actually be quite
different depending on variable bindings, definitions of the operands,
and the underlying mathematical theory. Using an XML language such as
*OpenMath*, we can represent the semantic aspects of a formula by binding
each symbol in a formula to its definition in an ontology. This can be
useful in *computer algebra systems* and *proof assistants*, where we need
to know the exact meaning of a math formula. However, a semantic
representation of a formula cannot be automatically obtained from the written
form and requires costly human annotations. Therefore, semantic math
representations have seen limited adoption in math-aware search engines
and are outside the scope of my thesis.

* * *

 /figures/pmml-and-cmml.tex

## Math Information Retrieval {#math-information-retrieval}

Now that we have properly represented math information, we need to retrieve it.
In the picture, you can see the general architecture of a math-aware search
engine, where we first index the representations and then allow the user to
search for answers using queries.

This does not seem too different from information retrieval in general. However,
although math information retrieval also focuses on the retrieval of documents,
other *retrieval units* of interest include math formulae and math statements
such as definitions, theorems, and proofs. Furthermore, the language of
mathematics is inherently more structured than natural text, which brings
unique opportunities and challenges. Finally, users may search for answers not
only with *text queries*, but also with *math* and *mixed queries*, as you can
see in the picture. ↷

There are two main approaches to math information retrieval and information
retrieval in general: *sparse retrieval* and *dense retrieval*.

In sparse retrieval we index documents as high-dimensional sparse vector
coordinates, where every nonzero coordinate represents a single word or
mathematical symbol from the document, as you can see in the left picture.
Sparse retrieval is fast and interpretable, but it has several weaknesses that
hurt its accuracy:

1. It disregards *word order*. Therefore, it does not represent the precise
   meaning of the text.

2. It does not model *word relatedness*. Therefore, two words or math symbols are
   either the same, or they are different. This means that a slight difference
   in terminology or math notation can make a big difference. ↷

By contrast, in dense retrieval, we index documents as low-dimensional dense
vector coordinates (so-called *sentence embeddings*) that have been produced by
deep neural network language models such as *BERT*, which you can see in the
picture. Producing the vectors is computationally demanding and they are not
immediately interpretable. Additionally, BERT can
only produce representations of short documents that contain at most hundreds
of words. Despite these limitations, dense retrieval models achieve
state-of-the-art performance on information retrieval.

In addition to sparse and dense retrieval, math-aware search engines may take
advantage of additional techniques such a *semantic matching* and *learning to
rank*. ↷

In *semantic matching* techniques, we first compute the distance between
different *tokens* such as words and math symbols using language models, as you
can see in the middle picture. Then, we use these distances to improve the
accuracy of the retrieval. For example, in the right picture, you can see the
*soft vector space model*, also known as the *soft cosine measure*, which
improves sparse retrieval, so that it can model word relatedness and is more
robust against slight differences in terminology and math notation. ↷

By contrast, in fusion-based learning-to-rank techniques, we don't improve the
accuracy of individual search engine themselves. Instead, we combine the
results of several search engines to produce more accurate results. *Rank-based
fusion* techniques only use the ranks of a document across the results, whereas
*score aggregation* techniques also take into account the similarity scores
produced by the search engines.

* * *

 /figures/math-information-retrieval.tex
 /figures/soft-vsm-obscured.tex
 /figures/sentence-bert.tex
 /figures/soft-vsm.tex
 /figures/learning-to-rank.tex

## Objectives and Evaluation {#objectives-and-evaluation}

Representations and retrieval techniques allow the math-aware search engines to
fulfil three major objectives:

1. the *accuracy* of their results for the users,
2. the *speed* at which they can be deployed, index new documents, and deliver
   results to the users, and
3. the *interpretability*, which determines how well they can explain the
   results to the users.

To measure how well a search engine fulfills an objective, we can use a number
of *evaluation measures*.

For accuracy, we distinguish between *intrinsic evaluation measures*, which
focus on small, well-defined aspects of the search engines and which can be
easily reproduced in a laboratory setting, and *extrinsic
evaluation measures*, which focus on complex real-world applications and which
may require human participants.

An example of an intrinsic evaluation measure is the *word analogy accuracy*,
which measures how well token embeddings can be
used for analogical reasoning, as you can see in the left picture. While the
word analogy accuracy does not directly tell us how useful the token embeddings
will be for semantic matching and math information retrieval, it is fast and can be a
useful proxy for extrinsic evaluation measures such as *the F₁-score*, *mean
average precision*, or *nDCG'*, which measure the relevance of search results
using costly human annotations.

To evaluate the speed of an search engine, we can measure how much real time is
spent to *train* token embeddings and other machine learning components,
*deploy* the search engine at a server, *index* documents, and respond to a
*query* from a user.

To evaluate the interpretability of a search engine, we can first determine
the interpretability of its representations using exploratory visualization
techniques such as *dimensionality reduction* to display high-dimensional data,
*clustering* and *correlation analysis* to discover patterns in the data, and
*formal concept analysis* to discover hierarchical relationships in the data.
Then, we can use the representations to produce different types of
*explanations* for search results and perform *usability testing* to measure
user satisfaction.

* * *

- Key objectives in math information retrieval are *accuracy*, *speed*, and *interpretability*.
- To measure how well a system fulfill the objectives, we use *evaluation measures*.
- For accuracy, we distinguish between *intrinsic* and *extrinsic* evaluation measures:

* * *

 /figures/word-analogy.pdf

* * *

|          | complete | graded | ranked |
+----------+----------+--------+--------+
| P, R, F₁ | Yes      | No     | No     |
| MAP      | Yes      | No     | Yes    |
| bpref    | No       | No     | Yes    |
| nDCG'    | No       | Yes    | Yes    |

: Different extrinsic measures for math information retrieval have different strengths.

* * *

- For speed, we measure time to *train*, *deploy*, *index* documents, and *process* queries.
- For interpretability, we can perform *usability testing* and measure *user satisfaction*.

# State of the Art

## Competitions {#competitions}

Despite the lack of attention math information retrieval has historically
received compared to information retrieval more generally, a substantial amount
of research has been conducted since the first workshop *Towards a Digital
Mathematics Library* in 2008 [@sojka2008towards].

In 2012, the *Math Information Retrieval workshop* included the historically
first friendly competition between two research groups.

In 2013, 2014, and 2016, three math information retrieval shared task
evaluations were held at the *NTCIR* workshops. In the last workshop, six
research groups participated, including my research group, which has developed
the *MIaS* search engine that has since been deployed in *the European Digital
Mathematical Library*.

In 2020 and 2021, two math information retrieval shared task evaluations were
held at the *Answer Retrieval for Questions on Math (ARQMath)* labs hosted by
the CLEF conferences. In the last lab, as many as 11 research groups
participated, and twelve research groups have already registered for the the
third ARQMath lab, which will take place later this year.

* * *

 /figures/competitions.tex

## Systems {#systems}

Seven math-aware search engines achieved state-of-the-art results in the
competitions at some point in time, as you can see in the table. The current
state-of-the-art math-aware search engines are *Tangent-L* for full-text search
and *Approach0* for formula search.

*Tangent-L* represents math
formulae using presentation math. It uses two separate sparse retrieval search
engines, one for text and one for math formulae, and uses score aggregation to
produce the final results. Tangent-L received state-of-the-art results in the
ARQMath labs and also post-hoc in the last NTCIR workshop.

*Approach0*
represents math formulae using content math. It uses structural matching and sparse
retrieval followed rank fusion to produce the final results. Approach0
received state-of-the-art results in the last ARQMath lab and also post-hoc
in the last NTCIR workshop.

* * *

 /figures/systems.tex

# Accuracy and Speed {#accuracy-and-speed}

All three research questions of my work focus at the quantitative evaluation of
semantic matching techniques and learning-to-rank techniques in terms of their
accuracy and their speed. I will now describe my experiments in this area.

## Math Representations and Representation Learning {#representations}

First, I will focus on my experiments, where I develop new representations
that can be used in semantic matching techniques.

% Section 4.1: Topological Math Representations for Accurate Math Information Retrieval

In 2019, @lample2020deep tokenized content math representations using pre-order
traversal, producing a topologically ordered sequence of tokens (also known as
the *prefix notation* or the *normal Polish notation*). Using their representations,
they trained a deep neural network language model to solve integrals,
first-order differential equations, and second-order differential equations
with significantly better results than commercial computer algebra systems.

Following their example, we developed two math representations based on
content math representation:

1. The *prefix* notation, which was produced by pre-order traversal, and
2. The *infix* notation, which was produced by in-order traversal.

We used the representations in our math-aware search engines and we compared
them against other commonly used representations such as \LaTeX{} and paths in
presentation and content math representations at the ARQMath 2020 lab. Both
prefix and infix notations outperformed the other representations. ↷

The accuracy of semantic matching techniques depends on accurate token embeddings
that capture the relationship between different words and math symbols.

% Section 4.2.1: Variable Control in Token Embedding Evaluation

The accuracy of token embeddings is often measured using the word analogy
accuracy. In my work, I have shown that there are several hidden parameters in
token embedding models and in the word analogy accuracy formula. These
parameters are rarely reported in publications, but I have shown that they can
cause up to 24\% difference in word analogy accuracy, which makes it difficult
to compare and meaningfully reproduce published results. [@novotny2020art] ↷

% Section 4.2.2: Heuristic Hyperparameter Optimization in Subword Language Models

Token embedding models also contain a number of parameters that are reported in
publications, but which are usually not tuned for different languages because of
training costs. However, the language of mathematics is significantly different
from natural languages, which makes it important to tune these
parameters when producing token embeddings for mathematical symbols.

In my work, I have shown that parameter tuning can result in up to 14\%
improvement in word analogy accuracy. On top of that, I have developed a
heuristic that can suggest the parameter values based on simple statistical
analysis that can replace costly tuning and is within 2\% of the
optimal word analogy accuracy. [@novotny2021one] ↷

% Section 4.2.3: Position-Independent Token Embeddings
% Section 5.2.2: Position-Independent Token Embeddings

State-of-the-art token embedding models also produce embeddings for different
positions in a sentence, which helps them better grasp the meaning of the
training data and produce more accurate token embeddings. However, these
*positional embeddings* affect all coordinates of the token embeddings as if
the meaning of a token was fully determined by its position in a sentence,
whereas in reality, a part of the meaning of a token is fixed and independent
on its position in a sentence.

In my work, I have shown that *constraining* the effect of positional embeddings
can improve the word analogy accuracy by 6\%, makes it possible to use longer
sentences in the training data, and makes the embedding model twice as fast
to train. [@novotny2021when] ↷

% Section 4.2.4: Quantized Token Embeddings
% Section 5.2.1: Quantized Token Embeddings with Fast Bitwise Arithmetic

Semantic matching techniques based on token embeddings can decrease the speed
of information retrieval. A common approach to increase the speed of machine
learning models is to reduce the size of embeddings and to speed up vector
arithmetic by converting the vector coordinates from double-precision floating
point numbers, which contain 64 bits of information and use expensive floating
point arithmetic, to binary zeros and ones, which contain just one bit of
information and use fast bitwise arithmetic.

In 2019, I consulted the master's thesis of @stefanik2019semantic, where we
have shown that *binary* token embeddings were as accurate as token embeddings
that used floating point numbers. We have also shown that approximating vector
addition as bitwise logical disjunction and vector subtraction as bitwise
logical non-implication improved the speed more than 20× and was within 2\% of
optimal word analogy accuracy, which is a novel result that extends beyond
information retrieval. ↷

% Section 4.2.5: Word Sense Disambiguation with Sense Embeddings

In natural languages and in the language of mathematics, words and symbols can
have several possible meanings or senses. However, token embeddings only assign
a single representation to a word or a symbol. Therefore, the accuracy of token
embeddings can be improved by determining in which sense a word or a symbol is
used and by assigning different representations to different senses. However,
prior work in training sense embeddings relies on sense-annotated corpora,
which are prohibitively small for natural languages and virtually non-existent
for mathematical symbols.
  
In my work, I have helped develop an augmentation technique for enlarging
sense-annotated corpora. Token embeddings trained on the enlarged
sense-annotated corpora achieved state-of-the-art accuracy on five out of six
word sense disambiguation tasks. [@ayetiran2021eds] ↷

% Section 4.2.6: Decontextualized Token Embeddings

Semantic matching techniques use so-called *global token embeddings*, which
assign a single representation to a token or a sense. However, state-of-the-art
deep neural network language models use so-called *contextual token
embeddings*, which assign different representations to tokens depending on their
surroundings. Contextual token embeddings cannot be directly used in semantic
matching techniques.

In my work, I converted contextual token embeddings into *decontextualized*
global token embeddings by taking the average representation of a token across
different contexts. I have shown that decontextualized token embeddings
achieved better accuracy than global token embeddings with two different
semantic matching techniques on machine translation evaluation.
[@stefanik2021regressive] ↷

% Section 4.2.7: Joint Token and Sentence Embeddings of Text and Math

Both token embeddings and sentence embeddings can be trained on multimodal
input data that contain both text and math. This allows the embeddings to
capture the relationship between words and math symbols. However, prior work
has trained embeddings on text and math separately.

In my work, I have developed two search engines that used such *joint*
embeddings of text and math and I placed among the three best search engines in
the ARQMath 2020 lab.  In the ARQMath 2021 lab, @mansouri2021dprl [Section 3]
also used joint sentence embeddings in two of their systems and achieved the
second and third best accuracies in the lab.

* * *

- @lample2020deep tokenized content math formulae into *normal Polish notation*.
- They trained model to solve integrals and differential equations better than CAS.
- We developed two math representations: *(Example formula: $x!! - y^2 = 0$)*
    - *Prefix notation*: U!eq O!minus O!double-factorial V!x O!SUP V!y N!2 N!0.
    - *Infix notation*: ( ( O!double-factorial ( V!x ) O!minus O!SUP ( V!y , N!2 ) ) U!eq N!0 ).
- At ARQMath 2020, we showed that our representations outperformed other options:

> \`\`In the first step, we selected the optimal representation
> of math from removing math tokens (0.7600), \LaTeX{} (0.7602),
> OPT (0.7606), SLT (0.7607), and prefix/infix (0.7612).
> @novotny2020three [Section 4.3]

* * *

|                                                            | Accuracy | Training time |
|:-----------------------------------------------------------|:---------|:--------------|
| No positional weighting                                    | 65.52\%  | 2h 06m 33s    |
| No positional weighting (three epochs)                     | 70.94\%  | 4h 41m 17s    |
| Positional weighting, same as vanilla word vectors         |*50.96\%* | 5h 01m 16s    |
| Positional weighting, identity positional vectors$^\*$     |*75.02\%* | 4h 59m 27s    |
| Positional weighting, same as word vectors (uniform)       | 74.31\%  | 4h 57m 25s    |
| Positional weighting, same as word vectors (sq.-normal)    | 74.95\%  | 5h 01m 11s    |
: English word analogy task accuracies and training times of word vectors without positional weighting and with different initializations for positional weighting. -- @novotny2020art [Table 3]

* * *

 /figures/suggested-subword-sizes.tex

* * *

 /figures/word2bits-speed.tex

* * *

 /figures/word-sense-disambiguation.tex

* * *

|              |         | SCM-tf | SCM-dec-tf | WMD    | WMD-dec |
|--------------|---------|--------|------------|--------|---------|
| MQM, ref     | zh→en   | *0.35* |  0.26      | *0.29* |  0.27   |
| MQM, ref     | zh→en × | *0.35* |  0.25      | *0.29* |  0.27   |
| MQM, ref     | en→de   |  0.14  | *0.18*     |  0.13  | *0.32*  |
| MQM, ref     | en→de × |  0.14  | *0.17*     |  0.13  | *0.32*  |
| DA 2016, src | cs→en   | *0.67* |  0.53      |  0.63  | *0.71*  |
| DA 2016, src | de→en   |  0.40  | *0.45*     |  0.36  | *0.51*  |
| DA 2016, src | fi→en   |  0.44  | *0.63*     |  0.45  | *0.58*  |
| DA 2016, src | ru→en   |  0.30  | *0.43*     |  0.33  | *0.48*  |
%
: Accuracies of the soft cosine measure and the word mover's distance %
  with and w/o decontextualized embeddings on machine %
  translation evaluation. %
  Language pairs marked with an × were evaluated cross-lingually. %
  -- @stefanik2021regressive [Section 3.4]

* * *

 /figures/arqmath-2021-accuracies.tex

## Learning to Rank {#learning-to-rank}

Next, I will focus on my experiments, where I develop new learning-to-rank
techniques.

% Section 4.3: Score Aggregation of Structural and Semantic Matching Techniques

Semantic matching techniques make naive assumptions about the syntax and the
semantics of the natural language and the language of mathematics. They also
rely on token embeddings of language models, which contain human-like
biases. [@caliskan2017semantics] These assumptions and biases cause systematic
errors and can decrease accuracy.

In my work, I have developed a score aggregation technique. I used my technique
to combine the results of 16 semantic matching techniques in
order to avoid the systematic errors of any individual technique. My score
aggregation technique achieved equal or better accuracy than any individual
technique on machine translation evaluation. [@stefanik2021regressive] ↷ ↷ ↷

% Section 4.4: Rank-Based Fusion of Math Information Retrieval Systems

The systematic errors of semantic matching techniques also affect the ranking
of results in math-aware search engines. Therefore, although different search
engines can agree on a small portion of the most relevant documents, any
individual system will miss most relevant documents.

In my work, I have developed three novel rank fusion techniques and adapted
the rank fusion technique of @cormack2009reciprocal. I used my techniques
to combine the results of all primary search engines that participated in the ARQMath labs.
My techniques achieved better accuracy than any individual system in both
ARQMath labs.

* * *

|                   |         | RegEMT | Best individual technique  |
|-------------------|---------|:------:|----------------------------|
| MQM, src          | zh→en   | *0.59* |  0.44  (BERTScr, WMD-cont) |
| MQM, src          | zh→en × | *0.49* |  0.44  (BERTScr, WMD-cont) |
| MQM, src          | en→de   | *0.36* |  0.28  (Reg-base)          |
| MQM, src          | en→de × | *0.31* |  0.28  (Reg-base)          |
| MQM, ref          | zh→en   | *0.62* |  0.51  (Comet)             |
| MQM, ref          | zh→en × | *0.62* |  0.51  (Comet)             |
|                   |         |  ...   |                            |
%                                              
: Accuracies of our ensemble and the best individual techniques %
  on twenty machine translation evaluation tasks. %
  Language pairs marked with an × were evaluated cross-lingually. %
  (continued on next slide) -- @stefanik2021regressive [Section 3.4]

|                   |         | RegEMT | Best individual technique  |
|-------------------|---------|:------:|----------------------------|
|                   |         |  ...   |                            |
| MQM, ref          | en→de   | *0.60* |  0.48  (Comet)             |
| MQM, ref          | en→de × |  0.38  | *0.48  (Comet)*            |
| DA 2016, src      | cs→en   |  0.86  | *0.89  (Comet)*            |
| DA 2016, src      | de→en   | *0.84* |  0.80  (BLEUrt)            |
| DA 2016, src      | fi→en   | *0.87* |  0.85  (Comet)             |
| DA 2016, src      | ru→en   | *0.79* |  0.78  (Comet)             |
| DA 2016, tgt      | cs→en   | *0.77* | *0.77  (Prism)*            |
|                   |         |  ...   |                            |
%
: (continued from the previous slide) -- @stefanik2021regressive [Section 3.4]

|                   |         | RegEMT | Best individual technique  |
|-------------------|---------|:------:|----------------------------|
|                   |         |  ...   |                            |
| DA 2016, tgt      | de→en   |  0.45  | *0.58  (Prism)*            |
| DA 2016, tgt      | fi→en   | *0.76* | *0.76  (Prism)*            |
| DA 2016, tgt      | ru→en   | *0.74* |  0.70  (Prism)             |
| Catastrophic, src | en→cs   | *0.45* |  0.35  (Prism)             |
| Catastrophic, src | en→de   |  0.34  | *0.43  (Prism)*            |
| Catastrophic, src | en→ja   | *0.20* |  0.18  (Reg-base)          |
| Catastrophic, src | en→zh   |  0.16  | *0.27  (SCM-dec)*          |
%
: (continued from the previous slide) -- @stefanik2021regressive [Section 3.4]

* * *

> \`\`The ensemble of all non-baseline primary submissions received *the highest
> accuracy in the competition (0.419)*, significantly better than the
> alpha05noReRank alternative submission of the MathDowsers team (0.345).''
> @novotny2020three [Section 7.3]

<!-- -->

> \`\`Ensembling all non-baseline primary submissions increased nDCG'
from 0.309 to 0.556: *the best result reported in the ARQMath 2021
competition*.'' @novotny2021ensembling [Section 4.1]

## Approximate Nearest-Neighbor Search {#approximate-search}

Lastly, I will focus on my experiments, where I make information retrieval in
general more accurate and faster.

% Section 4.5: Weighted Zone Scoring Across Different Domains

In information retrieval, documents are usually structured and contain *zones*
that are more important than the body text, such as the title, abstract, and
keywords. *Weighted zone scoring* techniques can be used to inform a search
engine about the importance of different zones and also to improve its
accuracy.

In my work, I have performed a statistical analysis of publicly available
community question answering datasets and shown that earlier comments in online
discussions were *significantly* more likely to be relevant than later comments. ↷

Then, I developed a search engine that assigned smaller weight to later comments
in a discussion and achieved state-of-the-art accuracy on an information
retrieval dataset, whereas the same search engine without the *weighted zone
scoring* achieved worse-than-baseline accuracy on the same dataset.
[@novotny2018weighting] ↷ ↷

Finally, I used weighted zone scoring in three out of nine math-aware search
engines of our research group in the ARQMath 2021 lab. All three search engines
achieved significantly better accuracy than the remaining six. ↷

% Section 4.6: Approximate Nearest-Neighbor Search in Vector Spaces
% Section 5.3: Approximate Nearest Neighbor Search in Vector Spaces

Recent results show that dense retrieval and semantic matching techniques can
achieve higher accuracy than sparse retrieval on semantic text similarity and
information retrieval tasks. [@charlet2017simbow; @lin2021batch] However,
digital mathematical libraries are already using math-aware search engines that
are based on industry-grade sparse retrieval systems such as Apache Lucene and
ElasticSearch, which makes it difficult to make a switch.

In my work, I have shown that dense embeddings can be *encoded* as sparse vector
coordinates with almost no loss of accuracy. I have also shown that our encoding
can be tuned to trade off retrieval accuracy for speed. [@rygl2017semantic;
@ruzicka2017flexible] ↷

Furthermore, I have proven that semantic matching techniques such as the soft
cosine measure can be regularized, so that they are as fast as the hard vector
space model, can be implemented into sparse retrieval systems
[@novotny2018implementation] and also achieve higher accuracy on text
classification [@novotny2020text]. ↷

% Section 5.1: Fast and Reproducible Deployment of Math Information Retrieval
%              Systems Using Docker and Continuous Integration

Surveys show that the research community surrounding math information retrieval
is still small and most results originate from a few research groups.
[@guidi2016survey] To improve the situation, it is crucial for further research
in the area that the research groups can share code that is reproducible and
easy to deploy. This will allow new research groups to take existing math-aware
search engines and use them in their own work.

In my work, I have used the example of the MIaS search engine to show how
*container virtualization with Docker* and *continuous integration* can be used
to create math-aware search engines that are not only more reproducible, but
also easier to develop, maintain, and faster to deploy. [@luptak2021webmias]

* * *

 /figures/godwins-law.tex

 /figures/godwins-law-semeval-2016.tex

 /figures/godwins-law-semeval-2017.tex

 /figures/arqmath-2021-accuracies.tex

* * *

 /figures/scaletext-speed.tex

 /figures/text-classification-accuracies.tex

* * *

 /figures/continuous-integration-dependency-tree.tex

# Interpretability {#interpretability}

My research questions focus at accuracy and speed of representations and
techniques that are *interpretable*.  To show the tangible benefits of
interpretability, I will now describe my qualitative evaluation in this area.

## Representation Learning of Words and Symbols {#interpretable-representations}

In 1982, @wille1982restructuring proposed the *formal concept analysis* as a
tool to study objects and their binary attributes. In 2015,
@priss2005linguistic noted that formal concept analysis can be used to encode
and analyze type-of token attributes such as *parts of speech*.

In 2019, I consulted the master's thesis of @stefanik2019semantic, where we
have shown that binary token embeddings are not just as fast and accurate
as token embeddings that used floating point numbers, but that they also
correlated with parts-of-speech and could be studied using formal analysis. ↷

I have already discussed positional embeddings and how they can help make
token embeddings faster to train and more accurate. However, whereas the
properties of token embeddings have been studied in the literature,
[@levy2014neural] the exact role of positional embeddings was unknown.

In my work, I have shown that the primary purpose of positional embeddings was
to attenuate the effect of distant tokens in the training data, which explained
why positional embeddings make it possible to use longer sentences in the
training data. I have also shown that there were three distinct clusters of
vector coordinates in the positional embeddings, which could determine the
grammatical properties of tokens such as parts of speech.

* * *

 /figures/word2bits-pos.tex
 
 /figures/positional-features.tex

## Strengths and Weaknesses of Retrieval Systems {#strengths-and-weaknesses}

In my work, I have shown that we can improve the accuracy of learning-to-rank
techniques at the ARQMath 2021 lab by first clustering the results of different
search engines and then only combine those that are highly diverse. To explain
the strengths and weaknesses of different search engines, I showed that we can
determine how capable the different search engines were at understanding text
and math and how capable they were at answering queries of different lengths.

* * *

 /figures/strengths-and-weaknesses.tex

## Interactive Visualizations of Retrieval Collections {#interactive-visualizations}

In 2021, I led the bachelor's thesis of @petr2021document, where we have
developed an *interactive demo* of the soft cosine measure technique on data
from the ARQMath 2021 lab. In the demo, users can explore a small set of
queries and their nearest results. To explain the results, we produce rich
snippets that show which pairs of words and symbols in the query and the
result contributed to its final score.

# Conclusion {#conclusion}

In my work, I aimed to answer three research questions, which I repeat in the
presentation slide. I will now summarize the results for each question.

As for the first research question, the answer is yes. I have shown that
semantic matching techniques based on token embeddings significantly improve
accuracy on math information retrieval compared to the hard vector space model,
especially in combination with *weighted zone scoring*. I have also shown how
the accuracy of token embeddings can be further improved using five additional
techniques on tasks that are related to math information retrieval.

As for the second question, the answer is also yes. I have proven that the
*soft cosine measure* technique can be as fast as the *hard vector space
model*. I have also shown how the speed of token embeddings can be improved in
terms of *vector arithmetic*, *training time*, and *query time*.

As for the third question, the answer is again yes. I have shown that four
different *rank-based fusion* learning-to-rank techniques can always improve
the accuracy compared to individual math-aware search engines. I have also
shown that my *score aggregation* learning-to-rank technique can consistently
improve the accuracy compared to individual systems on *machine learning
evaluation*.

* * *

# List of Author's Publications {#list-of-publications}

In my work, I co-authored and published two journal articles, one of them as
the main author. One of the journals is *Knowledge-based systems*, which is
among 25\% most influential journals in the area of artificial intelligence
according to SCIMAGO. The other is the *Journal of universal computer science*,
which is among the 50\% most influential journals in the area of empirical
computer science.

I also co-authored twelve proceeding articles, six of them as the main author
and two of them as the sole author. Furthermore, out of the twelve
articles, two were published at a *CORE A* conference. One publication that I
authored as the sole author in 2018 has already received 15 citations from
other researchers, seven of them in the last year and one in this year, which
shows the ongoing impact of my results.

* * *

# Common Remarks {#common-remarks}

First, if I may, I would like to briefly respond to remarks that have
appeared in both reports and that discuss the omission of technical details
in my thesis.

The Faculty of Informatics allows doctoral students to submit manuscript-style
doctoral theses, which only contain a brief introduction followed by the
published manuscripts and then a brief conclusion. This is the form of thesis
that I would have preferred, but I was informed that the Faculty would rather
that doctoral students submit traditional doctoral theses that tell a story and
provide a systematic account of their research. Therefore, after consulting
with my supervisor, I have decided that I would write a traditional thesis.

However, since my research consists of a wide variety of results across
different research areas, I felt it would be difficult to include all the
technical details from my manuscripts and also give a coherent account of how
my experiments contribute to the research questions of my thesis. Furthermore,
since math information retrieval has a small research community, I though it
would be appropriate to write my thesis in a way that is approachable even to
researchers outside math information retrieval.

Therefore, I have used the technique of Richard Feynman who says that a
researcher should simplify what they have learned, so that they can explain it
even to non-experts and draw useful connections, which would otherwise have
been obscured by the details. However, upon reflection, I do agree that this
high-level approach makes it difficult to determine whether research methods
have been properly applied. Therefore, an amended version of my thesis will
contain all 176 pages of my publications as an appendix.

* * *

> \[...] I found the surprisingly brief treatment of each research question
> to be problematic, lacking in the details that I needed to see in order to
> determine whether research methods had been properly applied and whether
> results had been properly analyzed and interpreted. I therefore found it
> necessary to refer to the published papers that I have mentioned as the basis
> for my assessment in the preceding section. -- prof.\ Douglas W.\ Oard

> Although the thesis is presented as a self-contained manuscript, it is actually
> a commentary to the individual published works \[...] Such a “commentary” form
> of doctoral thesis might be beneficial for the sake of brevity, however, it
> is often used in case the individual publications do not form an inherent
> body of research. \[...] -- prof.\ RNDr.\ Tomáš Skopal, Ph.D.

% If you can’t explain it to a six-year-old, you don't understand it yourself.
% -- Richard Feynman

# Response to the report of prof.\ Douglas W.\ Oard {#oard}

Next, I would like to thank professor Oard for the many editorial comments
in his report, most of which I have already incorporated into the amended
version of my thesis. I will now respond to the questions from the report.

## Soft Cosine Measure Questions {#soft-cosine-measure-questions}

The first question remarks that whereas the soft cosine measure uses a sum
of similarities between the token embeddings of a query term and all document
terms, the ColBERT technique of @khattab2020colbert uses the maximum of the
similarities. Additionally, whereas the soft cosine measure uses global
token embeddings that are independent on the context of a sentence, ColBERT
uses contextual token embeddings. The question then asks whether and how does
the first of these differences, that is using either the sum of similarities or
the maximum of similarities, benefit the two techniques.

First, let's look at the mathematical perspective. Using the maximum operator
is similar to the regularized soft cosine measure, where we only consider a
single most similar token. However, one important difference is that the
regularized soft cosine measure ensures symmetry, so if token $a$ is among the
most similar tokens for token $b$, then $b$ will also be among the most similar
tokens for $a$ even though this is not generally true in metric spaces. By
contrast, the maximum operator makes ColBERT asymmetric and in my work, I have
shown [@novotny2020text, Table 2] that asymmetry reduces accuracy on text
classification.

Next, let's look at some intuitive properties of the soft cosine measure and
ColBERT. The soft cosine measure uses soft matches to estimate how many times
a term from the query appears in the document if we disregard surface-level
differences. Therefore, the soft cosine measure can distinguish between a
document that only contains one instance of a query term and a document that contains
many instances of the term. By contrast, ColBERT would not be able to distinguish
these two documents and therefore only seems suitable for short passages, which
is also the task for which it was originally developed and where it was evaluated.

Therefore, it seems that it might be useful to extend ColBERT, so that it
considers not only the most similar term, but $k$ most similar terms, which
would bring it closer to what the soft cosine measure does and which would
make it suitable for longer documents. ↷

The second question asks what challenges would arise if we were to use standard
tools for fast approximate nearest neighbor search such as Facebook's Faiss or
Spotify's Annoy with the soft cosine measure.

The answer is that whereas Faiss and Annoy are low-dimensional dense vector
databases, the soft cosine measure is a sparse retrieval technique, where both
the query and the documents are represented as high-dimensional sparse vector
coordinates and every nonzero coordinate represents a single word or
symbol from the query or the document. Now, there is a gray zone between about
1\% and 50\% non-zeros in the vector coordinates where it is not clear whether
it is more efficient to handle them as sparse or dense, but here the non-zero
coordinates for most documents will be a fraction of a percent.  This would be
not only extremely time-inefficient, but also space-inefficient.  Imagine a
small corpus of 1 million documents. According to Heaps' law, the dictionary
will contain approximately 100,000 terms, which amounts to a matrix of 1
million times 100,000 floats, which amounts to about 1TiB of memory.

Therefore, we would need a sparse vector database. These are much more rare,
but in sparse retrieval, we typically use inverted indices such as Apache
Lucene and ElasticSearch as our sparse vector databases. These would be ideal
and in Section 4 of my CIKM 2018 paper, I have shown that the soft cosine
measure can be implemented into inverted indices as a query expansion technique
and that this will not lead to any increase in asymptotic time complexity if we
regularize the token embeddings.

An alternative approach would be to use dimensionality reduction techniques
such as the latent semantic analysis to covert the sparse high-dimensional
query and document vectors into dense low-dimensional vectors. However, prior
work [@atreya2011latent] indicates that this reduces information retrieval
accuracy. ↷

This should answer the question as stated, but if I may, I would like to
address a related concern from the first section of your report. You remark
that although I have proven in my CIKM 2018 paper that the soft cosine measure
can be as fast as sparse retrieval techniques, it was much slower than
other search engines that used sparse and dense retrieval techniques at the
ARQMath 2021 lab, which would seems to call into question the practical benefit
of my complexity analysis.

I should clarify that the implementation that I used for ARQMath 2021 was not
asymptotically optimal. In the soft cosine measure, we perform two dot
products, which should be performed as a single operation in order to achieve
constant time complexity. However, our Python implementation used the
high-level SciPy python library for convenience, which will compute the two dot
products as two separate operations. This makes the time complexity of our
implementation linear in the dictionary size, which was sufficient for research
purposes, but a proper asymptotically optimal implementation would be necessary
before the technique can be used in a real-world search engine. I discuss this
in more detail in the Future work subsection of Section 5.3.2 and also at the
Computer Science StackExchange question answering forum as an answer to a
question titled \`\`How can I optimize the systems in the paper Ensembling Ten
Math Information Retrieval Systems''.

In conclusion, it seems we would either need to use low-level code in C or C++
to implement the constant-time algorithm in Python, or we would need to
implement the soft cosine measure into an inverted index such as Apache
ElasticSearch, neither of which is trivial and will involve significant
programming effort.

* * *

> The Soft Cosine Measure (SCM) seems to me to have a structure similar to
> CoIBERT (@khattab2020colbert, SIGIR 2020), but with two differences: (1)
> CoIBERT uses the maximum over the document terms of the similarity between
> embeddings for each query term, whereas SCM uses the sum of those
> similarities, and (2) CoIBERT uses a transformer to learn contextual
> embeddings, whereas your implementation of SCM uses fastText to learn static.
> How consequential is the first of those differences: are there theoretical or
> practical advantages or disadvantages to CoIBERT’s max-sim operator compared
> with the inner product in SCM?

* * *

> What challenges would arise if you were to use standard tools for rapid
> approximate nearest neighbor computation (e.g., Faiss) with SCM rather than
> your regularization technique? Would there be a parameter similar to your
> $C$ that would need to be set to bound compute time?

* * *

> The CIKM2018 paper includes a theoretical analysis that indicates that the
> asymptotic time complexity of search using regularized (and thus sparse)
> vectors is linear in the product of the number of query terms and the number
> of nonzero elements per regularized document vector, both of which can be
> bounded.
>
> However, timing results at ARQMath 2021 indicate the author's regularization
> technique \[...] actually made retrieval much slower than faster, increasing
> the average query processing wall clock time from 5 seconds with
> 768-dimension dense vectors \[...] to 223 seconds with regularized (sparse
> vectors).
>
> \[... This] does call into question question the practical benefit of that
> asymptotic complexity analysis. \[...] Moreover, the failure to compare to
> standard implementations of Approximate Nearest Neighbor techniques for
> efficiently computing inner products of dense vectors \[...] is puzzling.

## Sentence-BERT Questions {#sentence-bert-questions}

The second set of questions concerns the Sentence-BERT dense retrieval model,
which I used in the ARQMath labs.

The first question asks whether there is any reason to believe that using a
proxy objective such as word analogy accuracy or machine translation to perform
model or parameter selection will improve performance on math information
retrieval.

Recent literature [@ghannay2016word; @chiu2016intrinsic; @rogers2018whats]
indicates that word analogy accuracy does not always correlate with extrinsic
natural language processing tasks such as part-of-speech tagging, named entity
recognition, and chunking. Therefore, while the word analogy accuracy can be
useful to detect undertrained token embeddings and also as an early stopping
criterion for training token embeddings, prior work suggests that it should be
combined with extrinsic evaluation measures for model and parameter selection.

As far as machine translation is concerned, I would like to emphasize that none
of my experiments directly involved machine translation. In sections 4.2.6 and
4.3, I discuss experiments in which we used token embeddings for machine
translation *evaluation*, which is a close cousin to semantic text similarity
or information retrieval and which involves deciding whether a translated
sentence is close to a reference translation. Baseline techniques for machine
translation evaluation include the BLEU score, the Levenshtein distance, or
METEOR, some of which are also used for question answering evaluation.

Machine translation evaluation poses specific challenges compared to
information retrieval. For example, there are usually several reference
translations in machine translation evaluation, whereas we usually only
have a single query in information retrieval. On the other hand, machine
translation evaluation usually only involves short sentences, whereas
in information retrieval, we have to grapple with documents that can be
hundreds of pages long. Nevertheless, the basic objective is to decide
the semantic text similarity between two texts, which makes it highly
likely that language representations that are useful for machine translation
evaluation will also be useful for math information retrieval and information
retrieval in general. ↷

The second question remarks that whereas Sentence-BERT averages the
contextualized embeddings for all tokens in a sentence to produce sentence
embeddings, the DPR system of @karpukhin2020dense uses the contextualized
embeddings of *the CLS token* as sentence embeddings. Additionally, whereas
Sentence-BERT produces the same sentence embeddings for queries and documents,
DPR uses different encoders for queries and documents. The question then asks
whether and how do these differences benefit Sentence-BERT.

In their work, @reimers2019sentencebert [Section 1] have experimented with
Sentence-BERT, using both pooling and CLS tokens on semantic text
classification tasks. They concluded that both achieved comparable performance.

The Siamese architecture of Sentence-BERT does not distinguish between
questions and answers. Therefore, in our experiments, we have prefixed the
input sentences either with *Question:* or *Answer:* to indicate whether the
sentences are queries or documents. I am not aware of prior work, which would
compare our approach with the approach of @karpukhin2020dense. However, the
advantage of using a single representation for both queries and documents is
faster convergence, which allows us to train a comparably accurate model with
less training data. This is an important property in an area such as math
information retrieval, where human annotations are scarce. ↷

The third question remarks that whereas in my experiments, I have trained
Sentence-BERT using simple random sampling of negatives, @xiong2020approximate
show that speed of convergence can be improved by stratified sampling of
negatives that are difficult to distinguish from positive samples. The question
then asks, whether I have experimented with any alternatives to simple random
sampling of negatives.

In our experiments at the ARQMath 2020 lab, we have experimented with a number
of adversarial strategies to negative sampling.  For example, we have picked
negative samples that had high lexical overlap with the accepted answer. We
also picked negative samples that answered the same question, but had the least
number of votes. However, we did not observe any improvements to accuracy on
our development dataset.

* * *

> Where system selection or tuning was based on tasks other than math retrieval
> (e.g., on word analogy or on machine translation), what basis is there to
> believe that the systems or parameters selected would be reasonable choices
> for a math retrieval application?

* * *

> Unlike Sentence-BERT, Dense Passage Retrieval (DPR, @karpukhin2020dense,
> EMNLP 2020) models queries and documents differently, and it uses a prefix
> (CLS) token as the representation rather than Sentence-BERT’s mean pooling.
> What benefits does Sentence-BERT have over DPR?

* * *

> Unlike your random selection of training examples, selecting negative
> examples that are highly confusable with positive examples has been shown to
> be a better approach. One application of this idea to BERT is ANN Negative
> Contrastive Estimation (ANCE, @xiong2020approximate, ICLR 2021), which
> iteratively learns to select negative examples. Did you experiment with any
> alternative to random selection of negative examples?

## System Combination Questions {#system-combination-questions}

The next question concerns the learning-to-rank techniques that I have
developed for the ARQMath shared task evaluations. The question remarks that
all my techniques use rank aggregation and asks whether it would be useful to
extend them with score aggregation. The question then asks how exactly I would
extend the IBC technique, which I developed for ARQMath 2020, with score
aggregation.

If we were to use score aggregation, we would need to normalize the scores
produced by different search engines, since they have different probability
distributions, and in sparse retrieval, they are usually unbounded and depend
on the lengths of the documents in the collection. Therefore, different score
normalization would be required for community question answering collections
such as ARQMath, where the average document is a short *post* that contains hundreds
of tokens *at most* and math understanding collections such as NTCIR-10, where the
average document is a scientific article that contains thousands of tokens.
After we have normalized the scores, we can directly use them in my IBC
technique instead of inverse ranks.

Prior work shows that rank-based fusion can produce results that are
competitive with score aggregation [@renda2003web; @cormack2009reciprocal]
on TREC information retrieval collections and there are no experimental
results comparing rank-based fusion with score aggregation on math information
retrieval. Therefore, it is difficult to know whether using scores instead of
ranks would improve the results, but I believe it would largely depend on the
score normalization that we would use.

* * *

> In your work you have focused exclusively on rank-based system combination.
> I believe it is the case that every system you have combined operates
> pointwise, assigning a score to every document and then sorting on those
> scores. One might expect those scores to have more information than is
> available from the ranks alone (for example, smaller score gaps might
> indicate a weaker commitment to the generated rank order). How might you
> adapt IBC to benefit from access to those document scores? Would you expect
> improved results?

## Interpretable Representation Questions {#interpretable-representation-questions}

The next set of questions concerns the interpretable representations, which
I study in Section 6 of my thesis.

The first question notes that I have used the definition of *interpretability*
from the 2018 book by @hall2018introduction and asks what an *interpretable
representation* means within this framework.

In their book, @hall2018introduction define interpretable as ,,the ability to
explain or to present in understandable terms to a human\`\`. Under this
definition, interpretable representations enable qualitative analysis that can
be used to explain what they represent in understandable terms to a human.

One example of an interpretable representation would be the soft vector space
model, which allows us to explain which exact keywords but also which pairs of
related words and symbols contributed to the final score of a result. In our
demo, we have used this information to produce rich snippets, but
other kinds of explanations could be produced. This shows that interpretable
representations can be used to produce interpretable search results.

To quantify how interpretable the results are and how this contributes to user
satisfaction, we would need to perform user studies. This was outside the
scope of my thesis, where I focused on the qualitative, not quantitative,
evaluation of the interpretable representations. ↷

The second question asks whether and how are simple representations such as the
surface forms of words or math formulae less interpretable than the
vectors used in sparse retrieval or even token embeddings, and in which sense
are token embeddings interpretable.

The surface forms of words and math formulae seem interpretable, since any part
of them can be explained to a human. By contrast, we may not be able to recover
the surface form from token embeddings. However, as I show in Section 6.1, we
can use token embeddings to determine the grammatical and other properties of
the tokens, which may not be obvious from the surface forms.

In my thesis, I did not attempt to quantify the interpretability, but let
us assume that the surface forms are more interpretable than token
embeddings. Should I also have studied them? As I state in my research
questions, I am interested in interpretable representations that can improve
the speed and the accuracy of search engines. Therefore, I don't think the
surface forms would be too relevant to the research questions of my thesis.

* * *

> You cite Hall and Gill’s 2018 O’Reilly book as defining interpretability, but
> they offer no specific definition of an “interpretable representation,” the
> term you use in the title of the thesis. In general, what characteristics
> would make a representation “interpretable”?

* * *

> In this thesis, you focus on embeddings as representations for text and math
> content. | would think of surface forms (e.g., words or LaTeX) as being more
> interpretable than the sparse vectors in what you call the hard vector space
> model, and those vectors as more interpretable than the dense vectors that we
> call embeddings. In what sense do you mean to claim that the embedding
> representations studied in this thesis are “interpretable”?

# Response to the report of prof.\ RNDr.\ Tomáš Skopal, Ph.D. {#skopal}

Finally, I would like to respond to the questions from the report of professor
Skopal.

## Defense Questions {#defense-questions}

The first question mentions that in Section 2.3 about math information
retrieval, only Section 2.3.3.1 about structural matching discusses techniques
that are specific to math information retrieval and not information retrieval
in general. The question then asks whether this is a sign of a small math
information retrieval community.

First, I would like to point out that there is a substantial body of work on
presentation, content, and semantic math representations, which are referenced
in Section 2.2 about math representations. Furthermore, Section 2 focuses on
the most prominent techniques, which were either used in the state-of-the-art
search engines that I list in Chapter 3 or which I encountered in my
experiments that I list in chapters 4 through 6. That is to say that the list
is not exhaustive. For example, there is a large body of related work that
focuses on computer algebra systems and proof assistants. This is its own
branch of math information retrieval, which is outside the scope of my thesis, but
I point the readers to the proceedings of the *International Conference of
Intelligent Computer Mathematics (CICM)*, which is the prime venue for new
results in this area, in section 2.2.3 on semantic math.

However, if we focus just on the subarea of math information retrieval that
involves traditional user-facing math-aware search engines, it is the case that
most recent results in the area were produced by a handful of research groups.
In their survey of math information retrieval, @guidi2016survey state that the
progress in the field ,,is mostly due to the steady work of a very few groups
that keep improving their own software.\`\` However, I should note that the
ARQMath labs have reinvigorated interest in the field and whereas only six
research groups have participated in ARQMath 2020, eleven groups have
participated in ARQMath 2021 and twelve have already registered for ARQMath
2022. ↷

The second question expands on the first question by asking what the impact
of math-specific techniques is and whether the reader of the thesis can
find a recommendation what techniques they should use in which retrieval
scenario.

In Chapter 3, I list the current state-of-the-art math-aware search engines,
which at the moment are Tangent-L for full-text search and Approach0 for
formula search.

Tangent-L uses the *symbol layout tree* presentation math representation and
*math normalization* techniques, which I describe in Section 2.2, and sparse
retrieval with the hard vector space model and BM25⁺ weighting, which I
describe in Section 2.3. Here, the math-specific ingredients include the math
representations and math normalization. The remaining techniques used by
Tengent-L are not math-specific.

Approach0 uses the *operator tree* content math representation, which I describe
in Section 2.2, and structural matching, query expansion, and sparse retrieval
with the hard vector space model and BM25 weighting, which I describe in
Section 2.3. Then, it combines the results using rank fusion, which I also
describe in Section 2.3. Here, the math-specific ingredients include the math
representations and structural matching. The remaining techniques used by
Approach0 are not math-specific.

Many of the techniques that I have developed have only been evaluated on tasks
that are related to math information retrieval, such as semantic text similarity,
machine translation evaluation, and causal language modeling. However, for
example, the soft cosine measure has received significantly better accuracy
(nDCG', two-sample t-test, 90\% confidence) than all the other math-aware search
engines that used the soft vector space model with the TF-IDF weighting on the
ARQMath 2021 shared task evaluation, which indicates that the technique can
be recommended for sparse math information retrieval. The two learning-to-rank
techniques that I have developed for ARQMath have also outperformed the
reciprocal rank fusion technique of @cormack2009reciprocal, which is a standard
fusion technique for information retrieval. Therefore, these techniques can
also be recommended.

* * *

> As mentioned in my comments to the thesis structure, there is only a few
> math-specific techniques mentioned in section 2.3 (actually just the
> structural matching in 2.3.3.1). Everything else in 2.3 is applicable to
> general IR. Some other MIR-related works are mentioned in subsections 4.x. Is
> this lack of math specific IR a sign of small research MIR community?

* * *

> Could you describe and quantify the impact of math-specific extensions to IR
> engines? In the thesis results for text-based and math-based retrieval are
> mixed. Could a reader of the thesis (or the referenced papers) find a
> recommendation of what MIR techniques to use in what retrieval scenario?
