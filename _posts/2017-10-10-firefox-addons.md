---
layout: post
title: Firefox Addons
date: '2017-10-10 16:59:00'
tags: tech
first_published_on: Ghost
---

I'm something of a browser geek. I'm not sure why but for some reason I particularly love looking through the history of web browsers and how they've changed over the years. With some major changes coming to my browser of choice over the coming months I thought I'd talk a bit about the history, but also what I'm using myself.

## History

I've been a proud Firefox user for years, long before I knew what Linux was, long before I even knew what open source was, possibly even before I knew what the internet was. One of the main attractions over the years has been the power of the add-ons, far more so than those for Chrome & IE. But Mozilla have been making some changes recently and word on the street is that it might be killing off their add-on advantage.

The first of these changes was [Electrolysis](https://blog.mozilla.org/futurereleases/2016/08/02/whats-next-for-multi-process-firefox/) (e10s) which landed &nbsp;in v54. In short it's a move to separate the browser itself and the web content it's viewing into two separate processes. It improves stability of the browser significantly by ensuring that if a heavy webpage crashes it'll only crash the content rather than the whole browser. This is just the first step in the multi-process move, the next being to for individual add-ons and webpages to each use their own processes too. This will mean one laggy webpage won't be able to crash another.

The second is still ongoing, set to land in v57, and is called [WebExtensions](https://wiki.mozilla.org/WebExtensions). While e10s broke a lot of add-ons and it took developers a while to adapt to the new system, WebExtensions will break all of them. It's an entirely new add-on API and everything will need to be rewritten to work with it. This time though the add-on conversion is happening much slower, and a lot of non-techy users are wake up to a nasty surprise on November 14th.

There's a [full list](https://docs.google.com/spreadsheets/d/1TFcEXMcKrwoIAECIVyBU0GPoSmRqZ7A0VBvqeKYVSww/edit) tracking rewrite of add-ons to WebExtensions. Looking at the stats page of that document it's evident that while a lot are being ported, more aren't and it's often because the new API doesn't include the functionality required. Even of those that have been ported most have lost some functionality for the same reason.\n\nThere are good reasons for making both these changes. The first will make for a faster, more stable browser experience and allow Firefox to keep pace with Chrome, and the second will make extensions more secure. It's coming at a price though since it may well mean that for the first time in years Firefox isn't going to be top dog when it comes to browser power.

## My Firefox

Its release cycle is roughly split into 4 channels: the [stable release](https://www.mozilla.org/en-US/firefox/new/) plus [3 others](https://www.mozilla.org/en-US/firefox/channel/desktop/) which roughly correspond to beta, alpha, and indev. Around the time [Australis](https://blog.mozilla.org/ux/2013/11/australis-is-landing-in-firefox-nightly/) (the last major UI redesign) was coming out I was an avid tester of the alpha version (then called Aurora), but these days I've settled into stable. I'm not about that cutting edge life any more.

On Android, while I have used the [main versions](https://play.google.com/store/apps/developer?id=Mozilla) I recently switched to [Firefox Focus](https://www.mozilla.org/en-US/firefox/focus/). This is a new single tab browser which automatically blocks incoming trackers and adverts, making it blazing fast and much more secure. I'd highly recommend everyone to give this a go.

Add-on wise I've had to make a few changes over the past few months to account for all the API changes which have happened. All those listed below will work with every version of Firefox, even after the upcoming changes.

- [uBlock Origin](https://github.com/gorhill/uBlock#ublock-origin). The best adblocker I've ever used, and immensely powerful once you start playing around with the tools under the hood.
- [Unicode Keyboard](https://addons.mozilla.org/en-GB/firefox/addon/unicode-keyboard/?src=api). This replaced [abcTajpu](http://lingvo.org/abctajpu) was what I use to input non-alphanumeric characters such as Greek letters or mathematical symbols. It's pretty much an essential for talking about mathematics or physics online.
- [Toolbox](https://reddit.com/r/toolbox). I'm a moderator of a few subreddits and this makes my life so much easier. Everything from username tags to pre-made responses are included.
- [Page Translator](https://github.com/jeremiahlee/page-translator). Translates webpages that aren't in English using Google Translate or Microsoft Translator. It's not perfect, but it's the best one I've found that will work post-v57.
- [Violet Monkey](https://violentmonkey.github.io/). A ([hopefully temporary](https://github.com/Foggalong/website-restyles)) replacement for [Greasemonkey](https://www.greasespot.net/), a [userscript](https://en.wikipedia.org/wiki/Userscript) manager. I make pretty heavy use of these mostly in the form of [personal website restyles](https://github.com/Foggalong/website-restyles).
- [Download Star](https://addons.mozilla.org/en-GB/firefox/addon/download-star/?src=api). A tool for downloading loads of content from a webpage (e.g. images from an Imgur album) at once. It gets the job done, but isn't quite as good as [DownThemAll](http://www.downthemall.net/re-downthemall-and-webextensions-or-why-why-i-am-done-with-mozilla/) which it replaced.
- [Vimium](https://github.com/philc/vimium). To be honest this is the one I'm least happy with. It replaced [VimFx](https://github.com/philc/vimium) which added powerful vim-like keyboard controls to the browser, but Vimium just isn't as responsive.

In case you didn't get the theme there towards the end, this API transition is killing off a lot of good extensions without adequate replacements. In some cases a replacement can't even be made because the API needed is just gone. Mozilla have done a reasonable job of sailing through times of change in the past[^1] but I really hope they still have it in them to do it again.

-----

[^1]: Between writing this blog and publishing it the Mozilla/Cliqz scandal happened, though I've not had time to really read into the nuances of the issue yet. As far as I can tell it doesn't really affect or change anything that I've talked about but I thought I'd explain its absence. Check out Mozilla's [press release](https://blog.mozilla.org/press-uk/2017/10/06/testing-cliqz-in-firefox/), [this zdnet article](http://www.zdnet.com/article/firefox-tests-cliqz-engine-which-slurps-user-browsing-data/), and [this Reddit thread](https://www.reddit.com/r/firefox/comments/74yo19/cliqz_and_mozilla_as_i_understand_it_and_metadrama/) discussing the topic further.
