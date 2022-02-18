# Interpretable Representations for Fast and Accurate Retrieval of Mathematical Information

 [![actions-badge][]][actions]

This repository contains [PDF][thesis.pdf] and [source][thesis.md] of the
doctoral thesis *Interpretable Representations for Fast and Accurate Retrieval
of Mathematical Information* by Vít Novotný as well as [PDF][presentation.pdf] and
[source][presentation.md] of the thesis defense presentation. A recording of the
thesis defense is available on [YouTube][presentation.mp4].

 [actions-badge]:    https://github.com/Witiko/doctoral-thesis/actions/workflows/test_and_publish.yml/badge.svg
 [actions]:          https://github.com/Witiko/doctoral-thesis/actions/workflows/test_and_publish.yml
 [thesis.md]:        main.md
 [thesis.pdf]:       https://github.com/Witiko/doctoral-thesis/releases/download/latest/thesis.pdf
 [presentation.md]:  defense.md
 [presentation.pdf]: https://github.com/Witiko/doctoral-thesis/releases/download/latest/defense.pdf
 [presentation.mp4]: https://youtu.be/880twOfJYek

## Abstract

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

## Citing

To cite my thesis in your work, you can use the following bibliographic entry:

> NOVOTNÝ, Vít. *Interpretable Representations for Fast and Accurate Retrieval
> of Mathematical Information* [online]. Brno. Available from:
> <https://is.muni.cz/th/o4thd/>. Doctoral thesis. Masaryk University, Faculty
> of Informatics. Thesis supervisor Petr SOJKA.

Alternatively, you can also use the following BibLaTeX code:

``` bib
@thesis{novotny2021interpretable,
  title = {Interpretable Representations for Fast and Accurate
           Retrieval of Mathematical Information},
  author = {Novotný, Vít},
  supervisor = {Sojka, Petr},
  institution = {Faculty of Informatics, Masaryk University},
  location = {Brno},
  type = {phdthesis},
  year = 2021,
  url = {https://is.muni.cz/th/o4thd/},
}
```
