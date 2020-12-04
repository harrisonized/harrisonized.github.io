---
layout: post
title: Properly Open-Sourcing My Code
date: 2020-12-04
categories: 
tags: 
image: /assets/article_images/2020-12-04-proper-open-source/building-ecosystem-open-source-data-science-blog-cover-image-small.png
image2: /assets/article_images/2020-12-04-proper-open-source/building-ecosystem-open-source-data-science-blog-cover-image-small.png
---



Ever since I started uploading code to [github](https://github.com/harrisonized), I had the intention of allowing anyone copy and use it. After all, it's the Internet, and I would be dumb to think no one would ever copy and paste my code. If I wanted to prevent people from using it, I would keep it private. I know that once something is on the Internet, [it stays on the Internet](https://www.al.com/breaking/2011/03/everything_posted_online_is_th.html). To clarify my intention, I put the following statement in my [about](/about/) page:

> If you are currently a data science student, you have my permission to reproduce any part of my projects, including file organization, code, or explanations, as long as you give me due credit.

In hindsight, even if people checked out one of my [github repositories](https://github.com/harrisonized?tab=repositories), it's highly unlikely that they then traveled over to my blog to read that blurb on the about page. Also, these instructions are so vague. Are you only supposed to reproduce the code if you are a "data science student"? Does "give me due credit" mean putting my name in a code comment? Would it matter if you never share your project that includes my code?

[Here's what Github has to say about this](https://choosealicense.com/no-permission/):

> If you find software that doesn’t have a license, that generally means you have no permission from the creators of the software to use, modify, or share the software. Although a code host such as GitHub may allow you to view and fork the code, this does not imply that you are permitted to use, modify, or share the software for any purpose.

In other words, even though I had that statement in my github blog, theoretically no one has permission to use my code! This is absolutely **NOT** what I intended. To clarify my intentions, I had to add licenses to my projects, past and present.



## Licensing My Own Code

Github has a great site called [Choose a License](https://choosealicense.com/), which briefly explains what licenses are available and why you should choose each one. On a deeper level, Catherine Raymond has a [Licening HOWTO](http://www.catb.org/~esr/Licensing-HOWTO.html) document that answers most of my basic questions pertaining to copyright, licensing, and open source.

 The licenses that interested me the most were the [MIT License](https://choosealicense.com/licenses/mit/) and the [GPLv3](https://choosealicense.com/licenses/gpl-3.0/). Most of my coding projects I wanted to release under the MIT License. The [code](https://github.com/harrisonized/harrisonized-climbing-app) for my [climbing dashboard](https://choosealicense.com/licenses/gpl-3.0/) I wanted to release under the GPLv3 license, because as unlikely as it is, if anyone creates a derivative work, I'd like for it to also be open-source and hopefully link their project back to me so I can learn from them.

Adding licenses to projects that I wrote myself was straightforward. For the MIT License, all I had to do was follow the instructions [here](https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/adding-a-license-to-a-repository), and updating the README is optional. For GPLv3, there was the additional step of adding some text to a License section in the README.



## Crediting Others

In [building my blog](/2020/11/22/blog-update.html), I borrowed most of the code from [mediator](https://github.com/dirkfabisch/mediator) by [Dirk Fabish](https://github.com/dirkfabisch/) and some from [Lagrange](https://github.com/LeNPaul/Lagrange) by [Paul Le](https://github.com/LeNPaul/). Both of these themes are licensed under the MIT License, and Dirk Fabish even goes one step further to add this to his [README](https://github.com/dirkfabisch/mediator/blob/master/README.md):

> #### Licensing
>
> [MIT](https://github.com/dirkfabisch/mediator/blob/master/LICENCE) with no added caveats, so feel free to use this on your site without linking back to me or using a disclaimer or anything silly like that.

In the spirit of open-source, I also want to release my code under the MIT License. What is the best way to accomplish this while still giving proper attribution to the original authors?

I found [this Quora thread](https://www.quora.com/How-do-I-properly-credit-an-original-codes-developer-for-her-open-source-contribution), in which [Gil Yehuda](https://www.quora.com/profile/Gil-Yehuda) mentions how [Yahoo](https://github.com/yahoo) does the attributions using a LICENSE file and a CREDITS.md file. I then found [the code](https://github.com/yahoo/covid-19-dashboard) for [Yahoo's COVID-19 dashboard](https://yahoo.github.io/covid-19-dashboard/#/Earth), which gave me an example of how they actually structured their [LICENSE](https://github.com/yahoo/covid-19-dashboard/blob/master/LICENSE) and [CREDITS.md](https://github.com/yahoo/covid-19-dashboard/blob/master/CREDITS.md) file. I figured that if a big company like Yahoo can do it this way, it's probably good enough for me too.



## Summary

Licensing my coding projects makes it abundantly clear that my intention is to allow anyone to copy and use my code. By following the precedent set by Yahoo, I have also provided a template for how to credit past contributors if anyone were to clone my blog template and use it for themselves. Lastly, if you have any questions pertaining to any of my projects, please feel free to [email me](mailto:harrison.c.wang@gmail.com).