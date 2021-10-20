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

- In sections~\ref{sec:topological-math-representations} and
  \ref{sec:representation-learning-accuracy}, I will describe my work in
  developing new representation of math and my experiments in learning neural
  representations of natural language words and math symbols.

- In sections~\ref{sec:ensembling-semantic-text-similarity-measures} and
  \ref{sec:rank-based-fusion}, I will describe my work in overcoming the
  weaknesses of different document representations by using score aggregation,
  rank-based fusion, and learning-to-rank techniques.

- In sections~\ref{sec:weighted-zone-scoring} and
  \ref{sec:approximate-nearest-neighbor-search-accuracy}, I will describe my
  work in making fast approximate search techniques more accurate.

## Topological Math Representations for Accurate Math Information Retrieval
\label{sec:topological-math-representations}

Although tree-structured math representations for the exchange of presentation
and content math are well-established, how to transform them into a linear
stream of tokens for math information retrieval is an open problem.

In 2019, @lample2019deep have linearized a tree-structured content math
representation using pre-order traversal, producing a topologically-orderer
sequence of tokens (also known as the prefix notation or the normal Polish
notation). To give an example, the formula $x!! - y^2 = 0$ would be represented
as the following comma-separated list of math symbols: $=$, $-$, $!!$, $x$,
$◌^◌$, $y$, $2$, $0$. Using this representation, they trained a Transformer
model to solve integrals, first-order differential equations, and second-order
differential equations with significantly better results than commercial CAS
such as Mathematica, MatLab, and Maple. Would the representation be equally
successful in math information retrieval?

### Experiments

In 2020, we have used the representation of @lample2019deep (further known as
the *prefix* notation) in our submissions to the ARQMath-1 lab to see if the
representation would also be useful for math information retrieval. Since
@lample2019deep do not specify the precise tree-structured content math
representation they used, we have used the OPT math representation in our
systems. [@novotny2020three, Section 2.1] To give an example, the formula $x!!-
y^2 = 0$ from the previous paragraph would be represented as the following
comma-separated list of OPT nodes: `U!eq`, `O!minus`, `O!double-factorial`,
`V!x`, `O!SUP`, `V!y`, `N!2`, `N!0`.

To see if the topological ordering produced by the pre-order traversal of OPT
trees is meaningful, we have also used a representation produced by the
in-order traversal of the OPT trees (further known as the *infix* notation) and
surrounding subtrees by parentheses. To give an example, the formula $x!! - y^2
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
removed), \LaTeX (every math formula becomes one token), adjacent pairs of OPT
nodes, and adjacent pairs of SLT nodes. [@novotny2020three, Section 4.3] For
the MIRMU-CompuBERT system, we have used the \LaTeX{} math representation for
comparison. [@novotny2020three, Table 2]

### Results

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
@lample2019deep is significant in deep neural language models. Surprisingly,
the \LaTeX{} representation has received better results that both prefix and
infix notations. We believe that this is due to tokenization: Unlike
@lample2019deep, we fine-tuned a language model whose tokenizer was pre-trained
on natural language text. In \LaTeX{}, commands have descriptive names in
natural language, which makes it more appropriate to use the same tokenizer
for both natural language text and math.

### Future Work

The state-of-the-art math information retrieval systems of Approach0 and
Tangent-L used hard vector space models to model paths in OPT and SLT trees,
respectively. This indicates that the failure of our soft vector space model
to exploit longer paths in OPT and SLT trees may be caused by insufficient
capacity of our fastText subword language model. Future experiments should
test this hypothesis by performing an end-to-end hyperparameter optimization
of fastText using relevance judgements from the ARQMath-1 and -2 labs.

### Reproducibility

The experimental code for both [MIRMU-SCM][1], [MIRMU-CompuBERT][2], and their
[hyperparameter optimization on the train dataset][3] is available online.

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

- In Section~\ref{sec:variable-control-in-token-embedding-evaluation}, I will
  describe my survey of methodology in previous token embedding experiments
  and show that a lack of variable control makes some previous experiments
  difficult to reproduce.

- In Section~\ref{sec:heuristical-hyperparameter-optimization-in-subword-language-models},
  I will report the optimal hyperparameters for the fastText subword language
  model on nine word analogy tasks. I will also show that expensive
  hyperparameter optimization can be replaced by a simple n-gram coverage model
  that consistently improves the accuracy compared to the default
  hyperparameters.

- In sections~\ref{sec:position-independent-token-embeddings-accuracy} through
  \ref{sec:decontextualized-token-embeddings}, I will show that constraining,
  quantization, disambiguation, and decontextualization of token word embeddings
  improves their accuracy on word analogy, word similarity, language modeling,
  part-of-speech tagging, and machine translation evaluation tasks.

- In Section~\ref{sec:joint-token-embeddings-of-text-and-math}, I will show
  that using a single language model to produce token embeddings of both
  natural language words and math symbols improves accuracy on math information
  retrieval.

### Variable Control in Token Embedding Evaluation
\label{sec:variable-control-in-token-embedding-evaluation}

### Heuristical Hyperparameter Optimization in Subword Language Models
\label{sec:heuristical-hyperparameter-optimization-in-subword-language-models}

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-accuracy}

### Quantized Token Embeddings
\label{sec:quantized-token-embeddings}

### Autoregressive Word Sense Disambiguation
\label{sec:autoregressive-word-sense-disambiguation}

### Decontextualized Token Embeddings
\label{sec:decontextualized-token-embeddings}

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
of math information retrieval system will always be just an approximation to
the relevance perceived by the users. Additionally, due to the amount of data
and time constraints, even this formal ranking criterion is usually
approximated.

In this section, I will report the results of my experiments in making
approximate math information retrieval more accurate:

- In Section~\ref{sec:dense-retrieval-in-inverted-indices-accuracy}, I will
  show that dense retrieval can be approximated with almost no loss in
  accuracy.

- In Section~\ref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy},
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

- In Section~\ref{sec:fast-and-reproducible-deployment}, I will describe my
  work in making math information retrieval search engines easier to build and
  deploy.

- In Section~\ref{sec:representation-learning-speed}, I will describe my
  work in making neural representations of natural language words and math
  symbols both faster to learn and faster to use.

- In Section~\ref{sec:approximate-nearest-neighbor-search-speed}, I will
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

- In Section~\ref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic},
  I will show that quantizing the Word2Vec language model makes it possible to
  compute a fast and accurate bitwise approximation of the cosine similarity
  between two token embeddings.

- In Section~\ref{sec:position-independent-token-embeddings-speed}, I will show
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

- In Section~\ref{sec:dense-retrieval-in-inverted-indices-speed}, I will
  show that approximation can greatly improve the speed of dense retrieval with
  almost no loss in accuracy.

- In Section~\ref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed},
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

- In Section~\ref{sec:representation-learning-interpretability}, I will
  describe my work in making neural representations of natural language words
  and math symbols more interpretable.

- In Section~\ref{sec:strengths-and-weaknesses}, I will describe my work in
  explaining the roles played by different math information retrieval systems
  when using rank-based fusion and learning-to-rank techniques.

- In Section~\ref{sec:interactive-visualizations}, I will describe my work in
  producing interactive visualizations of math information retrieval using
  interpretable document representations.

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-interpretability}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate, but also interpretable.

In this section, I will report the results of my experiments in learning
interpretable token embeddings:

- In Section~\ref{sec:formal-concept-analysis-with-quantized-token-embeddings},
  I will show that quantizing the Word2Vec language model makes it possible to
  determine hyperonymy and hyponymy relations between tokens.

- In Section~\ref{sec:position-independent-token-embeddings-interpretability},
  I will show that the fastText positional language model makes it possible to
  determine grammatical properties of tokens.

### Formal Concept Analysis with Quantized Token Embeddings
\label{sec:formal-concept-analysis-with-quantized-token-embeddings}

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
