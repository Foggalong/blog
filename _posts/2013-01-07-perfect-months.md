---
layout: post
title: Perfect Months
date: '2013-01-07 21:18:16'
modified_date: '2015-01-07 21:18:00'
tags: math
first_published_on: Wordpress
# first_published_url: imjustjoshing.wordpress.com
---

Every now and then a thought or idea will get into my head and won't leave until I let it out. This can be anything from prose to a joke but more often than not is just some weird thing that's popped up because I've been sat thinking too long. This once lead to me spending a month analysing hailstone sequences in the [Collatz conjecture](https://en.wikipedia.org/wiki/Collatz_conjecture), hours developing a web browser that only (kinda) uses two lines of code, and much more recently the idea of "perfect months".[^3]

> February 2010 was the last month we had where the dates naturally presented themselves as block of 7×4, _i.e._ the first day was a Monday and last was a Sunday. This is also known as a “perfect month”. When using the Georgian (standard) calendar they can only occur as Februaries[^1] which are not in leap years. This is because these are the only conditions which make a month with a number of days divisible by the length of a week. There isn’t another perfect month until February 2021.

While it may look copy-pasted it's only from my Facebook and is all my own words and working. There's no official thing of "perfect months", only in the respect that I have now decided to make them a thing. As far as I know it's not something people have mulled over before, it was just me being weird. I make my own calendars see: every year I write out all the dates on a sheet of paper instead of buying one. I was thinking while doing it this year, "I wish all the dates would fit into blocks. I wonder if they can? Well of course February can. I wonder when it does?", which all lead to some maths and some programming (more specifically, these three condensed lines of python).

```python
from calendar import isleap as l, monthrange as r
for y in range(1752, 2151):
    if (l(y) == 0 and r(y, 2)[0] == 0): print(y); y+=1
```

That short program will list every perfect month between the first adoption of the Gregorian calendar in 1752 and the invasion of the Daleks in 2150. For that period of nearly 400 years a list of 42 appears: 1762, 1773, 1779, 1790, 1802, 1813, 1819, 1830, 1841, 1847, 1858, 1869, 1875, 1886, 1897, 1909, 1915, 1926, 1937, 1943, 1954, 1965, 1971, 1982, 1993, 1999, 2010, 2021, 2027, 2038, 2049, 2055, 2066, 2077, 2083, 2094, 2100, 2106, 2117, 2123, 2134 and 2145.[^2]

I wouldn't have stopped thinking about perfect months if I'd not done this, just like I can't stop thinking of the Lego-combo-super-box or the Pokémon text adventure (did I tell you it was Pokémon themed now?), so I thought it was best to get it out there. Maybe I'll share some of my other brain-farts with you another time? I think I'd like that.

-Josh

-----

[^1]: I learnt that that the plural of February is _Februaries_ which confused my poor brain for some time.

[^2]: _"For what it's worth, February 2151 also begins on a Monday. And since the Gregorian calendar has a period of 400 years, that completes all the work that needs to be done for that - in any 400-year stretch there'll be 43 'perfect months' as you call them."_ - comment by [Joseph Nebus](https://nebusresearch.wordpress.com) on the original Wordpress version of this blog.

[^3]: I definitely [wasn't](https://ablestmage.wordpress.com/2009/02/10/february-2009-the-perfect-month/) [the first](https://web.archive.org/web/20210422194838/http://answers.yahoo.com/question/index?qid=20090203161805AAYVyfZ) to call these perfect months, the name didn't really seem to be standard until [2021](https://en.wikipedia.org/wiki/Perfect_month)'s.
