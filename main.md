# Introduction {#introduction}
## Problem Statement
## Research Questions
## Thesis Contributions
## Thesis Structure
# Background {#background}
## Digital Mathematical Libraries
### ArXiv
### Mini-DML
### The European Digital Mathematical Library
### Global Digital Mathematical Library
## Math Representations
## Math Information Retrieval
### Sparse Retrieval
### Dense Retrieval
### Structural and Semantic Matching
### Learning to rank
## Objectives and Evaluation
### Accuracy
### Interpretability
### Speed
# State of the Art {#state-of-the-art}
## Competitions
### Math Information Retrieval Workshop
### NTCIR-10 Math
### NTCIR-11 Math
### NTCIR-12 MathIR
### ARQMath-1
### ARQMath-2
### ARQMath-3
## Search Engines
### MathWebSearch
### Math Indexer and Searcher
### Mathcat
### Tangent
#### Tangent, Tangent-2, and Tangent-3
#### Tangent-S
#### Tangent-L
#### Approach0
#### Tangent-CFT
#### Tangent-CFTED
### TU\\\_DBS

# Accuracy {#accuracy}
There are many advanced features we may wish from a math information retrieval
system: the ability to reveal relationships between different pieces of
information, to visualize the information, or to use the information as a basis
for computation. However, all these advanced features are predicated on a much
more basic and fundamental feature: the ability of the math information
retrieval system to *accurately* retrieve the information that satisfy the
needs of its users.

In this chapter, I will report the results of my research of representations
that enable accurate math information retrieval:

- In Section \vref{sec:topological-math-representations} and Section
  \vref{sec:representation-learning-accuracy}, I will describe my work in
  developing new representation of math and my experiments in learning neural
  representations of natural language words and math symbols.

- In Section \vref{sec:score-aggregation-of-structural-and-semantic-matching-techniques} and
  Section \vref{sec:rank-based-fusion}, I will describe my work in overcoming the
  weaknesses of different document representations by using learning-to-rank techniques.

- In Section \vref{sec:weighted-zone-scoring} and Section
  \vref{sec:approximate-nearest-neighbor-search-accuracy}, I will describe my
  work in making fast approximate search techniques more accurate.

## Topological Math Representations for Accurate Math Information Retrieval
\label{sec:topological-math-representations}

Although tree-structured math representations for the exchange of presentation
and content math are well-established, how to transform them into a linear
stream of tokens for math information retrieval is an open problem.

In 2019, @lample2020deep have linearized a tree-structured content math
representation using pre-order traversal, producing a topologically-ordered
sequence of tokens (also known as the prefix notation or the normal Polish
notation). To give an example, the formula $x!! - y^2 = 0$ would be represented
as the following comma-separated list of math symbols: $=$, $-$, $!!$, $x$,
$◌^◌$, $y$, $2$, $0$.

Using their representation, @lample2020deep trained a Transformer model to
solve integrals, first-order differential equations, and second-order
differential equations with significantly better results than commercial CAS
such as Mathematica, MatLab, and Maple.

#### Experiments

In 2020, we have used the representation of @lample2020deep (further known as
the *prefix* notation) in our submissions to the ARQMath-1 lab to see if the
representation would also be useful for math information retrieval. Since
@lample2020deep do not specify the precise tree-structured content math
representation they used, we have used the OPT math representation in our
systems. [@novotny2020three, Section 2.1] To give an example, the formula $x!!-
y^2 = 0$ from the previous paragraph would be represented as the following
comma-separated list of OPT nodes: `U!eq`, `O!minus`, `O!double-factorial`,
`V!x`, `O!SUP`, `V!y`, `N!2`, `N!0`.

To see if the topological ordering produced by the pre-order traversal of OPT
trees is meaningful, we have also used a representation produced by the
in-order traversal of the OPT trees (further known as the *infix* notation) and
by surrounding subtrees by parentheses. To give an example, the formula $x!! - y^2
= 0$ would be represented as the following comma-separated list of math
symbols: $($, $($, $!!$, $($, $x$, $)$, $-$, $◌^◌$, $($, $y$, $2$, $)$, $)$,
$=$, $0$, $)$, and as the following comma-separated list of OPT nodes: `(`,
`(`, `O!double-factorial`, `(`, `V!x`, `)`, `O!minus`, `O!SUP`, `(`, `V!y`,
`N!2`, `)`, `)`, `U!eq`, `N!0`, `)`.

In the *answer retrieval* math information retrieval task of the ARQMath-1 lab,
we used the prefix and infix notations in two of our primary math
information retrieval systems that jointly modeled both text and math:

- *MIRMU-SCM* [@novotny2020three, Section 4], which was based on a soft vector
  space model with token embeddings from a fastText subword shallow
  log-bilinear language model, and

- *MIRMU-CompuBERT* [@novotny2020three, Section 6], which was based on the sentence
  embeddings of the Sentence BERT deep neural network language model.

In both systems, we have compared the prefix and infix notations to other math
representations on our train dataset. For the *MIRMU-SCM* system, we have used
the following math representations for comparison: none (math tokens were
removed), \LaTeX{} (every math formula becomes one token), adjacent pairs of OPT
nodes, and adjacent pairs of SLT nodes. [@novotny2020three, Section 4.3] For
the *MIRMU-CompuBERT* system, we have used the \LaTeX{} math representation for
comparison. [@novotny2020three, Table 2]

#### Results

In the *MIRMU-SCM* system, both the prefix and the infix notations received the
best results on the train dataset. The prefix and infix notations received
equivalent results, since they mainly differ by ordering (with TF-IDF weighting,
adding parenthesis tokens is insignificant). Since the soft vector space model
is insensitive to token ordering, both notations correspond to modeling
individual OPT nodes. The better results of the prefix and infix notations
compared to modeling adjacent pairs of OPT and SLT nodes show that in our soft
vector space model, modeling individual OPT nodes is better than modeling
longer paths in OPT and SLT trees.

In the *MIRMU-CompuBERT* system, the prefix notation has received better
results than the infix notation, showing that the topological ordering of
@lample2020deep is significant in deep neural language models. Surprisingly,
the \LaTeX{} representation has received better results that both prefix and
infix notations. We believe that this is due to tokenization: Unlike
@lample2020deep, we fine-tuned a language model whose tokenizer was pre-trained
on natural language text. In \LaTeX{}, commands have descriptive names in
natural language, which makes it more appropriate to use the same tokenizer
for both natural language text and math.

#### Future Work

The state-of-the-art Approach0 and Tangent-L systems use hard vector space
models to model paths in OPT and SLT trees, respectively. This indicates that
the failure of our soft vector space model to exploit longer paths in OPT and
SLT trees may be caused by the insufficient capacity of our fastText model.
Future work should test this hypothesis by performing an end-to-end
hyperparameter optimization of fastText using relevance judgements from the
ARQMath-1 and ARQMath-2 labs.

#### Reproducibility

The experimental code for both [MIRMU-SCM,][1] [MIRMU-CompuBERT,][2] and their
[hyperparameter optimization on the train dataset][3] is available on GitHub.

 [1]: https://github.com/MIR-MU/SCM-at-ARQMath
 [2]: https://github.com/MIR-MU/CompuBERT
 [3]: https://github.com/MIR-MU/ARQMath-eval (directory task1-votes)

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-accuracy}

Math information retrieval techniques for structural and semantic matching rely
on accurate neural representations (token embeddings) of natural language words
and math symbols.

In this section, I will report the results of my experiments in learning
accurate token embeddings:

- In Section \vref{sec:variable-control-in-token-embedding-evaluation}, I will
  describe my survey of methodology in previous token embedding experiments
  and show that a lack of variable control makes some previous experiments
  difficult to reproduce.

- In Section \vref{sec:heuristical-hyperparameter-optimization-in-subword-language-models},
  I will report the optimal hyperparameters for the fastText subword language
  model on nine word analogy tasks. I will also show that expensive
  hyperparameter optimization can be replaced by a simple n-gram coverage model
  that consistently improves the accuracy compared to the default
  hyperparameters.

- In sections \ref{sec:position-independent-token-embeddings-accuracy} through
  \ref{sec:decontextualized-token-embeddings}, I will show that constraining,
  quantization, disambiguation, and decontextualization of token word embeddings
  improves their accuracy on word analogy, word similarity, language modeling,
  and machine translation evaluation tasks.

- In Section \vref{sec:joint-token-embeddings-of-text-and-math}, I will show
  that using a single language model to produce token or sentence embeddings of
  both natural language words and math symbols improves accuracy on math
  information retrieval.

### Variable Control in Token Embedding Evaluation
\label{sec:variable-control-in-token-embedding-evaluation}

Although the usefulness of token embeddings for transfer learning is rarely
disputed, their theoretical foundations were only recently addressed by
@levy2014neural in 2014. In 2015, @levy2015improving have shown that several
pre-processing steps and fixed parameters can have a significant impact on
experimental results.

The accuracy of token embeddings is commonly evaluated on the intrinsic word
analogy task of @mikolov2013efficient, which measures the ability of word
vectors to answer the question ``Which word $b'$ is to $a'$ as $a$ is to $b$?''
In their 2017 paper, @rogers2017too discuss the limitations of the word analogy
task, including the selection of word pairs, the significant impact of
including/excluding the words $a, b,$ and $a'$ in the candidates for $b'$, and
the assumption that word relations are unique or even symmetric.

#### Experiments

In 2020, I conducted a survey of methodology in token embedding experiments.
[@novotny2020art] In my survey, I attempted to reproduce well-known
experimental results from the literature:

- @mikolov2013distributed @mikolov2018advances introduce a phrasing algorithm
  for merging commonly co-occurring words into multi-word expressions and they
  report state-of-the-art accuracy on the English word analogy task with their
  model.

- @mikolov2018advances @grave2018learning introduce the fastText positional
  language model and they report state-of-the-art accuracy on the English word
  analogy task with their model.

- All papers by Google and Facebook that introduce and improve the Word2Vec and
  fastText language models [@mikolov2013efficient; @mikolov2013distributed;
  @bojanowski2017enriching; @mikolov2018advances; @grave2018learning] use
  the word analogy task to evaluate their models.

I reported undisclosed variables that made the experiments difficult to
reproduce. Whereever applicable, I suggested default parameter values and
ways to make future experiments easier to reproduce.

#### Results

The phrasing algorithm contains three undisclosed parameters:

1. the bigram merging threshold $\delta$,
2. the incremental threshold decay, and
3. the maximum dictionary size $n$.

← For the threshold and the decay, only the parameter values were withheld.
For the maximum dictionary size, the parameter only appears in the reference
implementation. I suggested the following default parameter values: $\delta =
100, n = 5\cdot 10^8$.

The fastText positional language model does not disclose the initial
distribution of weights for the input and positional vectors. I described three
different initial weight distributions and I showed that up to 24\% of word
analogy accuracy can depend on them. I also showed that simpler weight
distributions suffered from numerical instability when the vector
dimensionality $D$ was high. I suggested using either the square-root normal
distribution $\mathcal{N}^{0.5}(0, \nicefrac{1}{3D^2})$ of @pinelis2018exp
or the continuous uniform $\mathcal{U}(\pm\nicefrac{\sqrt\[4\]{3}}{\sqrt{D}})$.
<!-- See also Section \vref{sec:position-independent-token-embeddings-accuracy},
where I discuss our further experiments with the fastText positional language
model. -->

The word analogy task contains three undisclosed parameters:

1. the maximum dictionary size $n$,
2. the case transformation applied to words $a, b, a'$, and $b'$, and
3. the Unicode locale used for the case transformation.

← For the maximum dictionary size, only the parameter values were withheld. For
the case transformations, the parameter only appears in the reference
implementation.  The Unicode locale appears in neither the published works nor
in the reference implementation. I showed that up to 4\% of word analogy
accuracy can depend on the Unicode locale and up to 18\% on the case
transformation and the locale. I suggested the following default parameter
values: $n = 3\cdot 10^5,$ Unicode case-folding, and the locale of the word
analogy task.

#### Reproducibility

[The experimental code for all my experiments is available GitHub.][4] I have
also released [the only public implementation of the fastText positional model.][5]

 [4]: https://github.com/MIR-MU/reproducible-ml
 [5]: https://github.com/MIR-MU/pine

### Heuristical Hyperparameter Optimization in Subword Language Models
\label{sec:heuristical-hyperparameter-optimization-in-subword-language-models}

In 2017, @bojanowski2017enriching developed the fastText subword language model
and showed that it was more accurate than the Word2Vec language model on the
English, German, Czech, and Italian word analogy tasks. They also showed the
optimal subword sizes of fastText on the English and German word analogy tasks.
However, they did not optimize the subword sizes of fastText on the Czech and
Italian word analogy tasks and used subwords of size 3--6[^6], which they
described as ``an arbitrary choice'' [@bojanowski2017enriching].

 [^6]: For subword sizes, we adopt the notation of @bojanowski2017enriching and
       @grave2018learning For example, subwords of size 3–6 are all subwords
       whose size is 3, 4, 5, or 6.

In 2018, @grave2018learning produced the French and Hindi word analogy tasks.
Furthermore, they also trained and publicly released fastText language models
for 157 languages.  Like @bojanowski2017enriching, they also neglected to
optimize the subword sizes.  Unlike @bojanowski2017enriching, they used
subwords of size 5--5 for all languages, noting that ``using character
$n$-grams of size 5, instead of using the default range of 3--6, does not
significantly decrease the accuracy (except for Czech).'' [@grave2018learning]

#### Experiments

In 2021, we attempted to reproduce the optimal subword sizes of fastText
on the English (4--5) and German (6--6) word analogy tasks reported by
@bojanowski2017enriching. Additionally, we reported the optimal subword sizes
of fastText on the Czech, Italian, Spanish, French, Hindi, Turkish, and Russian
word analogy tasks. [@novotny2021one]

Furthermore, we produced a simple $n$-gram coverage model that can suggest
subword sizes for under-resourced languages, where optimal subword sizes are
unknown.

#### Results

We reproduced the optimal subword sizes of @bojanowski2017enriching and we
reported the optimal subword sizes on the Czech (1--4), Italian (2--5), Spanish
(5--5), French (6--6), Hindi (2--2), Turkish (3--3), and Russian (5--6) word
analogy tasks. We showed that using the optimal subword sizes can result in up
to 14\% improvement in word analogy accuracy.

We also showed that using the suggested subword sizes of the $n$-gram coverage
model was never worse than the default subword sizes of
@bojanowski2017enriching @grave2018learning on our test dataset, improved the
word analogy accuracy by up to 3\% compared to the default subword sizes, and
was within 2\% word analogy accuracy of the optimal subword sizes.

#### Future Work

Although the word analogy task is a convenient proxy for the accuracy of
fastText, @ghannay2016word @chiu2016intrinsic @rogers2018whats
@kuchar2021evaluation show that it does not always correlate with extrinsic end
tasks. Future work should also evaluate our $n$-gram coverage model on
extrinsic tasks.

In recent machine translation models [@vaswani2017attention], text is tokenized
into words and subwords using word-piece [@yonghui2016google] and byte-pair
[@sennrich2016neural] models. Future work should evaluate the use of word-piece
and byte-pair models for subword selection in fastText.

#### Reproducibility

The experimental code for [the $n$-gram coverage model][7] is available on GitHub.

 [7]: https://github.com/MIR-MU/fasttext-optimizer

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-accuracy}

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate than the fastText subword language model
on the English word analogy task. However, they only evaluated the positional
model on the intrinsic word analogy task and not on extrinsic tasks.

In the subword model, tokens are represented by the parts of their meaning that
are *fixed* or dependent on the *broader context* that includes the
conversational setting, the time and location of an utterance, and the salient
common ground. [@bach2012context] Unlike in the positional model, they are
*not* represented by the parts of the meaning that are dependent the *narrow
context* of a paragraph, which makes the subword model less accurate than the
positional model. However, in the positional model, tokens are *only*
represented by the parts of their meaning that are dependent on the narrow
context of a paragraph and not the other parts, which can also make the
positional model less accurate.

#### Experiments

In 2021, we developed the fastText constrained positional model, which models
the parts of the meaning that are dependent on the narrow context of a
paragraph as well as the parts of the meaning that are fixed or dependent on
the broader context. [@novotny2021when, Section 2.2.4] We achieved this by
dividing the $D = 300$ input vector features into $D'$ narrow-context-dependent
features that would interact with positional embeddings, and the remaining $D -
D'$ fixed and broader-context-dependent features that would not interact with
positional embeddings.

We reported the optimal value of $D'$ on the English word analogy task
[@novotny2021when, Table 1] and we compared the subword, positional, and
constrained positional models on the English word analogy task
[@novotny2021when, figures 2 and 3] and an English language modeling task
[@novotny2021when, Section 4.4].

#### Results

We showed that with $D = 300$ features, using $D' = 60$
narrow-context-dependent features is optimal on the English word analogy task.

We also showed that the positional model is more accurate than the subword
model and that the constrained positional model is more accurate than the
positional model on the word analogy and language modeling tasks.

#### Future Work

@grave2018learning used the positional language model to produce token
embeddings for 157 languages, but prior work has only shown that the positional
and constrained positional models outperform the subword model on English
tasks. Future work should compare the subword, positional, and constrained
positional models on languages other than English.

#### Reproducibility

The experimental code for all our experiments is [available on GitHub][5]
together with the only public implementation of the positional and constrained
positional models.

### Quantized Token Embeddings
\label{sec:quantized-token-embeddings}

In 2018, @lam2018word2bits developed the Word2Bits quantized language model
and showed that it was more accurate than the Word2Vec language model on the
English word similarity and open-domain question answering tasks and equally as
accurate on the English word analogy task. However, word analogy accuracies for
the different categories of analogical reasoning were not reported, only total
word analogy accuracies.

#### Experiments

In 2019, I consulted the master's thesis of @stefanik2019semantic.[^32]
In the thesis, we aimed to reproduce the word analogy experiment of @lam2018word2bits
and analyze the word analogy accuracies for the different categories of
analogical reasoning. [@stefanik2019semantic, Section 3.1]

 [^32]: See also Section
        \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic} and
        Section \vref{sec:formal-concept-analysis-with-quantized-token-embeddings},
        where I discuss our further experiments with the quantized language
        model.

#### Results

We reproduced the total accuracy on the English word analogy task.
Additionally, we showed that although the quantized model way equally
as accurate as the Word2Vec language model in total accuracies, it was
significantly more accurate on the semantic categories of analogical
reasoning and significantly less accurate on the syntactic categories
of analogical reasoning.

#### Reproducibility

The experimental code for all our experiments is [available online.][8]

 [8]: https://is.muni.cz/th/bqz8u/attachments.zip

### Word Sense Disambiguation with Sense Embeddings
\label{sec:word-sense-disambiguation-with-sense-embeddings}

In both natural language and in content math, words and symbols can have
several possible senses. The accuracy of token embeddings can be improved
by accurate disambiguation of the word and symbol tokens into senses.

Previous work in training sense embeddings relies on sense-annotated training
corpora. These corpora have been manually disambiguated by human annotators and
are prohibitively small, a condition known as *the knowledge acquisition
bottleneck*.

Additionally, previous work in training sense embeddings only evaluates the
embeddings on word similarity tasks, where the sense representations are not
directly used, and not on sense-specific tasks such as word sense
disambiguation.

#### Experiments

In our work, we have developed an algorithm for enlarging sense-annotated
training corpora using a sense inventory. [@ayetiran2021eds, Section 4] We
used the enlarged corpora to train our sense embeddings without the knowledge
acquisition bottleneck.

We evaluated our sense embeddings on five English word similarity tasks.
[@ayetiran2021eds, Section 5.4] Additionally, we have developed a word sense
disambiguation algorithm for sense embeddings and used it to evaluate our sense
embeddings on six English word sense disambiguation tasks. [@ayetiran2021eds,
Section 5.3]

#### Results

Our sense embeddings achieved state-of-the-art accuracy on three out of five
word similarity tasks.

Compared to previous work in training sense embeddings using sense inventories
and other lexical resources, our sense embeddings achieved state-of-the-art
accuracy on five out of six word sense disambiguation tasks.

#### Future Work

Future work should investigate how our word sense disambiguation algorithm
can be used to disambiguate content math symbols into semantic math senses.

#### Reproducibility

Our sense embeddings are [available online.][9]

 [9]: https://mir.fi.muni.cz/EDS-MEMBED.zip

### Decontextualized Token Embeddings
\label{sec:decontextualized-token-embeddings}

Math information retrieval techniques for structural and semantic matching such
as the soft vector space model and the word mover's distance rely on global
token embeddings of shallow log-bilinear language models.

In 2019, @zhao2019moverscore showed that using contextual embeddings of deep
neural language models improves the accuracy of the word mover's distance on
machine translation evaluation tasks compared to using global embeddings.
However, using contextual embeddings in a math information retrieval system
requires expensive query-time inference.

#### Experiments

In 2021, we developed an algorithm that *decontextualized* contextual token
embeddings into global token embeddings by averaging across all contexts of a
token. [@stefanik2021regressive, Section 3.2]

We evaluated the soft vector space model and the word mover's distance with our
decontextualized embeddings and with the global embeddings of a shallow
log-bilinear fastText subword language model on eight machine translation
evaluation tasks.

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
: [The accuracies of the soft vector space and the word mover's distance %
   with and without our decontextualized token embeddings on eight machine %
   translation tasks]%
  {The accuracies of the soft vector space (SCM) and the word mover's distance (WMD) %
   with and without our decontextualized token embeddings on eight machine %
   translation evaluation tasks. [@stefanik2021regressive] %
   Language pairs marked with an × were evaluated cross-lingually. %
   [@stefanik2021regressive, Section 3.4]} \label{tab:decontextualized-token-embeddings}

#### Results

We showed that the soft vector space model and the word mover's distance were
more accurate with our decontextualized embeddings than with the global
embeddings of a fastText model on most tasks: five out of eight for the soft
vector space model and six out of eight for the word mover's distance, see
Table \vref{tab:decontextualized-token-embeddings}.

#### Future Work

Future work should evaluate decontextualized embeddings on math information
retrieval.

#### Reproducibility

The experimental code for our experiments is [available on GitHub][10]
and as [a Docker image.][11]

 [10]: https://github.com/MIR-MU/regemt
 [11]: https://hub.docker.com/r/miratmu/regemt

### Joint Token and Sentence Embeddings of Text and Math
\label{sec:joint-token-embeddings-of-text-and-math}

In 2018, @krstowski2018equation used the general shallow log-bilinear language
model of @rudolph2016exponential to train joint token embeddings of text and
math. They showed that they their embeddings were more accurate than the token
embeddings of other general shallow log-bilinear language models on a language
modeling task. However, they did not evaluate their token embeddings on math
information retrieval.

In 2017, @bojanowski2017enriching developed the fastText subword shallow
log-bilinear language model and showed that it was more accurate than general
shallow log-bilinear language models on word analogy tasks. However,
@krstowski2018equation did not use a subword language model to train their
token embeddings and they did not compare their token embeddings to subword
language models.

In 2019, @reimers2019sentencebert developed the Sentence BERT deep neural
network language model and achieved state-of-the-art accuracy with its sentence
embeddings on semantic text similarity tasks. However, they did not evaluate
their sentence embeddings on information retrieval, because the Sentence BERT
uses dense attention and therefore can only embed short sentences, not the
full-length documents that are commonly used in information retrieval tasks.
Additionally, Sentence BERT has never been used to train joint sentence
embeddings of text and math or evaluated on math information retrieval.

#### Experiments

In 2020 and 2021, we developed two primary math information retrieval
systems[^12] using joint token and sentence embeddings of text and math for the
*answer retrieval* math information retrieval task of the ARQMath-1 and
ARQMath-2 labs:

- *MIRMU-SCM* [@novotny2020three, Section 4; @novotny2021ensembling, Section
  3.3], which was based on a soft vector space model with token embeddings from
  a fastText subword shallow log-bilinear language model, and

- *MIRMU-CompuBERT* [@novotny2020three, Section 6; @novotny2021ensembling, Section
  3.4], which was based on the sentence embeddings[^13] of the Sentence BERT deep
  neural network language model.

 [^12]: See Section \vref{sec:topological-math-representations} for a
        discussion of the math representation that we have developed for our
        systems.

 [^13]: The questions and answers in the answer retrieval task are short,
        which made it feasible to use the Sentence BERT model despite its
        dense attention.

#### Results

At ARQMath-1, we showed that *MIRMU-SCM* was more accurate than any other
individual information retrieval system from our MIRMU team, including
*MIRMU-MIaS* of the European Digital Mathematical Library, which is based on
the hard vector space model and does not use token embeddings. This shows that
using joint token embeddings of text and math from subword language models
improves accuracy on math information retrieval. We also showed that
*MIRMU-SCM* was among the top three most accurate primary systems in the
competition.  *MIRMU-CompuBERT* suffered from a lack of accurate training data
and failed to reach accuracy that would be significantly better than zero at
ARQMath-1.

At ARQMath-2, we showed that with accurate training data, *MIRMU-CompuBERT*
was more accurate than *MIRMU-SCM*. @mansouri2021dprl [Section 3] also used
sentence embeddings of the Sentence BERT in two of their three math information
retrieval systems (*DPRL-RRF* and *DPRL-QASim*) and achieved the second and
third best accuracies in the competition.  This shows that using joint sentence
embeddings of text and math from deep neural network language models is
well-motivated and improved accuracy on math information retrieval.

#### Reproducibility

The experimental code for both [MIRMU-SCM][1] and [MIRMU-CompuBERT][2] from the
ARQMath-1 lab is available on GitHub. The experimental code for all our systems
from the ARQMath-2 lab is [available online.][14]

 [14]: https://mir.fi.muni.cz/arqmath-2021

## Score Aggregation of Structural and Semantic Matching Techniques
\label{sec:score-aggregation-of-structural-and-semantic-matching-techniques}

Techniques for structural and semantic matching such as the soft vector space
model and the word mover's distance make naive assumptions about the syntax and
the semantics of the natural language and the language of mathematics. They
also often rely on token embeddings of language models, which contain
human-like biases.  [@caliskan2017semantics] These assumptions and biases cause
systematic errors and decrease accuracy on machine translation evaluation
tasks.

#### Experiments

In 2021, we used score aggregation to ensemble 16 diverse structural and
semantic matching techniques in order to avoid the systematic errors of any
individual technique. [@stefanik2021regressive, Section 3.3] We trained and
evaluated our ensemble on 20 machine translation evaluation tasks.

|                   |         | RegEMT | Best individual system     |
|-------------------|---------|--------|----------------------------|
| MQM, src          | zh→en   | *0.59* |  0.44  (BERTScr, WMD-cont) |
| MQM, src          | zh→en × | *0.49* |  0.44  (BERTScr, WMD-cont) |
| MQM, src          | en→de   | *0.36* |  0.28  (Reg-base)          |
| MQM, src          | en→de × | *0.31* |  0.28  (Reg-base)          |
| MQM, ref          | zh→en   | *0.62* |  0.51  (Comet)             |
| MQM, ref          | zh→en × | *0.62* |  0.51  (Comet)             |
| MQM, ref          | en→de   | *0.60* |  0.48  (Comet)             |
| MQM, ref          | en→de × |  0.38  | *0.48  (Comet)*            |
| DA 2016, src      | cs→en   |  0.86  | *0.89  (Comet)*            |
| DA 2016, src      | de→en   | *0.84* |  0.80  (BLEUrt)            |
| DA 2016, src      | fi→en   | *0.87* |  0.85  (Comet)             |
| DA 2016, src      | ru→en   | *0.79* |  0.78  (Comet)             |
| DA 2016, tgt      | cs→en   | *0.77* | *0.77  (Prism)*            |
| DA 2016, tgt      | de→en   |  0.45  | *0.58  (Prism)*            |
| DA 2016, tgt      | fi→en   | *0.76* | *0.76  (Prism)*            |
| DA 2016, tgt      | ru→en   | *0.74* |  0.70  (Prism)             |
| Catastrophic, src | en→cs   | *0.45* |  0.35  (Prism)             |
| Catastrophic, src | en→de   |  0.34  | *0.43  (Prism)*            |
| Catastrophic, src | en→ja   | *0.20* |  0.18  (Reg-base)          |
| Catastrophic, src | en→zh   |  0.16  | *0.27  (SCM-dec)*          |
%                                              
: [The accuracies of the our ensemble and the best individual systems %
   on 20 machine translation evaluation tasks]%
  {The accuracies of the our ensemble (RegEMT) and the best individual systems %
   on 20 machine translation evaluation tasks. [@stefanik2021regressive] %
   Language pairs marked with an × were evaluated cross-lingually. %
   [@stefanik2021regressive, Section 3.4]} \label{tab:regemt}

#### Results

We showed that our ensemble was more or equally accurate than any single
technique on 15 out of 20 machine translation evaluation tasks, which confirms
that score aggregation is a useful technique for avoiding naive assumptions and
biases of individual semantic matching techniques, see Table \vref{tab:regemt}.

#### Future Work

Future work should evaluate the accuracy of our ensemble on math information
retrieval.

#### Reproducibility

The experimental code for our experiments is [available on GitHub][10]
and as [a Docker image.][11]

## Rank-Based Fusion of Math Information Retrieval Systems
\label{sec:rank-based-fusion}

The systematic errors of structural and semantic matching techniques also
affect the ranking of results in document retrieval systems. As a result,
different information retrieval systems can agree on a small portion of the
most relevant documents, but each individual system will miss the great
majority of relevant documents.

In 2009, @cormack2009reciprocal developed a rank-based fusion technique and
showed that it achieved better accuracy than any individual system on five out
of five information retrieval tasks and that it achieved state-of-the-art
accuracy on three out of five information retrieval tasks. However, they did
not evaluate their rank-based fusion technique on math information retrieval.

#### Experiments

In 2020, we developed the *MIRMU-Ensemble* rank-based fusion technique for the
*answer retrieval* math information retrieval task of the ARQMath-1 lab.
[@novotny2020three, Section 7]. We used our technique to ensemble the primary
systems of our MIRMU team.

In 2021, we developed two new supervised rank-based fusion techniques
(*MIRMU-WIBC* and *MIRMU-RBC*) and we used them together with our unsupervised
MIRMU-Ensemble (further known as *MIRMU-IBC*) and the unsupervised technique of
@cormack2009reciprocal (further known as *MIRMU-RRF*) for the answer retrieval
math information task of the ARQMath-2 lab. [@novotny2021ensembling, Section 4]
We used our technique to ensemble all ten systems of our MIRMU and MSM
teams.[^17]

 [^17]: We entered the ARQMath-2 lab as two teams: our lab (MIRMU) and the
        students of the FI:PV211 information retrieval course taught at the
        Masaryk University (MSM).

To see if our unsupervised techniques can benefit from a large number of
systems, we also used them to ensemble all non-baseline primary systems at the
ARQMath-1 and ARQMath-2 labs. To see if our techniques can benefit from a small
number of diverse systems, we also used them to ensemble six selected systems
out of all our ten systems at the ARQMath-2 lab.

#### Results

At ARQMath-1, we showed that *MIRMU-Ensemble* was more accurate than any
individual information retrieval system from our MIRMU team. When we used
*MIRMU-Ensemble* to ensemble all non-baseline primary systems, we received the
best accuracy in the competition, which indicates that *MIRMU-Ensemble* can
benefit from a large number of systems.

At ARQMath-2, we showed that all our four rank-based fusion techniques were
more accurate than any individual information retrieval system from our MIRMU
and MSM teams. We also showed that our *MIRMU-WIBC* can be more accurate than
the *MIRMU-RRF* of @cormack2009reciprocal.

When we used the unsupervised *MIRMU-IBC* and *MIRMU-RRF* to ensemble all
non-baseline primary systems, we received the best (*MIRMU-RRF*) and the
second best (*MIRMU-IBC*) accuracies in the competition, which indicates that
both techniques can benefit from a large number of systems. Ensembling only six
selected systems out of all our ten systems increased the accuracy of three out
of our four techniques, which indicates that all our techniques can benefit
from a small number of diverse systems.

#### Reproducibility

The experimental code for [MIRMU-Ensemble][18] from the ARQMath-1 lab is
available on GitHub. The experimental code for all our systems from the
ARQMath-2 lab is [available online.][14]

 [18]: https://github.com/MIR-MU/SCM-at-ARQMath (file scripts/combine\\_serps.py)

## Weighted Zone Scoring Across Different Domains
\label{sec:weighted-zone-scoring}

In information retrieval, retrieval units are usually structured and contain
zones that are more important than the body text, such as the title, abstract,
and keywords, and zones that are less important than the body text, such as the
appendices, footnotes, and comments. Weighted zone scoring techniques can be
used to inform an information retrieval system about the importance of
different zones and improve its accuracy. [@manning2008introduction, Section 6.1]

#### Experiments

In 2018, we analyzed the datasets and the relevance judgements from the
*community question answering* information retrieval task of the SemEval-2016
and SemEval-2017 competitions. [@novotny2018weighting]
The datasets consist of questions and threads of comments. The relevance
judgements show how relevant the individual comments (subtask A) and the whole
threads (subtask A) are to a question.

Following the empirical law of @godwin1991re, we investigated whether later
comments in a thread were less likely to be relevant. We used the results of
our investigation to develop an information retrieval system that retrieved
threads using weighted zone scoring and we evaluated the system on the subtask
B of the *community question answering* information retrieval task of
SemEval-2016 and SemEval-2017.

In 2021, we developed three primary math information retrieval systems using
weighted zone scoring for the *answer retrieval* math information retrieval
task of the ARQMath-2 lab:[^17]

- *MSM-MG* [@novotny2021ensembling, Section 3.1], which represents every answer
  as its body text and the title, the body text, and the tags of its parent
  question, all with equal weights,

- *MSM-PZ* [@novotny2021ensembling, Section 3.2], which represents every answer
  as its body text and the title and the tags of its parent question, where the
  title and the tags of the parent question are given three times as much
  weight as the body text of the answer, and

- *MIRMU-SCM* [@novotny2021ensembling, Section 3.3], which represents every
  as its body text and the title, the body text, and the tags of its parent
  question, where the title of the parent question is given three times as much
  weight as any of the remaining parts.

← To show how important weighted zone scoring was, we compared the *MSM-MG*,
*MSM-PZ*, and *MIRMU-SCM* systems to the remaining six systems of the MSM team
that used similar information retrieval techniques but did not use weighted
zone scoring.

#### Results

We showed that in the datasets from the *community question answering*
information retrieval task of the SemEval-2016 and SemEval-2017 competitions,
later comments were significantly less likely to be relevant and that their
relevance followed a Zipf distribution, see Figure \vref{fig:weighted-zone-scoring}.

We then developed three information retrieval systems: one that used weighted
zone scoring to place smaller weight to later comments in a thread, one that
did not use zoned weighting but continuously decreased the weights of later
tokens in a thread, and one that used neither [@novotny2018weighting, Figure 3]
We showed that our system that used zoned weighting achieved the
state-of-the-art accuracy on both datasets, whereas our two systems that did
not use zoned weighting achieved worse-than-baseline accuracy on both datasets.
[@novotny2018weighting, tables 1 and 2] This shows that weighted zone scoring
can be important for information retrieval.

 /figures/godwins-law.tex

We also showed that our three math information retrieval systems that used
weighted zone scoring all achieved better accuracy than our six systems that
did not use weighted zone scoring on the *answer retrieval* math information
retrieval task of the ARQMath-2 lab. This shows that weighted zone scoring can
be important for math information retrieval.

#### Future Work

Future work should study in more detail the impact of weighted zone scoring on
math information retrieval.

#### Reproducibility

The code for [our SemEval experiments][19] is available on GitHub.
The experimental code for all our systems from the ARQMath-2 lab is [available
online.][14]

 [19]: https://github.com/witiko-masters-thesis/segmentation

## Approximate Nearest-Neighbor Search in Vector Spaces
\label{sec:approximate-nearest-neighbor-search-accuracy}

Until we have solved the mystery that is the human mind, ranking criteria of
math information retrieval systems will always be just an approximation to the
relevance perceived by the users. Additionally, due to the amount of data and
time constraints, even this formal ranking criteria are usually approximated.

In this section, I will report the results of my experiments in making
approximate math information retrieval more accurate:

- In Section \vref{sec:dense-retrieval-in-inverted-indices-accuracy}, I will
  show that dense retrieval can be approximated with almost no loss in
  accuracy.

- In Section \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy},
  I will show that sparse retrieval can be approximated in a way that increases
  both speed and accuracy.

### Dense Retrieval in Inverted Indices
\label{sec:dense-retrieval-in-inverted-indices-accuracy}

Recent results show that dense retrieval techniques can achieve higher accuracy
than sparse retrieval techniques on information retrieval tasks.
[@lin2021batch] At the *answer retrieval* math information retrieval task of
the ARQMath-1 and ARQMath-2 labs, sparse information retrieval techniques still
achieved the best accuracy, but the gap is closing: Whereas at ARQMath-1, only
three[^19] out of 23 systems (13\%) used dense retrieval and the best dense
retrieval system only received the seventh best accuracy in the competition,
[@zanibbi2020overview, Table A1], at ARQMath-2, nine[^20] out of 40 systems
(23\%) used dense retrieval and the best dense retrieval systems received the
second and third best accuracies in the competition. [@behrooz2021overview,
Table 4]

 [^19]: The three systems were, by decreasing accuracy:

    1. *PSU1*,
    2. *PSU3*, and
    3. *MIRMU-CompuBERT*.

 [^20]: The nine systems were, by decreasing accuracy:

    1. *DPRL-MathSE*,
    2. *DPRL-QASim*,
    3. *TU\\_DBS\\_P*,
    4. *TU\\_DBS\\_A2*,
    5. *TU\\_DBS\\_A3*,
    6. *TU\\_DBS\\_A1*,
    7. *MIRMU-CompuBERT*,
    8. *PSU*, and
    9. *TU\\_DBS\\_A4*.

Digital mathematical libraries use industry-grade inverted indices such as
ElasticSearch, Solr, and Lucene for sparse retrieval. However, as dense
retrieval is becoming more accurate than sparse retrieval, libraries face the
economic dilemma of either being inaccurate or replacing their tried-and-tested
sparse retrieval architectures with dense vector databases.

#### Experiments

In 2017, we showed how dense embeddings can be encoded as sparse vectors.
[@rygl2017semantic, Section 2.2] Using our encoding, we developed a dense
information retrieval system that used an inverted index as its sparse vector
database, and we evaluated how accurate it was at the retrieval of the dense
embeddings of tokens, sentences, and image descriptors. [@rygl2017semantic,
Section 3.1; @ruzicka2017flexible, Section 3]

#### Results

We showed that our system could perform exact dense retrieval with almost
no loss in accuracy. [@rygl2017semantic, Table 2; @ruzicka2017flexible, Section 4]
We also showed that both our encoding and our system can be configured to trade
off accuracy for speed and perform fast approximate dense retrieval[^37]
[@rygl2017semantic, Figure 1; @ruzicka2017flexible, figures 1--14], see also
Section \vref{sec:dense-retrieval-in-inverted-indices-speed}.

 [^37]: See Section \vref{sec:dense-retrieval-in-inverted-indices-speed} for a
        discussion of the trade offs between speed and accuracy.

Our results showed that the dilemma of the digital mathematical libraries is
false: Using our system, libraries can use more accurate dense retrieval
without replacing their tried-and-tested sparse retrieval architectures.

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy}

State-of-the-art math information retrieval systems rely on the hard soft
vector space model of @salton1988termweighting, which is fast and
interpretable, but whose accuracy is limited by its failure to recognize that
the meaning of different words and symbols can be similar or related.

In 2014, @sidorov2014soft developed the soft vector space model, which can
recognize the semantic relatedness of words and symbols. However, the soft
vector space model was not practically fast. Additionally, the soft vector
space model required a measure of relatedness between words and symbols.
Furthermore, the soft vector space model had not been evaluated on math
information retrieval.

In 2017, @charlet2017simbow developed two approximate measures of relatedness
between words and symbols based on the edit distance and on token embeddings
and they achieved state-of-the-art accuracy on a semantic text similarity task.

In 2018, @lam2018word2bits developed the Word2Bits quantized language model
technique and showed that it was more accurate than the Word2Vec language model
on the English word similarity and open-domain question answering tasks and
equally as accurate on the English word analogy task. The token embeddings of
the quantized model could make the soft vector space model practically fast.
However, they the soft vector space model with the quantized token embeddings
had not been never evaluated.

#### Experiments

In 2018, I developed an algorithm for the soft vector space model that achieved
practical speed[^21] by placing restrictions on the measures of relatedness
that the soft vector space model could use. [@novotny2018implementation,
Section 3]

 [^21]: See Section \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}
        for a discussion about our improvements to the speed of the soft vector
        space model.

In 2020, we developed an *orthogonalization* technique for token embeddings and
we proved that using the measure of @charlet2017simbow with orthogonalized
embeddings was consistent with the restrictions of my fast algorithm for the
soft vector space model. [@novotny2020text, Section 4.2] We evaluated the soft
vector space model with quantized token embeddings and with regularized token
embeddings on six text classification tasks. [@novotny2020text, Section 5]
We also reported the optimal hyperparameter values for our orthogonalization
algorithm on all six tasks. [@novotny2020text, Table 2]

In 2020 and 2021, we evaluated the soft vector space model on the *answer
retrieval* math information retrieval task of the ARQMath-1 and ARQMath-2 labs
to see if the soft vector space model would be more accurate than the hard
vector space model. [@novotny2020three, Section 4; @novotny2021ensembling,
Section 3.3] We also reported the optimal hyperparameter values for our
orthogonalization algorithm. [@novotny2020three, Table 1]
 
 [^22]: See Section \vref{sec:joint-token-embeddings-of-text-and-math} for a
        discussion about the joint token embeddings of text and math that we
        developed for soft vector space model in the ARQMath-1 and ARQMath-2 labs.

#### Results

We showed that quantized and (especially) regularized token embeddings not only
did not decrease the accuracy of the soft vector space, but actually improved
it on five out of six text classification tasks. We also showed that the soft
vector space model with any of our token embeddings was more accurate than the
hard vector space model on all six tasks.

We also showed that the soft vector space can be more accurate than math
information retrieval systems that used similar information retrieval techniques
but used the hard vector space model instead of the soft vector space model:
For example, at ARQMath-1, the soft vector space model (*MIRMU-SCM*) was more
accurate than *MIRMU-MIaS* of the European Digital Mathematical Library and
at ARQMath-2, the soft vector space model was more accurate than five systems
of the MSM team.[^23]

 [^23]: *MIRMU-SCM* is more accurate even when we disable weighted zone scoring,
        which the five systems of the MSM team did not use, see also
        Section \ref{sec:weighted-zone-scoring}.

#### Future Work

At the ARQMath-1 and ARQMath-2 labs, state-of-the-art accuracy was achieved
by systems that used the BM-25 term weighting scheme, whereas our soft vector
space model used the TF-IDF term weighting scheme. Future work should study in
more detail the impact of term weighting schemes on the accuracy of the soft
vector space model on math information retrieval.

#### Reproducibility

I have released [the only public implementation of the soft vector space model][24]
in the free open-source Gensim NLP library [@rehurek2010software] through a
series of pull requests on GitHub. [@novotny2018implementa;
@novotny2018implementb; @novotny2020reduce; @novotny2021use]

The experimental code for the soft vector space model on the text classification
tasks is [available on GitHub.][25]

The experimental code for the soft vector space model from the ARQMath-1 lab is
[available on GitHub.][1] The experimental code for all our systems from the
ARQMath-2 lab is [available online.][14]

 [24]: https://radimrehurek.com/gensim/similarities/termsim.html
 [25]: https://github.com/MIR-MU/regularized-embeddings

# Speed {#speed}
The accuracy as well as the availability of more advanced features in a math
information retrieval system are fundamentally limited by the *speed* at which
we can deploy the system, store new information in it, and recall information
that satisfy the needs of its users.

In this chapter, I will report the results of my research of representations
and techniques that enable fast math information retrieval:

- In Section \vref{sec:fast-and-reproducible-deployment}, I will describe my
  work in making math information retrieval search engines easier to build and
  deploy.

- In Section \vref{sec:representation-learning-speed}, I will describe my
  work in making neural representations of natural language words and math
  symbols both faster to learn and faster to use.

- In Section \vref{sec:approximate-nearest-neighbor-search-speed}, I will
  describe my work in developing fast approximate search techniques.

## Fast and Reproducible Deployment of Math Information Retrieval Systems Using Docker and Continuous Integration
\label{sec:fast-and-reproducible-deployment}

Docker is a lightweight container-based virtualization tool, which makes it
possible to distribute software together with all its dependencies as a single
image. Additionally, processes running in Docker are comparable in speed to
regular non-virtualized processes.

In software engineering, continuous integration is the practice of merging the
work of all developers to a shared repository several times a day. To maintain
code quality, best practices for continuous integration include automating the
builds and making the builds self-testing.

In 2018, we have publicly released our MIaS search engine of the European
Digital Mathematical Library [on GitHub.][28] [@sojka2018mias] We have also
developed a solution for deploying MIaS and we have made it [available on
GitHub.][26] However, our solution was slow, because we always needed to build
MIaS during the deployment. Additionally, our builds would suddenly fail due to
the changing code base of MIaS and a lack of code quality control. Furthermore,
our solution had been written in the GNU Make language, which was difficult to
maintain.

 [26]: https://github.com/MIR-MU/ntcir-mias-deployment
 [28]: https://github.com/MIR-MU/WebMIaS

In 2020, we have developed a new solution for deploying MIaS at the ARQMath-1
lab and we have also made it [available on GitHub.][27] Our new solution had
been written in Python, which was easier to maintain than GNU Make. However,
our new solution was still slow and our builds would suddenly fail.

 [27]: https://github.com/MIR-MU/ARQMath-eval (file docs/MIaS-at-ARQMath.ipynb)

#### Experiments

In 2021, we have developed a new solution for deploying MIaS using Docker
continuous integration. [@luptak2021deploying] [^29] With Docker, our solution
is no longer slow, because we do not need to build MIaS during the deployment,
but we can download a pre-built Docker image. With continuous integration, our
builds would no suddenly fail during the deployment, because any change to the
code base would build and test MIaS immediately.

 [^29]: For continuous integration, we have used the GitHub Actions service,
        which offers a free plan to public open-source repositories.

#### Reproducibility

Our fast deployment solution for MIaS is [available on GitHub][30] and as [a
Docker image.][31]

 [30]: https://github.com/MIR-MU/WebMIaS
 [31]: https://hub.docker.com/r/miratmu/webmias/tags

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-speed}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate, but also fast to train and use.

In this section, I will report the results of my experiments in learning fast
token embeddings:

- In Section \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic},
  I will show that quantizing the Word2Vec language model makes it possible to
  compute a fast and accurate bitwise approximation of the similarity between
  a pair of token embeddings.

- In Section \vref{sec:position-independent-token-embeddings-speed}, I will show
  that constraining the fastText positional language model improves its
  training speed.

### Quantized Token Embeddings with Fast Bitwise Arithmetic
\label{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}

Math information retrieval techniques for structural and semantic matching rely
on fast measures of similarity between token embeddings of natural language
words and math symbols. However, common similarity measures such as the cosine
similarity require expensive floating point multiplication and division.

In 2018, @lam2018word2bits developed the Word2Bits quantized language model and
showed that it was more accurate than the Word2Vec language model on the
English word similarity and open-domain question answering tasks and equally as
accurate on the English word analogy task. With the quantized language mode, we
can approximate the similarity between a pair of token embeddings using fast
bitwise arithmetic. However, the speed and the accuracy of these approximations
had not been evaluated.

#### Experiments

In 2019, I consulted the master's thesis of @stefanik2019semantic.[^33] In the
thesis, we approximated the cosine similarity $\cos(\vec x, \vec y)$ between a
pair of quantized token embeddings $\vec x$ and $\vec y$ as the inverse of
their Hamming distance $‖\vec x ⊻ \vec y‖\_1$.  [@stefanik2019semantic, Section
3.8] For our evaluation on the English word analogy task, we also approximated
the vector addition $\vec{x} + \vec{y}$ as the logical disjunction $\vec x ∨
\vec y$, and the vector subtraction $\vec x - \vec y$ as the logical
non-implication $\vec x ⇏ \vec y$.

 [^33]: See also Section \vref{sec:quantized-token-embeddings} and
        Section \vref{sec:formal-concept-analysis-with-quantized-token-embeddings},
        where I discuss our further experiments with the quantized language
        model.

#### Results

We showed that our bitwise approximation can be more than 20× faster than the
float arithmetic and we also showed that our approximation was within 2\%
accuracy of the cosine similarity on the English word analogy task, see
Figure \vref{fig:quantized-token-embeddings-with-fast-bitwise-arithmetic}. This
shows that quantized token embeddings can be used for fast structural and
semantic matching.

 /figures/word2bits-speed.tex

#### Future Work

Future work should evaluate our approximation on math information retrieval.

#### Reproducibility

Our implementation of the word analogy task using fast bitwise arithmetic is
[available on GitHub.][34]

 [34]: https://github.com/witiko/word2bits (branch non-binary-and-bitwise-compute-accuracy)

The experimental code for all our experiments is [available online.][8]

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-speed}

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate than the fastText subword language model
on the English word analogy task. However, they did not release a public
implementation of the positional model or show that the model was practically
fast to train.

#### Experiments

In 2020, I implemented the positional model[^35] and I evaluated if it can
achieve better accuracy than the subword language model on the English word
analogy task when we trained both models for an equal amount of time.
[@novotny2020art, Table 3]

 [^35]: See Section \vref{sec:variable-control-in-token-embedding-evaluation}
        for a discussion of my implementation of the positional model.

In 2021, we developed the fastText constrained positional model,[^36] which was
less complex than the positional model and which achieved better accuracy than
the positional model on word analogy and language modeling tasks.
[@novotny2021when, Section 4] We compated the training time of both models.

 [^36]: See Section \vref{sec:position-independent-token-embeddings-accuracy}
        for a discussion of the development and the accuracy of the contrained
        positional model.

#### Results

We showed that that positional model could achieve 4\% better accuracy than the
subword language model on the English word analogy task when we trained both
models for five hours. We also showed that our constrained positional model
trained 2× faster than the positional model.

#### Reproducibility

[The experimental code for all my experiments is available GitHub.][4] I have
also released [the only public implementation of the fastText positional model
and our fastText constrained positional model.][5]

## Approximate Nearest-Neighbor Search in Vector Spaces
\label{sec:approximate-nearest-neighbor-search-speed}

The amount of information that a math information retrieval system must store
is increasing at an exponential rate. At the same time, users are less willing
to wait for the system to recall information. [@apica2017digital]

In this section, I will report the results of my experiments in making math
information retrieval faster by using an approximate ranking criterion:

- In Section \vref{sec:dense-retrieval-in-inverted-indices-speed}, I will
  show that approximation can greatly improve the speed of dense retrieval with
  almost no loss in accuracy.

- In Section \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed},
  I will show that sparse retrieval can be approximated in a way that increases
  both speed and accuracy.

### Dense Retrieval in Inverted Indices
\label{sec:dense-retrieval-in-inverted-indices-speed}

In 2017, we showed how dense embeddings can be encoded as sparse vectors.
[@rygl2017semantic, Section 2.2] Using our encoding, we developed a dense
information retrieval system that used an inverted index as its sparse vector
database, and we showed that it could perform exact dense retrieval with almost
no loss in accuracy[^38] [@rygl2017semantic, Table 2] and that both our
encoding and our system could be configured to trade off accuracy for speed and
perform fast approximate dense retrieval[^37] [@rygl2017semantic, Figure 1;
@ruzicka2017flexible, figures 1--14] Let us now discuss the speed of our system
in more detail.

 [^38]: See Section \vref{sec:dense-retrieval-in-inverted-indices-accuracy} for
        a discussion about the development and the accuracy of our system.

#### Experiments

We proved that our encoding improved the time complexity of the retrieval
compared to naive search and that removing unimportant features improved both
time and space complexity of the retrieval.  [@rygl2017semantic, Table 1]
We also evaluated the speed of several configurations of our encoding and our
system.

#### Results

We showed that there ways an optimal configuration of our encoding and our
system, where our system wass both fast and accurate, see Figure
\ref{fig:scaletext-speed}.

 /figures/scaletext-speed.tex

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}

However, computing the similarity of two texts with the soft vector space model
has an unpractical time complexity of $\mathcal{O}(n^2)$ for a vocabulary of
$n$ words and symbols. 

# Interpretability {#interpretability}
In math information retrieval, accuracy and speed ensure that we can recall
information that a user needs when she needs them. However, the ability of a
system to explain the information to the user is fundamentally limited by the
*interpretability* of its internal representations.

In this chapter, I will report the results of my research of interpretable
representations that enable explainable information retrieval:

- In Section \vref{sec:representation-learning-interpretability}, I will
  describe my work in making neural representations of natural language words
  and math symbols more interpretable.

- In Section \vref{sec:strengths-and-weaknesses}, I will describe my work in
  explaining the roles played by different math information retrieval systems
  when using rank-based fusion and learning-to-rank techniques.

- In Section \vref{sec:interactive-visualizations}, I will describe my work in
  producing interactive visualizations of math information retrieval using
  interpretable document representations.

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-interpretability}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate, but also interpretable.

In this section, I will report the results of my experiments in learning
interpretable token embeddings:

- In Section \vref{sec:formal-concept-analysis-with-quantized-token-embeddings},
  I will show that quantizing the Word2Vec language model makes it possible to
  determine type-of relations between tokens.

- In Section \vref{sec:position-independent-token-embeddings-interpretability},
  I will show that the fastText positional language model makes it possible to
  determine grammatical properties of tokens.

### Formal Concept Analysis with Quantized Token Embeddings
\label{sec:formal-concept-analysis-with-quantized-token-embeddings}

In 1982, @wille1982restructuring proposed the formal concept analysis as a
reformulation of the lattice theory that focused on objects and their binary
attributes. The binary features of the Word2Bits quantized language model
of @lam2018word2bits can be studied using the formal concept analysis.

In 2015, @priss2005linguistic has noted that formal concept analysis can be
used to encode and analyze type-of (hyponymy-hyperonymy) word relations.
Parts of speech are a type-of relation. However, the quantized language model
has not been evaluated on a part-of-speech tagging task.

#### Experiments

In 2019, I consulted the master's thesis of @stefanik2019semantic.
<!-- See also Sections \vref{sec:quantized-token-embeddings}
and Section \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}, where I
discuss our further experiments with the quantized language model. -->
In the thesis, we evaluated the quantized model on an English part-of-speech
tagging task: For each part-of-speech tag, we reported the most well-correlated
token embedding feature. To see if the part-of-speech relation would be
eventually distributed between different features, we repeated the experiment
for different vector dimensionalities $D$. [@stefanik2019semantic, Section 3.3]

#### Results

We showed that for vector dimensionality $D = 30$, nouns, verbs, and
adjectives are significantly correlated with their token embedding feature.
The correlations decrease as the vector dimensionality $D$ increases and become
they insignificant for $D\geq 600$, which indicates that the part-of-speech
relation has been distributed between different token embedding features.

#### Future Work

#### Reproducibility

The experimental code for all our experiments is [available online.][8]

 [8]: https://is.muni.cz/th/bqz8u/attachments.zip

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-interpretability}

## Strengths and Weaknesses of Math Information Retrieval Systems
\label{sec:strengths-and-weaknesses}

## Interactive Visualizations of Math Information Retrieval Collections
\label{sec:interactive-visualizations}

# Conclusion {#conclusion}
## Thesis Summary
### Research Questions
### Thesis Contributions
## Future Work
