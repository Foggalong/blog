---
layout: post
title: How to make Spoiler Tags in Ghost
date: '2017-08-07 15:48:00'
tags: coding
first_published_on: Ghost
---

_This blog was originally written for Ghost 0.x but the HTML support in Ghost 1.x on has changed significantly. At some point I'll return to this and see if it can be fixed._

-----

If you're a Redditor you're probably no stranger to the [comment spoiler tag](https://www.reddit.com/r/startrek/comments/1e2bz3/some_advice_for_trekkies_before_seeing_into/c9w5rsk/). It's a great way to safely talk about the plot of a film or game without ruining anything for someone who just happens upon your comment. But there's nothing about it in the default [markdown syntax](https://daringfireball.net/projects/markdown/syntax) nor in [Reddit's implementation](https://www.reddit.com/wiki/commenting)[^1], so where does it come from?

Subreddit moderators have to implement for it themselves using custom stylesheets[^2]] which is why the particular workings are different in each subreddit. From the users point of view the syntax is normally something like this:

> `I loved the prequels: [Jar Jar being Darth Plagueis](#spoiler) was genius!`

Which will then appear like this:

> I loved the prequels: Jar Jar being Darth Plagueis was genius![^3]

It's a really useful tool but because it's not included by default it's absent from a lot of services which make use of markdown including GitHub, Diaspora\*, and crucially Ghost. I'd like the ability to hide spoilers for a few review blogs I have planned so I spent a couple of hours today playing around with a few different ways it can be implemented.

## Button Reveal

The first method I tried was using a button as [detailed here](http://www.bloggersentral.com/2009/12/create-show-hide-or-peek-boo.html). For single use it's fairly simple from a UI perspective and is the most obvious in how it's used. Click to reveal the hidden spoilers and then click again to hide them.

```html
<button style="height: 40px; line-height: 0px; padding-left:10px; padding-right:10px; background:#555; color:#fff; border-radius:10px" title="Click to show/hide content" type="button" onclick="if(document.getElementById('spoiler') .style.display=='none') {document.getElementById('spoiler') .style.display=''}else{document.getElementById('spoiler') .style.display='none'}">
> CLICK FOR SPOILERS
> </button>
> I loved the prequels: Jar Jar being Darth Plagueis was genius!
```

From a code point of view though it's much less neat, even without accounting for styling:

```html
<button
    type="button"
    onclick="
        if (document.getElementById('spoiler').style.display == 'none') {
            document.getElementById('spoiler').style.display = ''
        } else {
            document.getElementById('spoiler').style.display = 'none'
        }"
>
CLICK FOR SPOILERS
</button>
I loved the prequels: <a id="spoiler" style="display:none"> Jar Jar being Darth Plagueis</a> was genius!
```

It has some pretty obvious downsides too. It's limited to one use per page unless you change the ID each time. Even then each reveal requires a new button, making it clunky if it's used more than once in the same paragraph. There's also no placeholder for the spoiler text making it confusing to read.

## Title Text Reveal

Title Text is an attribute given to HTML which shows a given string when you hover over that HTML. I mostly see it in [web](https://xkcd.com/444/) [comics](http://www.dumbingofage.com/2013/comic/book-3/04-just-hangin-out-with-my-family/hangtime/) to add extra jokes or information, but there're also a few subreddits which use it to conceal spoilers.

> I loved the prequels: SPOILER was genius!

```html
I loved the prequels: <a title="Jar Jar being Darth Plagueis">SPOILER</a> was genius!
```

While the code is simple the appearance leaves much to be desired: it's poor if there's lots of text, it prevents clipboarding any spoilers, and it makes the sentence less fluid to read. That's not even mentioning the fact that most mobile browsers don't support title text for non-image elements.

The only (admittedly big) benefit is that it doesn't depend on Javascript or CSS so it's more likely to function well with RSS readers. Most will strip out any JS & CSS to reduce download size and in those cases this is the only implementation featured here which will function properly.[^4]

## Select Reveal

This is the one used in the introductory section of this blog. To reveal the spoiler just highlight the blocked section with your cursor (or finger on phone). It's also by far the simplest in terms of code.

> I loved the prequels: Jar Jar being Darth Plagueis was genius!

```html
I loved the prequels: <a style="color:#555;background:#555">Jar Jar being Darth Plagueis</a> was genius!
```

I like this one a lot but worry that it's not the most intuitive for the reader, especially if you're not particularly tech savvy. Also if other software is preventing selection (usually done to stop the user from copying and pasting[^5]) the text is left inaccessible.

## Hover Event Reveal

Building on the previous it's possible to make it both much prettier and a little more intuitive to use, without adding too much cruft to the code. For this one the user just has to hover the cursor (or tap on phone) to reveal the spoiler.

> I loved the prequels: Jar Jar being Darth Plagueis is genius!

```html
I loved the prequels: <a
    style="background:#555; color:#555"
    onmouseover="this.style.background='#fff';
    this.style.color='#555'"
    onmouseout="this.style.background='#555';
    this.style.color='#555'"
>
    Jar Jar being Darth Plagueis
</a> is genius!
```

The above version works using a combination of CSS and mouseover events, but the same effect can be achieved with CSS alone:

```html
<style>
    #spoiler { background:#555; color:#555 }
    #spoiler:hover { background:#fff; }
</style>
I loved the prequels: <a id="spoiler">Jar Jar being Darth Plagueis</a> is genius!
```

While the `<style>` tags might seem like they add extra baggage it has a big return if multiple spoiler tags are being used. You can also get a bit creative with the CSS and add some fade in/out animations if you want to go fancy. A minor disadvantage of using the `:hover` selector is that it's [unsupported](https://www.w3schools.com/cssref/sel_hover.asp) in some older browsers.

## Which is best?

I'm not sure which I'll use going forward. The CSS hover implementation feels like it strikes the right balance between appearance, simplicity, and intuitiveness. At the same time is that worth it if apps like RSS feeders are going to render the spoiler tag useless?

There are a lot of different ways to do this including countless ones not mentioned here, but these are ones I found which seemed to make the most sense in the context of blogs. If you found this useful or have a different solution then [drop me a Tweet](https://twitter.com/Foggalong), I'd love to hear it!

-----

[^1]: Though this [might change soon](https://reddit.com/r/announcements/comments/5or86n/spoilers_tags_for_posts/dclfk4s/?context=5).

[^2]: This was one of the 100s of reasons that moderators were so annoyed when the admins announced they were [canning custom CSS](https://redd.it/66q4is)

[^3]: Not actually a spoiler but a brilliant fan theory, explored in detail [in this thread](https://redd.it/3qvj6w). What a trilogy that would have been.

[^4]: Didn't actually realise this would be the case until I published the blog and saw the mess TTRSS made of it. I quickly pulled the blog and republished it with a load of additions and caveats to cover that. Sorry if this meant my blog showed up in your reader twice!

[^5]: I've used RSS readers before which did this. I don't know why developers do it, but it's mindbogglingly aggravating.
