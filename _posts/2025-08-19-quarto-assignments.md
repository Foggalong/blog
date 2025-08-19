---
layout: post
title: Reviving Wren
tags: [coding, blog]
date: '2025-08-19 16:30:00'
---

Over the last couple of days I've been writing problem sheets for an upcoming maths course. To make these as accessible as possible, rather than using $\LaTeX$, Microsoft Word, or similar, I've been using markdown-based [Quarto](https://quarto.org/). I'll save _'why Quarto?'_ for another time; for now I just want to focus on my experience using it to write mathematics problem sheets.

## End Goal

My aim is to write a single Quarto file that contains both the problems and their solutions that can be rendered into two different documents: one where the solutions are absent, and one where shown in a [callout block](https://quarto.org/docs/authoring/callouts.html) or otherwise visually distinct section.

> **Question 1**. What is $3\times2$?
>
> | _Solution 1_ |
> | :------- |
> | We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$. |

The other restriction is to have a structure that isn't too technical. Most of the teachers and support staff on this course aren't programmers, so anything that's getting into custom Lua filters that will be difficult to maintain is a none starter.

## Parametrised Documents

One of the first solutions I tried came from [this excellent post](https://nrennie.rbind.io/blog/r-tutorial-worksheets-quarto/) by Nicola Rennie. In it she combines Quarto parameter functions with conditional content to create the following solution:

```markdown
---
...
params: 
  hide_answers: true
---

**Question 1**. What is $3\times2$?

`r if (params$hide_answers) "::: {.content-hidden}"`
 _Solution 1_

We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$.
`r if (params$hide_answers) ":::"`
```

This solution is nice in that it's all just R + Quarto and doesn't require any additional files. To add the styling I was after and make the syntax neater, I did try expanding this with a wee external R function `solution.R`.

```r
solution = function() {
  if (params$show_solutions) {
    "{.callout-note title=Solution}"
  } else {
    "{.content-hidden}"
  }
}
```

In the markdown, this would be used as below.

````markdown
---
...
params:
  show_solutions: true
---

```{r}
#| echo: false
source("solution.R")
```

**Question 1**. What is $3\times2$?

::: `r solution()`
We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$.
:::
````

However, this was all starting to feel a bit hacky. I couldn't find whether this approach for constructing divs is intended to work in Quarto, or if it's a bug. From here, I followed Natalie's signpost to the [`assign`](https://github.com/coatless-quarto/assign) extension by James J Balamuta.

## Quarto Extensions

At face value, the `assign` Quarto extension was doing _exactly_ what I wanted. After installing the project by

```shell
quarto add coatless-quarto/assign
```

the markdown is simple.

```markdown
---
...
filters:
  - assign
---

**Question 1**. What is $3\times2$?

:::{.sol}
We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$.
:::
```

This works by leveraging Quarto profiles, which unfortunately turns out to be the extensions major downside. Neither RStudio or VS Code currently expose profiles through their GUIs, so documents now have to be rendered through terminal commands.

Reading how others worked around this led to finding [`unilur`](https://github.com/ginolhac/unilur) by Aurélien Ginolhac. Rather than using profiles, it creates custom outputs types; these _are_ supported by RStudio and VSCode.

```shell
quarto add ginolhac/unilur
```

```markdown
---
...
format:
  unilur-html: default
  unilur-html+solution:
    output-file: example-solution.html
---

**Question 1**. What is $3\times2$?

:::{.unilur-solution}
We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$.
:::
```

Programmatically this is ideal and very easy to use. Frustratingly though, `unilur` makes several style choices that are difficult to change:

1. Solutions are collapsed by default, and don't have an icon to indicate they can be expanded.
2. To avoid overlap, the extension changes the colour of the tip callout, but [leaves the icon broken](https://github.com/ginolhac/unilur/issues/5).
3. Headers of solutions use a style distinct to all other callouts, creating a mismatched appearance.

## Pure Quarto

It's around this time, digging into the Lua filters used by `assign` and `unilur` to fix these issues, I stopped and asked myself:

> _Am I overcomplicating this? Could I just have done this in pure Quarto all along?_

Fairly early on I'd tried and failed combining the classes `{.callout-tip .content-hidden}` in a single div, but I hadn't tried much beyond that before finding Natalie's post. Exploring more, Quarto also doesn't seem to allow using conditional spans within the attributes of a div. For example

```markdown
::: {[.callout-tip]{.content-visible when-meta=answers} [.content-hidden]{.content-visible unless-meta=answers}}
We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$.
:::
```

just displayed the outer div definition as plaintext, rather than using it as render instructions. This makes sense when I think of the analogy to HTML's `<span>` and `<div>`, though it does reaffirm the uncertainty about running R snippets to define span attributes.

Best I can tell, the only way to do this in pure Quarto is through two nested divs.

```markdown
---
...
answers: false
---

**Question 1**. What is $3\times2$?

::::{.content-hidden unless-meta=answers}
:::{.callout-tip title=Solution}
We can think of it as 'three lots of two', so $2 + 2 + 2 = 6$.
:::
::::
```

This is clearly more long winded than ideal, but it's not doing anything overly complicated and doesn't require the use of any additional commands or config files. That's something at least.

## Conclusion

I'm not really sure what approach is best, all have their own strengths and weaknesses. Is more verbose Quarto clearer than an R constructor? Is the ease of using an extension worth the compromise on functionality? Hopefully with use in practice (and writing more of these docs with others) a preferred approach will emerge.
