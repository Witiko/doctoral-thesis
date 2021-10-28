# Introduction {#introduction}
## Problem Statement
## Research Questions
## Thesis Contributions
## Thesis Structure

# Background {#background}

In order to understand math information retrieval systems, we have to
understand their applications, the kinds of information they retrieve, the
techniques they use for the retrieval, and the different objectives that they
have to fulfill.

In this chapter, I will describe the following:

- In Section \vref{sec:digital-mathematical-libraries}, I will describe
  digital mathematical libraries as the main application of math information
  retrieval systems.

- In Section \vref{sec:math-representations}, I will discuss the presentation,
  structural, and semantic aspects of math information and show how they are
  commonly represented in math information retrieval systems.

- In Section \vref{sec:math-information-retrieval}, I will describe the math
  information retrieval techniques based on sparse data, dense data,
  structural and semantic matching, and learning to rank.

- In Section \vref{sec:objectives-and-evaluation}, I will describe the three
  major objectives of math information retrieval and how we can evaluate math
  information retrieval systems on these objectives.

## Digital Mathematical Libraries
\label{sec:digital-mathematical-libraries}

According to @bouche2008some, the first definition of a digital mathematical
library goes back to the eponymous project of the Cornell University
[@thomas2004digital] with the following ambitious vision statement:

> In light of mathematicians' reliance on their discipline's rich published
> heritage and the key role of mathematics in enabling other scientific
> disciplines, the Digital Mathematics Library strives to make the entirety of
> past mathematics scholarship available online, at reasonable cost, in the
> form of an authoritative and enduring digital collection, developed and
> curated by a network of institutions.
>
> > DML project vision, Cornell Library, 2002

← Although this ambitious vision has not been fully realized by the project,
it motivated the creation of many smaller digital mathematical libraries, some
examples of which I will discuss in sections \ref{sec:arxiv}--\ref{sec:eudml}.
In Section \vref{sec:gdml}, I will discuss the future of digital mathematical
libraries.

### ArXiv
\label{sec:arxiv}

With more than 1,000,000 mathematical documents [@tracyone2014] and counting,
[arXiv][48] is the largest open-access centralized digital mathematical
library. ArXiv has been deployed since 1991 and is now operated and maintained
by the Cornell University in Ithaca, New York, USA.

 [48]: https://arxiv.org

Documents in arXiv span the fields of mathematics, physics, astronomy,
electrical engineering, computer science, quantitative biology, statistics,
mathematical finance, and economics.

The information retrieval system of arXiv uses both the metadata of documents,
such as author names, document titles, and abstracts, and document fulltexts.

### Mini-DML
\label{sec:mini-dml}

[Mini-DML][49] is a decentralized digital mathematical library that indexes
more than 450,000 documents from fifteen national centralized digital
mathematical libraries in six European countries -- Germany, France,
Czech~Republic, Poland, Italy, Portugal -- in the United States, and in Japan.
Mini-DML has been deployed since 2008 [@bouche2004introducing] and is now
operated and maintained by the Cellule MathDoc research group at Université
Grenoble Alpes in Grenoble, France.

 [49]: http://minidml.mathdoc.fr/

The information retrieval system of Mini-DML uses only metadata of documents,
such as author names, document titles, and unique identifiers. At the time of
writing, full texts of the documents and math formulae from the
documents are not used by the system.

### European Digital Mathematical Library
\label{sec:eudml}

[The European Digital Mathematical Library (EuDML)][51] is a decentralized
digital mathematical library that indexes more than 250,000 documents from
fifteen centralized digital mathematical libraries in ten European countries:
Germany, France, Czech~Republic, Russia, Poland, Spain, Greece, Italy,
Portugal, and Bulgaria. EuDML has been deployed since
2013 [@anghelache2013eudml; @bouche2013final; @lee2013toolset;
@wojciechowski2013toolset; @wojciechowski2013eudml] and is now operated and
maintained by Uniwersytet Warszawski, Warsaw, Poland under the EuDML consortium
chaired from Grenoble, France.

The EuDML uses the Math Indexer and Searcher (MIaS) information retrieval
system,[^50] which uses metadata, full texts, and math formulae from
the indexed documents.

 [^50]: See Section \vref{sec:mias} for a description of the MIaS system.

 [51]: https://eudml.org/

### Global Digital Mathematical Library
\label{sec:gdml}

In reaction to the European Digital Mathematical library, an ambitious idea
of the Global Digital Mathematical Library (GDML) is being pursued by two
independent committees:

1. The Committee on Planning a Global Library of the Mathematical
   Sciences of the U.S National Research Council held meetings,
   conducted public data gathering sessions, and publicized GDML
   in 2012 and 2013.
   
   In 2014, the committee published a report [@cole2014developing] in which
   they described previous digital mathematical library efforts and laid out a
   roadmap towards the GDML.

2. The Committee on Electronic Information and Communication (CEIC)
   of the International Mathematical Union (IMU) organized a panel
   on the GDML [@daubechies2021arqmath] at the International Congress
   of Mathematicians (ICM) in 2014. In the same year, CEIC has assembled
   a working group, which was tasked with designing a roadmap towards the
   GDML.
   
   In 2016, the working group has co-organized a workshop titled [*Semantic
   Representation of Mathematical Knowledge*][52] held at the Fields Institute
   in Toronto, Canada, aimed at developing a language for representing the
   semantic math in the GDML.
   
   In 2018, the working group has organized a panel on GDML at the ICM.

 [52]: https://video-archive.fields.utoronto.ca/list/event/1053

Both initiatives instill hope that in the near future, the mathematicians'
dream of a comprehensive digital collection of the world's mathematics may
become a reality.

## Math Representations
\label{sec:math-representations}

Written natural languages have rules of orthography and typography, which
govern how the language is presented to the reader, syntax, which governs how
words can be combined into phrases and sentences, and semantics, which give the
words, phrases, and sentences their meaning.

Similarly, we can represent expressions in the language of mathematics using
their presentation, structural, and semantic aspects. In this section, we will
discuss each of these aspects, show examples of formal languages that are
commonly used for their representation and how they can interoperate.

### Presentation Math

Unlike in natural language writing systems, where words are typically written
in the order in which they are uttered, it is common to have math formulae
with multiple baselines and with multiple levels of nesting that may have no
apparent reading order:%
%
\begin{equation}
t' = t · \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}}
\end{equation}

Therefore, although presentation aspects of written natural text are typically
ignored in information retrieval, the placement of symbols (further known as
*presentation math*) in the language of mathematics is often useful for math
information retrieval.

In math information retrieval systems, presentation math can be represented
using the following formal languages:

% TeX, AMSTeX, LaTeX
% Unicode Math [@beeton2017unicode]

% Presentation MathML [@ausbrooks2014minermathematical, Section 3; @novotny2020three, Figure 2]
% Presentation MathML normalization

% Symbol Layout Tree [@novotny2020three, Figure 3]

### Content Math

Writing systems of natural languages can offer several alternative ways to
present one sentence using different word breaks, punctuation, and hyphenation
without changing the structure or the meaning of the sentence. Similarly, the
language of mathematics can offer several ways to present one math formula
($a\times b$) through different or historical notations ($□ ab$[^47]) without
changing its structure (further known as *content math*) or
meaning.

 [^47]: For more examples of historical mathematical notations, see
        @cajori1993history.

In math information retrieval systems, content math can be represented using
Content MathML ...

% Content MathML [@ausbrooks2014minermathematical, Section 4; @novotny2020three, Figure 2]
% LaTeXML [@stamerjohanns2008transforming]

% Operator Tree [@novotny2020three, Figure 4]

### Semantic Math

In natural languages, a word (bank) may have a different meaning depending
on its surroundings (commercial bank and river bank) and different words
may have nearly the same meaning (begin and initiate). A sentence may also have
a different meaning depending on the genre of the text, the sociocultural
setting, or even the reader themselves. Conversely, two different sentences may
have the same meaning.

Similarly, in the language of mathematics, two math formulae may
have the same structure but different meaning (further known as *semantic
math*) depending on variable bindings, the definitions of the operands, and the
underlying mathematical theory.  Conversely, two mathematical structures may
have a different structure but the same meaning ($a · b$ and $b · a$).

% CAS and proof assistants
% Examples of encoding theorems and definitions
% OpenMath [@glensbrecht2006special]
% OMDoc [@kohlhase2006omdoc], has been developed by the German mathematician and computer scientist Michael Kohlhase since 1998.
% @kamareddine2021intelligent
% https://video-archive.fields.utoronto.ca/list/event/1053 @avigad2016type

% Semantic normalization

## Math Information Retrieval
\label{sec:math-information-retrieval}

% Obrázek 8 v [Math OpenQA proposalu][58]
%
%  [58]: https://www.overleaf.com/project/615acd56fbda7f7599e9ba47

Here is a textbook definition of information retrieval:

> Information retrieval (IR) is finding material (usually documents) of
> an unstructured nature (usually text) that satisfies an information need
> from within large collections (usually stored on computers).
>
> > @manning2008introduction [Chapter 1]

*Math information retrieval* differs from this definition in several aspects:
Although math information retrieval also focuses on document retrieval, other
*retrieval units* of interest include math formulae and math statements such as
definitions, theorems, and proofs. Furthermore, the language of mathematics is
inherently more structured than natural text, which brings unique opportunities
and challenges. Finally, whereas in today's information retrieval, users
commonly express their *information needs* as queries in natural language, math
information retrieval offers more variety:

In math information retrieval, users can express information needs as queries in
natural language (further known as *text queries*), but they can also be
expressed in the language of mathematics (further known as *math queries*) or a
combination of both (further known as *mixed queries*).
However, studies have shown that up to 95\% of queries in existing math
information retrieval systems are text queries. [@libbrecht2006methods, Section
5; @miller2013three, Section 5]
This shows that techniques for *joint modeling of text and math* that allow
users to ask natural language questions about information captured in the
language of mathematics are crucially important for today's math information
retrieval.
It also shows that we may need a more expressive language for math queries
that would include elements such as *wildcards* [@altamimi2008math;
@aizawa2013ntcir; @aizawa2014ntcir; @zanibbi2016ntcir], special math symbols
that may match any other math symbol or math subformulae, and *simto regions*
[@zanibbi2016ntcir], which specify the parts of a math formula that must match
perfectly and parts that may only be similar.

In the rest of this section, I will discuss four common approaches to math
information retrieval: sparse retrieval, dense retrieval, structural and
semantic matching, and learning to rank.

### Sparse Retrieval
\label{sec:sparse-retrieval}

Over the course of their history, information retrieval systems have changed
from expert systems to everyday appliances.

In this section, I will describe the concepts of the traditional sparse
approach to information retrieval and show how they had developed since the
development of *boolean retrieval* in the 1960s.

##### Boolean Retrieval

In boolean retrieval, documents were indexed as sets of their keywords and
users would search by specifying keywords that corresponded to their
information needs using queries in prepositional calculus:
[@manning2008introduction, Chapter 1]%
%
\begin{equation}
`Computer science` ∧ (`Information retrieval` ∨ `Artificial intelligence`)
\end{equation}
%
However, only expert users could specify queries in prepositional calculus.
Additionally, boolean retrieval systems would present the users with an
*unranked* list of *all* documents that matched their query. For large document
collections, either too many documents or too few (if any) documents would
often be retrieved, a problem commonly referred to as *feast or famine*.

##### Ranked Retrieval

In 1988, @salton1988termweighting developed the *hard vector space model*.
At this point in time, digital and digitized fulltexts were becoming
increasingly more available together with the computational resources to
process them. Therefore, in addition to and instead of using keywords and
other document metadata, information retrieval systems would process document
fulltexts into lists of *tokens* that corresponded to (often *lemmatized* or
*stemmed*) words and multi-word expressions. [@manning2008introduction, Chapter
2] In the hard vector space model, documents were indexed as weighted sets
of their tokens and users would search by specifying queries in free text,
which the system would also process into weighted sets of tokens:
[@manning2008introduction, Chapter 3]%
%
\begin{equation}
`Using computers for information retrieval and artificial intelligence`
\end{equation}
%
Free text queries made information retrieval accessible to a wide range of
users. Additionally, the hard vector space model would present the users
with a *ranked* list of documents starting with the most relevant, which resolved
the *feast or famine* problem of boolean retrieval. Furthermore, advanced
techniques for token weighting such as *TF-IDF* [@salton1988termweighting] and
*BM25* [@robertson1994okapi] and for weighted zone scoring
[@manning2008introduction, Section 6.1] were later developed for the hard
vector space model, further improving its accuracy in the eyes of its users.

In concept, both documents and queries are represented as vectors in the hard
vector space model and the relevance of a document to a query is estimated
either as the *dot product* $\vec xᵀ · \vec y$ between their vectors $\vec x$
and $\vec y$, or their *cosine similarity* $\cos(\vec x, \vec y)$
[@manning2008introduction, Section 6.3]:%
%
\begin{equation}
  \cos(\vec x, \vec y) = \frac{\vec xᵀ · \vec y}{‖x‖\_2 · ‖y‖\_2}.
\end{equation}
%
In any document or query vector, most features will correspond to tokens that
are missing from the document or the query, which makes the feature values zero
and the vector extremely sparse. Therefore, information retrieval using the
hard vector space model is commonly referred to as *sparse retrieval* and
documents are indexed in a special sparse matrix data structure called the
*inverted index*.

### Dense Retrieval
\label{sec:dense-retrieval}

Despite the initial success of sparse retrieval, dense vector spaces have since
proven useful for modeling document topics and token relations and recent
advances in deep neural network language models show that *dense retrieval* has
already superseded sparse retrieval in terms of accuracy.
[@khattab2020colbert]

In this section, I will discuss two important classes of language models
and how they can be used to produce representations of tokens, sentences,
and documents for dense retrieval.

#### Shallow Log-Bilinear Language Models

After a long reign of topic modeling methods,[^53] [@deerwester1990indexing]
*shallow log-bilinear language models* emerged as a faster method for learning
continuous representations of tokens (further known as *token embeddings*),
which can be stored in a dense vector database and used to retrieve
semantically related tokens.[^54]

 [^53]: The sparse singular value decomposition used in topic modeling scales
        quadratically with the vector dimensionality, [@rehurek2011subspace, Section
        2.3] whereas shallow log-bilinear language models are linear in both
        vector dimensionality and corpus size.

 [^54]: See Section \vref{sec:semantic-matching} for a discussion of how token
        embeddings can be used to retrieve documents.

##### General Model

% První obrázek ze strany 11 [prezentace na semináři PV173][55]
%
% [55]: https://www.overleaf.com/project/6062da8447b92d21b68c5472)

In 2013, @mikolov2013efficient developed the *Word2Vec general language
model*, which learned continuous representations of tokens (further known as
*token embeddings*). As its objective, the model would predict a *masked token*
$w_t$ from its context $C\_t = w\_{t - c}$, …, $w\_{t - 1}$, $w\_{t + 1}$, …,
$w\_{t + c}$, where $c$ is the *context window size* and $w\_1, …, w\_T$ is the
corpus:%
%
\begin{equation}
  \argmin\_{\vec{\theta}} \Big[-Σ\_{t = 1}^T \log\Pr(w\_t\mid C\_t;
  \vec{\theta})\Big].
\end{equation}
%
To estimate $\Pr(w\_t\mid C\_t)$, @mikolov2013distributed used a simplified
variant of the noise contrastive approximation [@gutmann2012noise] called
negative sampling:%
%
\begin{equation}
  \Pr(w\_t\mid C\_t)=\sigma(s(w\_t, C\_t))
  \prod\_{n ∈ N\_{C\_t}}\sigma(-s(n, C\_t)),
\end{equation}
%
where $\sigma$ is the logistic function $x\mapsto \nicefrac{1}{1 + e^{-x}},$
$N\_{C\_t}$ is a set of negative examples $n$ for context $C\_t$, and $s(w\_t,
C\_t)$ is a scoring function that measures how well the masked token $w\_t$
matches the context $C\_t$:%
%
\begin{equation}
  s(w\_t, C\_t) = \vec{u}\_{C\_t}ᵀ · \vec{v}\_{w\_t},
  \vec{u}\_{C\_t} = \frac{1}{|C\_t|}Σ\_{w ∈ C\_t}\vec{u}\_{w}.
\end{equation}
%
Here, $\vec{u}\_{w} ∈\mathbb{R}^D$ is the *input vector* of the context token
$w$, $\vec{v}\_{w\_t} ∈\mathbb{R}^D$ is the output vector of the masked token
$w\_t$, $\vec{u}\_{C\_t}$ is the *context vector* and $D$ is the number of token
vector features (further known as *vector dimensionality*).

##### Subtoken Model

% Druhý obrázek ze strany 11 [prezentace na semináři PV173][55]
%
% [55]: https://www.overleaf.com/project/6062da8447b92d21b68c5472)

The general model only learns representations for tokens that are present in
the training corpus. Additionally, input vectors for different inflectional
forms of a token share no weights, which delays training convergence for
morphologically rich languages.

In 2017, @bojanowski2017enriching developed the *fastText subtoken model*,
which extended the general model by modeling subtokens instead of tokens: The
input vector $\vec{u}\_w$ for a token $w$ becomes a sum of the input vectors
$\vec{u}\_g$ for the subtokens $g ∈ G\_w$ of $w$:%
%
\begin{equation}
  \vec{u}\_w = Σ\_{g ∈ G\_w} \vec{u}\_g.
\end{equation}

##### Quantized Model

The token embeddings of the general and subtoken models occupied significant
memory and storage, and their use required expensive floating point operations,
which makes them unsuitable for resource-limited computing.

In 2018, @lam2018word2bits developed the *Word2Bits quantized model*, which
changed the general model by using quantized input vectors $\vec{u}\_w$ and
output vectors $\vec{v}\_{w\_t}$:%
%
\begin{equation}
  \vec{u}\_w = \nicefrac{1}{3} · \sign \vec{u}'\_w,
  \vec{v}\_{w\_t} = \nicefrac{1}{3} · \sign \vec{v}'\_{w\_t},
  \nabla\sign = \vec 1.
\end{equation}
%
This decreased the size of the token embeddings 16× and allowed the use of fast
bitwise arithmetic for their manipulatiob.

##### Positional Model

% Obrázek ze strany 12 [prezentace na semináři PV173][55]
%
% [55]: https://www.overleaf.com/project/6062da8447b92d21b68c5472)

In many sentences, the position of context tokens influences their syntactic
function, which is important for predicting the masked token. Additionally, if
the context $C\_t$ is large, distant tokens only introduce noise to the context
vector $\vec{u}\_{C\_t}$.

To better adapt to these situations, we would like to have separate input
vectors $\vec{u}\_{w, p}$ for different positions $p ∈ P$ of a context token
$w$:%
%
\begin{equation}
  \vec{u}\_{C\_t} = \frac{1}{|P|}Σ\_{p ∈ P}\vec{u}\_{w\_{t + p}, p}.
\end{equation}
%
Since this would increase the size of the vocabulary by a factor of $|P| = 2c$,
@mikolov2018advances developed the *fastText positional*, which adopts the
positional weighting of @mnih2013advances:%
%
\begin{equation}
  \vec{u}\_{w\_{t + p}, p} = \vec{u}\_{w\_{t + p}} ⊙ \vec{d}\_p,
\end{equation}
%
Here, $\vec{d}\_p  ∈ \mathbb{R}^{D'}, p ∈ P$ are *positional embeddings* with
$D' = D$ features and $⊙ :\mathbb{R}^D\times\mathbb{R}^D\to\mathbb{R}^D$ is
the Hadamard vector product.

#### Deep Neural Network Language Models

Deep neural network language models can be used to produce token, sentence, and
document embeddings, which can be stored in a dense vector database and used to
retrieve semantically related tokens, sentences, and documents.

##### Token Embeddings

Unlike shallow log-bilinear language models, which produce a single *global
token embedding* for every token, deep neural network language models produce
*contextual token embeddings* that depend both on the token and on the context
of its sentence. [@peters2018deep] This makes it easier to disambiguate the
specific *sense* of a possibly ambiguous token.

##### Sentence and Document Embeddings

Early neural machine translation used *encoder-decoder models*, where an
encoder recurrent neural network RNN would first read and encode a *source
sequence* into a *fixed-length context vector* and a decoder RNN would then
produce a *translated target sequence* from the context vector
[@sutskever2014sequence; @cho2014learning]. Due to the context vector's fixed
length, translation performance would deteriorate for longer sequences
[@cho2014properties]. To enable the translation of longer source sequences,
@bahdanau2014neural equipped the decoder with a *dense attention* mechanism:
Instead of having a single encoded vector for the entire source sequence, dense
attention constructed a different context vector for each target token.  Here,
the context vector would be a weighted average of the encoder’s hidden states,
where the weights would be trained to relate relevant source tokens to the
target token.

Following the success of dense attention in neural machine translation,
@cheng2016long proposed to use the dense attention directly in the
long-short-term memory (LSTM) cells of RNNs: Instead of computing the current
*memory* and *hidden state* using the previous memory and hidden state alone,
the current memory and hidden state would be computed as weighted averages of
all previous memories and hidden states. Dense attention would act as a
random-access memory mechanism, enabling the LSTM to recall long-range
memories. In 2017, @vaswani2017attention proposed the *Transformer* architecture,
which successfully replaced recurrence by the vertical stacking of attention
and which has since been shown to be a Turing-complete [@perez2019turing]
universal approximator [@yun2020transformers].

Transformer-based language models, such as Sentence BERT
[@reimers2019sentencebert] and ColBERT [@khattab2020colbert], produce
*sentence embeddings* that can be stored in a dense vector database and used
to retrieve semantically related sentences. However, since dense attention
learns weights for all pairs of source and target tokens, its space complexity
is quadratic in the source sentence length, which makes Transformer-based
language models unsuitable for the retrieval of longer documents. In reaction,
several *attention sparsification* schemes have been proposed in the literature
to enable the translation of longer source sequences by making the space
complexity linear without sacrificing the universal approximation and
Turing-completeness of Transformers [@dai2019transformerxl;
@child2019generating; @beltagy2020longformer; @zaheer2020bigbird].

### Structural and Semantic Matching
\label{sec:structural-and-semantic-matching}

Unlike natural text, the language of mathematics is inherently more structured,
which makes it possible to use *structural matching* techniques for math
information retrieval. Both information retrieval and math information
retrieval can also use *semantic matching* techniques that lie at the boundary
of sparse retrieval an dense retrieval and use token embeddings for sentence
and document retrieval.

#### Structural Matching
\label{sec:structural-matching}

To retrieve math formulae $\big(f(a + b, g(c))\big)$ and their generalizations
$\big(f(x, y)\big)$, we can use *substitution trees*. [@graf1995substitution]

To determine the pairwise structural similarity of two math formulae, we can
use distance measures defined on ordered trees such as
the *tree[^56] edit distance* [@tai1979tree; @zhang1989simple],
the *top-down distance* [@selkow1977tree; @yang1991identifying],
the *bottom-up distance* [@valiente2001efficient]
the *alignment distance* [@jiang1995alignment], and
the *isolated-subtree distance* [@tanaka1988tree].
The worst-case time complexity of some of these techniques is linear in the
tree size. Nevertheless, if constant-time retrieval in the index size is
required, it is only practical to use pairwise similarity to *rerank* a small
number of top retrieval results, not for the retrieval itself.

 [^56]: See [the website][57] of the Database Research Group at the
        \foreignlanguage{german}{Paris Lodron Universität Salzburg}
        for an overview of existing algorithms, their implementations,
        and recent literature on the tree edit distance.
 
 [57]: http://tree-edit-distance.dbresearch.uni-salzburg.at/

#### Semantic Matching
\label{sec:semantic-matching}

% [@novotny2020three, Figure 6]

To retrieve semantically similar documents, we can use the *soft vector space
model* [@sidorov2014soft]. Unlike the hard vector space model, which will only
find exact *hard matches* between tokens, the soft vector space model will also
consider fuzzy *soft matches* between related tokens. In the soft vector space
model, the relevance of a document to a query is estimated as the *soft cosine
similarity* $\softcos(\vec x, \vec y)$ between their sparse vectors $\vec x$
and $\vec y$:
%
\begin{equation}
  \softcos(\vec x, \vec y) = \frac{\vec xᵀ · S · \vec y}%
  {\sqrt{\vec xᵀ · S · \vec x} · \sqrt{\vec yᵀ · S · \vec y}}.
\end{equation}
%
where $s\_{ij}$ can be the cosine similarity $\cos(\vec i, \vec j)$ between the
global token embeddings $\vec i$ and $\vec j$ of tokens $i$ and $j$ or another
measure of semantic token relatedness. [@charlet2017simbow] Intuitively, the soft
vector space model will expand the query with related tokens and then use
cosine similarity. The worst-case time complexity of the soft cosine measure is
$\mathcal{O}(n^2)$, where $n$ is the number of unique tokens in the documents,
but it can be reduced to $\mathcal{O}(n)$ (the same as the hard vector space
model) by restricting the measures of relatedness that the soft vector space
model can use.  [@novotny2018implementation]

An alternative technique to determine the pairwise semantic similarity of two
documents is the *word mover's distance* [@kusner2015from], which will find the
*minimum-cost flow* $F$ between the sparse vectors $\vec{x}$ and $\vec{y}$ of
two documents:%
%
\begin{equation}
  \min\_F Σ\_i Σ\_j f\_{ij} · s\_{ij}
  `  subject to  ` Σ\_j f\_{ij} = x\_i, Σ\_i f\_{ij} = y\_j,
\end{equation}
%
where $S$ is a measure of token relatedness like in the soft cosine similarity.
Intuitively, the word mover's distance will match each token in one document
with the most related tokens in the other document and the word mover's
distance reflects the quality of the matches. As with the pairwise structural
similarity, it is only practical to use the word mover's distance to rerank top
retrieval results.  Additionally, the worst-case time complexity of the word
mover's distance is $\mathcal{O}(n^3 \log n)$, where $n$ is the number of
unique tokens in the documents, which makes the word mover's distance only
practical to use the word mover's distance for sentences and short documents.

### Learning to Rank
\label{sec:learning-to-rank}

Information retrieval systems make make naive assumptions about the syntax and
the semantics of the natural language and the language of mathematics. They
also often rely on token embeddings of language models, which contain
human-like biases. [@caliskan2017semantics] These assumptions and biases cause
systematic errors and affect the retrieval results.

In this section, I will describe *Learning to rank* techniques, which attempt
to remove these systematic errors and improve the ranking of results by
combining the results of several individual systems.

*Score aggregation* is a learning to rank technique that combines several
structural and semantic matching techniques and corrects their errors before
they can influence the retrieval results. An example of score aggregation
is the *CombMNZ* technique [@lee1997analyses], which combines the scores
$s\_i$ that a document $d$ received from $N\_d$ matching techniques to a
corrected score:
%
\begin{equation}
    `CombMNZScore`(d) = N\_d · Σ\_i s\_i(d)
\end{equation}

*Rank-based fusion* is a learning to rank technique that combines the results
of several information retrieval systems and corrects the results before users
can see them. An example of rank-based fusion is the *reciprocal rank fusion*
[@cormack2009reciprocal], which combines the ranks $r\_i(d)$ that a document
$d$ received from several information retrieval systems to a final score that
determines the rank of the document in the corrected results:
%
\begin{equation}
  `RRFScore`(d) = Σ\_i \frac{1}{k + r\_i(d)},
\end{equation}
%
where $k$ is a constant that determines the importance of lower-ranked
documents compared to higher-ranked documents and that is typically set to
the value $k = 60$. [@buttcher2010information, Section 11.1.2]

## Objectives and Evaluation
\label{sec:objectives-and-evaluation}

Information retrieval systems must fulfil three major objectives: the
*accuracy* of their results for the information needs of their users, the
*speed* at which they deliver the results to their users before the results
have lost their relevance, and the *interpretability* of their results, which
determines how well they can explain the information in the results to their
users.

To measure how well the systems fulfil these three objectives, we can use a
number of *evaluation measures*. In this section, I will discuss the existing
evaluation measures for each of the three objectives.

### Accuracy

An important distinction between evaluation measures of information retrieval
accuracy is whether they are *intrinsic* or *extrinsic*. Intrinsic evaluation
measures are defined on small tasks and are easily reproducible in a laboratory
setting. In contrast, extrinsic evaluation measures are defined on real-world
applications and may require the participation of a human in the loop.

#### Intrinsic Measures

To evaluate the accuracy of global token embeddings as a source of token
relatedness, we can use the *word analogy accuracy* intrinsic evaluation
measure of @mikolov2013efficient.

The word analogy accuracy uses a set of human-annotated analogies that are divided
into syntactic and semantic categories. An example analogy from a syntactic
category is \`\`Certain is to uncertain as possible is to impossible.'' An
example of an analogy from a semantic category is \`\`Man is to woman as king
is to queen.''

In the evaluation, we measure the ability of token embeddings to answer the
question \`\`Which word $b'$ is to $a'$ as $a$ is to $b$?'' To determine the
answer, we take the corresponding token embeddings $\vec{a}, \vec{b},$ and
$\vec{a'}$ and we find the token embedding $\vec{b'}$ that maximizes the cosine
similarity $\cos(\vec{a'} - \vec{a} + \vec{b}, \vec{b'})$.

The primary evaluation measure is the *total accuracy*, which is the percentage
of correct answers in all categories. The *syntactic* and *semantic accuracies*
are also sometimes reported and they correspond to the percentages of correct
answers in the different categories.

#### Extrinsic Measures

To evaluate the accuracy of an information retrieval system for the information
needs of its users, we can use extrinsic evaluation measures.

Extrinsic evaluation measures use a set of *queries* $Q$ that correspond to
information needs, a collection of retrieval units $D$ that correspond to
information, and *relevance judgements* from *human assessors* that specify
whether the information in a retrieval unit $d$ is relevant to the information
need behind query $q$.
The relevance judgements can either be *complete* and cover all combinations $Q
× D$ of queries and documents or *incomplete*, depending on the size of the
collection, and either *graded* with several levels of relevance (further known
as *gain*) or *binary*.

In the evaluation, we submit queries $Q$ to an information retrieval system
and we compare the retrieval results with the relevance judgements. To evaluate
unranked retrieval results, we can use the *precision (P)* and *recall (R)*
[@manning2008introduction, Section 8.3] evaluation measures for complete and
binary relevance judgements. To evaluate ranked retrieval results, we can use
the *precision at $k$ (P@$k$)* and *mean average precision (MAP)*
[@manning2008introduction, Section 8.4] also for complete and binary
judgements, *Bpref* [@buckley2004retrieval] for incomplete and binary
judgements, and *NDCG* for complete and graded judgements.
[@manning2008introduction, Section 8.4]

In 2008, @sakai2008information developed a technique that converts measures
for complete judgements (P, R, P@$k$, MAP, and NDCG) to measures for incomplete
judgements (P', R', P'@$k$, MAP', and NDCG') by removing all documents without
a relevance judgement from the retrieval results.

### Speed

% System Deployment Time
% Training and indexing time
% Query time

### Interpretability

% Clustering and Silhouette Score

% Formal Concept Analysis

% Usability Testing

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
\label{sec:mias}

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
notation). To give an example, the expression $x!! - y^2 = 0$ would be represented
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
systems. [@novotny2020three, Section 2.1] To give an example, the expression $x!!-
y^2 = 0$ from the previous paragraph would be represented as the following
comma-separated list of OPT nodes: `U!eq`, `O!minus`, `O!double-factorial`,
`V!x`, `O!SUP`, `V!y`, `N!2`, `N!0`.

To see if the topological ordering produced by the pre-order traversal of OPT
trees is meaningful, we have also used a representation produced by the
in-order traversal of the OPT trees (further known as the *infix* notation) and
by surrounding subtrees by parentheses. To give an example, the expression $x!! - y^2
= 0$ would be represented as the following comma-separated list of math
symbols: $($, $($, $!!$, $($, $x$, $)$, $-$, $◌^◌$, $($, $y$, $2$, $)$, $)$,
$=$, $0$, $)$, and as the following comma-separated list of OPT nodes: `(`,
`(`, `O!double-factorial`, `(`, `V!x`, `)`, `O!minus`, `O!SUP`, `(`, `V!y`,
`N!2`, `)`, `)`, `U!eq`, `N!0`, `)`.

In the *answer retrieval* math information retrieval task of the ARQMath-1 lab,
we used the prefix and infix notations in two of our primary math
information retrieval systems that jointly modeled both text and math:

- *MIRMU-SCM* [@novotny2020three, Section 4], which was based on a soft vector
  space model with token embeddings from a fastText subtoken shallow
  log-bilinear language model, and

- *MIRMU-CompuBERT* [@novotny2020three, Section 6], which was based on the sentence
  embeddings of the Sentence BERT deep neural network language model.

In both systems, we have compared the prefix and infix notations to other math
representations on our train dataset. For the *MIRMU-SCM* system, we have used
the following math representations for comparison: none (math tokens were
removed), \LaTeX{} (every math formula becomes one token), adjacent
pairs of OPT nodes, and adjacent pairs of SLT nodes. [@novotny2020three,
Section 4.3] For the *MIRMU-CompuBERT* system, we have used the \LaTeX{} math
representation for comparison. [@novotny2020three, Table 2]

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

- In Section \vref{sec:heuristical-hyperparameter-optimization-in-subtoken-language-models},
  I will report the optimal hyperparameters for the fastText subtoken language
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
analogy task of @mikolov2013efficient, which measures the ability of token
embeddings to answer the question ``Which word $b'$ is to $a'$ as $a$ is to $b$?''
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
100, n = 5 · 10^8$.

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
values: $n = 3 · 10^5,$ Unicode case-folding, and the locale of the word
analogy task.

#### Reproducibility

[The experimental code for all my experiments is available GitHub.][4] I have
also released [the only public implementation of the fastText positional model.][5]

 [4]: https://github.com/MIR-MU/reproducible-ml
 [5]: https://github.com/MIR-MU/pine

### Heuristical Hyperparameter Optimization in Subword Language Models
\label{sec:heuristical-hyperparameter-optimization-in-subtoken-language-models}

In 2017, @bojanowski2017enriching developed the fastText subtoken language model
and showed that it was more accurate than the Word2Vec language model on the
English, German, Czech, and Italian word analogy tasks. They also showed the
optimal subtoken sizes of fastText on the English and German word analogy tasks.
However, they did not optimize the subtoken sizes of fastText on the Czech and
Italian word analogy tasks and used subtokens of size 3--6[^6], which they
described as ``an arbitrary choice'' [@bojanowski2017enriching].

 [^6]: For subtoken sizes, we adopt the notation of @bojanowski2017enriching and
       @grave2018learning For example, subtokens of size 3–6 are all subtokens
       whose size is 3, 4, 5, or 6.

In 2018, @grave2018learning produced the French and Hindi word analogy tasks.
Furthermore, they also trained and publicly released fastText language models
for 157 languages.  Like @bojanowski2017enriching, they also neglected to
optimize the subtoken sizes.  Unlike @bojanowski2017enriching, they used
subtokens of size 5--5 for all languages, noting that ``using character
$n$-grams of size 5, instead of using the default range of 3--6, does not
significantly decrease the accuracy (except for Czech).'' [@grave2018learning]

#### Experiments

In 2021, we attempted to reproduce the optimal subtoken sizes of fastText
on the English (4--5) and German (6--6) word analogy tasks reported by
@bojanowski2017enriching. Additionally, we reported the optimal subtoken sizes
of fastText on the Czech, Italian, Spanish, French, Hindi, Turkish, and Russian
word analogy tasks. [@novotny2021one]

Furthermore, we produced a simple $n$-gram coverage model that can suggest
subtoken sizes for under-resourced languages, where optimal subtoken sizes are
unknown.

#### Results

We reproduced the optimal subtoken sizes of @bojanowski2017enriching and we
reported the optimal subtoken sizes on the Czech (1--4), Italian (2--5), Spanish
(5--5), French (6--6), Hindi (2--2), Turkish (3--3), and Russian (5--6) word
analogy tasks. We showed that using the optimal subtoken sizes can result in up
to 14\% improvement in word analogy accuracy.

We also showed that using the suggested subtoken sizes of the $n$-gram coverage
model was never worse than the default subtoken sizes of
@bojanowski2017enriching @grave2018learning on our test dataset, improved the
word analogy accuracy by up to 3\% compared to the default subtoken sizes, and
was within 2\% word analogy accuracy of the optimal subtoken sizes.

#### Future Work

Although the word analogy task is a convenient proxy for the accuracy of
fastText, @ghannay2016word @chiu2016intrinsic @rogers2018whats
@kuchar2021evaluation show that it does not always correlate with extrinsic end
tasks. Future work should also evaluate our $n$-gram coverage model on
extrinsic tasks.

In recent machine translation models [@vaswani2017attention], text is tokenized
into words and subtokens using word-piece [@yonghui2016google] and byte-pair
[@sennrich2016neural] models. Future work should evaluate the use of word-piece
and byte-pair models for subtoken selection in fastText.

#### Reproducibility

The experimental code for [the $n$-gram coverage model][7] is available on GitHub.

 [7]: https://github.com/MIR-MU/fasttext-optimizer

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-accuracy}

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate than the fastText subtoken language model
on the English word analogy task. However, they only evaluated the positional
model on the intrinsic word analogy task and not on extrinsic tasks.

In the subtoken model, tokens are represented by the parts of their meaning that
are *fixed* or dependent on the *broader context* that includes the
conversational setting, the time and location of an utterance, and the salient
common ground. [@bach2012context] Unlike in the positional model, they are
*not* represented by the parts of the meaning that are dependent the *narrow
context* of a paragraph, which makes the subtoken model less accurate than the
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
[@novotny2021when, Table 1] and we compared the subtoken, positional, and
constrained positional models on the English word analogy task
[@novotny2021when, figures 2 and 3] and an English language modeling task
[@novotny2021when, Section 4.4].

#### Results

We showed that with $D = 300$ features, using $D' = 60$
narrow-context-dependent features is optimal on the English word analogy task.

We also showed that the positional model is more accurate than the subtoken
model and that the constrained positional model is more accurate than the
positional model on the word analogy and language modeling tasks.[^42]

 [^41]: See Section \vref{sec:position-independent-token-embeddings-speed}
        for a discussion about the speed of the positional and constrained
        positional models. See Section
        \vref{sec:position-independent-token-embeddings-interpretability}
        for a discussion about the interpretability of the positional model.

#### Future Work

@grave2018learning used the positional language model to produce token
embeddings for 157 languages, but prior work has only shown that the positional
and constrained positional models outperform the subtoken model on English
tasks. Future work should compare the subtoken, positional, and constrained
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
are prohibitively small, a condition commonly referred to as *the knowledge
acquisition bottleneck*.

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
log-bilinear fastText subtoken language model on eight machine translation
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

In 2017, @bojanowski2017enriching developed the fastText subtoken shallow
log-bilinear language model and showed that it was more accurate than general
shallow log-bilinear language models on word analogy tasks. However,
@krstowski2018equation did not use a subtoken language model to train their
token embeddings and they did not compare their token embeddings to subtoken
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
  a fastText subtoken shallow log-bilinear language model, and

- *MIRMU-CompuBERT* [@novotny2020three, Section 6; @novotny2021ensembling, Section
  3.4], which was based on the sentence embeddings[^13] of the Sentence BERT deep
  neural network language model.

 [^12]: See Section \vref{sec:topological-math-representations} for a
        discussion about the math representation that we have developed for our
        systems.

 [^13]: The questions and answers in the answer retrieval task are short,
        which made it feasible to use the Sentence BERT model despite its
        dense attention.

#### Results

At ARQMath-1, we showed that *MIRMU-SCM* was more accurate than any other
individual information retrieval system from our MIRMU team, including
*MIRMU-MIaS* of the European Digital Mathematical Library, which is based on
the hard vector space model and does not use token embeddings. This shows that
using joint token embeddings of text and math from subtoken language models
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
human-like biases. [@caliskan2017semantics] These assumptions and biases cause
systematic errors and decrease accuracy on machine translation evaluation
tasks.

#### Experiments

In 2021, we used score aggregation to ensemble 16 diverse structural and
semantic matching techniques in order to avoid the systematic errors of any
individual technique. [@stefanik2021regressive, Section 3.3] We trained and
evaluated our ensemble on 20 machine translation evaluation tasks.

|                   |         | RegEMT | Best individual technique  |
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
: [The accuracies of the our ensemble and the best individual techniques %
   on 20 machine translation evaluation tasks]%
  {The accuracies of the our ensemble (RegEMT) and the best individual techniques %
   on 20 machine translation evaluation tasks. [@stefanik2021regressive] %
   Language pairs marked with an × were evaluated cross-lingually. %
   [@stefanik2021regressive, Section 3.4]} \label{tab:regemt}

#### Results

We showed that our ensemble was more or equally accurate than any individual
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
affect the ranking of results in information retrieval systems. As a result,
different information retrieval systems can agree on a small portion of the
most relevant documents, but each individual system will miss the great
majority of relevant documents.

In 2009, @cormack2009reciprocal developed the reciprocal rank fusion technique and
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
*MIRMU-Ensemble* (further known as *MIRMU-IBC*) and the unsupervised reciprocal
rank fusion (further known as *MIRMU-RRF*) for the *answer retrieval*
math information retrieval task of the ARQMath-2 lab. [@novotny2021ensembling,
Section 4] We used our technique to ensemble all ten systems of our MIRMU and
MSM teams.[^17]

 [^17]: We entered the ARQMath-2 lab as two teams: our lab (MIRMU) and the
        students of the FI:PV211 information retrieval course taught at the
        Masaryk University (MSM).

To see if our unsupervised techniques could benefit from a large number of
systems, we also used them to ensemble all non-baseline primary systems at the
ARQMath-1 and ARQMath-2 labs. To see if our techniques could benefit from a small
number of diverse systems,[^41] we also used them to ensemble five selected
systems out of all our ten systems at the ARQMath-2 lab.

 [^41]: See Section \vref{sec:strengths-and-weaknesses} for a discussion of how
        we selected our five diverse systems and their strengths and weaknesses.

#### Results

At ARQMath-1, we showed that *MIRMU-Ensemble* was more accurate than any
individual information retrieval system from our MIRMU team. When we used
*MIRMU-Ensemble* to ensemble all non-baseline primary systems, we received the
best accuracy in the competition, which indicates that *MIRMU-Ensemble* can
benefit from a large number of systems.

At ARQMath-2, we showed that all our four rank-based fusion techniques were
more accurate than any individual information retrieval system from our MIRMU
and MSM teams. We also showed that our *MIRMU-WIBC* can be more accurate than
*MIRMU-RRF*.

When we used the unsupervised *MIRMU-IBC* and *MIRMU-RRF* to ensemble all
non-baseline primary systems, we received the best (*MIRMU-RRF*) and the
second best (*MIRMU-IBC*) accuracies in the competition, which indicates that
both techniques can benefit from a large number of systems. Ensembling only five 
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

 /figures/godwins-law.tex

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
did not use weighted zone scoring but continuously decreased the weights of later
tokens in a thread, and one that used neither [@novotny2018weighting, Figure 3]
We showed that our system that used weighted zone scoring achieved the
state-of-the-art accuracy on both datasets, whereas our two systems that did
not use weighted zone scoring achieved worse-than-baseline accuracy on both
datasets. [@novotny2018weighting, tables 1 and 2] This shows that weighted
zone scoring can be important for information retrieval.

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
the ARQMath-1 and ARQMath-2 labs, sparse information retrieval systems still
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
        discussion about the trade-offs between speed and accuracy.

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
embeddings on six text classification tasks. [@novotny2020text, Figure 8]
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
by systems that used the BM25 term weighting scheme, whereas our soft vector
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

 /figures/word2bits-speed.tex

#### Results

We showed that our bitwise approximation can be more than 20× faster than the
float arithmetic and we also showed that our approximation was within 2\%
accuracy of the cosine similarity on the English word analogy task, see
Figure \vref{fig:quantized-token-embeddings-with-fast-bitwise-arithmetic}. This
shows that quantized token embeddings can be used for fast structural and
semantic matching.

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
and showed that it was more accurate than the fastText subtoken language model
on the English word analogy task. However, they did not release a public
implementation of the positional model or show that the model was practically
fast to train.

#### Experiments

In 2020, I implemented the positional model[^35] and I evaluated if it can
achieve better accuracy than the subtoken language model on the English word
analogy task when we trained both models for an equal amount of time.
[@novotny2020art, Table 3]

 [^35]: See Section \vref{sec:variable-control-in-token-embedding-evaluation}
        for a discussion about my implementation of the positional model.

In 2021, we developed the fastText constrained positional model,[^36] which was
less complex than the positional model and which achieved better accuracy than
the positional model on word analogy and language modeling tasks.
We compated the training time of both models. [@novotny2021when, Section 4]

 [^36]: See Section \vref{sec:position-independent-token-embeddings-accuracy}
        for a discussion about the development and the accuracy of the contrained
        positional model.

#### Results

We showed that the positional model could achieve 4\% better accuracy than the
subtoken language model on the English word analogy task when we trained both
models for five hours. We also showed that our constrained positional model
could train 2× faster than the positional model.[^41]

 [^41]: See Section \vref{sec:position-independent-token-embeddings-interpretability}
        for a discussion about the interpretability of the positional model.

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

Recent results show that dense retrieval techniques can achieve higher accuracy
than sparse retrieval techniques on information retrieval tasks. However, digital
mathematical libraries use inverted indices for sparse retrieval and replacing
them with dense vector databases has many costs.

In 2017, we showed how dense embeddings can be encoded as sparse vectors.
[@rygl2017semantic, Section 2.2] Using our encoding, we developed a dense
information retrieval system that used an inverted index as its sparse vector
database, and we showed that it could perform exact dense retrieval with almost
no loss in accuracy[^38] [@rygl2017semantic, Table 2] and that both our
encoding and our system could be configured to trade off accuracy for speed and
perform fast approximate dense retrieval [@rygl2017semantic, Figure 1;
@ruzicka2017flexible, figures 1--14] In this section, I will discuss the speed
of our system in detail.

 [^38]: See Section \vref{sec:dense-retrieval-in-inverted-indices-accuracy} for
        a discussion about the development and the accuracy of our system.

 /figures/scaletext-speed.tex

#### Experiments

We proved that our encoding improved the time complexity of the retrieval
compared to naive search and that removing unimportant features improved both
time and space complexity of the retrieval.  [@rygl2017semantic, Table 1]
We also evaluated the speed of several configurations of our encoding and our
system.

#### Results

We showed that there ways an optimal configuration of our encoding and our
system, where our system wass both fast and accurate, see Figure
\ref{fig:dense-retrieval-in-inverted-indices-speed}.

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}

State-of-the-art math information retrieval systems rely on the hard soft
vector space model of @salton1988termweighting, but the soft vector space model
of @sidorov2014soft can more accurately model both the natural language and the
language of mathematics. However, computing the similarity between two
documents in the soft vector space model had an impractical worst-case time
complexity of $\mathcal{O}(n^2)$ for a vocabulary of $n$ words and symbols.
Additionally, converting the soft vector space model to a hard vector space
model using Gaussian elimination has an impractical worst-case time complexity
of $\mathcal{O}(n^4)$.

In 2018, I developed an algorithm for the soft vector space model that could
achieve practical speed by placing restrictions on the measures of relatedness
that the soft vector space model could use. [@novotny2018implementation,
Section 3] In 2020, we showed that our restricted model was more accurate[^39]
than the non-restricted model on five out of six text classification tasks.
[@novotny2020text, Figure 8] In this section, I will discuss the speed of the
my restricted soft vector space model in detail.

 [^39]: See Section \ref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy}
        for a discussion about the development and the accuracy of the soft vector space model.

#### Experiments

In 2018, I proved that if we restricted the soft vector space model to measures
of relatedness that could be represented by a sparse similarity matrix $S$ with
no more than $C$ non-zero elements in any of its columns, where $C$ is constant,
then computing the similarity between a query and a document in the soft vector
space model had a worst-case time complexity of $\mathcal{O}(1)$ if the maximum
size of a query was constant. [@novotny2018implementation, Algorithm 1]

Additionally, I proved that if $S$ was symmetric and positive definite, the
soft vector space model could be converted to a hard vector space model using
Cholesky factorization, which had a more practical worst-case time complexity
of $\mathcal{O}(n^3)$. [@novotny2018implementation, Section 3]

Furthermore, I developed transformations that allowed the use of the soft
vector space model in vector databases and inverted indices that only supported
the hard vector space model and I proved the correctness of my transformations.
[@novotny2018implementation, Section 4]

In 2020, we compared the query time of the restricted model and the
non-restricted model on six text classification tasks. [@novotny2020text,
Figure 9]

In 2021, we evaluated the query time of the restricted model on the *answer
retrieval* math information retrieval task of the ARQMath-1 and ARQMath-2 labs.
[@novotny2020three, Table 5; @novotny2021ensembling, Figure 11]

#### Results

We showed that the restricted model was 3× slower than the non-restricted model
on the six text classification tasks and we theorized that this was because the
restricted model had been implemented using slower sparse matrix operations
whereas the non-restricted model had been implemented using faster dense matrix
operations. We offer further explanation below.

We also showed that the restricted model was the slowest in query time of all
our systems on the *answer retrieval* math information retrieval task of the
ARQMath-1 and ARQMath-2 labs.

#### Future Work

My algorithm for computing the similarity $\vec{x}ᵀ · S · \vec{y}$
between a query $\vec{x}$ and a document $\vec{y}$ in the restricted model with
$\mathcal{O}(1)$ worst-case time complexity assumed that we would compute the
similarity as a single operation. This would allow us to eliminate any index
$i$ and $j$ where $x\_i = 0, y\_i = 0,$ or $s\_{ij} = 0$ in the following
summation:%
%
\begin{equation}
\vec{x}ᵀ · S · \vec{y} = Σ\_i Σ\_j x\_i · s\_{ij} · y\_j.
\end{equation}

However, my implementation of the restricted model used the SciPy Python
library, which would separate $\vec{x}ᵀ · S · \vec{y}$ into two
operations: $\vec{x}ᵀ · S$ and $◌ · \vec{y}$, where the worst-case time
complexity of both operations was $\mathcal{O}(n)$ if the size of query
$\vec{x}$ was constant. Future work should investigate how to improve my
implementation to achieve the theoretical $\mathcal{O}(1)$ worst-case time
complexity.

#### Reproducibility

I have released [the only public implementation of the restricted soft vector
space model][24] in the free open-source Gensim NLP library
[@rehurek2010software] through a series of pull requests on GitHub.
[@novotny2018implementa; @novotny2018implementb; @novotny2020reduce;
@novotny2021use]

The experimental code for the soft vector space model on the text classification
tasks is [available on GitHub.][25]

The experimental code for the soft vector space model from the ARQMath-1 lab is
[available on GitHub.][1] The experimental code for all our systems from the
ARQMath-2 lab is [available online.][14]

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

 /figures/word2bits-implications.tex

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

We also developed two algorithms for seciding whether a feature of a token
embedding should be negated, so that its value was one if and only if the token
had the interesting property that was associated with the feature. We used our
algorithms to show the probabilities $P(i ⇒ j)$ of the implications $i ⇒ j$
between the interesting properties $i$ and $j$ associated with the token
embedding features. [@stefanik2019semantic, Section 3.7]

#### Results

We showed that for vector dimensionality $D = 30$, nouns, verbs, and
adjectives are significantly correlated with their token embedding feature.
The correlations decrease as the vector dimensionality $D$ increases and become
they insignificant for $D\geq 600$, which indicates that the part-of-speech
relation has been distributed between different token embedding features.

We show the probabilities $P(i ⇒ j)$ produced by one of our algorithms in
Figure \vref{fig:formal-concept-analysis-with-quantized-token-embeddings}.

#### Future Work

Future work should investigate whether formal concept analysis can be used to
extract type-of word relations from quantized token embeddings.

#### Reproducibility

The experimental code for all our experiments is [available online.][8]

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-interpretability}

In 2013, @mikolov2013efficient showed that token embeddings of shallow
log-bilinear language models were useful for analogical reasoning. However,
the reason for their usefulness was unclear until 2014, when @levy2014neural
showed that the token embeddings factorized the pointwise mutual information
matrix. This helped interpret token embeddings of shallow log-bilinear language
models and demystified their properties.

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate[^40] than the fastText subtoken language
model on the English word analogy task. In 2021, we showed that the positional
model was more accurate than the subtoken model on an English word analogy task.
This showed that positional embeddings played an important role in the accuracy
of shallow log-bilinear language models. However, their exact function was unclear.

 [^40]: See Section \vref{sec:position-independent-token-embeddings-accuracy}
        for a discussion about the accuracy of the positional model.

In 2021, we showed that the positional model could benefit from larger context
sizes than the subtoken language model. [@novotny2021when, Table 1] However, the
reason for this was unclear.

#### Experiments

In 2021, we developed two measures for the importance of positions and one
measure for the importance of tokens. [@novotny2021when, Section 3.4]
We used our measures to explain the role of positional embeddings in the
positional positional model. [@novotny2021when, Section 4.3]

 /figures/positional-features.tex

#### Results

We showed that the English positional model used the positional embeddings to
attenuate distant context words. [@novotny2021when, Figure 4] This shows why
the positional model could use larger context window sizes than the subtoken
language model.

We also showed that there were three distinct clusters of features in the
positional embeddings of the English positional model. We called the clusters
*antepositional*, *postpositional*, and *informational*, see Figure
\vref{fig:position-independent-token-embeddings-interpretability}.
For each cluster, we gave examples of tokens whose importance was maximized by
the positional features in the cluster:

- *antepositional*: in, for, and coca
- *positional*: ago, else, and cola
- *informational*: finance, sports, and politics

← This shows that the positional embeddings can be used to classify tokens by
their grammatical properties.

#### Future Work

Future work should inspect the positional embeddings of the positional model on
languages other than English.

#### Reproducibility

The experimental code for all our experiments is [available on GitHub][5]
together with the only public implementation of the positional model.

## Strengths and Weaknesses of Math Information Retrieval Systems
\label{sec:strengths-and-weaknesses}

In 2021, we used two unsupervised rank-based fusion[^42] techniques for the
*answer retrieval* math information retrieval task of the ARQMath-2 lab
[@novotny2021ensembling, Section 4] to ensemble all ten systems of our teams.
We were also interested if our techniques could benefit from a small number of
diverse systems. However, it was unclear how to select a small number of
diverse systems and how to interpret their diversity.

 [^42]: See Section \vref{sec:rank-based-fusion} for the discussion of the
        development and the accuracy of our rank-based fusion techniques.

#### Experiments

First, we used the correlations between the rankings of results from our
systems to cluster the systems. Then, we used the Silhouette score to select
the optimal number of clusters [@novotny2021ensembling, Figure 7] Finally,
we selected a random representative from each cluster. These representatives
were our small number of diverse systems.

To interpret the diversity of the systems, we developed two measures of their
strengths and weaknesses that showed: [@novotny2021ensembling, Figure 8]

1. how capable the systems were at mathematical and text queries and
2. how capable the systems were at long and short queries.

 /figures/strengths-and-weaknesses.tex

#### Results

We selected five diverse systems: *MIRMU-CompuBERT*, *MIRMU-SCM*, *MSM-LM*,
*MSM-MP*, and *MSM-VS*.[^43]

 [^43]: See Section \vref{sec:joint-token-embeddings-of-text-and-math}
        for a discussion of the *MIRMU-CompuBERT* and *MIRMU-SCM* systems
        and their accuracy.

We also showed that *MIRMU-CompuBERT* received the most consistent results
across both text and math queries, excelled at short queries, but its
performance deteriorated for long queries that didn't fit into the short
context window of its dense attention. By contrast, the remaining four systems
that were based on the vector space model excelled at text queries, but their
performance deteriorated for mathematical queries. *MIRMU-SCM*, which uses
techniques for structural and semantic matching, could exploit both short and
long queries. See Figure \vref{fig:strengths-and-weaknesses}.

#### Reproducibility

The experimental code for all our systems from the ARQMath-2 lab is [available
online.][14]

## Interactive Visualizations of Math Information Retrieval Collections
\label{sec:interactive-visualizations}

In 2011, @liska2011web developed the MREC dataset for the Math Information
Retrieval Workshop. In 2014, @hanel2014vizualizace developed an interactive
visualization for the exploration of the MREC dataset. However, the interactive
visualization ignored math and modeled only text.

In 2020, a Math StackExchange dataset was released for the ARQMath-1 lab. On
the *answer retrieval* math information retrieval task of ARQMath-1, we showed
that our *MIRMU-SCM* system[^44] was more accurate than any other individual
information retrieval system from our MIRMU team. Additionally, *MIRMU-SCM*
modeled both text and math. However, no interactive visualization for exploring
the Math StackExchange dataset using the *MIRMU-SCM* system had been created. 

 [^44]: See Section \vref{sec:joint-token-embeddings-of-text-and-math}
        for a discussion of the *MIRMU-SCM* systems and its accuracy.

 /figures/document-maps.tex

#### Experiments

In 2021, I led the bachelor's thesis of @petr2021document. In the thesis, we
developed an interactive visualization of the Math StackExchange dataset using
our *MIRMU-SCM* system, see Figure \vref{fig:document-maps}.

#### Results

The interactive visualization is [available online.][46]

 [46]: https://mir.fi.muni.cz/document-maps-arqmath-2021

#### Future Work

Future work should focus at usability testing of our interactive visualizations
and on the role of interpretability in the user experience.

#### Reproducibility

The code of the interactive visualization is [available on GitHub.][45]

 [45]: https://github.com/MIR-MU/document-maps

# Conclusion {#conclusion}
## Thesis Summary
### Research Questions
### Thesis Contributions
## Future Work
