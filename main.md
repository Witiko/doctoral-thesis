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
### Structural and Semantic matching
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
  \ref{sec:representation-learning-accuracy}, I will describe our new
  representation of math that we have developed for the ARQMath labs and our
  experiments in learning neural representations of natural language words and
  math symbols.

- In sections~\ref{sec:ensembling-semantic-text-similarity-measures} and
  \ref{sec:rank-based-fusion}, I will describe our work in overcoming the
  weaknesses of different document representations by using score aggregation,
  rank-based fusion, and learning-to-rank techniques.

- In sections~\ref{sec:weighted-zone-scoring} and
  \ref{sec:approximate-nearest-neighbor-search-accuracy}, I will describe our
  work in making fast approximate search techniques more accurate.

## Topological Math Representations for Accurate Math Information Retrieval
\label{sec:topological-math-representations}

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-accuracy}

### Variable Control in Word Embedding Evaluation
### Heuristical Hyperparameter Optimization in Subword Language Models
### Position-Independent Word Embeddings
### Quantized Word Embeddings
### Autoregressive Word Sense Disambiguation
### Decontextualized Word Embeddings
### Joint Word Embeddings of Text and Math

## Ensembling Semantic Text Similarity Measures
\label{sec:ensembling-semantic-text-similarity-measures}

## Rank-Based Fusion of Math Information Retrieval Systems
\label{sec:rank-based-fusion}

## Weighted Zone Scoring Across Different Domains
\label{sec:weighted-zone-scoring}

## Approximate Nearest-Neighbor Search in Vector Spaces
\label{sec:approximate-nearest-neighbor-search-accuracy}

### Dense Retrieval in Inverted Indices
### Sparse Retrieval in Inverted Indices and Vector Databases

# Speed {#speed}
The accuracy as well as the availability of more advanced features in a math
information retrieval system are fundamentally limited by the *speed* at which
we can deploy the system, store new information in it, and recall information
that satisfy the needs of its users.

In this chapter, I will report the results of my research of representations
and techniques that enable fast math information retrieval:

- In Section~\ref{sec:fast-and-reproducible-deployment}, I will describe our
  work in making math information retrieval search engines easier to build and
  deploy.

- In Section~\ref{sec:representation-learning-speed}, I will describe our
  work in making neural representations of natural language words and math
  symbols both faster to learn and faster to use.

- In Section~\ref{sec:approximate-nearest-neighbor-search-speed}, I will
  describe our work in developing fast approximate search techniques.

## Fast and Reproducible Deployment of Math Information Retrieval Systems Using Docker
\label{sec:fast-and-reproducible-deployment}

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-speed}

### Quantized Word Embeddings with Fast Bitwise Arithmetic
### Constrained Positional Weighting of Word Embeddings

## Approximate Nearest-Neighbor Search in Vector Spaces
\label{sec:approximate-nearest-neighbor-search-speed}

### Dense Retrieval in Inverted Indices
### Sparse Retrieval in Inverted Indices and Vector Databases

# Interpretability {#interpretability}
In math information retrieval, accuracy and speed ensure that we can recall
information that a user needs when she needs them. However, the ability of a
system to explain the information to its users is fundamentally limited by the
*interpretability* of its internal representations.

In this chapter, I will report the results of my research of interpretable
representations that enable explainable information retrieval:

- In Section~\ref{sec:representation-learning-interpretability}, I will
  describe our work in making neural representations of natural language words
  and math symbols more interpretable.

- In Section~\ref{sec:strengths-and-weaknesses}, I will describe our work in
  explaining the roles played by different math information retrieval systems
  when using rank-based fusion and learning-to-rank techniques.

- In Section~\ref{sec:interactive-visualizations}, I will describe our work in
  producing interactive visualizations of math information retrieval using
  interpretable document representations.

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-interpretability}

### Formal Concept Analysis with Quantized Word Embeddings
### Measuring Language Distances with Subword Language Models
### Position-Independent Word Embeddings

## Strengths and Weaknesses of Math Information Retrieval Systems
\label{sec:strengths-and-weaknesses}

## Interactive Visualizations of Math Information Retrieval Collections
\label{sec:interactive-visualizations}

# Conclusion {#conclusion}
## Thesis Summary
### Research Questions
### Thesis Contributions
## Future Work
