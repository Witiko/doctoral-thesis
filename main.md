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
