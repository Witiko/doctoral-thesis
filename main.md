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
system, including the ability to reveal relationships between different pieces of
information, to visualize the information, and to use the information as a
basis for computation. However, all these advanced features are predicated on a
much more basic and fundamental feature: the ability of the math information
retrieval system to *accurately* retrieve the information that satisfies the
need of a user.

In this chapter, I will report the results of my research of document
representations that enable accurate math information retrieval:

- In sections~\ref{sec:topological-math-representations} and
  section~\ref{sec:representation-learning-accuracy}, I will describe our new
  representation of math that we have developed for the ARQMath labs and our
  experiments in learning neural representations of natural language words and
  math symbols.

- In sections~\ref{sec:ensembling-semantic-text-similarity-measures} and
  \ref{sec:rank-based-fusion}, I will describe our work in overcoming the
  weaknesses of different information retrieval techniques by using score
  aggregation, rank-based fusion, and learning-to-rank techniques.

- In Section~\ref{sec:weighted-zone-scoring}, I will describe our work in
  dividing retrieval units into smaller zones of varying importance for the
  purpose of more accurate information retrieval.

- In Section~\ref{sec:approximate-nearest-neighbor-search-accuracy}, I will
  describe our work in making fast approximate search techniques seem accurate
  from the viewpoint of a user.

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
## Fast and Reproducible Deployment of Math Information Retrieval Systems Using Docker
## Representation Learning of Words and Symbols for Sparse Retrieval
### Quantized Word Embeddings with Fast Bitwise Arithmetic
### Constrained Positional Weighting of Word Embeddings
## Approximate Nearest-Neighbor Search in Vector Spaces
### Dense Retrieval in Inverted Indices
### Sparse Retrieval in Inverted Indices and Vector Databases
# Interpretability {#interpretability}
## Formal Concept Analysis with Quantized Word Embeddings
## Interactive Visualization of Math Information Retrieval Collections Using Semantic Text Similarity Measures
## Strengths and Weaknesses of Math Information Retrieval Systems
## Measuring Language Distances with Subword Language Models
## Position-Independent Word Embeddings
# Conclusion {#conclusion}
## Thesis Summary
### Research Questions
### Thesis Contributions
## Future Work
