---
title: "Design: Edge Cases and Legibility"
tags: ["design"]
date: 2020-06-17
lastmod: 2020-06-20
draft: false
---

From [A Designer's Code of Ethics](https://muledesign.com/2017/07/a-designers-code-of-ethics ):

> ## A designer does not believe in edge cases.
>
> When you decide who you’re designing for, you’re making an implicit statement about who you’re not designing for. For years we referred to people who weren’t crucial to our products’ success as “edge cases”. We were marginalizing people. And we were making a decision that there were people in the world whose problems weren’t worth solving.
>
> Facebook now claims to have two billion users. 1% of two billion people, which most products would consider an edge case, is twenty million people. Those are the people at the margins.
>
> > “When you call something an edge case, you’re really just defining the limits of what you care about.” 
> >
> > -- Eric Meyer
>
> These are the trans people who get caught on the edges of “real names” projects. These are the single moms who get caught on the edges of “both parents must sign” permission slips. These are the elderly immigrants who show up to vote and can’t get ballots in their native tongues.
>
> They are not edge cases. They are human beings, and we owe them our best work.

I don't think the main issue is that designers think consciously about an edge case and decide not to care about it.  That is an issue -- but to me, the bigger problem is that there's a lot of edge cases that just... don't occur to the designer, or anyone else involved in the product.  Designers wind up making a lot of assumptions subconsciously, without even realizing what's happening.  Thus, it's not enough to just say "oh, I'll just deal properly with all the edge cases as they come up" -- you must actively work to divest yourself of these assumptions.

To me, "good" design (universal, accessible, and to some extent pleasing) consists of making as few assumptions as possible about your users.  When design does make excessive assumptions, or false assumptions, it often has the effect of forcing the users to fit the design.  This is upsetting to the users, who (if they have a choice in the matter) may then choose to stop using the product (if it is a product).  However, if the product or design is imposed on the user by an outside force, the resulting effect -- of forcing the user to fit the designer's assumptions -- gets far more dangerous.

What I wish to do now is provide context: a list of links that have informed and shaped my thinking on this topic.

Epistemological resources to help you on your quest:

* Of prime importance is the concept of legibility, which I first encountered in the SlateStarCodex review of "Seeing like a State", [here](https://slatestarcodex.com/2017/03/16/book-review-seeing-like-a-state/ ).  
* Further reading on legibility: [Notes on the Legibility War](https://notebook.drmaciver.com/posts/2019-06-04-09:58.html ), [Legibility Privileges](https://notebook.drmaciver.com/posts/2020-02-23-09:37.html ), and [Legibility as a relationship](https://notebook.drmaciver.com/posts/2020-03-02-09:31.html ) by D. R. MacIver.  These posts also link to other resources on legibility.
* [Notes on the Legibility War](https://notebook.drmaciver.com/posts/2019-06-04-09:58.html ) also mentions the idea of torque, which is the "force that acts between lives and classification systems, shaping each to the other".  I tend to prefer Siderea's concept of Procrustean Epistemologies: [Part 1](https://siderea.dreamwidth.org/1540620.html ), [Part 2](https://siderea.dreamwidth.org/1541132.html ), [Metalogue 1](https://siderea.dreamwidth.org/1548040.html ).
* Also related is the concept of the [typical mind fallacy](https://wiki.lesswrong.com/wiki/Typical_mind_fallacy ).

Lots and lots (and lots) of examples of specific problems:

 * [Means Tests and Self-Employment](https://siderea.dreamwidth.org/1430601.html ) and [De Facto Sexism](https://siderea.livejournal.com/1220766.html ), also by Siderea
 * [Dear Tech, You Suck at Delight](https://medium.com/talking-microcopy-writing-ux/dear-tech-you-suck-at-delight-86382d101575 )
 * [Inadvertent Algorithmic Cruelty](http://meyerweb.com/eric/thoughts/2014/12/24/inadvertent-algorithmic-cruelty/ )
 * [Personal Histories](http://www.sarawb.com/2015/01/13/personal-histories/ )
 * [I tried tracking my period and it was even worse than I could have imagined](https://medium.com/@maggied/i-tried-tracking-my-period-and-it-was-even-worse-than-i-could-have-imagined-bb46f869f45 )
 * [The deadly truth about a world built for men](https://www.theguardian.com/lifeandstyle/2019/feb/23/truth-world-built-for-men-car-crashes )
    * Also: the standard computer keyboard key size was designed for men's hands.  There were some experiments in Asia with smaller keyboards, which many women apparently find more comfortable, but they never caught on (and can't even be found in the ergo keyboard market today as far as I'm aware, and I'm pretty obsessive about keyboards).  Like it or not, the standard key size is 19mm, and you're stuck with it even more than you're stuck with QWERTY -- at least layout can be reprogrammed in software.  
 * [You're Always Coming Out](https://thefourthvine.tumblr.com/post/81447259791/youre-always-coming-out )
    * I'd like to especially call your attention to the quote "we only ever fit on forms designed by people like us".  If someone on your team frequently falls into an often-overlooked edge case, they're more likely to notice it in your product.  It's the best practical argument I know of for diversity in the workplace.
 * [Twitter thread on TSA body scanners](https://twitter.com/DataPup_/status/1118516662305685504 )
 * [Falsehoods Programmers Believe About Names, with examples](https://shinesolutions.com/2018/01/08/falsehoods-programmers-believe-about-names-with-examples/ )
    * I'd like to append to this one the vast number of systems that have trouble dealing with trans people, whose deadnames can often still be their legal names, but who would prefer to be known in all situations by a different chosen name.  Particularly medical and administrative systems, as well as anything that insists on a "legal name" for no apparent reason.  If you are involved in the design of a system that deals with names, please let the user provide a "preferred name" that may bear no relation to their legal name, and use the preferred name whenever absolutely possible.  Do not, if you are a doctor's office, shout a patient's deadname to the whole waiting room.  Do not, if you are sending users push notifications or messages, refer to them by deadname in the notification.
 * Don't have a link for this on hand, but please consider people who've recently lost their mothers and people who've cut off toxic or abusive mothers when writing cutesy Mother's Day email copy (similar deal for Father's Day and all other parent- or family-related holidays).
 * My rant about the FitBit [here]({{< ref "/posts/fitbit.md" >}})

> "Indifference towards people and the reality in which they live is actually the one and only cardinal sin in design."  
> -- Dieter Rams


