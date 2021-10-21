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

- In sections \ref{sec:topological-math-representations} and
  \ref{sec:representation-learning-accuracy}, I will describe my work in
  developing new representation of math and my experiments in learning neural
  representations of natural language words and math symbols.

- In sections \ref{sec:ensembling-semantic-text-similarity-measures} and
  \ref{sec:rank-based-fusion}, I will describe my work in overcoming the
  weaknesses of different document representations by using score aggregation,
  rank-based fusion, and learning-to-rank techniques.

- In sections \ref{sec:weighted-zone-scoring} and
  \ref{sec:approximate-nearest-neighbor-search-accuracy}, I will describe my
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

In the answer retrieval task of ARQMath-1, we have used the prefix and infix
notations in two of our primary math information retrieval systems that jointly
modeled both text and math:

- *MIRMU-SCM* [@novotny2020three, Section 4], which was based on a soft vector
  space model with token embeddings from a fastText subword shallow
  log-bilinear language model,
- *MIRMU-CompuBERT* [@novotny2020three, Section 6], which was based on the sentence
  embeddings of the Sentence BERT deep neural network language model.

In both systems, we have compared the prefix and infix notations to other math
representations on our train dataset. For the MIRMU-SCM system, we have used
the following math representations for comparison: none (math tokens were
removed), \LaTeX{} (every math formula becomes one token), adjacent pairs of OPT
nodes, and adjacent pairs of SLT nodes. [@novotny2020three, Section 4.3] For
the MIRMU-CompuBERT system, we have used the \LaTeX{} math representation for
comparison. [@novotny2020three, Table 2]

#### Results

In the MIRMU-SCM system, both the prefix and the infix notations received the
best results on the train dataset. The prefix and infix notations received
equivalent results, since they mainly differ by ordering (with TF-IDF weighting,
adding parenthesis tokens is insignificant). Since the soft vector space model
is insensitive to token ordering, both notations correspond to modeling
individual OPT nodes. The better results of the prefix and infix notations
compared to modeling adjacent pairs of OPT and SLT nodes show that in our soft
vector space model, modeling individual OPT nodes is better than modeling
longer paths in OPT and SLT trees.

In the MIRMU-CompuBERT system, the prefix notation has received better results
than the infix notation, showing that the topological ordering of
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
 [3]: https://github.com/MIR-MU/ARQMath-eval/tree/master/task1-votes

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-accuracy}

Math information retrieval techniques for structural and semantic matching rely
on accurate neural representations (token embeddings) of natural language words
and math symbols.

In this section, I will report the results of my experiments in learning
accurate token embeddings:

- In Section \ref{sec:variable-control-in-token-embedding-evaluation}, I will
  describe my survey of methodology in previous token embedding experiments
  and show that a lack of variable control makes some previous experiments
  difficult to reproduce.

- In Section \ref{sec:heuristical-hyperparameter-optimization-in-subword-language-models},
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

- In Section \ref{sec:joint-token-embeddings-of-text-and-math}, I will show
  that using a single language model to produce token embeddings of both
  natural language words and math symbols improves accuracy on math information
  retrieval.

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
  report state-of-the-art results on the English word analogy task with their
  model.

- @mikolov2018advances @grave2018learning introduce the fastText positional
  language model and they report state-of-the-art results on the English word
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
       @grave2018learning. For example, subwords of size 3–6 are all subwords
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
on the English word analogy task. However, the positional model has only been
evaluated on the intrinsic word analogy task and not on extrinsic tasks.

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

In 2021, we produced the fastText constrained positional model, which models
the parts of the meaning that are dependent on the narrow context of a
paragraph as well as the parts of the meaning that are fixed or dependent on
the broader context. [@novotny2021when, Section 2.2.4] We achieved this by
dividing the $D = 300$ input vector features into $D'$ narrow-context-dependent
features that would interact with positional embeddings, and the remaining $D -
D'$ fixed and broader-context-dependent features that would not interact with
positional embeddings.

We reported the optimal value of $D'$ on the English word analogy task
[@novotny2021when, Section 4.1] and we compared the subword, positional, and
constrained positional models on an English language modeling task
[@novotny2021when, Section 4.4].

#### Results

We showed that with $D = 300$ features, using $D' = 60$
narrow-context-dependent features is optimal on the English word analogy task.

We also showed that the positional model is more accurate than the subword
model and that the constrained positional model is more accurate than the
positional model on the English language modeling task.

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
technique and showed that it was more accurate than the Word2Vec language model
on the English word similarity and open-domain question answering tasks and
equally as accurate on the English word analogy task. However, word analogy
accuracies for the different categories of analogical reasoning were not
reported, only total word analogy accuracies.

#### Experiments

In 2019, I consulted the master's thesis of @stefanik2019semantic.
<!-- See also sections \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}
and \vref{sec:formal-concept-analysis-with-quantized-token-embeddings}, where I
discuss our further experiments with the quantized language model. -->
In the thesis, we aimed to reproduce the word analogy experiment of @lam2018word2bits
and analyze the word analogy accuracies for the different categories of
analogical reasoning. [@stefanik2019semantic, Section 3.1]

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

Our sense embeddings achieved state-of-the-art results on three out of five
word similarity tasks.

Compared to previous work in training sense embeddings using sense inventories
and other lexical resources, our sense embeddings achived state-of-the-art
results on five out of six word sense disambiguation tasks.

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
log-bilinear fastText subword language model on six machine translation
evaluation tasks. [@stefanik2021regressive, Section 4]

#### Results

We showed that the soft vector space model and the word mover's distance were
more accurate with our decontextualized embeddings than with the global
embeddings of a fastText model on most tasks.

#### Future Work

Future work should evaluate decontextualized embeddings on math information
retrieval.

#### Reproducibility

The experimental code for all our experiments is [available on GitHub][10]
and as [a Docker image.][11]

 [10]: https://github.com/MIR-MU/regemt
 [11]: https://hub.docker.com/r/miratmu/regemt

### Joint Token Embeddings of Text and Math
\label{sec:joint-token-embeddings-of-text-and-math}

## Ensembling Semantic Text Similarity Measures
\label{sec:ensembling-semantic-text-similarity-measures}

## Rank-Based Fusion of Math Information Retrieval Systems
\label{sec:rank-based-fusion}

## Weighted Zone Scoring Across Different Domains
\label{sec:weighted-zone-scoring}

## Approximate Nearest-Neighbor Search in Vector Spaces
\label{sec:approximate-nearest-neighbor-search-accuracy}

Until we have solved the mystery that is the human mind, the ranking criteria
of math information retrieval systems will always be just an approximation to
the relevance perceived by the users. Additionally, due to the amount of data
and time constraints, even this formal ranking criterion is usually
approximated.

In this section, I will report the results of my experiments in making
approximate math information retrieval more accurate:

- In Section \ref{sec:dense-retrieval-in-inverted-indices-accuracy}, I will
  show that dense retrieval can be approximated with almost no loss in
  accuracy.

- In Section \ref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy},
  I will show that sparse retrieval can be approximated in a way that increases
  both speed and accuracy.

### Dense Retrieval in Inverted Indices
\label{sec:dense-retrieval-in-inverted-indices-accuracy}

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy}

# Speed {#speed}
The accuracy as well as the availability of more advanced features in a math
information retrieval system are fundamentally limited by the *speed* at which
we can deploy the system, store new information in it, and recall information
that satisfy the needs of its users.

In this chapter, I will report the results of my research of representations
and techniques that enable fast math information retrieval:

- In Section \ref{sec:fast-and-reproducible-deployment}, I will describe my
  work in making math information retrieval search engines easier to build and
  deploy.

- In Section \ref{sec:representation-learning-speed}, I will describe my
  work in making neural representations of natural language words and math
  symbols both faster to learn and faster to use.

- In Section \ref{sec:approximate-nearest-neighbor-search-speed}, I will
  describe my work in developing fast approximate search techniques.

## Fast and Reproducible Deployment of Math Information Retrieval Systems Using Docker
\label{sec:fast-and-reproducible-deployment}

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-speed}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate, but also fast to train and use.

In this section, I will report the results of my experiments in learning fast
token embeddings:

- In Section \ref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic},
  I will show that quantizing the Word2Vec language model makes it possible to
  compute a fast and accurate bitwise approximation of the cosine similarity
  between two token embeddings.

- In Section \ref{sec:position-independent-token-embeddings-speed}, I will show
  that constraining the fastText positional language model improves its
  training speed.

### Quantized Token Embeddings with Fast Bitwise Arithmetic
\label{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-speed}

## Approximate Nearest-Neighbor Search in Vector Spaces
\label{sec:approximate-nearest-neighbor-search-speed}

The amount of information that a math information retrieval system must store
is increasing at an exponential rate. At the same time, users are less willing
to wait for the system to recall information. [@apica2017digital]

In this section, I will report the results of my experiments in making math
information retrieval faster by using an approximate ranking criterion:

- In Section \ref{sec:dense-retrieval-in-inverted-indices-speed}, I will
  show that approximation can greatly improve the speed of dense retrieval with
  almost no loss in accuracy.

- In Section \ref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed},
  I will show that sparse retrieval can be approximated in a way that increases
  both speed and accuracy.

### Dense Retrieval in Inverted Indices
\label{sec:dense-retrieval-in-inverted-indices-speed}

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}

# Interpretability {#interpretability}
In math information retrieval, accuracy and speed ensure that we can recall
information that a user needs when she needs them. However, the ability of a
system to explain the information to the user is fundamentally limited by the
*interpretability* of its internal representations.

In this chapter, I will report the results of my research of interpretable
representations that enable explainable information retrieval:

- In Section \ref{sec:representation-learning-interpretability}, I will
  describe my work in making neural representations of natural language words
  and math symbols more interpretable.

- In Section \ref{sec:strengths-and-weaknesses}, I will describe my work in
  explaining the roles played by different math information retrieval systems
  when using rank-based fusion and learning-to-rank techniques.

- In Section \ref{sec:interactive-visualizations}, I will describe my work in
  producing interactive visualizations of math information retrieval using
  interpretable document representations.

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-interpretability}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate, but also interpretable.

In this section, I will report the results of my experiments in learning
interpretable token embeddings:

- In Section \ref{sec:formal-concept-analysis-with-quantized-token-embeddings},
  I will show that quantizing the Word2Vec language model makes it possible to
  determine type-of relations between tokens.

- In Section \ref{sec:position-independent-token-embeddings-interpretability},
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
<!-- See also sections \vref{sec:quantized-token-embeddings}
and \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}, where I
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
