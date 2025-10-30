---
layout: post
title: Creating Accessible Maths
tags: [coding, teaching]
date: '2025-10-30 17:00:00'
---

In my [last post](https://fogg.uk/2025/08/19/quarto-assignments.html) I mentioned that I'd been using markdown-based Quarto (rather than using $\mathrm{\LaTeX}$, Microsoft Word, or similar) to make my teaching materials for mathematics as accessible as possible. In this post, I want to give a little bit of an answer to the '_why Quarto?_' question. This is intended as a (rough) explainer of the problem to colleagues _outside_ STEM; fellow mathematicians, you'll find far better explainers elsewhere!

## The Scourge of PDF  

In higher education mathematics where I work, PDF is still almost universal as the format to share documents with students. Our colleagues across the arts, humanities, and social sciences, or even those closer to home in biology or chemistry, might find this a bit of an anachronism. Aren't we all just using Word and PowerPoint now? 

An output from the University of Nottingham Accessibility Conference in June 2024 was [this excellent guide](https://www.learningapps.co.uk/moodle/xertetoolkits/play.php?template_id=3052#page1) on why PDF became popular as a format, the accessiblity issues the format has, and how those issues can be either worked around or addressed by using other formats. However, it comes with the following caveat:

> There are many specialist uses of PDFs in data analysis, science, technology, engineering and mathematics. We have not included these in the guidance because these subjects include specialist formats like Markdown or LaTeX – all of which export to PDF. There are a number of unresolved challenges in getting STEM content to export to accessible PDFs. This is a work in progress.

So what are these 'specialist uses' the delegates mention? The biggest is how equations are written. The sheer number and complexity of the maths featured in materials makes using Micrsoft's [what you see is what you get](https://en.wikipedia.org/wiki/WYSIWYG) editor cumborsome at best, and even then it has its own set of accessiblity issues. This alone is largely responsible for PDF endurance in other maths heavy subjects across science, technology, and engineering.

Another big one is the inclusion of code for programming. To make this useful for the reader, it needs a separate monospace font and syntax highlighting. If you're unsure why this is difficult, consider all the steps incolved to create even a short Word Document that looked like this.

> ### Quadratic Formula
> 
> A quadratic equation $ax^2 + bx + c = 0$ has roots at
> 
> $$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}.$$
>
> This is sometimes called the **quadratic formula**. In Python, we could define `quadRoot` to find these.
> 
> ```python
> def quadRoot(a, b, c)
>     discrim = b**2 - 4*a*c
>     x1 = (-b - discrim**0.5)/(2*a)
>     x2 = (-b + discrim**0.5)/(2*a)
>     return (x1, x2)
> ```
> 
> Try running this on your machine. What happens?

To avoid that ordeal, in STEM specialised software for writing documents is used, the default output of which is typically PDF. For example, the following $\mathrm{\LaTeX}$ source when 'rendered' would produce this desired PDF output.

```latex
\documentclass{article}
\usepackage{listings}
\title{Quadratic Formula}

\begin{document}
    \maketitle
    A quadratic equation \(ax^2 + bx + c = 0\) has roots at
    \[
        x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}.
    \]
    This is sometimes called the \textbf{quadratic formula}.
    In Python, we could define \texttt{quadRoot} to find these.
    \begin{lstlisting}[language=Python]
        def quadRoot(a, b, c)
            discrim = b**2 - 4*a*c
            x1 = (-b - discrim**0.5)/(2*a)
            x2 = (-b + discrim**0.5)/(2*a)
            return (x1, x2)
    \end{lstlisting}
    Try running this on your machine. What happens?
\end{document}
```

$\mathrm{\LaTeX}$ is certainly powerful, but it's also complicated and infamous for its steep learning curve. Despite steallar resources like [Overleaf's guide](https://www.overleaf.com/learn/), its reputation among maths undergrads just starting out is understandably formiddible.

## Markdown's Rise

In 2004, John Gruber created markdown as a way to write simple documents in a simple text editor, rather than needing a word processor like Microsoft Word or a rendering engine like $\mathrm{\LaTeX}$. Flash forward to the present and markdown is used to make it easy for users across the internet to format text, including on GitHub, Reddit, Stack Exchange, and even Discord! Many markdown viewers include support for accessible, $\mathrm{\LaTeX}$-formatted equations through [MathJax](https://docs.mathjax.org/en/latest/basic/mathjax.html).

This combination now makes it possible to write documents that feature heavy maths that can be viewed in an accessible way. In markdown, the same document as before might be written[^1] as follows.

````md
# Quadratic Formula

A quadratic equation \(ax^2 + bx + c = 0\) has roots at
\[
    x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}.
\]
This is sometimes called the **quadratic formula**. In
Python, we could define `quadRoot` to find these.

```python
def quadRoot(a, b, c)
    discrim = b**2 - 4*a*c
    x1 = (-b - discrim**0.5)/(2*a)
    x2 = (-b + discrim**0.5)/(2*a)
    return (x1, x2)
```

Try running this on your machine. What happens?
````

So with that, why hasn't every document for the last 20 years just used markdown??

Well, markdown is (by design) a very [simple set of tools](https://commonmark.org/help/). Basic features like footnotes, tables, and yes equations too are widely-accepted extensions to the specification, rather than a core part of it. This makes using common markdown to create complicated files like lecture notes or a presentation difficult at best. But as I've hinted, markdown can be _extended_ to meet that need.

## Enter Quarto

Developed by [Posit](https://posit.co/) of RStudio fame, [Quarto](https://quarto.org/) is an open-source scientific and technical publishing system for writing markdown documents that can be rendered as articles, presentations, dashboard, websites, and books. It uses does this using Pandoc markdown which includes syntax for equations, citations, crossrefs, figure panels, callouts, advanced layout, and more.

Crucially for my use case, Quarto documents render to HTML by default, maximising accessibility, while also supporting formats like PDF, Microsoft Word, and ePub for portability. The example we've been looking at so far would be written as follows if we wanted to output as HTML.

````md
---
title: "Quadratic Formula"
format: html
---

A quadratic equation $ax^2 + bx + c = 0$ has roots at
$$
    x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}.
$$
This is sometimes called the **quadratic formula**. In
Python, we could define `quadRoot` to find these.

```{python}
#| echo: true
def quadRoot(a, b, c)
    discrim = b**2 - 4*a*c
    x1 = (-b - discrim**0.5)/(2*a)
    x2 = (-b + discrim**0.5)/(2*a)
    return (x1, x2)
```

Try running this on your machine. What happens?
````

Note the strong similarities to common markdown, but the presence of parameters in both the header and the code block. These both hint at some of the more powerful Pandoc formatting options that Quarto exposes, while not making authoring the document any more complicated overall.

This isn't to say Quarto is perfect or even without signficiant downsides, but it has allowed me to write maths-and-code-heavy lecture notes, presentations, and workshop sheets that render in an accessible format by default.

-----

[^1]: This is indeed almost identical to the markdown source that produced the original!
