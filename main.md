# Acknowledgements {#thanks}

Throughout the writing of this dissertation I have received a great deal of
support and assistance.

I would first like to express my most sincere gratitude to my advisor,
Petr Sojka, for sharing his impeccable expertise, which was invaluable
in formulating my research questions and the methods of my research. Your
thought-provoking insights have improved my dissertation in too many ways to
count.

I would also like to acknowledge the South Moravian Centre for International
Mobility (JCMM), who financially supported my research through the Brno Ph.D.
talent project and organized events where I could learn new skills and meet my
colleagues, and the Ministry of Education, Youth, and Sports of the Czech
Republic, who supplied me with computational resources through the
\`\`e-Infrastruktura CZ'' (e-INFRA CZ LM2018140) grant project.

In addition, I am grateful to Radim Řehůřek, Gordon Mohr, and Michael Penkov
from the Gensim project. You volunteered your time to help me translate my
ideas into code and become a better programmer.

I would also like to offer special thanks to my colleagues from the math
information retrieval (MIR-MU) research group for their wonderful
collaboration. I would like to single out Michal Růžička, who was always
willing to help me with the management of computational resources, and
Michal Štefánik, who would never refuse to discuss research ideas.

I would like to thank my parents and grandparents for their wise counsel and
sympathetic ear. You are always there for me.

Finally, I could not have completed this dissertation without the support of
Marie, Lída, Monika, and Anička, who looked after Samuel when I was at work
and who provided stimulating discussions as well as happy distractions to rest
my mind outside of my research.

# Abstract {#abstract}

Mathematicians dream of a digital library of all peer-reviewed math literature
ever published. Math information retrieval emerged when it became clear that
standard information retrieval techniques used for text documents were
inadequate to retrieve math documents from digital mathematical libraries.

Math information retrieval systems use advanced representations of math to
ensure fast and accurate retrieval. However, they rarely focus on the
interpretability of their representations, which makes the retrieval results
difficult to explain to users.

In my thesis, I focus on representations and techniques that can make math
information retrieval systems more interpretable: token embeddings, semantic
matching techniques, and learning-to-rank techniques. The aim of my thesis
is to determine whether these techniques can also make math information
retrieval systems more accurate and keep them reasonably fast.

I show that semantic matching techniques based on token embeddings and
learning-to-rank techniques can improve the accuracy of math information
retrieval systems and can achieve the same speed as non-interpretable systems.
I also show the tangible benefits of interpretability.

# Abstrakt {#abstract-cs}

Matematici sní o digitální knihovně s veškerou světovou matematickou
literaturou. Matematické vyhledávání vzniklo v reakci na tento sen a zaobírá se
technikami pro vyhledávání matematických textů v digitálních matematických
knihovnách.

Matematické vyhledávače využívají pokročilé reprezentace matematiky pro
zajištění rychlého a přesného vyhledávání. Zřídkakdy se však zaměřují na
interpretovatelnost svých reprezentací, čímž činí výsledky svého vyhledávání
hůře vysvětlitelnými.

Ve své práci se zaměřuji na reprezentace a techniky, které mohou zvýšit
interpretovatelnost matematického vyhledávání: slovní embedingy a techniky pro
hledání sémantické shody a agregaci výsledků. Cílem mé práce je určit, jestli
tyto reprezentace a techniky mohou zvýšit přesnost matematického vyhledávání
při zachování praktické rychlosti.

Ukazuji, že techniky pro hledání sémantické shody na základě slovních embedingů
a techniky pro agregaci výsledků mohou zvýšit přesnost matematického
vyhledávání a mohou dosáhnout stejné rychlosti jako neinterpretovatelné
techniky. Ilustruji také praktické výhody interpretovatelnosti.

# Introduction {#introduction}

Mathematicians dream of a digital library of all peer-reviewed math literature
ever published. [@sojka2010towardsb] However, although historical math
literature amounts to less than 100 million pages [@trott2016overview],
only a small portion has ever been digitized[^80] [@bouche2008some, Section 2].
Additionally, the amount of math literature doubles approximately every 16
years [@trott2016overview], which makes it increasingly more difficult to
organize.

 [^80]: \`\`The amount of resources available to digitize mathematics is
        significantly less than the resources available to digitize cats.''
        [@watt2016semantic]

Math information retrieval emerged in 2012 when it became clear that standard
information retrieval techniques used for text documents were inadequate to
retrieve math documents from digital mathematical libraries. Math information
retrieval systems use optical character recognition techniques to extract
information from historical math documents and advanced representations of math
to ensure fast and accurate retrieval.

↑ However, math information retrieval systems rarely focus on the
interpretability of their representations, which makes the retrieval results
difficult to explain. Studies have shown that up to 95\% of queries in existing
math information retrieval systems are simple text queries.
[@libbrecht2006methods; @miller2013three] This shows that advanced queries
produce results that are confusing to the users.

## Research Questions

In my research, I focused on representations and techniques that can make math
information retrieval systems more interpretable, namely:

- *token embeddings*, which can represent semantic relationships between text
  words and math symbols and which can be used with *semantic matching*
  techniques to explainably retrieve mathematical documents and

- *learning-to-rank* techniques, which can combine several interpretable
  math information retrieval systems to produce a more accurate retrieval
  system without any loss in interpretability.

← I aimed to answer three research questions:

1. Can semantic matching techniques that use token embeddings improve the
   accuracy of math information retrieval systems?

2. Can semantic matching techniques based on token embeddings be reasonably
   fast for math information retrieval systems?

3. Can learning-to-rank techniques consistently improve accuracy compared to
   individual math information retrieval systems?

## Thesis Structure

In Chapter \vref{cha:background}, I will describe the background of math
information retrieval, including different kinds of token embeddings in
Section \vref{sec:shallow-models}, semantic matching techniques in Section
\vref{sec:semantic-matching}, and learning-to-rank techniques in Section
\vref{sec:learning-to-rank}.

In Chapter \vref{cha:state-of-the-art}, I will describe math information
retrieval competitions and state-of-the-art math information retrieval systems
that achieved the best accuracy in the competitions.

In Chapter \vref{cha:accuracy}, I will report the results of my research on
representations that enable accurate math information retrieval. I will also
answer research questions 1 and 3.

In Chapter \vref{cha:speed}, I will report the results of my research on
representations that enable fast math information retrieval. I will also answer
research question 2.

In Chapter \vref{cha:interpretability}, I will report the results of my
research on interpretable representations that enable explainable math
information retrieval and I will show some tangible benefits of
interpretability.

I will conclude in Chapter \vref{cha:conclusion} by summarizing the results of
my research with respect to my research questions. I will also offer directions
for future work.

# Background {#background}
\label{cha:background}

In order to understand math information retrieval systems, we have to
understand their applications, the kinds of information they retrieve, the
techniques they use for the retrieval, and the different objectives that they
have to fulfill.

↑ In this chapter, I'll describe the background of math information retrieval:

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
library goes back to the project of the Cornell University [@thomas2004digital]
with the following vision statement:

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

The information retrieval system of arXiv uses both the metadata of documents
(such as author names, document titles, and abstracts) and document fulltexts.

### Mini-DML
\label{sec:mini-dml}

[Mini-DML][49] is a decentralized digital mathematical library that indexes
more than 450,000 documents from fifteen national centralized digital
mathematical libraries in six European countries (Germany, France,
Czech~Republic, Poland, Italy, Portugal), in the United States, and in Japan.
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
of a Global Digital Mathematical Library (GDML) is being pursued by two
independent committees:

1. The Committee on Planning a Global Library of the Mathematical
   Sciences of the US National Research Council held meetings,
   conducted public data gathering sessions, and publicized GDML
   in 2012 and 2013.
   
   In 2014, the committee published a report [@cole2014developing] in which
   they described previous digital mathematical library efforts and laid out a
   roadmap towards the GDML.↓

2. The Committee on Electronic Information and Communication (CEIC)
   of the International Mathematical Union (IMU) organized a panel
   on the GDML [@daubechies2021arqmath] at the International Congress
   of Mathematicians (ICM) in 2014. In the same year, CEIC has assembled
   a working group, which was tasked with designing a roadmap towards the
   GDML.
   
   In 2016, the working group has co-organized a [*Semantic Representation of
   Mathematical Knowledge*][52][][53] workshop held at the Fields Institute in
   Toronto, Canada, aimed at developing a language for representing math in the
   GDML.
   
   In 2018, the working group has organized a panel on GDML at the ICM.

 [52]: https://video-archive.fields.utoronto.ca/list/event/1053
 [53]: https://youtu.be/psSyM1zp82k

Both initiatives give hope that in the near future, the mathematicians' dream
of a comprehensive digital collection of the world's mathematics may become a
reality.

## Math Representations
\label{sec:math-representations}

Written natural languages have rules of orthography and typography, which
govern how the language is presented to the reader, syntax, which governs how
words can be combined into phrases and sentences, and semantics, which give the
words, phrases, and sentences their meaning.

Similarly, we can represent expressions in the language of mathematics using
their presentation aspects, structural aspects, and semantic aspects. In this
section, I will discuss each of these aspects and show examples of formal
languages that are commonly used for their representation.

### Presentation Math

📖{presentation math|first}

Unlike in natural language writing systems, where words are typically written
in the order in which they are uttered, it is common to have math formulae
with multiple baselines and with multiple levels of nesting that may have no
apparent reading order:%
%
\begin{equation}
t' = t · \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}}
\end{equation}
%
Therefore, although presentation aspects of written natural text are typically
ignored in information retrieval, the placement of symbols (further known as
*presentation math*) in the language of mathematics is often useful for math
information retrieval.

In the rest of this section, I will describe languages that can be used to
represent presentation math in math information retrieval systems.

 /figures/pmml-and-cmml.tex

``` dot %
    [Symbol Layout Tree representation]%
    {Symbol Layout Tree (SLT) representation of the math formula $x!! - y^2 = 0$. %
     Edge labels n and a stand for next and above. %
     \cite[Figure 3]{novotny2020three}%
     \label{fig:slt}}

digraph tree {
  node [fontname = "times"];
  edge [fontname = "times"];

  margin  = 0;
  rankdir = "LR";

  x -> doublefactorial [label = "n"];
  doublefactorial -> minus [label = "n"];
  minus -> y [label = "n"];
  y -> two [label = "a"];
  y -> equals [label = "n"];
  equals -> zero [label = "n"];

  x [label = "V!x"];
  doublefactorial [label = "!!"];
  minus [label = "–"];
  y [label = "V!y"];
  equals [label = "="];
  zero [label = "N!0"];
  two [label = "N!2"];
}

```

``` dot %
    [Operator Tree representation]%
    {↑ Operator Tree (OPT) representation of the math formula $x!! - y^2 = 0$. %
     Edge labels 0 and 1 stand for first and second operands. %
     \cite[Figure 4]{novotny2020three}%
     \label{fig:opt}}

digraph tree {
  node [fontname = "times"];
  edge [fontname = "times"];

  margin  = 0;
  rankdir = "LR";

  equals -> minus [label = "0"];
  equals -> zero [label = "1"];
  minus -> doublefactorial [label = "0"];
  doublefactorial -> x [label = "0"];
  minus -> sup [label = "1"];
  sup -> y [label = "0"];
  sup -> two [label = "1"];

  x [label = "V!x"];
  doublefactorial [label = "O!double-factorial"];
  minus [label = "O!minus"];
  y [label = "V!y"];
  sup [label = "O!SUP"];
  equals [label = "U!eq"];
  zero [label = "N!0"];
  two [label = "N!2"];
}

```

##### \TeX{}

📖{TeX@\TeX{}|first}

Since 1977, @knuth86tex has been developing the programming language of \TeX{}
to author mathematical documents with precise presentation requirements. At the
present time, \TeX{} is the most popular authoring language for mathematical
documents.  To give an example, the formula $x!! - y^2 = 0$ can be represented
as `$x!! - y^2 = 0$` in \TeX.

##### Presentation MathML

In 1998,[^59] the World Wide Web Consortium developed the Presentation MathML
XML language, which \`\`can be used to describe the layout structure of
mathematical notation.'' [@buswell1998mathematical, Section 3.1]
To give an example, Figure \vref{fig:pmml-and-cmml} shows how the formula $x!!
- y^2 = 0$ can be represented in Presentation MathML.

 [^59]: Presentation MathML can be extracted from \TeX{} by [\LaTeX ML.][60]
        [@stamerjohanns2010transforming]

 [60]: https://dlmf.nist.gov/LaTeXML

In Presentation MathML, visually equivalent math formulae may receive different
representations, which affects retrieval accuracy. Therefore, the problem of
*presentation normalization* has received a lot of attention in the literature.
[@archambault2006canonical; @formanek2012normalization; @altamimi2007more;
@miner2007approach, Section 3]

##### Symbol Layout Tree

📖{Symbol Layout Tree (SLT)|first}

In 2016,[^61] @zanibbi2016multi developed the *symbol layout tree (SLT)*
as a simplified representation of Presentation MathML that can be directly
used for information retrieval without further normalization.
To give an example, Figure \vref{fig:slt} shows how the formula $x!!
- y^2 = 0$ can be represented in the SLT.

 [^61]: SLT can be extracted from Presentation MathML using the [Tangent-S][62]
        system. [@davila2017layout]

 [62]: https://github.com/MIR-MU/TangentCFT (module TangentS)

##### Unicode Math

In 2017, the American Mathematical Society developed Unicode support for
Mathematics [@beeton2017unicode], which can be used to used to encode
presentation math in plain text without special markup.

### Content Math

📖{content math|first}

Writing systems of natural languages can offer several alternative ways to
present one sentence using different word breaks, punctuation, and hyphenation
without changing the structure or the meaning of the sentence. Similarly, the
language of mathematics can offer several ways to present one math formula
($a\times b$) through different or historical notations ($□ ab$[^47]) without
changing its structure (further known as *content math*) or
meaning.

 [^47]: For more examples of historical mathematical notations, see
        @cajori1993history.

In the rest of this section, I will describe languages that can be used to
represent content math in math information retrieval systems.

##### Content MathML

Since 1998,[^63] the World Wide Web Consortium has been developing the Content
MathML XML language, which can provide \`\`an explicit encoding of the
*underlying mathematical meaning* of an expression, rather than any particular
rendering for the expression'' [@buswell1998mathematical, Section 4.1.1]
To give an example, Figure \vref{fig:pmml-and-cmml} shows how the formula $x!!
- y^2 = 0$ can be represented in Content MathML.

 [^63]: Content MathML can be extracted from \TeX{} by [\LaTeX ML.][60]
        [@stamerjohanns2010transforming]

##### Operator Tree

📖{Operator Tree (OPT)|first}

↑ In 2017,[^64] @davila2017layout developed the *operator tree (OPT)*
as a simplified representation of Content MathML that can be directly
used for information retrieval.
To give an example, Figure \vref{fig:opt} shows how the formula $x!!
- y^2 = 0$ can be represented in the OPT.

 [^64]: OPT can also be extracted from Presentation MathML using the
        Tangent-S system.

### Semantic Math

📖{semantic math|first}

In natural languages, a word (bank) may have a different meaning depending
on its surroundings (commercial bank and river bank) and different words
may have nearly the same meaning (begin and start). A sentence may also have
a different meaning depending on the genre of the text, the sociocultural
setting, or even the reader themselves. Conversely, two different sentences may
have the same meaning.

Similarly, in the language of mathematics, two math formulae may
have the same structure but different meaning (further known as *semantic
math*) depending on variable bindings, the definitions of the operands, and the
underlying mathematical theory.  Conversely, two mathematical structures may
have a different structure but the same meaning ($a · b$ and $b · a$).

In the rest of this section, I will describe languages[^65] that can be used to
represent semantic math in math information retrieval systems.

 [^65]: For more information about languages for semantic math representation,
        see recent proceedings of the *International Conference of Intelligent
        Computer Mathematics (CICM)* [@kamareddine2021intelligent], or talks
        from the *Semantic Representation of Mathematical Knowledge*
        workshop (see Section \vref{sec:eudml}).

##### Computer Algebra Systems and Proof Assistants

📖{computer algebra system (CAS)|first}
📖{Mathematica|first}
📖{MatLab|first}
📖{Maple|first}

*Computer algebra systems* are general-purpose tools that can perform symbolic
rather than numeric computation. Examples of computer algebra systems include
*Mathematica*, *MatLab*, and *Maple*. *Proof assistants* are more focused tools
that can assist with the development of formal proofs.

##### OpenMath

Since 1990, the OpenMath Society has been developing the *OpenMath* XML
language, which can represent \`\`semantically rich mathematical
objects''. To represent the potentially infinite number of math symbols from
different math theories, *OpenMath* uses third-party *content directories*[^66]
that define math symbols and their meaning.

 [^66]: See [the web site][67] of the OpenMath society for a curated list of
        existing content directories.

 [67]: http://openmath.org/cdnames/

In OpenMath, semantically equivalent math formulae may receive different
representations, which affects retrieval accuracy. The problem of
*semantic normalization* has received some attention in the literature
[@normann2007extended, Section 2; @gauthier2014matching;
@delahaye1999information], but it is generally undecidable by Gödel's first
incompleteness theorem.

##### OMDoc

Since 2000, @kohlhase2000omdoc has been developing the *OMDoc* XML language,
which uses a combination of Content MathML and OpenMath to represent math
formulae and adds its own support for *math statements* such as definitions,
theorems, and proofs and for *math theories* that serve as an advanced
alternative to OpenMath content directories.

## Math Information Retrieval
\label{sec:math-information-retrieval}

 /figures/math-information-retrieval.tex

The following is a textbook definition of information retrieval:

> Information retrieval (IR) is finding material (usually documents) of
> an unstructured nature (usually text) that satisfies an information need
> from within large collections (usually stored on computers).
>
> > @manning2008introduction [Chapter 1]

📖{retrieval unit|first}
📖{information need|first}

*Math information retrieval* differs from this definition in several aspects:
Although math information retrieval also focuses on document retrieval (see
Figure \vref{fig:math-information-retrieval}), other *retrieval units* of
interest include math formulae and math statements such as definitions,
theorems, and proofs. Furthermore, the language of mathematics is inherently
more structured than natural text, which brings unique opportunities and
challenges. Finally, whereas in today's information retrieval, users commonly
express their *information needs* as queries in natural language, math
information retrieval offers more variety.

📖{mixed query|first}
📖{math query|first}
📖{text query|first}
📖{joint modeling|first}
📖{wildcard|first}
📖{simto region|first}

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
from systems for experts to everyday appliances.

In this section, I will describe the concepts of the traditional sparse
approach to information retrieval and show how they had developed since the
development of *Boolean retrieval* in the 1960s.

##### Boolean Retrieval

In Boolean retrieval, documents were indexed as sets of their keywords and
users would search by specifying keywords that corresponded to their
information needs using queries in prepositional calculus:
[@manning2008introduction, Chapter 1]%
%
\begin{equation}
`Computer science` ∧ (`Information retrieval` ∨ `Artificial intelligence`)
\end{equation}
%
However, only expert users could specify queries in prepositional calculus.
Additionally, Boolean retrieval systems would present the users with an
*unranked* list of *all* documents that matched their query. For large document
collections, either too many documents or too few (if any) documents would
often be retrieved, a problem commonly referred to as *feast or famine*.

##### Ranked Retrieval

📖{tokenization|first}
📖{hard vector space model|first}

In 1975, @salton1975vector developed the *vector space model*
(further known as the *hard vector space model*).
At this point in time, digital and digitized fulltexts were becoming
increasingly more available together with the computational resources to
process them. Therefore, in addition to and instead of using keywords and
other document metadata, information retrieval systems would process document
fulltexts into lists of *tokens* (further known as *tokenization*) that
corresponded to (often *lemmatized* or *stemmed*) words and multi-word
expressions. [@manning2008introduction, Chapter 2] In the hard vector space
model, documents were indexed as weighted sets of their tokens and users would
search by specifying queries in free text, which the system would also process
into weighted sets of tokens: [@manning2008introduction, Chapter 3]%
%
\begin{equation}
`Using computers for information retrieval and artificial intelligence`
\end{equation}
%
📖{TF-IDF|first}%
📖{BM25|first}%
📖{weighted zone scoring|first}%
%
Free text queries made information retrieval accessible to a wide range of
users. Additionally, the hard vector space model would present the users
with a *ranked* list of documents starting with the most relevant, which resolved
the *feast or famine* problem of Boolean retrieval. Furthermore, advanced
techniques for *term weighting* such as *TF-IDF* [@salton1975vector],
*BM25* [@robertson1994okapi], and *BM25⁺* [@lv2011lower] and for *weighted zone
scoring* [@manning2008introduction, Section 6.1] were later developed for the
hard vector space model, further improving its accuracy.

📖{cosine similarity|first}
📖{similarity score|first}

In concept, both documents and queries are represented as vectors in the hard
vector space model and the relevance of a document to a query is estimated
by a *similarity score*, which is either the *dot product* $\vec xᵀ · \vec y$
between their vectors $\vec x$ and $\vec y$, or their *cosine similarity*
$\cos(\vec x, \vec y)$ [@manning2008introduction, Section 6.3]:%
%
\begin{equation}
  \cos(\vec x, \vec y) = \frac{\vec xᵀ · \vec y}{‖x‖\_2 · ‖y‖\_2}.
\end{equation}
%
📖{sparse retrieval|first}%
📖{inverted index|first}%
%
In any document or query vector, most features will correspond to tokens that
are missing from the document or the query, which makes the feature values zero
and the vector extremely sparse. Therefore, information retrieval using the
hard vector space model is commonly referred to as *sparse retrieval* and
documents are indexed in a special sparse matrix data structure called the
*inverted index*.

### Dense Retrieval
\label{sec:dense-retrieval}

📖{dense retrieval|first}

Despite the initial success of sparse retrieval, dense vector spaces have since
proven useful for modeling document topics and token relations and recent
advances in deep neural network language models show that *dense retrieval* has
already superseded sparse retrieval in terms of accuracy.
[@khattab2020colbert]

In this section, I will discuss two important classes of language models
and how they can be used to produce representations of tokens, sentences,
and documents for dense retrieval.

#### Shallow Log-Bilinear Language Models
\label{sec:shallow-models}

After a long reign of topic modeling methods [@deerwester1990indexing],[^53]
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

📖{general model|first}
📖{masked token|first}
📖{context window|first}

In 2013, @mikolov2013efficient developed the *Word2Vec general language
model*, which could learn token embeddings. As its objective, the model
would predict a *masked token* $w_t$ from its context $C\_t = w\_{t - c}$, …,
$w\_{t - 1}$, $w\_{t + 1}$, …, $w\_{t + c}$, where $c$ is the *context window
size* and $w\_1, w\_2, …$ is the corpus:%
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
C\_t)$ is a *scoring function* that measures how well the masked token $w\_t$
matches the context $C\_t$:%
%
\begin{equation}
  s(w\_t, C\_t) = \vec{u}\_{C\_t}ᵀ · \vec{v}\_{w\_t},
  \vec{u}\_{C\_t} = \frac{1}{|C\_t|}Σ\_{w ∈ C\_t}\vec{u}\_{w}.
\end{equation}
%
📖{input vector|first}%
📖{vector dimensionality $D$|first}%
%
Here, $\vec{u}\_{w} ∈\mathbb{R}^D$ is the *input vector* of the context token
$w$, $\vec{v}\_{w\_t} ∈\mathbb{R}^D$ is the *output vector* of the masked token
$w\_t$, $\vec{u}\_{C\_t}$ is the *context vector* and $D$ is the number of token
vector features (further known as *vector dimensionality*).

 /figures/general-model.tex

 /figures/subword-model.tex

 /figures/positional-model.tex

See Figure \vref{fig:general-model} for an illustration of the model.

##### Subtoken Model

The general model only learns representations for tokens that are present in
the training corpus. Additionally, input vectors for different inflectional
forms of a token share no weights, which delays training convergence for
morphologically rich languages.

📖{subtoken model|first}

In 2017, @bojanowski2017enriching developed the *fastText subtoken model*,
which extended the general model by modeling subtokens instead of tokens: The
input vector $\vec{u}\_w$ for a token $w$ becomes a sum of the input vectors
$\vec{u}\_g$ for the *subtokens* $g ∈ G\_w$ of $w$:%
%
\begin{equation}
  \vec{u}\_w = Σ\_{g ∈ G\_w} \vec{u}\_g.
\end{equation}

See Figure \vref{fig:subword-model} for an illustration of the model.

##### Quantized Model

The token embeddings of the general and subtoken models occupied significant
memory and storage, and their use required expensive floating point operations,
which makes them unsuitable for resource-limited computing.

📖{quantized model|first}

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
bitwise arithmetic for their manipulation.

##### Positional Model

In many sentences, the position of context tokens influences their syntactic
function, which is important for predicting the masked token. Additionally, if
the context $C\_t$ is large, distant tokens only introduce noise to the context
vector $\vec{u}\_{C\_t}$.

To better adapt to these situations, it would be useful to have separate input
vectors $\vec{u}\_{w, p}$ for different positions $p ∈ P$ of a context token
$w$:%
%
\begin{equation}
  \vec{u}\_{C\_t} = \frac{1}{|P|}Σ\_{p ∈ P}\vec{u}\_{w\_{t + p}, p}.
\end{equation}
%
📖{positional model|first}%
%
Since this would increase the size of the vocabulary by a factor of $|P| = 2c$,
@mikolov2018advances developed the *fastText positional model*, which adopts the
positional weighting of @mnih2013advances:%
%
\begin{equation}
  \vec{u}\_{w\_{t + p}, p} = \vec{u}\_{w\_{t + p}} ⊙ \vec{d}\_p,
\end{equation}
%
📖{positional embedding|first}%
%
Here, $\vec{d}\_p  ∈ \mathbb{R}^{D'}, p ∈ P$ are *positional embeddings* with
$D' = D$ features and $⊙ :\mathbb{R}^D\times\mathbb{R}^D\to\mathbb{R}^D$ is
the Hadamard vector product.

See Figure \vref{fig:positional-model} for an illustration of the model.

#### Deep Neural Network Language Models

Deep neural network language models can be used to produce token, sentence, and
document embeddings, which can be stored in a dense vector database and used to
retrieve semantically related tokens, sentences, and documents.

##### Token Embeddings

📖{global embedding|first}
📖{contextual embedding|first}
📖{disambiguation|first}

Unlike shallow log-bilinear language models, which produce a single *global
token embedding* for every token, deep neural network language models produce
*contextual token embeddings* that depend both on the token and on the context
of its sentence. [@peters2018deep] This makes it easier to *disambiguate* the
specific *sense* of a possibly ambiguous token.

##### Sentence and Document Embeddings

📖{dense attention|first}

Early neural machine translation used *encoder-decoder models*, where an
encoder recurrent neural network (RNN) would first read and encode a *source
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
memories.

📖{Transformer|first}

In 2017, @vaswani2017attention proposed the *Transformer* architecture,
which successfully replaced recurrence by the vertical stacking of attention
and which has since been shown to be a Turing-complete [@perez2019turing]
universal approximator [@yun2020transformers].

📖{sentence embedding|first}
📖{Sentence BERT|first}

Transformer-based language models, such as *Sentence BERT*
[@reimers2019sentencebert] and *ColBERT* [@khattab2020colbert], produce
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

📖{structural matching|first}
📖{semantic matching|first}
📖{dense retrieval}

Unlike natural text, the language of mathematics is inherently more structured,
which makes it possible to use *structural matching* techniques for math
information retrieval. Both information retrieval and math information
retrieval can also use *semantic matching* techniques that lie at the boundary
of sparse retrieval and dense retrieval and use token embeddings for sentence
and document retrieval.

#### Structural Matching
\label{sec:structural-matching}

📖{generalization|first}
📖{subformula|first}
📖{substitution tree|first}

To retrieve math formulae $\big(f(a + b, g(c))\big)$ and their *generalizations*
$\big(f(x, y)\big)$, we can use *substitution trees*. [@graf1995substitution]
To also retrieve their *subformulae* ($a + b$), we must index them separately.

📖{tree edit distance|first}

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

 /figures/soft-vsm.tex

📖{soft vector space model|first}
📖{soft cosine similarity|first}
📖{hard vector space model}

To retrieve semantically similar documents, we can use the *soft vector space
model* [@sidorov2014soft]. Unlike the hard vector space model, which will only
find exact *hard matches* between tokens, the soft vector space model will also
consider fuzzy *soft matches* between related tokens, see Figure
\vref{fig:soft-vsm}. In the soft vector space model, the relevance of a
document to a query is estimated as the *soft cosine similarity* $\softcos(\vec
x, \vec y)$ between their sparse vectors $\vec x$ and $\vec y$:
%
\begin{equation}
  \softcos(\vec x, \vec y) = \frac{\vec xᵀ · S · \vec y}%
  {\sqrt{\vec xᵀ · S · \vec x} · \sqrt{\vec yᵀ · S · \vec y}},
\end{equation}
%
📖{cosine similarity}%
📖{global embedding}%
📖{hard vector space model}%
%
where $S\_{ij}$ can either be the cosine similarity $\cos(\vec i, \vec j)$
between the global token embeddings $\vec i$ and $\vec j$ of tokens $i$ and $j$
or another measure of semantic token relatedness. [@charlet2017simbow]
Intuitively, the soft vector space model will *expand the query*[^78] with
related tokens and then use cosine similarity. The worst-case time complexity
of the soft cosine measure is $\mathcal{O}(n^2)$, where $n$ is the number of
unique tokens in the documents. It can be reduced to $\mathcal{O}(n)$ (the
same as the hard vector space model) by restricting the measures of relatedness
that the soft vector space model can use.  [@novotny2018implementation]

📖{RM3|first}

 [^78]: Other useful query expansion techniques include *RM3* [@abdul2004umass].

📖{word mover's distance|first}

An alternative technique to determine the pairwise semantic similarity of two
documents is the *word mover's distance* [@kusner2015from], which will find the
*minimum-cost flow* $F$ between the sparse vectors $\vec{x}$ and $\vec{y}$ of
two documents using linear programming:%
%
\begin{equation}
  \min\_F Σ\_i Σ\_j F\_{ij} · S\_{ij}
  `  subject to  ` Σ\_j F\_{ij} = x\_i, Σ\_i F\_{ij} = y\_j,
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
practical to use for sentences and short documents.

### Learning to Rank
\label{sec:learning-to-rank}

Information retrieval systems make naive assumptions about the syntax and
the semantics of the natural language and the language of mathematics. They
also often rely on token embeddings of language models, which contain
human-like biases. [@caliskan2017semantics] These assumptions and biases cause
systematic errors and affect the retrieval results.

In this section, I will describe *learning to rank* techniques, which attempt
to remove these systematic errors and improve the ranking of results by
combining the results of several individual systems.

##### Score Aggregation

📖{score aggregation|first}

A learning to rank technique that combines several structural and semantic
matching techniques and corrects their errors before they can influence the
retrieval results is called *score aggregation*. An example of score
aggregation is the *CombMNZ* technique [@lee1997analyses], which combines the
scores $s\_i$ that a document $d$ received from $N\_d$ matching techniques to a
corrected score:
%
\begin{equation}
    `CombMNZScore`(d) = N\_d · Σ\_i s\_i(d)
\end{equation}

##### Rank-Based Fusion

📖{rank-based fusion|first}
📖{reciprocal rank fusion|first}

A learning to rank technique that combines the results of several information
retrieval systems and corrects the results before users can see them is called
*rank-based fusion*. An example of rank-based fusion is the *reciprocal rank
fusion* [@cormack2009reciprocal], which combines the ranks $r\_i(d)$ that a
document $d$ received from several information retrieval systems to a final
score that determines the rank of the document in the corrected results:
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

📖{information need}

Information retrieval systems must fulfil three major objectives: the
*accuracy* of their results for the information needs of their users, the
*speed* at which they can be deployed, index new documents, and deliver results
to their users before the results have lost their relevance, and their
*interpretability*, which determines how well they can explain the results to
their users.

To measure how well the systems fulfil these three objectives, we can use a
number of *evaluation measures*. In this section, I will discuss the existing
evaluation measures for each of the three objectives.

### Accuracy

📖{intrinsic measure|first}
📖{extrinsic measure|first}

An important distinction between evaluation measures of information retrieval
accuracy is whether they are *intrinsic* or *extrinsic*. Intrinsic evaluation
measures are defined on small tasks and are easily reproducible in a laboratory
setting. In contrast, extrinsic evaluation measures are defined on real-world
applications and may require human participants.

#### Intrinsic Measures

📖{word analogy|first}
📖{global embedding}

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

📖{total accuracy|first}
📖{semantic accuracy|first}
📖{syntactic accuracy|first}

The primary evaluation measure is the *total accuracy*, which is the percentage
of correct answers in all categories. The *syntactic* and *semantic accuracies*
are also sometimes reported and they correspond to the percentages of correct
answers in the different categories.

#### Extrinsic Measures

To evaluate the accuracy of an information retrieval system for the information
needs of its users, we can use extrinsic evaluation measures.

📖{relevance judgement|first}
📖{information need}
📖{retrieval unit}

Extrinsic evaluation measures use a set of queries $Q$ that correspond to
information needs, a collection of retrieval units $D$ that correspond to
information, and *relevance judgements* from *human assessors* that specify
whether the information in a retrieval unit $d$ is relevant to the information
need in a query $q$.
Relevance judgements can either be *complete* and cover all combinations $Q
× D$ of queries and documents or *incomplete*, depending on the size of the
collection.
Relevance judgements can also be either *graded* with several levels of
relevance or *binary*.

📖{precision (P)|first}
📖{precision at $k$ (P"@"$k$)|first}
📖{mean average precision (MAP)|first}
📖{bpref|first}
📖{nDCG|first}

In the evaluation, we submit queries $Q$ to an information retrieval system
and we compare the retrieval results with the relevance judgements. To evaluate
unranked retrieval results, we can use the *precision (P)* and *recall (R)*
[@manning2008introduction, Section 8.3] evaluation measures for complete and
binary relevance judgements. To evaluate ranked retrieval results, we can use
the *precision at $k$ (P@$k$)* and *mean average precision (MAP)*
[@manning2008introduction, Section 8.4] for complete and binary
judgements, *bpref* [@buckley2004retrieval] for incomplete and binary
judgements, and *nDCG* for complete and graded judgements.
[@manning2008introduction, Section 8.4]

In 2008, @sakai2008information developed a technique that converts measures
for complete judgements (P, R, P@$k$, MAP, and nDCG) to measures for incomplete
judgements (P', R', P'@$k$, MAP', and nDCG') by removing all documents without
a relevance judgement from the retrieval results.

📖{binarized judgement|first}

We can also *binarize* graded judgements by selecting which relevance levels
will be considered relevant and which relevance levels will be considered
irrelevant. This allows us to use measures for binary judgements.

### Speed

To evaluate the speed of an information retrieval system, we can measure the
time experienced by the users (further known as *wall clock time*) at various
stages of the system's life cycle:

📖{deployment time|first}
📖{training time|first}
📖{query time|first}

We can measure how fast we can deploy our system at a server (further known as
the *deployment time*), train token embeddings and other machine learning
components of our system (further known as the *training time*), index new
documents before the retrieval (further known as the *indexing time*), and
deliver results to a query (further known as *query time*).

### Interpretability

📖{dimensionality reduction|first}
📖{clustering|first}
📖{formal concept analysis|first}

To help determine the interpretability [@hall2018introduction, Section 1] of
the various components of an information retrieval systems, we can use
exploratory visualization techniques such as *dimensionality reduction*
[@van2008visualizing; @cox2008multidimensional; @hinton2006reducing] to show
high-dimensional data, *clustering* [@manning2008introduction, Chapter 16] and
*correlation analysis* to discover patterns in the data, and *formal concept
analysis* [@wille1982restructuring; @priss2005linguistic] to discover
hierarchical relationships in the data.

📖{usability testing|first}

To determine the quality of the explanations provided by the information
retrieval systems, we can perform *usability testing* [@nielsen1993usability]
by deploying the system with different types of explanations and measuring user
satisfaction. [@manning2008introduction, Section 8.6.3]

# State of the Art {#state-of-the-art}
\label{cha:state-of-the-art}

Despite the lack of attention math information retrieval has historically
received compared to information retrieval more generally, a substantial amount
of research has been conducted since the first workshop in 2008.

⇈ In this chapter, I'll describe the state of the art in math information
retrieval:

- In Section \vref{sec:competitions}, I will describe the past and future
  math information retrieval competitions.

- In Section \vref{sec:systems}, I will describe the math information retrieval
  systems that achieved the best accuracy in the competitions.

## Competitions
\label{sec:competitions}

↑ Even though the idea of digital mathematical libraries dates back to the
2002, it wasn't until ten years later until the first competitions[^3] of math
information retrieval systems took place, see Figure \vref{fig:competitions}.

 [^3]: Here and throughout the rest of the thesis, the term *competitions* is
       used for its familiarity to the general audience instead of the more
       technically correct term *shared task evaluations*.

 /figures/competitions.tex

During 2003--2015, thirteen *International Conferences on Mathematical
Knowledge Management (MKM)* [@asperti2003mathematical] have been held that
provided the first opportunity for individuals and research groups interested
in math information retrieval to meet and collaborate.

Since 2008, MKM was co-located with the *International Conferences of
Intelligent Computer Mathematics (CICM)*. During 2008--2011, a group
of MKM attendees interested in digital mathematical libraries
held a series of *Towards a Digital Mathematics Library* workshops
[@sojka2008towards; @sojka2009towards; @sojka2010towards; @sojka2011towards],
which were also co-located with CICM. In 2012, the *Math Information
Retrieval (MIR) workshop*, which included the historically first
competition of math information retrieval systems, was co-located with CICM.

 [69]: https://cicm-conference.org/2012/cicm.php?event=mir&menu=general

In 2013, 2014, and 2016, three math information retrieval competitions
were held as *Math* tasks [@aizawa2013ntcir; @aizawa2014ntcir;
@zanibbi2016ntcir] at the *NII Testbeds and Community for Information access
Research (NTCIR)* workshops hosted by the National Institute of Informatics
(NII) in Tokyo, Japan.

 [70]: https://ntcir-math.nii.ac.jp

Following a four year pause, in 2020 and 2021, two math information retrieval
competitions were held as *Answer Retrieval for Questions on Math (ARQMath)*
labs at the *Conferences and Labs of the Evaluation Forum (CLEF)*. In 2022, the
third (and likely final) ARQMath lab will be held.

 [71]: https://www.cs.rit.edu/~dprl/ARQMath

### Math Information Retrieval Workshop

📖{content math}
📖{presentation math}
📖{Mathematical REtrieval Collection (MREC)|first}

In 2012, [the MIR workshop][69] was held at CICM. For the workshop, a dataset
of 10,000 math documents and 10,000 math formulae was sampled from the
[*Mathematical REtrieval Collection (MREC)*][72] [@liska2011web] of arXiv
documents and extended with documents hand-picked by members of a jury.
[@liska2013evaluation, Section 2.2.4] Math formulae in the dataset were
represented using presentation math (Presentation MathML) and content math
(Content MathML).

 [72]: https://mir.fi.muni.cz/MREC

The two teams that participated competed in three subtasks:

📖{retrieval unit}

1. *formula search*, where the retrieval units were math formulae,
2. *full-text search*, where the retrieval units were math documents, and
3. *open information retrieval*, where the retrieval units were to be decided
   by the participants.

📖{information need}
📖{mixed query}
📖{text query}
📖{wildcard}

← In the first two subtasks, the information needs were specified using mixed
queries with wildcards. In open information retrieval, the information needs
were specified using text queries.

For the evaluation, one relevant document was provided for every query in the
first two subtasks. [@liska2013evaluation, Section 5.1] No relevance judgements
were produced in the open information retrieval subtask and the subtask was
judged by members of the jury during the event.

### NTCIR-10 Math

📖{TeX@\TeX{}}

In 2013, [the pilot *Math* task][74] was held at the 10th NTCIR workshop (NTCIR-10).
[@aizawa2013ntcir]
For NTCIR-10 Math, a dataset of 100,000 math documents and approximately 100×
as many math formulae from arXiv was developed. In addition to the math
representations in the MREC dataset, math formulae in the NTCIR-10 dataset were
also represented using \TeX.

 [74]: http://ntcir-math.nii.ac.jp/ntcir10-math

📖{information need}

The participants competed in formula search and full-text search with the
same representation of information needs as in the MIR workshop. Six teams
participated in formula search and two in full-text search.

📖{relevance judgement}
📖{mean average precision (MAP)}
📖{precision at $k$ (P"@"$k$)}
📖{precision (P)}
📖{binarized judgement}

For the evaluation, incomplete and graded relevance judgements were produced
with three levels of relevance: *relevant*, *partially relevant*, and
*not relevant*. Retrieval results were evaluated using the MAP, P@5, P@10, and P
evaluation measures. Since these measures are for complete judgements, the
judgements were made complete by considering unannotated documents to be
not relevant. Since these measures are also for binary judgements, the judgements
were binarized using two different approaches:

1. In the *partialy relevant* approach, both partially relevant and relevant
   documents were considered relevant.

2. In the *relevant* approach, partially relevant documents were considered
   not relevant and only relevant judgements were considered relevant.

### NTCIR-11 Math-2

In 2014, [the *Math-2* task][73] was held at the 11th NTCIR workshop (NTCIR-11).~[@aizawa2014ntcir]
For NTCIR-11 Math, a dataset of 8,301,578 paragraphs from approximately 105,120
documents from arXiv was developed. Math formulae were represented as in the
NTCIR-10 Math dataset.

 [73]: https://ntcir-math.nii.ac.jp/ntcir11-math

📖{mixed query}
📖{retrieval unit}
📖{wildcard}

The eight teams that participated competed in the *main* subtask, where the
retrieval units were paragraphs, so that the retrieval results could be more
focused. As in the full-text search subtask from NTCIR-10 Math, the information
needs were specified using mixed queries with wildcards.

📖{bpref}
📖{relevance judgement}

In addition to the evaluation measures from NTCIR-10 Math, the bpref evaluation
measure for incomplete and binary judgements was also used.

### NTCIR-12 MathIR

In 2016, [the *MathIR* task][70] was held at the 12th NTCIR workshop (NTCIR-12).
[@zanibbi2016ntcir]
For NTCIR-12 MathIR, a dataset of 319,689 math documents and over 590,000 math
formulae from the English Wikipedia was developed in addition to the NTCIR-11
Math-2 dataset from arXiv. Math formulae were represented as in the NTCIR-11
Math-2 and NTCIR-10 Math datasets.

The participants competed in four subtasks (the number of participating teams
is in parentheses):

- *arXiv main* (5) and *arXiv simto* (2), where the retrieval units were paragraphs
  from the NTCIR-11 Math-2 dataset,
- *math wiki* (6), where the retrieval units were math documents from the English
  Wikipedia, and
- *math wiki formula* (2), where the retrieval units were math formulae from the
  English Wikipedia.

📖{information need}
📖{mixed query}
📖{simto region}
📖{wildcard}

← In the *arXiv main*, *math wiki*, and *math wiki formula* subtasks, the
information needs were specified using mixed queries with wildcards. In the
*arXiv simto* subtask, the information needs were specified using mixed queries
with wildcards and also simto regions.

📖{precision at $k$ (P"@"$k$)}

Retrieval results were evaluated using the P@5, P@10, P@15, and P@20 evaluation
measures.

### ARQMath-1

In 2020, the pilot [*ARQMath-1* lab][71] was held at CLEF. [@zanibbi2020overview]
For ARQMath-1, a dataset of 2,466,080 math questions, answers, and comments and
28,320,920 math formulae from the Math StackExchange question answering website
was developed. Math formulae were represented as in the NTCIR Math datasets.

The participants competed in two tasks:

📖{find answers|first}
📖{retrieval unit}

1. *find answers*, where the retrieval units were math answers and
2. *formula search*, where the retrieval units were math formulae.

📖{information need}
📖{mixed query}

← In both tasks, the information needs were specified using mixed queries.
Five teams participated in finding answers and three in formula search.

📖{relevance judgement}
📖{binarized judgement}
📖{nDCG}
📖{mean average precision (MAP)}
📖{precision at $k$ (P"@"$k$)}
📖{retrieval unit}

For the evaluation, incomplete and graded relevance judgements were produced
with four levels of relevance: *high*, *medium*, *low*, and *not relevant*.
Retrieval results were primarily evaluated using the nDCG' evaluation measure.
The MAP' and P@10 measures were also reported. Since the P@10 measure is for
complete judgements, the judgements were made complete by considering
unannotated documents to be non-relevant. Since the MAP' and P@10 measures are
for binary judgements, the judgements were binarized by considering retrieval
units with high and medium relevance to be relevant.

### ARQMath-2

In 2021, the *ARQMath-2* lab was held at CLEF. [@behrooz2021overview]
For ARQMath-2, the dataset from ARQMath-1 was used.

📖{find answers}

The participants competed the same task as in ARQMath-1.
Nine teams participated in finding answers and six in formula search.

📖{mean average precision (MAP)}
📖{precision at $k$ (P"@"$k$)}
📖{nDCG}

Retrieval results were evaluated using the P'@10 evaluation measure instead of
the P@10 evaluation measure from ARQMath-1. The primary evaluation measure was
still nDCG'.

### ARQMath-3

In 2022, the *ARQMath-3* lab is going to be held at CLEF.
Unless prolonged, CLEF labs are only held for three years, and the organizers
announced their intention not to prolong the ARQMath lab. [@zanibbi2021arqmath]
Therefore, ARQMath-3 may be the last ARQMath lab.

📖{find answers}

For ARQMath-3, I proposed the historically first *math open-domain question
answering task (Math OpenQA)*. [@novotny2021proposala; @novotny2021proposalb]
Unlike systems for finding answers, which can only retrieve existing documents,
Math OpenQA systems can generate answers to novel questions from multiple
sources.

## Systems
\label{sec:systems}

 /figures/systems.tex

In this section, I will describe the math information retrieval search engines
that achieved the best accuracies in the competitions, see Table \vref{tab:systems}.
@liska2015building [Section 2.1] @guidi2016survey @ruzicka2018math [Section
2.3] describe other systems of historical significance.

### MathWebSearch

The *MathWebSearch (MWS)* system [@kohlhase2013mathwebsearch;
@hambasan2014mathwebsearch] has been developed by Michael Kohlhase's
KWARC research group and has been deployed in [several digital mathematical
libraries][75] since 2005. MWS participated in the NTCIR-10 Math and NTCIR-11
Math-2 tasks.

 [75]: https://kwarc.info/systems/mws

📖{content math}
📖{substitution tree}
📖{TF-IDF}
📖{hard vector space model}
📖{retrieval unit}

MWS indexes content math formulae using substitution trees. If the retrieval
units are not math formulae, their text is indexed using a hard vector space
model with the TF-IDF term weighting scheme.

📖{hard vector space model}
📖{retrieval unit}

In the retrieval, retrieval units with math formulae that structurally match
the query are presented first. If the retrieval units are not math formulae,
then the remaining retrieval units are presented next in the order of the hard
vector space model.

### Math Indexer and Searcher
\label{sec:mias}

The [*Math Indexer and Searcher (MIaS)* system][28] [@sojka2018mias] has been
developed by Petr Sojka's MIR research group and has been deployed in [the
European Digital Mathematical Library][51] since 2013. MIaS participated
in the NTCIR Math tasks and the ARQMath-1 lab.

📖{presentation math}
📖{content math}
📖{TF-IDF}
📖{hard vector space model}
📖{subformula}
📖{generalization}
📖{retrieval unit}

MIaS indexes retrieval units using a hard vector space model with the TF-IDF
term weighting scheme. Presentation and content math in the retrieval units is
tokenized into math subformulae and their generalizations.

📖{mixed query}

In the retrieval, a *query reduction* technique is used to produce several
mixed queries from the input query. Then, the results for all mixed queries are
interleaved.

### Mathcat

The [*Mathcat (MCAT)* system][76] [@topic2013mcat; @kristianto2014mcat; @kristianto2016mcat]
has been developed by the Aizawa laboratory, which participated in the NTCIR Math tasks.
MCAT has not been publicly deployed in a digital mathematical library nor does it have a
live demo.

 [76]: https://www-al.nii.ac.jp/mathcat-project/

📖{presentation math}
📖{content math}
📖{TF-IDF}
📖{weighted zone scoring}
📖{hard vector space model}
📖{structural matching}
📖{semantic matching}
📖{similarity score}

MCAT indexes presentation and content math formulae using a hard vector space
model with the TF-IDF weighting scheme and weighted zone scoring. The math
formulae are tokenized using several strategies and include text descriptions
that are extracted from the math documents or paragraphs that contain the formulae.

📖{hard vector space model}
📖{retrieval unit}
📖{structural matching}
📖{semantic matching}

In the retrieval, math formulae are first retrieved with the hard vector space
model and then reranked using structural and semantic matching techniques. If
the retrieval units are not math formulae, the similarity score of a retrieval
unit is the sum of the similarity scores of all formulae in the retrieval unit.

### Tangent

``` dot %
    [Evolutionary tree of Tangent systems]%
    {The evolutionary tree of the Tangent systems}%
    \label{fig:tangent}

digraph tree {
  node [fontname = "times"];

  margin  = 0;
  rankdir = "LR";

  tangent    -> tangentS;
  tangent    -> tangentL;
  tangentS   -> tangentCFT;
  tangentS   -> approach0;

  tangent      [label = "Tangent,\nTangent-2, and\nTangent-3"];
  tangentS     [label = "Tangent-S"];
  tangentCFT   [label = "Tangent-CFT and\nTangent-CFTED"];
  tangentL     [label = "Tangent-L"];
  approach0    [label = "Approach0"];
}

```

The *Tangent* systems have been developed by the research groups of Hui Fang
(InfoLab), Frank Tompa, and Richard Zanibbi (DPLR), which participated in the
NTCIR-11 Math-2 and NTCIR-12 MathIR tasks and in the ARQMath labs. Although the
Tangent systems have not been publicly deployed in a digital mathematical
library, Approach0 has [a live demo.][77]

 [77]: https://approach0.xyz/

See Figure \vref{fig:tangent} for a quick overview of the existing Tangent
systems and how they evolved from one another.

#### Tangent, Tangent-2, and Tangent-3

↑ The *Tangent* system [@wangari2014discovering; @stalnaker2015math] was
developed by Richard Zanibbi's DPLR research group and evolved into the
*Tangent-2* [@pattaniyil2014combining] and *Tangent-3* [@davila2016tangent]
systems that participated in the NTCIR-11 Math-2 and NTCIR-12 MathIR tasks.

📖{hard vector space model}
📖{presentation math}
📖{retrieval unit}
📖{TF-IDF}

Tangent-2 and Tangent-3 index presentation math formulae using a hard vector
space model with the TF-IDF term weighting scheme. The math formulae are
tokenized by extracting paths in the SLT math representation. If the retrieval
units are not math formulae, their text is indexed using a different hard
vector space model.

📖{retrieval unit}
📖{score aggregation}

In the retrieval, Tangent-2 and Tangent-3 retrieve math formulae that match the
query from the formula index and rerank them using structural matching
techniques. If the retrieval units are not math formulae, then Tangent-2 and
Tangent-3 also retrieve the units that match the query from the text index.
Then, the similarity score of a retrieval unit is the sum of the similarity
scores of all formulae in the unit aggregated with the similarity score of the
unit's text.

#### Tangent-L

The *Tangent-L* system [@fraser2018choosing; @ng2020dowsing; @ng2021dowsing]
evolved from Tangent-3, was developed by Frank Tompa's research group, and
participated in the ARQMath labs and post-hoc in the NTCIR-12 MathIR task.

📖{BM25}
📖{hard vector space model}
📖{Symbol Layout Tree (SLT)}
📖{TF-IDF}
📖{tokenization}

Unlike Tangent-3, which uses the hard vector space model with the TF-IDF term
weighting scheme, Tangent-L uses the more accurate BM25 and BM25⁺ term
weighting schemes.  Unlike Tangent-3, which only tokenizes math formulae by
extracting paths in the SLT, Tangent-L uses several tokenization strategies.

In the retrieval, unlike Tangent-3, which reranks the retrieved math formulae,
Tangent-L does not rerank the retrieved math formulae.

#### Tangent-S

The *Tangent-S* system [@davila2017layout], also evolved from Tangent-3, was
developed by DPLR, and was used as a *baseline* system in the ARQMath labs.

📖{presentation math}
📖{Symbol Layout Tree (SLT)}
📖{content math}
📖{Operator Tree (OPT)}

Unlike Tangent-3, which only indexes presentation math formulae in the SLT
representation, Tangent-S also indexes content math formulae in the OPT
representation.

📖{score aggregation}

In the retrieval, unlike Tangent-3, which immediately reranks the retrieved
math formulae using structural matching techniques, Tangent-S applies score
aggregation to the techniques before the reranking.

#### Approach0

The *Approach0* system [@zhong2015novel; @zhong2016opmes; @zhong2019structural;
@zhong2021approach], evolved from Tangent-S, was developed by InfoLab and DPLR,
was used as a baseline system in the ARQMath-1 lab, and participated in the
ARQMath-2 lab and post-hoc in the NTCIR-12 MathIR task.

📖{presentation math}
📖{Symbol Layout Tree (SLT)}
📖{content math}
📖{Operator Tree (OPT)}

Unlike Tangent-S, which still indexes presentation math formulae in the SLT
representation, Approach0 only indexes content math formulae in the OPT
representation.

📖{RM3}
📖{structural matching}
📖{semantic matching}

In the retrieval, unlike Tangent-S, which reranks the retrieved math formulae
using structural matching techniques, Approach0 uses both structural and
semantic matching techniques for the reranking. Additionally, in the ARQMath-2
lab, Approach0 used the RM3 query expansion technique.

#### Tangent-CFT and Tangent-CFTED

The *Tangent-CFT* and *Tangent-CFTED* systems [@mansouri2019tangent;
@mansouri2020dprl; @mansouri2021dprl], also evolved from Tangent-S, were
developed by DLPR, and participated in the ARQMath labs and post-hoc in the
NTCIR-12 MathIR task.

📖{hard vector space model}
📖{Operator Tree (OPT)}
📖{Symbol Layout Tree (SLT)}
📖{soft vector space model}

Unlike Tangent-S, which uses the hard vector space model, Tangent-CFT and
Tangent-CFTED use the soft vector space model with fastText token embeddings as
a source of token relatedness. Additionally, unlike Tangent-S, which tokenizes
math formulae by extracting paths in the SLT and OPT, Tangent-CFT and
Tangent-CFTED only extract tuples of neighboring nodes.

📖{tree edit distance}

In the retrieval, unlike Tangent-S, which reranks the retrieved math formulae
using structural matching techniques, Tangent-CFT does not rerank the retrieved
math formulae and Tangent-CFTED uses a different structural matching technique:
the tree edit distance.

# Accuracy {#accuracy}
\label{cha:accuracy}

There are many advanced features we may wish for from a math information retrieval
system: the ability to reveal relationships between different pieces of
information, to visualize the information, or to use the information as a basis
for computation. However, all these advanced features are predicated on a much
more basic and fundamental feature: the ability of the math information
retrieval system to *accurately* retrieve the information that satisfy the
needs of its users.

In this chapter, I will report the results of my research on representations
that enable accurate math information retrieval:

- In Section \vref{sec:topological-math-representations} and Section
  \vref{sec:representation-learning-accuracy}, I will describe my work in
  developing a new representation of math along with my experiments in learning neural
  representations of natural language words and math symbols.

- In Section \vref{sec:score-aggregation-of-structural-and-semantic-matching-techniques} and
  Section \vref{sec:rank-based-fusion}, I will describe my work in overcoming the
  weaknesses of different document representations by using learning-to-rank techniques.

- In Section \vref{sec:weighted-zone-scoring} and Section
  \vref{sec:approximate-nearest-neighbor-search-accuracy}, I will describe my
  work in making fast approximate search techniques more accurate.

## Topological Math Representations for Accurate Math Information Retrieval
\label{sec:topological-math-representations}

📖{tokenization}
📖{presentation math}
📖{content math}

Although tree-structured math representations for the exchange of presentation
and content math are well established, how to tokenize them into a linear
stream of tokens for math information retrieval is an open problem.

📖{content math}
📖{tokenization}

In 2019, @lample2020deep tokenized a tree-structured content math
representation using pre-order traversal, producing a topologically ordered
sequence of tokens (also known as the prefix notation or the normal Polish
notation). To give an example, the expression $x!! - y^2 = 0$ would be represented
as the following comma-separated list of math symbols: $=$, $-$, $!!$, $x$,
$◌^◌$, $y$, $2$, $0$.

📖{computer algebra system (CAS)}
📖{Mathematica}
📖{MatLab}
📖{Maple}
📖{Transformer}

Using their representation, @lample2020deep trained a Transformer model to
solve integrals, first-order differential equations, and second-order
differential equations with significantly better results than commercial CAS
such as Mathematica, MatLab, and Maple.

#### Experiments

📖{content math}
📖{Operator Tree (OPT)}

In 2020, we used the representation of @lample2020deep (further known as
the *prefix* notation) in our submissions to the ARQMath-1 lab to see if the
representation would also be useful for math information retrieval. Since
@lample2020deep do not specify the precise tree-structured content math
representation they used, we have used the OPT math representation in our
systems. [@novotny2020three, Section 2.1] To give an example, the expression $x!!-
y^2 = 0$ from the previous paragraph would be represented as the following
comma-separated list of OPT nodes: `U!eq`, `O!minus`, `O!double-factorial`,
`V!x`, `O!SUP`, `V!y`, `N!2`, `N!0`.

📖{Operator Tree (OPT)}

To see if the topological ordering produced by the pre-order traversal of OPT
trees is meaningful, we have also used a representation produced by the
in-order traversal of the OPT trees (further known as the *infix* notation) and
by surrounding subtrees by parentheses. To give an example, the expression $x!! - y^2
= 0$ would be represented as the following comma-separated list of math
symbols: $($, $($, $!!$, $($, $x$, $)$, $-$, $◌^◌$, $($, $y$, $2$, $)$, $)$,
$=$, $0$, $)$, and as the following comma-separated list of OPT nodes: `(`,
`(`, `O!double-factorial`, `(`, `V!x`, `)`, `O!minus`, `O!SUP`, `(`, `V!y`,
`N!2`, `)`, `)`, `U!eq`, `N!0`, `)`.

📖{find answers}
📖{joint modeling}

In the *find answers* task of the ARQMath-1 lab, we used the prefix and infix
notations in two of our primary math information retrieval systems that jointly
modeled both text and math:

📖{Sentence BERT}

- *MIRMU-SCM* [@novotny2020three, Section 4], which was based on a soft vector
  space model with token embeddings from a fastText subtoken shallow
  log-bilinear language model, and

- *MIRMU-CompuBERT* [@novotny2020three, Section 6], which was based on the sentence
  embeddings of the Sentence BERT deep neural network language model.

📖{Operator Tree (OPT)}
📖{Symbol Layout Tree (SLT)}
📖{TeX@\TeX{}}

In both systems, we have compared the prefix and infix notations to other math
representations on our train dataset. For the *MIRMU-SCM* system, we have used
the following math representations for comparison: none (math tokens were
removed), \TeX{} (every math formula becomes one token), adjacent
pairs of OPT nodes, and adjacent pairs of SLT nodes. [@novotny2020three,
Section 4.3] For the *MIRMU-CompuBERT* system, we have used the \TeX{} math
representation for comparison. [@novotny2020three, Table 2]

#### Results

📖{Operator Tree (OPT)}
📖{soft vector space model}
📖{Symbol Layout Tree (SLT)}
📖{TF-IDF}

In the *MIRMU-SCM* system, both the prefix and the infix notations achieved the
best results on the train dataset. The prefix and infix notations achieved
equivalent results, since they mainly differ by ordering. (With TF-IDF term
weighting, adding parenthesis tokens is insignificant.) Since the soft vector
space model is insensitive to token ordering, both notations correspond to
modeling individual OPT nodes. The better results of the prefix and infix
notations compared to modeling adjacent pairs of OPT and SLT nodes show that in
our soft vector space model, modeling individual OPT nodes is better than
modeling longer paths in OPT and SLT trees.

📖{TeX@\TeX{}}

In the *MIRMU-CompuBERT* system, the prefix notation has achieved better
results than the infix notation, showing that the topological ordering of
@lample2020deep is significant in deep neural language models. Surprisingly,
the \TeX{} representation has achieved better results than both prefix and
infix notations. We believe that this is due to tokenisation: Unlike
@lample2020deep, we fine-tuned a language model whose tokenizer was pre-trained
on natural language text. In \TeX{}, commands have descriptive names in
natural language, which makes it more appropriate to use the same tokenizer
for both natural language text and math.

#### Future Work

📖{hard vector space model}
📖{Operator Tree (OPT)}
📖{relevance judgement}
📖{soft vector space model}
📖{Symbol Layout Tree (SLT)}

The state-of-the-art Approach0 and Tangent-L systems use hard vector space
models to model paths in OPT and SLT trees respectively. This indicates that
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

📖{structural matching}
📖{semantic matching}

Math information retrieval techniques for structural and semantic matching rely
on accurate neural representations (token embeddings) of natural language words
and math symbols.

In this section, I will report the results of my experiments in learning
accurate token embeddings:

- In Section \vref{sec:variable-control-in-token-embedding-evaluation}, I will
  describe my survey of methodology in previous token embedding experiments
  and show how a lack of variable control makes some previous experiments
  difficult to reproduce.

- In Section \vref{sec:heuristic-hyperparameter-optimization-in-subtoken-language-models},
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
@levy2014neural in 2014. In 2015, @levy2015improving showed that several
pre-processing steps and fixed parameters can have a significant impact on
experimental results.

📖{intrinsic measure}
📖{word analogy}

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

📖{general model}
📖{word analogy}

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
reproduce. Wherever applicable, I suggested default parameter values and
ways to make future experiments easier to reproduce.

#### Results

The phrasing algorithm contains three undisclosed parameters:

1. the bigram merging threshold $\delta$,
2. the incremental threshold decay, and
3. the maximum dictionary size $n$.

← For the threshold and the decay, only the parameter values were omitted.
For the maximum dictionary size, the parameter only appears in the reference
implementation. I suggested the following default parameter values: $\delta =
100, n = 5 · 10^8$.

📖{vector dimensionality $D$}
📖{word analogy}
📖{positional embedding}

The fastText positional language model does not disclose the initial
distribution of weights for the input and positional embeddings. I described three
different initial weight distributions and I showed that up to 24\% of word
analogy accuracy can depend on them. I also showed that simpler weight
distributions suffered from numerical instability when the vector
dimensionality $D$ was high. I suggested using either the square-root normal
distribution $\mathcal{N}^{0.5}(0, \nicefrac{1}{3D^2})$ of @pinelis2018exp
or the continuous uniform $\mathcal{U}(\pm\nicefrac{\sqrt\[4\]{3}}{\sqrt{D}})$.
<!-- See also Section \vref{sec:position-independent-token-embeddings-accuracy}
where I discuss our further experiments with the fastText positional language
model. -->

📖{word analogy}

The word analogy task contains three undisclosed parameters:

1. the maximum dictionary size $n$,
2. the case transformation applied to words $a, b, a'$, and $b'$, and
3. the Unicode locale used for the case transformation.

← For the maximum dictionary size, only the parameter values were omitted. For
the case transformations, the parameter only appears in the reference
implementation. The Unicode locale appears in neither the published works nor
in the reference implementation. I showed that up to 4\% of word analogy
accuracy can depend on the Unicode locale and up to 18\% on the case
transformation and the locale. I suggested the default parameter
values $n = 3 · 10^5,$ Unicode case-folding, and the locale of the word
analogy task.

#### Reproducibility

[The experimental code for all my experiments is available on GitHub.][4] I have
also released [the only public implementation of the fastText positional model.][5]

 [4]: https://github.com/MIR-MU/reproducible-ml
 [5]: https://github.com/MIR-MU/pine

### Heuristic Hyperparameter Optimization in Subword Language Models
\label{sec:heuristic-hyperparameter-optimization-in-subtoken-language-models}

📖{general model}
📖{word analogy}

In 2017, @bojanowski2017enriching developed the fastText subtoken language model
and showed that it was more accurate than the Word2Vec language model on the
English, German, Czech, and Italian word analogy tasks. They also showed the
optimal subtoken sizes of fastText on the English and German word analogy tasks.
However, they did not optimize the subtoken sizes of fastText on the Czech and
Italian word analogy tasks and used subtokens of size 3--6[^6], which they
described as ``an arbitrary choice'' [@bojanowski2017enriching].

 [^6]: For subtoken sizes, we adopt the notation of @bojanowski2017enriching and
       @grave2018learning For example, subtokens of size 3–6 are all subtokens
       whose sizes are 3, 4, 5, or 6.

📖{word analogy}

In 2018, @grave2018learning produced the French and Hindi word analogy tasks.
Furthermore, they also trained and publicly released fastText language models
for 157 languages.  Like @bojanowski2017enriching, they also neglected to
optimize the subtoken sizes.  Unlike @bojanowski2017enriching, they used
subtokens of size 5--5 for all languages, noting that ``using character
$n$-grams of size 5 instead of using the default range of 3--6 does not
significantly decrease the accuracy (except for Czech).'' [@grave2018learning]

#### Experiments

📖{word analogy}

In 2021, we attempted to reproduce the optimal subtoken sizes of fastText
on the English (4--5) and German (6--6) word analogy tasks reported by
@bojanowski2017enriching. Additionally, we reported the optimal subtoken sizes
of fastText on the Czech, Italian, Spanish, French, Hindi, Turkish, and Russian
word analogy tasks. [@novotny2021one]

Furthermore, we produced a simple $n$-gram coverage model that can suggest
subtoken sizes for under-resourced languages where optimal subtoken sizes are
unknown.

#### Results

📖{word analogy}

We reproduced the optimal subtoken sizes of @bojanowski2017enriching and
reported the optimal subtoken sizes on the Czech (1--4), Italian (2--5), Spanish
(5--5), French (6--6), Hindi (2--2), Turkish (3--3), and Russian (5--6) word
analogy tasks. We showed that using the optimal subtoken sizes can result in up
to 14\% improvement in word analogy accuracy.

📖{word analogy}

We also demonstrated that using the suggested subtoken sizes of the $n$-gram coverage
model was never worse than the default subtoken sizes of
@bojanowski2017enriching @grave2018learning on our test dataset. It improved the
word analogy accuracy by up to 3\% compared to the default subtoken sizes, and
was within 2\% word analogy accuracy of the optimal subtoken sizes.

#### Future Work

📖{extrinsic measure}
📖{word analogy}

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

📖{extrinsic measure}
📖{intrinsic measure}
📖{word analogy}

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate than the fastText subtoken language model
on the English word analogy task. However, they only evaluated the positional
model on the intrinsic word analogy task and not on extrinsic tasks.

📖{positional model}

In the subtoken model, tokens are represented by the parts of their meaning that
are *fixed* or dependent on the *broader context* that includes the
conversational setting, the time and location of an utterance, and the salient
common ground. [@bach2012context] Unlike in the positional model, they are
*not* represented by the parts of the meaning that are dependent on the *narrow
context* of a paragraph, which makes the subtoken model less accurate than the
positional model. However, in the positional model, tokens are *only*
represented by the parts of their meaning that are dependent on the narrow
context of a paragraph and not the other parts, which can also make the
positional model less accurate.

#### Experiments

📖{input vector}
📖{vector dimensionality $D$}
📖{positional embedding}
📖{positional model}
📖{constrained positional model|first}

In 2021, we developed the fastText constrained positional model, which models
the parts of the meaning that are dependent on the narrow context of a
paragraph as well as the parts of the meaning that are fixed or dependent on
the broader context. [@novotny2021when, Section 2.2.4] We achieved this by
dividing the $D = 300$ input vector features into $D'$ narrow-context-dependent
features that would interact with positional embeddings, and the remaining $D -
D'$ fixed and broader-context-dependent features that would not interact with
positional embeddings.

📖{constrained positional model}
📖{word analogy}

We reported the optimal value of $D'$ on the English word analogy task
[@novotny2021when, Table 1] and compared the subtoken, positional, and
constrained positional models on the English word analogy task
[@novotny2021when, figures 2 and 3] and an English language modeling task
[@novotny2021when, Section 4.4].

#### Results

📖{vector dimensionality $D$}
📖{word analogy}

We showed that with $D = 300$ features, using $D' = 60$
narrow-context-dependent features is optimal on the English word analogy task.

📖{positional model}
📖{constrained positional model}
📖{word analogy}

We also showed that the positional model is more accurate than the subtoken
model and that the constrained positional model is more accurate than the
positional model on the word analogy and language modeling tasks.[^2]

 [^2]: See Section \vref{sec:position-independent-token-embeddings-speed}
       for a discussion about the speed of the positional and constrained
       positional models. See Section
       \vref{sec:position-independent-token-embeddings-interpretability}
       for a discussion about the interpretability of the positional model.

#### Future Work

📖{positional model}
📖{constrained positional model}

@grave2018learning used the positional language model to produce token
embeddings for 157 languages, but prior work has only shown that the positional
and constrained positional models outperform the subtoken model on English
tasks. Future work should compare the subtoken, positional, and constrained
positional models on languages other than English.

#### Reproducibility

📖{positional model}
📖{constrained positional model}

The experimental code for all our experiments is [available on GitHub][5]
together with the only public implementation of the positional and constrained
positional models.

### Quantized Token Embeddings
\label{sec:quantized-token-embeddings}

📖{general model}
📖{quantized model}
📖{word analogy}

In 2018, @lam2018word2bits developed the Word2Bits quantized language model
and showed that it was more accurate than the Word2Vec language model on the
English word similarity and open-domain question answering tasks and equally as
accurate on the English word analogy task. However, word analogy accuracies for
the different categories of analogical reasoning were not reported; only total
word analogy accuracies were.

#### Experiments

📖{word analogy}

In 2019, I consulted on the master's thesis of @stefanik2019semantic.[^32]
In the thesis, we aimed to reproduce the word analogy experiment of @lam2018word2bits
and analyse the word analogy accuracies for the different categories of
analogical reasoning. [@stefanik2019semantic, Section 3.1]

 [^32]: See also Section
        \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic} and
        Section \vref{sec:formal-concept-analysis-with-quantized-token-embeddings},
        where I discuss our further experiments with the quantized language
        model.

#### Results

📖{general model}
📖{quantized model}
📖{total accuracy}
📖{semantic accuracy}
📖{syntactic accuracy}
📖{word analogy}

We reproduced the total accuracy on the English word analogy task.
Additionally, we showed that although the quantized model was equally
as accurate as the Word2Vec language model in total accuracies, it was
significantly more accurate on the semantic categories of analogical
reasoning and significantly less accurate on the syntactic categories
of analogical reasoning.

#### Reproducibility

The experimental code for all our experiments is [available online.][8]

 [8]: https://is.muni.cz/th/bqz8u/attachments.zip

### Word Sense Disambiguation with Sense Embeddings
\label{sec:word-sense-disambiguation-with-sense-embeddings}

📖{content math}
📖{disambiguation}

In both natural language and content math, words and symbols can have
several possible senses. The accuracy of token embeddings can be improved
by accurate disambiguation of the word and symbol tokens into senses.

Previous work in training sense embeddings relies on sense-annotated training
corpora. These corpora have been manually disambiguated by human annotators and
are prohibitively small, a condition commonly referred to as *the knowledge
acquisition bottleneck*.

📖{disambiguation}

Additionally, previous work in training sense embeddings only evaluates the
embeddings on word similarity tasks, where the sense representations are not
directly used, and not on sense-specific tasks such as word sense
disambiguation.

#### Experiments

In our work, we have developed an algorithm for enlarging sense-annotated
training corpora using a sense inventory. [@ayetiran2021eds, Section 4] We
used the enlarged corpora to train our sense embeddings without the knowledge
acquisition bottleneck.

📖{disambiguation}

We evaluated our sense embeddings on five English word similarity tasks.
[@ayetiran2021eds, Section 5.4] Additionally, we have developed a word sense
disambiguation algorithm for sense embeddings and used it to evaluate our sense
embeddings on six English word sense disambiguation tasks. [@ayetiran2021eds,
Section 5.3]

#### Results

Our sense embeddings achieved state-of-the-art accuracy on three out of five
word similarity tasks.

📖{disambiguation}

Compared to previous work in training sense embeddings using sense inventories
and other lexical resources, our sense embeddings achieved state-of-the-art
accuracy on five out of six word sense disambiguation tasks.

#### Future Work

📖{content math}
📖{disambiguation}
📖{semantic math}

Future work should investigate how our word sense disambiguation algorithm
can be used to disambiguate content math symbols into semantic math senses.

#### Reproducibility

Our sense embeddings are [available online.][9]

 [9]: https://mir.fi.muni.cz/EDS-MEMBED.zip

### Decontextualized Token Embeddings
\label{sec:decontextualized-token-embeddings}

📖{global embedding}
📖{structural matching}
📖{semantic matching}
📖{soft vector space model}
📖{word mover's distance}

Math information retrieval techniques for structural and semantic matching such
as the soft vector space model and the word mover's distance rely on global
token embeddings of shallow log-bilinear language models.

📖{contextual embedding}
📖{global embedding}
📖{word mover's distance}

In 2019, @zhao2019moverscore showed that using contextual embeddings of deep
neural language models improves the accuracy of the word mover's distance on
machine translation evaluation tasks compared to using global embeddings.
However, using contextual embeddings in a math information retrieval system
requires expensive query-time inference.

#### Experiments

📖{contextual embedding}
📖{global embedding}
📖{decontextualization|first}

In 2021, we developed an algorithm that *decontextualized* contextual token
embeddings into global token embeddings by averaging across all contexts of a
token. [@stefanik2021regressive, Section 3.2]

📖{global embedding}
📖{soft vector space model}
📖{word mover's distance}
📖{decontextualization}

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
: [Accuracies of the soft vector space and the word mover's distance %
   with and without our decontextualized token embeddings on eight machine %
   translation tasks]%
  {The accuracies of the soft vector space (SCM) and the word mover's distance (WMD) %
   with and without our decontextualized token embeddings on eight machine %
   translation evaluation tasks. [@stefanik2021regressive] %
   Language pairs marked with an × were evaluated cross-lingually. %
   [@stefanik2021regressive, Section 3.4]}%
  \protect📖{soft vector space model}%
  \protect📖{word mover's distance}%
  \protect📖{decontextualization}%
  \label{tab:decontextualized-token-embeddings}

#### Results

📖{soft vector space model}
📖{word mover's distance}
📖{decontextualization}

We showed that the soft vector space model and the word mover's distance were
more accurate with our decontextualized embeddings than with the global
embeddings of a fastText model on most tasks: five out of eight for the soft
vector space model and six out of eight for the word mover's distance. See
Table \vref{tab:decontextualized-token-embeddings}.

#### Future Work

📖{decontextualization}

Future work should evaluate decontextualized embeddings on math information
retrieval.

#### Reproducibility

📖{Docker|first}

The experimental code for our experiments is [available on GitHub][10]
and as [a Docker image.][11]

 [10]: https://github.com/MIR-MU/regemt
 [11]: https://hub.docker.com/r/miratmu/regemt

### Joint Token and Sentence Embeddings of Text and Math
\label{sec:joint-token-embeddings-of-text-and-math}

📖{general model}
📖{joint modeling}

In 2018, @krstowski2018equation used the general shallow log-bilinear language
model of @rudolph2016exponential to train joint token embeddings of text and
math. They showed that they their embeddings were more accurate than the token
embeddings of other general shallow log-bilinear language models on a language
modeling task. However, they did not evaluate their token embeddings on math
information retrieval.

📖{general model}
📖{word analogy}

In 2017, @bojanowski2017enriching developed the fastText subtoken shallow
log-bilinear language model and demonstrated that it was more accurate than general
shallow log-bilinear language models on word analogy tasks. However,
@krstowski2018equation did not use a subtoken language model to train their
token embeddings, nor did they compare their token embeddings to subtoken
language models.

📖{dense attention}
📖{Sentence BERT}
📖{joint modeling}
📖{sentence embedding}

In 2019, @reimers2019sentencebert developed the Sentence BERT deep neural
network language model and achieved state-of-the-art accuracy with its sentence
embeddings on semantic text similarity tasks. However, they did not evaluate
their sentence embeddings on information retrieval because the Sentence BERT
uses dense attention and therefore can only embed short sentences, as opposed to the
full-length documents that are commonly used in information retrieval tasks.
Additionally, Sentence BERT has never been used to train joint sentence
embeddings of text and math or evaluated on math information retrieval.

#### Experiments

📖{find answers}
📖{joint modeling}
📖{sentence embedding}

In 2020 and 2021, we developed two primary math information retrieval
systems[^12] using joint token and sentence embeddings of text and math for the
*find answers* task of the ARQMath-1 and ARQMath-2 labs:

📖{soft vector space model}

- *MIRMU-SCM* [@novotny2020three, Section 4; @novotny2021ensembling, Section
  3.3], which was based on a soft vector space model with token embeddings from
  a fastText subtoken shallow log-bilinear language model, and

    📖{Sentence BERT}
    📖{sentence embedding}

- *MIRMU-CompuBERT* [@novotny2020three, Section 6; @novotny2021ensembling, Section
  3.4], which was based on the sentence embeddings[^13] of the Sentence BERT deep
  neural network language model.

 [^12]: See Section \vref{sec:topological-math-representations} for a
        discussion on the math representation that we have developed for our
        systems.

 [^13]: The questions and answers in the answer retrieval task are short,
        which made it feasible to use the Sentence BERT model despite its
        dense attention.

#### Results

📖{hard vector space model}
📖{joint modeling}

At ARQMath-1, we showed that *MIRMU-SCM* was more accurate than any other
individual information retrieval system from our MIRMU team, including
*MIRMU-MIaS* of the European Digital Mathematical Library, which is based on
the hard vector space model and does not use token embeddings. This shows that
using joint token embeddings of text and math from subtoken language models
improves accuracy on math information retrieval. We also showed that
*MIRMU-SCM* was among the top three most accurate primary systems in the
competition. *MIRMU-CompuBERT* suffered from a lack of accurate training data
and failed to reach accuracy that would have been significantly better than zero at
ARQMath-1.

📖{joint modeling}
📖{Sentence BERT}
📖{sentence embedding}

At ARQMath-2, we showed that with accurate training data, *MIRMU-CompuBERT*
was more accurate than *MIRMU-SCM*. @mansouri2021dprl [Section 3] also used
sentence embeddings of Sentence~BERT in two of their three math information
retrieval systems (*DPRL-RRF* and *DPRL-QASim*) and achieved the second and
third best accuracies in the competition. This shows that using joint sentence
embeddings of text and math from deep neural network language models is
well motivated and improved accuracy on math information retrieval.

#### Reproducibility

The experimental code for both [MIRMU-SCM][1] and [MIRMU-CompuBERT][2] from the
ARQMath-1 lab is available on GitHub. The experimental code for all our systems
from the ARQMath-2 lab is [available online.][14]

 [14]: https://mir.fi.muni.cz/arqmath-2021

## Score Aggregation of Structural and Semantic Matching Techniques
\label{sec:score-aggregation-of-structural-and-semantic-matching-techniques}

📖{structural matching}
📖{semantic matching}
📖{soft vector space model}
📖{word mover's distance}

Techniques for structural and semantic matching such as the soft vector space
model and the word mover's distance make naive assumptions about the syntax and semantics of the natural language and the language of mathematics. They
also often rely on token embeddings of language models, which contain
human-like biases. [@caliskan2017semantics] These assumptions and biases cause
systematic errors and decrease accuracy on machine translation evaluation
tasks.

#### Experiments

📖{score aggregation}
📖{structural matching}
📖{semantic matching}

In 2021, we used score aggregation to ensemble 16 diverse structural and
semantic matching techniques in order to avoid the systematic errors of any
individual technique. [@stefanik2021regressive, Section 3.3] We trained and
evaluated our ensemble on twenty machine translation evaluation tasks.

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
: [Accuracies of our ensemble and the best individual techniques %
   on twenty machine translation evaluation tasks]%
  {The accuracies of the our ensemble (RegEMT) and the best individual techniques %
   on twenty machine translation evaluation tasks. [@stefanik2021regressive] %
   Language pairs marked with an × were evaluated cross-lingually. %
   [@stefanik2021regressive, Section 3.4]}%
  \label{tab:regemt}

#### Results

📖{score aggregation}
📖{semantic matching}

We showed that our ensemble was more or equally accurate than any individual
technique on 15 out of 20 machine translation evaluation tasks, which confirms
that score aggregation is a useful technique for avoiding naive assumptions and
biases of individual semantic matching techniques. See Table \vref{tab:regemt}.

#### Future Work

Future work should evaluate the accuracy of our ensemble on math information
retrieval.

#### Reproducibility

📖{Docker}

The experimental code for our experiments is [available on GitHub][10]
and as [a Docker image.][11]

## Rank-Based Fusion of Math Information Retrieval Systems
\label{sec:rank-based-fusion}

📖{structural matching}
📖{semantic matching}

The systematic errors of structural and semantic matching techniques also
affect the ranking of results in information retrieval systems. As a result,
different information retrieval systems can agree on a small portion of the
most relevant documents, but each individual system will miss the great
majority of relevant documents.

📖{rank-based fusion}
📖{reciprocal rank fusion}

In 2009, @cormack2009reciprocal developed the reciprocal rank fusion technique and
showed that it achieved better accuracy than any individual system on five out
of five information retrieval tasks. It achieved state-of-the-art
accuracy on three out of five information retrieval tasks. However, they did
not evaluate their rank-based fusion technique on math information retrieval.

#### Experiments

📖{find answers}
📖{rank-based fusion}

In 2020, we developed the *MIRMU-Ensemble* rank-based fusion technique for the
*find answers* task of the ARQMath-1 lab.  [@novotny2020three, Section 7]. We
used our technique to ensemble the primary systems of our MIRMU team.

📖{find answers}
📖{rank-based fusion}
📖{reciprocal rank fusion}

In 2021, we developed two new supervised rank-based fusion techniques
(*MIRMU-WIBC* and *MIRMU-RBC*) and used them together with our unsupervised
*MIRMU-Ensemble* (further known as *MIRMU-IBC*) and the unsupervised reciprocal
rank fusion (further known as *MIRMU-RRF*) for the *find answers*
task of the ARQMath-2 lab. [@novotny2021ensembling, Section 4] We used our
technique to ensemble all ten systems of our MIRMU and MSM teams.[^17]

 [^17]: We entered the ARQMath-2 lab as two teams: our lab (MIRMU) and the
        students of the FI:PV211 information retrieval course taught at the
        Masaryk University (MSM).

To see whether our unsupervised techniques could benefit from a large number of
systems, we also used them to ensemble all non-baseline primary systems at the
ARQMath-1 and ARQMath-2 labs. To see if our techniques could benefit from a small
number of diverse systems,[^41] we also used them to ensemble five selected
systems out of all of our ten systems at ARQMath-2.

 [^41]: See Section \vref{sec:strengths-and-weaknesses} for a discussion of how
        we selected our five diverse systems and their strengths and weaknesses.

#### Results

At ARQMath-1, we showed that *MIRMU-Ensemble* was more accurate than any
individual information retrieval system from our MIRMU team. If we used
*MIRMU-Ensemble* to ensemble all non-baseline primary systems, we would have
achieved the best accuracy in the competition, which indicates that
*MIRMU-Ensemble* can benefit from a large number of systems.

📖{rank-based fusion}

At ARQMath-2, we showed that all our four rank-based fusion techniques were
more accurate than any individual information retrieval system from our MIRMU
and MSM teams. We also showed that our *MIRMU-WIBC* can be more accurate than
*MIRMU-RRF*.

When we used the unsupervised *MIRMU-IBC* and *MIRMU-RRF* to ensemble all
non-baseline primary systems, we achieved the best (*MIRMU-RRF*) and the
second-best (*MIRMU-IBC*) accuracies in the competition. This indicates that
both techniques can benefit from a large number of systems. Ensembling only five
selected systems out of all ten systems increased the accuracy of three out
of our four techniques, which indicates that all our techniques can benefit
from a small number of diverse systems.

#### Reproducibility

The experimental code for [MIRMU-Ensemble][18] from the ARQMath-1 lab is
available on GitHub. The experimental code for all our systems from the
ARQMath-2 lab is [available online.][14]

 [18]: https://github.com/MIR-MU/SCM-at-ARQMath (file scripts/combine\\_serps.py)

## Weighted Zone Scoring Across Different Domains
\label{sec:weighted-zone-scoring}

📖{retrieval unit}
📖{weighted zone scoring}

In information retrieval, retrieval units are usually structured and contain
zones that are more important than the body text, such as the title, abstract,
and keywords, and zones that are less important than the body text, such as the
appendices, footnotes, and comments. Weighted zone scoring techniques can be
used to inform an information retrieval system about the importance of
different zones and also to improve its accuracy. [@manning2008introduction, Section 6.1]

 /figures/godwins-law.tex

#### Experiments

📖{relevance judgement}

In 2018, we analysed the datasets and the relevance judgements from the
*community question answering* information retrieval task of the SemEval-2016
and SemEval-2017 competitions. [@novotny2018weighting]
The datasets consist of questions and threads of comments. The relevance
judgements show how relevant the individual comments (subtask A) and the whole
threads (subtask B) are to a question.

📖{weighted zone scoring}

Following the empirical law of @godwin1991re, we investigated whether later
comments in a thread were less likely to be relevant. We used the results of
our investigation to develop an information retrieval system that retrieved
threads using weighted zone scoring and we evaluated the system on the subtask
B of the *community question answering* information retrieval task of
SemEval-2016 and SemEval-2017.

📖{find answers}
📖{weighted zone scoring}

In 2021, we developed three primary math information retrieval systems using
weighted zone scoring for the *find answers* task of the ARQMath-2 lab:[^17]

- *MSM-MG* [@novotny2021ensembling, Section 3.1], which represents every answer
  as its body text and the title, body text, and tags of its parent
  question, all with equal weights

- *MSM-PZ* [@novotny2021ensembling, Section 3.2], which represents every answer
  as its body text and the title and the tags of its parent question, where the
  title and the tags of the parent question are given three times as much
  weight as the body text of the answer, and

- *MIRMU-SCM* [@novotny2021ensembling, Section 3.3], which represents every
  answer as its body text and the title, body text, and tags of its parent
  question, where the title of the parent question is given three times as much
  weight as any of the remaining parts.

📖{weighted zone scoring}

← To show how important weighted zone scoring was, we compared the *MSM-MG*,
*MSM-PZ*, and *MIRMU-SCM* systems to the remaining six systems of the MSM team
that used similar information retrieval techniques but did not use weighted
zone scoring.

#### Results

We showed that in the datasets from the *community question answering*
information retrieval task of the SemEval-2016 and SemEval-2017 competitions,
later comments were significantly less likely to be relevant and their
relevance followed a Zipf distribution. See Figure \vref{fig:weighted-zone-scoring}.

📖{weighted zone scoring}

We then developed three information retrieval systems: one that used weighted
zone scoring to assign smaller weight to later comments in a thread, one that
did not use weighted zone scoring but continuously decreased the weights of later
tokens in a thread, and one that used neither. [@novotny2018weighting, Figure 3]
We showed that our system that used weighted zone scoring achieved
state-of-the-art accuracy on both datasets, whereas our two systems that did
not use weighted zone scoring achieved worse-than-baseline accuracy on both
datasets. [@novotny2018weighting, tables 1 and 2] This shows that weighted
zone scoring can be important for information retrieval.

📖{find answers}
📖{weighted zone scoring}

We also showed that our three math information retrieval systems that used
weighted zone scoring all achieved better accuracy than our six systems that
did not on the *find answers* task of the ARQMath-2 lab. This shows that
weighted zone scoring can be important for math information retrieval.

#### Future Work

📖{weighted zone scoring}

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
time constraints, even these formal ranking criteria are usually approximated.

In this section, I will report the results of my experiments in making
approximate math information retrieval more accurate.

- In Section \vref{sec:dense-retrieval-in-inverted-indices-accuracy}, I will
  show that dense retrieval can be approximated with almost no loss in
  accuracy.

- In Section \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy},
  I will show that sparse retrieval can be approximated in a way that increases
  both speed and accuracy.

### Dense Retrieval in Inverted Indices
\label{sec:dense-retrieval-in-inverted-indices-accuracy}

📖{dense retrieval}
📖{find answers}

Recent results show that dense retrieval techniques can achieve higher accuracy
than sparse retrieval techniques on information retrieval tasks.
[@lin2021batch] At the *find answers* task of the ARQMath-1 and ARQMath-2 labs,
sparse information retrieval systems still achieved the best accuracy, but the
gap is closing. At ARQMath-1, only three[^19] out of 23 systems (13\%)
used dense retrieval, with the best dense retrieval system only receiving the
seventh best accuracy in the competition.  [@zanibbi2020overview, Table A1] At
ARQMath-2, nine[^20] out of 40 systems (23\%) used dense retrieval, with the
best dense retrieval systems receiving the second and third best accuracies in
the competition. [@behrooz2021overview, Table 4]

 [^19]: The three systems by decreasing accuracy: *PSU1*, *PSU3*, and *MIRMU-CompuBERT*.

 [^20]: The nine systems by decreasing accuracy: *DPRL-MathSE*, *DPRL-QASim*,
        *TU\\_DBS\\_P*, *TU\\_DBS\\_A2*, *TU\\_DBS\\_A3*, *TU\\_DBS\\_A1*, *MIRMU-CompuBERT*,
        *PSU*, and *TU\\_DBS\\_A4*.

📖{inverted index}

Digital mathematical libraries use industry-grade inverted indices such as
ElasticSearch, Solr, and Lucene for sparse retrieval. However, as dense
retrieval is becoming more accurate than sparse retrieval, libraries face the
economic dilemma of either being inaccurate or replacing their tried-and-tested
sparse retrieval architectures with dense vector databases.

#### Experiments

📖{dense retrieval}
📖{inverted index}

In 2017, we showed how dense embeddings can be encoded as sparse vectors.
[@rygl2017semantic, Section 2.2] Using our encoding, we developed a dense
information retrieval system that used an inverted index as its sparse vector
database, and we evaluated how accurate it was at the retrieval of the dense
embeddings of tokens, sentences, and image descriptors. [@rygl2017semantic,
Section 3.1; @ruzicka2017flexible, Section 3]

#### Results

📖{dense retrieval}
📖{query time}

We showed that our system could perform approximate dense retrieval with almost
no loss in accuracy. [@rygl2017semantic, Table 2; @ruzicka2017flexible, Section 4]
We also showed that both our encoding and our system can be configured to trade
off accuracy for speed and perform fast approximate dense retrieval[^37]
[@rygl2017semantic, Figure 1; @ruzicka2017flexible, figures 1--14]. See also
Section \vref{sec:dense-retrieval-in-inverted-indices-speed}.

 [^37]: See Section \vref{sec:dense-retrieval-in-inverted-indices-speed} for a
        discussion about the trade-offs between speed and accuracy.

📖{dense retrieval}

Our results showed that the dilemma of the digital mathematical libraries is
false. Using our system, libraries can use more accurate dense retrieval
without replacing their tried-and-tested sparse retrieval architectures.

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy}

📖{hard vector space model}

State-of-the-art math information retrieval systems rely on the hard
vector space model of @salton1975vector, which is fast and
interpretable but whose accuracy is limited by its failure to recognize that
the meaning of different words and symbols can be similar or otherwise related.

📖{soft vector space model}

In 2014, @sidorov2014soft developed the soft vector space model, which can
recognize the semantic relatedness of words and symbols. However, this model was not practically fast and it required a measure of relatedness between words and symbols.
Furthermore, the soft vector space model had not been evaluated on math
information retrieval.

In 2017, @charlet2017simbow developed two approximate measures of relatedness
between words and symbols based on the edit distance and on token embeddings
and they achieved state-of-the-art accuracy on a semantic text similarity task.

📖{general model}
📖{quantized model}
📖{soft vector space model}
📖{word analogy}

In 2018, @lam2018word2bits developed the Word2Bits quantized language model
and showed that it was more accurate than the Word2Vec language model
on the English word similarity and open-domain question answering tasks and
equally as accurate on the English word analogy task. The token embeddings of
the quantized model could make the soft vector space model practically fast.
However, the soft vector space model with the quantized token embeddings
had never been evaluated.

#### Experiments

📖{soft vector space model}

In 2018, I developed an algorithm for the soft vector space model that achieved
practical speed[^21] by placing restrictions on the measures of relatedness
that the soft vector space model could use. [@novotny2018implementation,
Section 3]

 [^21]: See Section \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}
        for a discussion about our improvements to the speed of the soft vector
        space model.

📖{quantized model}
📖{orthogonalization|first}
📖{soft vector space model}

In 2020, my colleagues and I developed an *orthogonalization* technique for token embeddings and
we proved that using the measure of @charlet2017simbow with orthogonalized
embeddings was consistent with the restrictions of my fast algorithm for the
soft vector space model. [@novotny2020text, Section 4.2] We evaluated the soft
vector space model with quantized token embeddings and with orthogonalized token
embeddings on six text classification tasks. [@novotny2020text, Figure 8]
We also reported the optimal hyperparameter values for our orthogonalization
algorithm on all six tasks. [@novotny2020text, Table 2]

📖{hard vector space model}
📖{find answers}
📖{soft vector space model}

In 2020 and 2021, we evaluated the soft vector space model on the *find
answers* task of the ARQMath-1 and ARQMath-2 labs to see if the soft vector
space model would be more accurate than the hard vector space model.
[@novotny2020three, Section 4; @novotny2021ensembling, Section 3.3] We also
reported the optimal hyperparameter values for our orthogonalization algorithm.
[@novotny2020three, Table 1]
 
 [^22]: See Section \vref{sec:joint-token-embeddings-of-text-and-math} for a
        discussion about the joint token embeddings of text and math that we
        developed for soft vector space model in the ARQMath-1 and ARQMath-2 labs.

#### Results

📖{hard vector space model}
📖{quantized model}
📖{soft vector space model}

We showed that quantized and (especially) orthogonalized token embeddings
did not decrease the accuracy of the soft vector space. In fact, they actually improved
it on five out of six text classification tasks. We also showed that the soft
vector space model with any of our token embeddings was more accurate than the
hard vector space model on all six tasks.

📖{hard vector space model}
📖{soft vector space model}

We also showed that the soft vector space model can be more accurate than math
information retrieval systems that used similar information retrieval techniques
but used the hard vector space model instead of the soft vector space model.
For example, at ARQMath-1, the soft vector space model (*MIRMU-SCM*) was more
accurate than *MIRMU-MIaS* of the European Digital Mathematical Library, and
at ARQMath-2, the soft vector space model was more accurate than five systems
of the MSM team.[^23]

 [^23]: *MIRMU-SCM* is more accurate even when we disable weighted zone scoring,
        which the five systems of the MSM team did not use. See also
        Section \ref{sec:weighted-zone-scoring}.

#### Future Work

📖{BM25}
📖{soft vector space model}
📖{TF-IDF}

At the ARQMath-1 and ARQMath-2 labs, state-of-the-art accuracy was achieved
by systems that used the BM25 and BM25⁺ term weighting schemes, whereas our
soft vector space model used the TF-IDF term weighting scheme. Future work
should study in more detail the impact of term weighting schemes on the
accuracy of the soft vector space model on math information retrieval.

#### Reproducibility

📖{soft vector space model}

I have released [the only public implementation of the soft vector space model][24]
in the free open-source Gensim NLP library [@rehurek2010software] through a
series of pull requests on GitHub. [@novotny2018implementa;
@novotny2018implementb; @novotny2020reduce; @novotny2021use]

📖{soft vector space model}

The experimental code for the soft vector space model on the text classification
tasks is [available on GitHub.][25]

📖{soft vector space model}

The experimental code for the soft vector space model from the ARQMath-1 lab is
[available on GitHub.][1] The experimental code for all our systems from the
ARQMath-2 lab is [available online.][14]

 [24]: https://radimrehurek.com/gensim/similarities/termsim.html
 [25]: https://github.com/MIR-MU/regularized-embeddings

# Speed {#speed}
\label{cha:speed}

The accuracy as well as the availability of more advanced features in a math
information retrieval system are fundamentally limited by the *speed* at which
we can deploy the system, store new information in it and recall information
that satisfy the needs of its users.

In this chapter, I will report the results of my research on representations
and techniques that enable fast math information retrieval:

📖{deployment time}

- In Section \vref{sec:fast-and-reproducible-deployment}, I will describe my
  work in making math information retrieval search engines easier to build and
  deploy.

- In Section \vref{sec:representation-learning-speed}, I will describe my
  work in making neural representations of natural language words and math
  symbols faster to both learn and use.

- In Section \vref{sec:approximate-nearest-neighbor-search-speed}, I will
  describe my work in developing fast approximate search techniques.

## Fast and Reproducible Deployment of Math Information Retrieval Systems Using Docker and Continuous Integration
\label{sec:fast-and-reproducible-deployment}

📖{Docker}

Docker is a lightweight container-based virtualization tool, which makes it
possible to distribute software together with all its dependencies as a single
image. Additionally, processes running in Docker are comparable in speed to
regular non-virtualized processes.

In software engineering, continuous integration is the practice of merging the
work of all developers to a shared repository several times a day. To maintain
code quality, best practices for continuous integration include automating the
builds and making the builds self-testing.

📖{deployment time}

In 2018, we publicly released our MIaS search engine of the European
Digital Mathematical Library [on GitHub.][28] [@sojka2018mias] We also
developed a solution for deploying MIaS, made [available on
GitHub.][26] However, our solution was slow because we always needed to build
MIaS during the deployment. Additionally, our builds would suddenly fail due to
the changing code base of MIaS and a lack of code quality control. Furthermore,
our solution had been written in the GNU Make language, which was difficult to
maintain.

 [26]: https://github.com/MIR-MU/ntcir-mias-deployment
 [28]: https://github.com/MIR-MU/WebMIaS

↑ In 2020, we developed a new solution for deploying MIaS at the ARQMath-1
lab and made it [available on GitHub.][27] Our new solution was written in
Python, which was easier to maintain than GNU Make. However, our new solution
was still slow and our builds would still suddenly fail.

 [27]: https://github.com/MIR-MU/ARQMath-eval (file docs/MIaS-at-ARQMath.ipynb)

#### Experiments

📖{deployment time}
📖{Docker}

In 2021, we have developed a new solution for deploying MIaS using Docker
continuous integration. [@luptak2021webmias] [^29] With Docker, our solution
is no longer slow because we do not need to build MIaS during the deployment,
but we can download a pre-built Docker image. With continuous integration, our
builds would no longer suddenly fail during the deployment because any change to the
code base would build and test MIaS immediately.

 [^29]: For continuous integration, we have used the GitHub Actions service,
        which offers a free plan to public open-source repositories.

#### Reproducibility

📖{Docker}

Our fast deployment solution for MIaS is [available on GitHub][30] and as [a
Docker image.][31]

 [30]: https://github.com/MIR-MU/WebMIaS
 [31]: https://hub.docker.com/r/miratmu/webmias/tags

## Representation Learning of Words and Symbols for Sparse Retrieval
\label{sec:representation-learning-speed}

📖{structural matching}
📖{semantic matching}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate but also fast to train and use.

In this section, I will report the results of my experiments in learning fast
token embeddings:

📖{general model}

- In Section \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic},
  I will show that quantizing the Word2Vec language model makes it possible to
  compute a fast and accurate bitwise approximation of the similarity between
  a pair of token embeddings.

- In Section \vref{sec:position-independent-token-embeddings-speed}, I will show
  that constraining the fastText positional language model improves its
  training speed.

### Quantized Token Embeddings with Fast Bitwise Arithmetic
\label{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}

📖{quantized model}
📖{structural matching}
📖{semantic matching}

Math information retrieval techniques for structural and semantic matching rely
on fast measures of similarity between token embeddings of natural language
words and math symbols. However, common similarity measures such as the cosine
similarity require expensive floating point multiplication and division.

📖{general model}
📖{quantized model}
📖{word analogy}

In 2018, @lam2018word2bits developed the Word2Bits quantized language model and
showed that it was more accurate than the Word2Vec language model on the
English word similarity and open-domain question answering tasks and equally as
accurate on the English word analogy task. With the quantized language model, we
can approximate the similarity between a pair of token embeddings using fast
bitwise arithmetic. However, the speed and accuracy of these approximate
similarity measures had not been evaluated.

#### Experiments

📖{cosine similarity}
📖{quantized model}
📖{word analogy}

In 2019, I consulted on the master's thesis of @stefanik2019semantic.[^33] In the
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

📖{cosine similarity}
📖{quantized model}
📖{structural matching}
📖{semantic matching}
📖{word analogy}

We showed that our bitwise approximation can be more than 20× faster than the
float arithmetic. We also showed that our approximation was within 2\%
accuracy of the cosine similarity on the English word analogy task. See
Figure \vref{fig:quantized-token-embeddings-with-fast-bitwise-arithmetic}. This
shows that quantized token embeddings can be used for fast structural and
semantic matching.

#### Future Work

Future work should evaluate our approximation on math information retrieval.

#### Reproducibility

📖{word analogy}

Our implementation of the word analogy task using fast bitwise arithmetic is
[available on GitHub.][34]

 [34]: https://github.com/witiko/word2bits (branch non-binary-and-bitwise-compute-accuracy)

The experimental code for all our experiments is [available online.][8]

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-speed}

📖{positional model}
📖{word analogy}

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate than the fastText subtoken language model
on the English word analogy task. However, they did not release a public
implementation of the positional model or show that the model was practically
fast to train.

#### Experiments

📖{positional model}
📖{training time}
📖{word analogy}

In 2020, I implemented the positional model[^35] and evaluated whether it could
achieve better accuracy than the subtoken language model on the English word
analogy task when we trained both models for an equal amount of time.
[@novotny2020art, Table 3]

 [^35]: See Section \vref{sec:variable-control-in-token-embedding-evaluation}
        for a discussion on my implementation of the positional model.

📖{positional model}
📖{constrained positional model}
📖{training time}
📖{word analogy}

In 2021, we developed the fastText constrained positional model,[^36] which was
less complex than the positional model and achieved better accuracy than
the positional model on word analogy and language modeling tasks.
We compared the training time of both models. [@novotny2021when, Section 4]

 [^36]: See Section \vref{sec:position-independent-token-embeddings-accuracy}
        for a discussion about the development and accuracy of the constrained
        positional model.

#### Results

📖{positional model}
📖{constrained positional model}
📖{training time}
📖{word analogy}

We showed that the positional model could achieve 4\% better accuracy than the
subtoken language model on the English word analogy task when we trained both
models for five hours. We also showed that our constrained positional model
could be trained 2× faster than the positional model.[^1]

 [^1]: See Section \vref{sec:position-independent-token-embeddings-interpretability}
       for a discussion on the interpretability of the positional model.

#### Reproducibility

📖{positional model}
📖{constrained positional model}

[The experimental code for all my experiments is available GitHub.][4] I have
also released [the only public implementation of the fastText positional model
and our fastText constrained positional model.][5]

## Approximate Nearest Neighbor Search in Vector Spaces
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

📖{dense retrieval}
📖{inverted index}

Recent results show that dense retrieval techniques can achieve higher accuracy
than sparse retrieval techniques on information retrieval tasks. However, digital
mathematical libraries use inverted indices for sparse retrieval and replacing
them with dense vector databases has many costs.

📖{dense retrieval}
📖{inverted index}
📖{query time}

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
        a discussion on the development and accuracy of our system.

 /figures/scaletext-speed.tex

#### Experiments

📖{query time}

We proved that our encoding improved the time complexity of the retrieval
compared to naive search and that removing unimportant features improved both
time and space complexity of the retrieval.  [@rygl2017semantic, Table 1]
We also evaluated the speed of several configurations of our encoding and our
system.

#### Results

We showed that there existed an optimal configuration of our encoding and our
system, where our system was both fast and accurate. See Figure
\ref{fig:dense-retrieval-in-inverted-indices-speed}.

### Sparse Retrieval in Inverted Indices and Vector Databases
\label{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}

📖{soft cosine similarity}
📖{hard vector space model}
📖{soft vector space model}

State-of-the-art math information retrieval systems rely on the hard
vector space model of @salton1975vector, but the soft vector space model
of @sidorov2014soft can more accurately model both the natural language and the
language of mathematics. However, computing the soft cosine similarity between two
documents in the soft vector space model had an impractical worst-case time
complexity of $\mathcal{O}(n^2)$ for a vocabulary of $n$ words and symbols.
Additionally, converting the soft vector space model to a hard vector space
model using Gaussian elimination has an impractical worst-case time complexity
of $\mathcal{O}(n^4)$.

📖{soft vector space model}

In 2018, I developed an algorithm for the soft vector space model that could
achieve practical speed by placing restrictions on the measures of relatedness
that the soft vector space model could use. [@novotny2018implementation,
Section 3] In 2020, we showed that our restricted model was more accurate[^39]
than the non-restricted model on five out of six text classification tasks.
[@novotny2020text, Figure 8] In this section, I will discuss the speed of
my restricted soft vector space model in detail.

 [^39]: See Section \ref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy}
        for a discussion about the development and accuracy of the soft vector space model.

#### Experiments

📖{soft cosine similarity}
📖{soft vector space model}

In 2018, I proved that if we restricted the soft vector space model to measures
of relatedness that could be represented by a sparse similarity matrix $S$ with
no more than $C$ non-zero elements in any of its columns, where $C$ was constant,
then computing the soft cosine similarity between a query and a document in the soft vector
space model had a worst-case time complexity of $\mathcal{O}(1)$ if the maximum
size of a query was constant. [@novotny2018implementation, Algorithm 1]

📖{hard vector space model}
📖{soft vector space model}

Additionally, I proved that if $S$ was symmetric and positive definite, the
soft vector space model could be converted to a hard vector space model using
Cholesky factorization, which had a more practical worst-case time complexity
of $\mathcal{O}(n^3)$. [@novotny2018implementation, Section 3]

📖{hard vector space model}
📖{inverted index}

Furthermore, I developed transformations that allowed the use of the soft
vector space model in vector databases and inverted indices that only supported
the hard vector space model and I proved the correctness of my transformations.
[@novotny2018implementation, Section 4]

In 2020, we compared the query time of the restricted model and the
non-restricted model on six text classification tasks. [@novotny2020text,
Figure 9]

In 2021, we evaluated the query time of the restricted model on the *find
answers* task of the ARQMath-1 and ARQMath-2 labs. [@novotny2020three,
Table 5; @novotny2021ensembling, Figure 11]

#### Results

We showed that the restricted model was three times slower than the non-restricted model
on the six text classification tasks. We theorized that this was because the
restricted model had been implemented using slower sparse matrix operations
whereas the non-restricted model had been implemented using faster dense matrix
operations. We offer further explanation below.

📖{find answers}

We also showed that the restricted model was the slowest in query time of all
our systems on the *find answers* task of the ARQMath labs.

#### Future Work

📖{soft cosine similarity}

My algorithm for computing the soft cosine similarity $\vec{x}ᵀ · S · \vec{y}$
between a query $\vec{x}$ and a document $\vec{y}$ in the restricted model with
$\mathcal{O}(1)$ worst-case time complexity assumed that we would compute the
similarity as a single operation. This would allow us to eliminate any index
$i$ and $j$ where $x\_i = 0, y\_i = 0,$ or $S\_{ij} = 0$ in the following
summation:%
%
\begin{equation}
\vec{x}ᵀ · S · \vec{y} = Σ\_i Σ\_j x\_i · S\_{ij} · y\_j.
\end{equation}

However, my implementation of the restricted model used the SciPy Python
library, which would separate $\vec{x}ᵀ · S · \vec{y}$ into two
operations, $\vec{x}ᵀ · S$ and $◌ · \vec{y}$, where the worst-case time
complexity of both operations was $\mathcal{O}(n)$ if the size of query
$\vec{x}$ was constant. Future work should investigate how to improve my
implementation to achieve the theoretical $\mathcal{O}(1)$ worst-case time
complexity.[^4]

 [^4]: For more information about my implementation of the soft cosine
       similarity and its limitations, see a discussion at [the Computer
       Science StackExchange question answering website.][6]

 [6]: https://cs.stackexchange.com/a/144284/12931

#### Reproducibility

I have released [the only public implementation of the restricted soft vector
space model][24] in the free open-source Gensim NLP library
[@rehurek2010software] through a series of pull requests on GitHub.
[@novotny2018implementa; @novotny2018implementb; @novotny2020reduce;
@novotny2021use]

📖{soft vector space model}

The experimental code for the soft vector space model on the text classification
tasks is [available on GitHub.][25]

📖{soft vector space model}

The experimental code for the soft vector space model from the ARQMath-1 lab is
[available on GitHub.][1] The experimental code for all our systems from the
ARQMath-2 lab is [available online.][14]

# Interpretability {#interpretability}
\label{cha:interpretability}

In math information retrieval, accuracy and speed ensure that we can recall
information that a user needs when they need them. However, the ability of a
system to explain the information to the user is fundamentally limited by the
*interpretability* of its internal representations.

In this chapter, I will report the results of my research on interpretable
representations that enable explainable math information retrieval:

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

📖{structural matching}
📖{semantic matching}

Math information retrieval techniques for structural and semantic matching rely
on neural representations (token embeddings) of natural language words and math
symbols that are not only accurate but also interpretable.

In this section, I will report the results of my experiments in learning
interpretable token embeddings:

📖{general model}

- In Section \vref{sec:formal-concept-analysis-with-quantized-token-embeddings},
  I will illustrate how quantizing the Word2Vec language model makes it possible to
  determine type-of relations between tokens.

- In Section \vref{sec:position-independent-token-embeddings-interpretability},
  I will show that the fastText positional language model makes it possible to
  determine grammatical properties of tokens.

### Formal Concept Analysis with Quantized Token Embeddings
\label{sec:formal-concept-analysis-with-quantized-token-embeddings}

📖{formal concept analysis}
📖{quantized model}

In 1982, @wille1982restructuring proposed the formal concept analysis as a
reformulation of the lattice theory that focused on objects and their binary
attributes. The binary features of the Word2Bits quantized language model
of @lam2018word2bits can be studied using the formal concept analysis.

📖{formal concept analysis}

In 2015, @priss2005linguistic noted that formal concept analysis can be
used to encode and analyze type-of (hyponymy-hyperonymy) token relations.
Parts of speech are a type-of relation. However, the quantized language model
has not been evaluated on a part-of-speech tagging task.

 /figures/word2bits-implications.tex

#### Experiments

📖{vector dimensionality $D$}
📖{quantized model}

In 2019, I consulted on the master's thesis of @stefanik2019semantic.
<!-- See also Sections \vref{sec:quantized-token-embeddings}
and Section \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic}, where I
discuss our further experiments with the quantized language model. -->
In the thesis, we evaluated the quantized model on an English part-of-speech
tagging task: For each part-of-speech tag, we reported the most well-correlated
token embedding feature. To see if the part-of-speech relation would be
eventually distributed between different features, we repeated the experiment
for different vector dimensionalities $D$. [@stefanik2019semantic, Section 3.3]

We also developed two algorithms for deciding whether a feature of a token
embedding should be negated so that its value was one if, and only if, the token
had the *interesting property*[^81] that was associated with the feature. We used our
algorithms to show the probabilities $P(i ⇒ j)$ of the implications $i ⇒ j$
between the interesting properties $i$ and $j$ associated with the token
embedding features. [@stefanik2019semantic, Section 3.7]

 [^81]: A property of a token is interesting if its presence is more surprising
        than its absence. For example, the token *blue* has the interesting
        property that is is a color. Therefore, if the embedding of *blue* has
        a feature for being a color, its value should be one.

#### Results

📖{vector dimensionality $D$}

We showed that for vector dimensionality $D = 30$, nouns, verbs, and
adjectives are significantly correlated with their token embedding feature.
The correlations decrease as the vector dimensionality $D$ increases and they become insignificant for $D\geq 600$, which indicates that the part-of-speech
relation has been distributed between different token embedding features.

We show the probabilities $P(i ⇒ j)$ produced by one of our algorithms in
Figure \vref{fig:formal-concept-analysis-with-quantized-token-embeddings}.

#### Future Work

📖{formal concept analysis}
📖{quantized model}

Future work should investigate whether formal concept analysis can be used to
extract type-of token relations from quantized token embeddings.

#### Reproducibility

The experimental code for all our experiments is [available online.][8]

### Position-Independent Token Embeddings
\label{sec:position-independent-token-embeddings-interpretability}

📖{word analogy}

In 2013, @mikolov2013efficient showed that token embeddings of shallow
log-bilinear language models were useful for analogical reasoning. However,
the reason for their usefulness was unclear until 2014, when @levy2014neural
showed that the token embeddings factorized the pointwise mutual information
matrix. This helped interpret token embeddings of shallow log-bilinear language
models and demystified their properties.

📖{positional embedding}
📖{word analogy}

In 2018, @mikolov2018advances developed the fastText positional language model
and showed that it was more accurate[^40] than the fastText subtoken language
model on the English word analogy task. In 2021, we showed that the positional
model was more accurate than the subtoken model on an English word analogy task.
This showed that positional embeddings played an important role in the accuracy
of shallow log-bilinear language models. However, their exact function was unclear.

 [^40]: See Section \vref{sec:position-independent-token-embeddings-accuracy}
        for a discussion about the accuracy of the positional model.

📖{context window}
📖{positional model}
📖{constrained positional model}

In 2021, we showed that the positional model could benefit from larger context
sizes than the subtoken language model. [@novotny2021when, Table 1] However, the
reason for this was unclear.

#### Experiments

📖{positional embedding}
📖{positional model}

In 2021, we developed two measures for the importance of positions and one
measure for the importance of tokens. [@novotny2021when, Section 3.4]
We used our measures to explain the role of positional embeddings in the
positional model. [@novotny2021when, Section 4.3]

 /figures/positional-features.tex

#### Results

📖{context window}
📖{positional embedding}
📖{positional model}

We showed that the English positional model used the positional embeddings to
attenuate distant context words. [@novotny2021when, Figure 4] This shows why
the positional model could use larger context window sizes than the subtoken
language model.

📖{clustering}
📖{positional embedding}
📖{positional model}

We also showed that there were three distinct clusters of features in the
positional embeddings of the English positional model. We called the clusters
*antepositional*, *postpositional*, and *informational*, see Figure
\vref{fig:position-independent-token-embeddings-interpretability}.
For each cluster, we gave examples of tokens whose importance was maximized by
the positional features in the cluster:

- *antepositional*: in, for, and coca
- *positional*: ago, else, and cola
- *informational*: finance, sports, and politics

📖{positional embedding}

← This shows that the positional embeddings can be used to classify tokens by
their grammatical properties.

#### Future Work

📖{positional embedding}
📖{positional model}

Future work should inspect the positional embeddings of the positional model on
languages other than English.

#### Reproducibility

📖{positional model}

The experimental code for all our experiments is [available on GitHub][5]
together with the only public implementation of the positional model.

## Strengths and Weaknesses of Math Information Retrieval Systems
\label{sec:strengths-and-weaknesses}

📖{find answers}
📖{rank-based fusion}

In 2021, we used two unsupervised rank-based fusion[^42] techniques for the
*find answers* task of the ARQMath-2 lab [@novotny2021ensembling, Section 4] to
ensemble all ten systems of our teams.  We were also interested in whether our
techniques could benefit from a small number of diverse systems. However, it
was unclear how to select a small number of diverse systems and interpret their
diversity.

 [^42]: See Section \vref{sec:rank-based-fusion} for the discussion of the
        development and accuracy of our rank-based fusion techniques.

#### Experiments

📖{clustering}

First, we used the correlations between the rankings of results from our
systems to cluster the systems. Then, we used the Silhouette score to select
the optimal number of clusters. [@novotny2021ensembling, Figure 7] Finally,
we selected a random representative from each cluster. These representatives
were our small number of diverse systems.

To interpret the diversity of the systems, we developed two measures of their
strengths and weaknesses that showed… [@novotny2021ensembling, Figure 8]

📖{math query}

1. how capable the systems were at text and math queries and
2. how capable the systems were at long and short queries.

 /figures/strengths-and-weaknesses.tex

#### Results

We selected five diverse systems: *MIRMU-CompuBERT*, *MIRMU-SCM*, *MSM-LM*,
*MSM-MP*, and *MSM-VS*.[^43]

 [^43]: See Section \vref{sec:joint-token-embeddings-of-text-and-math}
        for a discussion of the *MIRMU-CompuBERT* and *MIRMU-SCM* systems
        and their accuracy.

📖{context window}
📖{dense attention}
📖{structural matching}
📖{semantic matching}
📖{math query}
📖{text query}

We also showed that *MIRMU-CompuBERT* achieved the most consistent results
across both text and math queries, it excelled at short queries, but its
performance deteriorated for long queries that didn't fit into the short
context window of its dense attention. By contrast, the remaining four systems
that were based on the vector space model excelled at text queries, but their
performance deteriorated for math queries. *MIRMU-SCM*, which uses
techniques for structural and semantic matching, could exploit both short and
long queries. See Figure \vref{fig:strengths-and-weaknesses}.

#### Reproducibility

The experimental code for all our systems from the ARQMath-2 lab is [available
online.][14]

## Interactive Visualizations of Math Information Retrieval Collections
\label{sec:interactive-visualizations}

📖{Mathematical REtrieval Collection (MREC)}

In 2011, @liska2011web developed the MREC dataset for the Math Information
Retrieval workshop. In 2014, @hanel2014vizualizace developed an interactive
visualization for the exploration of the MREC dataset. However, the interactive
visualization ignored math and modeled only text.

📖{find answers}

In 2020, a Math StackExchange dataset was released for the ARQMath-1 lab. On
the *find answers* task of ARQMath-1, we showed that our *MIRMU-SCM*
system[^44] was more accurate than any other individual information retrieval
system from our MIRMU team. Additionally, *MIRMU-SCM* modeled both text and
math. However, no interactive visualization for exploring the Math
StackExchange dataset using the *MIRMU-SCM* system had been created. 

 [^44]: See Section \vref{sec:joint-token-embeddings-of-text-and-math}
        for a discussion of the *MIRMU-SCM* systems and its accuracy.

 /figures/document-maps.tex

#### Experiments

📖{dimensionality reduction}

In 2021, I led the bachelor's thesis of @petr2021document. In the thesis, we
developed an interactive visualization of the Math StackExchange dataset using
our *MIRMU-SCM* system and dimensionality reduction techniques. See Figure
\vref{fig:document-maps}.

#### Results

The interactive visualization is [available online.][46]

 [46]: https://mir.fi.muni.cz/document-maps-arqmath-2021

#### Future Work

📖{usability testing}

Future work should focus at usability testing of our interactive visualizations
and on the role of interpretability in the user experience.

#### Reproducibility

The code of the interactive visualization is [available on GitHub.][45]

 [45]: https://github.com/MIR-MU/document-maps

# Conclusion {#conclusion}
\label{cha:conclusion}

In my work, I aimed to answer three research questions:

📖{semantic matching}

1. Can semantic matching techniques that use token embeddings improve the
   accuracy of math information retrieval systems?

2. Can semantic matching techniques based on token embeddings be reasonably
   fast for math information retrieval systems?

3. Can learning-to-rank techniques consistently improve accuracy compared to
   individual math information retrieval systems?

← Below, I summarize the results of my research with respect to my research
questions and offer directions for future work:

📖{hard vector space model}
📖{semantic matching}
📖{weighted zone scoring}

1.  I have shown that semantic matching techniques based on token embeddings
    significantly improve accuracy on math information retrieval compared to
    the hard vector space model. This is especially true in combination with
    weighted zone scoring.
    See Section
    \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-accuracy}
    and Section \vref{sec:weighted-zone-scoring}.

    📖{orthogonalization}
    📖{decontextualization}
    📖{disambiguation}

    I have also shown how the accuracy of token embeddings can be further
    improved using hyperparameter optimization, constrained positional
    modeling, word sense disambiguation, decontextualization, and
    orthogonalization. See sections
    \ref{sec:heuristic-hyperparameter-optimization-in-subtoken-language-models}%
    --\ref{sec:decontextualized-token-embeddings} on pages
    \pageref{sec:heuristic-hyperparameter-optimization-in-subtoken-language-models}%
    --\pageref{sec:decontextualized-token-embeddings}.
    However, some of my results were only evaluated on
    intrinsic tasks. Future work should also evaluate these
    improvements on math information retrieval.↓

    📖{intrinsic measure}
    📖{constrained positional model}
    📖{quantized model}
    📖{hard vector space model}
    📖{soft vector space model}

2.  I have proven that the soft vector space model with token embeddings can be
    restricted to be as fast as the hard vector space model and I have shown
    that this restriction does not harm accuracy.  However, my implementation
    has not achieved the theoretical speed of the soft vector space model and
    future work should focus on improving it.
    See Section \vref{sec:sparse-retrieval-in-inverted-indices-and-vector-databases-speed}.

    📖{training time}
    📖{query time}

    I have also shown how the speed of token embeddings can be improved in terms
    of their arithmetic, training time, and query time. See Section
    \vref{sec:quantized-token-embeddings-with-fast-bitwise-arithmetic},
    Section \vref{sec:position-independent-token-embeddings-speed}, and
    Section \vref{sec:dense-retrieval-in-inverted-indices-speed}.↓

    📖{rank-based fusion}

3.  I have shown that four different rank-based fusion learning-to-rank techniques
    can always improve the accuracy compared to individual systems on math
    information retrieval. See Section \vref{sec:rank-based-fusion}.

    📖{score aggregation}
   
    I have also shown that my score aggregation learning-to-rank technique can
    consistently improve the accuracy compared to individual systems on machine
    translation evaluation tasks.  Future work should evaluate score
    aggregation on math information retrieval. 
    See Section \vref{sec:score-aggregation-of-structural-and-semantic-matching-techniques}.

To make it easy for others to reproduce my work, I have publicly released more
than twenty digital artifacts, including Docker images, Jupyter notebooks,
pre-trained token embeddings, online demos, and Git repositories.

# List of Author's Publications {#list-of-publications}

↑ In this appendix, I will list my international publications in journals and
conference proceedings ordered by publication date. I will also state my
research roles using the [*CRediT* taxonomy][79] and my contributions in percent.

 [79]: https://casrai.org/credit/

## Journal Articles

1. @novotny2021when

    - *Author's roles:* Conceptualization, Data curation, Formal analysis,
      Investigation, Methodology, Software, Validation, Visualization,
      Writing -- original draft, Writing -- review \& editing

    - *Author's contribution:* 70\%

2. ⇈ @ayetiran2021eds

    - ⇈ *Author's roles:* Investigation, Resources, Software, Writing -- review \& edit.

    - ⇊ *Author's contribution:* 20\%

## Proceeding Articles

3. @stefanik2021regressive

    - *Author's roles:* Investigation, Methodology, Resources, Software, Validation,
      Writing -- review \& editing

    - *Author's contribution:* 40\% ↔

4. @novotny2021ensembling

    - *Author's roles:* Conceptualization, Date curation, Formal analysis,
      Investigation, Methodology, Project administration, Resources, Software,
      Validation, Visualization, Writing -- original draft, Writing -- review \& editing

    - *Author's contribution:* 50\%

5. ⇈ @novotny2021one

    - *Author's roles:* Conceptualization, Data curation, Formal analysis,
      Investigation, Methodology, Resources, Software, Validation, Visualization,
      Writing -- original draft, Writing -- review \& editing

    - *Author's contribution:* 75\%

6. @luptak2021webmias

    - *Author's roles:* Conceptualization, Investigation, Methodology, Software,
      Visualization, Writing -- original draft

    - *Author's contribution:* 70\% ↔

7. @novotny2020art

    - *Author's roles:* Conceptualization, Formal analysis, Investigation,
      Methodology, Resources, Software, Validation, Visualization, Writing --
      original draft, Writing -- review \& editing

    - *Author's contribution:* 100\%

8. @novotny2020three

    - *Author's roles:* Conceptualization, Date curation, Formal analysis,
      Investigation, Methodology, Resources, Software, Validation,
      Visualization, Writing -- original draft, Writing -- review \& editing

    - *Author's contribution:* 50\%

9. @sojka2019quo

    - *Author's roles:* Investigation, Writing -- original draft, Writing --
      review \& editing

    - *Author's contribution:* 20\% ↔

10. @novotny2018implementation

    - ↑ *Author's roles:* Conceptualization, Formal analysis, Investigation, Software,
      Validation, Writing -- original draft, Writing -- review \& edit.

    - ⇊ *Author's contribution:* 100\%

11. @sojka2018mias

    - *Author's roles:* Software, Visualization, Writing -- original draft,
      Writing -- review \& editing

    - *Author's contribution:* 30\%

12. @novotny2018weighting

    - *Author's roles:* Conceptualization, Data curation, Formal analysis, Investigation,
      Methodology, Software, Validation, Visualization, Writing -- original draft,
      Writing -- review \& editing

    - *Author's contribution:* 80\%

13. @ruzicka2017flexible

    - *Author's roles:* Formal analysis, Validation, Visualization, Writing -- review \& editing

    - *Author's contribution:* 20\% ↔

14. @rygl2017semantic

    - *Author's roles:* Formal analysis, Validation, Writing -- review \& edit.

    - *Author's contribution:* 15\%

# Author's Publications {#publications}

In this appendix, I will include my international publications in journals and
conference proceedings ordered by publication date.

 /publications/novotny2021when.pdf
 (When FastText Pays Attention: Efficient Estimation of Word Representations
  using Constrained Positional Weighting)

 /publications/ayetiran2021eds.pdf
 (EDS-MEMBED: Multi-sense embeddings based on enhanced distributional semantic
  structures via a graph walk over word senses)

 /publications/stefanik2021regressive.pdf
 (Regressive Ensemble for Machine Translation Quality Evaluation)

 /publications/novotny2021ensembling.pdf
 (Ensembling Ten Math Information Retrieval Systems: MIRMU and MSM at ARQMath 2021)

 /publications/novotny2021one.pdf
 (One Size Does Not Fit All: Finding the Optimal Subword Sizes for FastText
  Models across Languages)

 /publications/luptak2021webmias.pdf
 (WebMIaS on Docker: Deploying Math-Aware Search in a Single Line of Code)

 /publications/novotny2020art.pdf
 (The Art of Reproducible Machine Learning: A Survey of Methodology in Word
  Vector Experiments)

 /publications/novotny2020three.pdf
 (Three is Better than One: Ensembling Math Information Retrieval Systems)

 /publications/sojka2019quo.pdf
 (Quo vadis, Math Information Retrieval)

 /publications/novotny2018implementation.pdf
 (Implementation Notes for the Soft Cosine Measure)

 /publications/sojka2018mias.pdf
 (Math-Aware Retrieval in Digital Mathematical Libraries)

 /publications/novotny2018weighting.pdf
 (Weighting of Passages in Question Answering)

 /publications/ruzicka2017flexible.pdf
 (Flexible Similarity Search of Semantic Vectors Using Fulltext Search Engines)

 /publications/rygl2017semantic.pdf
 (Semantic Vector Encoding and Similarity Search Using Fulltext Search Engines)
