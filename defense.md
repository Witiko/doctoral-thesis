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
make the results of math-aware search engines more interpretable to the users.
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
   techniques add complexity to a search engine, I figured that it would be
   good to know whether they can still be reasonably fast.

3. Learning to rank techniques and voting algorithms have been shown
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

In my thesis, I first describe math information retrieval and information
retrieval in general, including existing digital mathematical libraries,
math representations, retrieval models, different kinds of token embeddings,
semantic matching techniques, and fusion-based learning-to-rank techniques.
Next, I describe past competitions of math-aware search engines and the search
engines that achieved state-of-the-art accuracy in the competitions.

Then, in the following two chapters, I describe the methods and the results of
my research on representations that enable fast and accurate math-aware search.
Here, I also answer all of my research questions.
In the final chapter before the conclusion, I draw from the work of my
undergraduate and graduate students to I showcase some tangible qualitative
benefits of interpretability.

I conclude by summarizing my results and outlining venues for future
work.

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
Ar$\chi$iv, the European Digital Mathematical Library, or the ACM Digital
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
and $\frac{a}{b}$ will be equivalent in Content MathML. This is especially
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
representations have seen limited adoption in math-aware search engines.

* * *

 /figures/pmml-and-cmml.tex

## Math Information Retrieval {#math-information-retrieval}

Now that we have properly represented math information, we need to retrieve it.
In the picture, you can see the general architecture of a math-aware search
engine, where we first index the representations and then allow the user to
search for answers using queries. This does not seem too different from
information retrieval in general, but the devil is in the detail.

Although math information retrieval also focuses on the retrieval of documents,
other *retrieval units* of interest include math formulae and math statements
such as definitions, theorems, and proofs. Furthermore, the language of
mathematics is inherently more structured than natural text, which brings
unique opportunities and challenges. Finally, users may search for answers not
only with *text queries*, but also as *math* and *mixed queries*, as you can
see in the picture. ↷

There are two main approaches to math information retrieval and information
retrieval in general: *sparse retrieval* and *dense retrieval*.

In sparse retrieval we index documents as high-dimensional sparse vector
coordinates, where every nonzero dimension represents a single word or
mathematical symbol from the document, as you can see in the left picture.
Sparse retrieval is fast and interpretable, but it has several weaknesses that
hurt its accuracy:

1. It disregards *word order*. Therefore, it does not really represent the
   meaning of the text. Instead, it reduces meaningful statements in human and
   mathematical languages to an unintelligible salad of words and math symbols.

2. It does not model *word relatedness*. Therefore, two words or math symbols are
   either the same, or they are different. This means that a slight difference
   in terminology or math notation can make a big difference, which is not
   always what we want. ↷

By contrast, in dense retrieval, we index documents as low-dimensional dense
vector coordinates (so-called *sentence embeddings*) that have been produced by
deep neural network language models such as *BERT*, which you can see in the
picture. Producing the vectors is computationally demanding and they are not
immediately interpretable. Additionally, BERT and similar language models can
only produce representations of short documents that contain at most hundreds
of words. Despite these limitations, dense retrieval models achieve
state-of-the-art performance on information retrieval.

In addition to sparse and dense retrieval, math-aware search engines may take
advantage of additional techniques such a *semantic matching* and *learning to
rank*. ↷

In *semantic matching* techniques, we first compute the distance between
different tokens such as words and math symbols using language models, as you
can see in the middle picture. Then, we use these distances to improve the
accuracy of the retrieval. For example, in the right picture, you can see the
*soft vector space model*, which improves sparse retrieval, so that it can
models word relatedness and is more robust against slight differences in
terminology and math notation. ↷

By contrast, in fusion-based learning-to-rank techniques, we don't improve the
accuracy of an individual search engine themselves. Instead, we combine the
results of several search engines to produce more accurate results. *Rank-based
fusion* techniques such as *the reciprocal rank fusion* only use the ranks of a
document across the results, whereas *score aggregation* techniques such as
*ScoreMNZ* also take into account the similarity scores produced by the search
engines.

* * *

 /figures/math-information-retrieval.tex
 /figures/soft-vsm-obscured.tex
 /figures/sentence-bert.tex
 /figures/soft-vsm.tex
 /figures/learning-to-rank.tex

## Objectives and Evaluation {#objectives-and-evaluation}

Representations and retrieval techniques allow the math-aware search engines to
fulfil three major objectives:

1. the *accuracy* of their results for the information needs of the users,
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
which measures how well token embeddings produced by language models can be
used for analogical reasoning, as you can see in the picture. While the word analogy accuracy does not
directly tell us how useful the token embeddings will be for semantic matching
and math-aware search, it is fast and can be a useful proxy ↷ for extrinsic
evaluation measures such as *the F₁-score*, *mean average precision*,
or *nDCG'*, which measure the relevance of search results using costly human
annotations.

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

% ...

# Response to Prof. Oard's Report {#oard}

## The Results of the Work {#oard-results-of-the-work}

* * *

> **Research Question 2.** Can semantic matching techniques based on token
> embeddings be reasonably fast for math information retrieval systems?
>
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

* * *

> **Research Question 3.** Can learning-to-rank techniques consistently improve
> accuracy compared to individual math information retrieval systems?
>
> \[...] When the two unweighted techniques were applied to the 8 systems that
> participating teams in ARQMath 2021 had designated as their first priority
> for evaluation (which included the best of the 37 systems), the unsupervised
> RRF technique (0.556) achieved a higher nDCG’ than the unsupervised IBC
> technique (0.514), and was notably above the best individual system (0.434);
> results for that 8-system combination were not reported for the supervised
> RBC or WIBC techniques.

* * *

> Notably, despite the title of the thesis, there is no stated research
> question involving the term “interpretable representation” at the start of
> that title.
>
> \[...] The thesis contains no definition of what it means for a
> representation or a system to be interpretable, although section 2.4.3 does
> name five techniques that might generally be used to aid in interpretation of
> data, and Chapter 6 does illustrate uses of three of those five (correlation
> analysis, clustering, and visualization).
>
> \[...] I therefore conclude that although the author himself is interested in
> interpreting things, original research on the design or evaluation of
> “interpretable representations” does not seem to be a focus of the thesis.

## Thesis Structure {#oard-thesis-structure}

* * *

> The thesis consists of an introduction stating three research questions, two
> chapters reviewing related work, three chapters that together contain 22
> short research abstracts (each typically 2--3 pages) describing a broad range
> of research conducted by the author, and a short conclusion that summarizes
> where the three research questions were answered.
>
> \[...] I found the surprisingly brief treatment of each research question
> to be problematic, lacking in the details that I needed to see in order to
> determine whether research methods had been properly applied and whether
> results had been properly analyzed and interpreted. I therefore found it
> necessary to refer to the published papers that I have mentioned as the basis
> for my assessment in the preceding section.

* * *

> There are also several instances in which I was not able to determine why the
> author had chosen to summarize specific work in this thesis. Sections 4.2.6
> and 4.3 describe machine translation research, a task very different from
> information retrieval. Section 4.2.5 evaluates the effect of data
> augmentation on word sense disambiguation. Section 6.1.1 describes research
> on part of speech tagging. A substantial portion of Section 4.5 describes
> experiments with giving more weight to early posts in threaded discussion
> lists; I don’t see how that work with (text only) discussion lists sheds any
> light on the question of how different fields of a single message should be
> weighted in math retrieval (which is the other focus of section 4.5).

## Language and Graphics Level {#oard-language-and-graphics-level}

* * *

> The use of language and illustrations generally meet the expected
> professional standard.  exceptions are Figures 5.2 and 6.1, which are not
> adequately explained.

## Defense Questions {#oard-defense-questions}

* * *

Soft Cosine Measure questions:

> The Soft Cosine Measure (SCM) seems to me to have a structure similar to
> CoIBERT (Khattab and Zaharia, SIGIR 2020), but with two differences: (1)
> CoIBERT uses the maximum over the document terms of the similarity between
> embeddings for each query term, whereas SCM uses the sum of those
> similarities, and (2) CoIBERT uses a transformer to learn contextual
> embeddings, whereas your implementation of SCM uses fastText to learn static.
> How consequential is the first of those differences: are there theoretical or
> practical advantages or disadvantages to CoIBERT’s max-sim operator compared
> with the inner product in SCM?

> What challenges would arise if you were to use standard tools for rapid
> approximate nearest neighbor computation (e.g., Faiss) with SCM rather than
> your regularization technique? Would there be a parameter similar to your C
> that would need to be set to bound compute time?

% However, there is a gray zone between 1% and 50% non-zeros in the vectors
% where it is not clear whether it is more efficient to handle them as sparse or
% dense. How sparse are your vectors?
% -- https://github.com/facebookresearch/faiss/issues/754

* * *

Sentence-BERT questions:

> Where system selection or tuning was based on tasks other than math retrieval
> (e.g., on word analogy or on machine translation), what basis is there to
> believe that the systems or parameters selected would be reasonable choices
> for a math retrieval application?

> Unlike Sentence-BERT, Dense Passage Retrieval (DPR, Karpukhin et al, EMNLP
> 2020) models queries and documents differently, and it uses a prefix (CLS)
> token as the representation rather than Sentence-BERT’s mean pooling. What
> benefits does Sentence-BERT have over DPR?

> Unlike your random selection of training examples, selecting negative
> examples that are highly confusable with positive examples has been shown to
> be a better approach. One application of this idea to BERT is ANN Negative
> Contrastive Estimation (ANCE, Xiong et al, ICLR 2021), which iteratively
> learns to select negative examples. Did you experiment with any alternative
> to random selection of negative examples?

* * *

System Combination question:

> In your work you have focused exclusively on rank-based system combination.
> I believe it is the case that every system you have combined operates
> pointwise, assigning a score to every document and then sorting on those
> scores. One might expect those scores to have more information than is
> available from the ranks alone (for example, smaller score gaps might
> indicate a weaker commitment to the generated rank order). How might you
> adapt IBC to benefit from access to those document scores? Would you expect
> improved results?

* * *

Interpretable Representation questions:

> You cite Hall and Gill’s 2018 O’Reilly book as defining interpretability, but
> they offer no specific definition of an “interpretable representation,” the
> term you use in the title of the thesis. In general, what characteristics
> would make a representation “interpretable”?

> In this thesis, you focus on embeddings as representations for text and math
> content. | would think of surface forms (e.g., words or LaTeX) as being more
> interpretable than the sparse vectors in what you call the hard vector space
> model, and those vectors as more interpretable than the dense vectors that we
> call embeddings. In what sense do you mean to claim that the embedding
> representations studied in this thesis are “interpretable”?

## Editorial Issues {#oard-editorial-issues}

* * *

> P23 competitions -> shared task evaluations (Shared task evaluations exist to
> support comparisons between systems, but not all comparisons have competition
> as their motive.  Organizers of some shared task evaluations have been clear
> to indicate that they do not intend their venue to be a competition. Of
> course, some participants may see their participation as competition, but
> that does not make the event a competition any more than an exam in a course
> would be a competition. This word “competition” appears throughout the
> thesis, and it would be more properly replaced by shared task evaluation in
> every case.)

* * *

> P4O what does it mean for some features to “interact with” positional
> embeddings?

* * *

> P44 In the caption for Table 4.1, what does it mean for a language pair to be
> evaluated cross-lingually? The same question arises in the caption for Table
> 4.2 on page 48.

* * *

> P50 Does “significantly” mean statistically significantly? If so, by what
> test? If not, “substantially” might be the more appropriate claim.

* * *

> P59 The second paragraph of section 5.2.1 says “showed it was more accurate”
> and “the ... accuracy of these approximations have not been evaluated”. How
> was it shown to be more accurate if accuracy was not evaluated?

* * *

> P69 What makes a property interesting?

# Response to Prof. Skopal's Report {#skopal}

## The Results of the Work {#skopal-results-of-the-work}

* * *

> One journal publication was in Q1 (Knowledge-based Systems), while the second
> in Q4 (J. of Universal Computer Science).

% Q2, see e-mail by Peter Sojka from 2022-01-12 and SCIMAGO

## Thesis Structure and Evaluation {#skopal-thesis-structure-and-evaluation}

* * *

> Although the thesis is presented as a self-contained manuscript,
> commentary to the individual published works listed above.
>
> [...] Such a “commentary” form of doctoral thesis might be beneficial for the
> sake of brevity, however, it is often used in case the individual
> publications do not form an inherent body of research. Unfortunately, this
> seems to be the case of this thesis. The individual contributions are
> scattered through many subproblems in the field of information retrieval or
> machine-learning (and rarely contribute to the specific MathIR domain). The
> results are not presented in a research framework or pipeline that
> systematically addresses a more complex research goal. Therefore, also the
> novelty of the individual results is quite low.

## Defense Questions {#skopal-defense-questions}

* * *

> As mentioned in my comments to the thesis structure, there is only a few
> math-specific techniques mentioned in section 2.3 (actually just the
> structural matching in 2.3.3.1). Everything else in 2.3 is applicable to
> general IR. Some other MIR-related works are mentioned in subsections 4.x. Is
> this lack of mathspecific IR a sign of small research MIR community? Please
> comment on this.

> Could you describe and quantify the impact of math-specific extensions to IR
> engines? In the thesis results for text-based and math-based retrieval are
> mixed. Could a reader of the thesis (or the referenced papers) find a
> recommendation of what MIR techniques to use in what retrieval scenario?

# Conclusion {#rebuttal-conclusion}

* * *

Prof. Oard's Conclusion:

> [...] If evaluated on the basis of the results of the work actually
> performed, as described in the cited publications, I would have graded this
> dissertation as a B (above the average, but with minor errors) on the
> European Credit Transfer and Accumulation System (ECTS) scale. However, if
> evaluated on the basis of the content of the thesis itself, without reliance
> on details reported only in other publications, | would grade this thesis as
> FX on the ECTS scale (fail -- some more work required before the credit can
> be awarded). [...]
