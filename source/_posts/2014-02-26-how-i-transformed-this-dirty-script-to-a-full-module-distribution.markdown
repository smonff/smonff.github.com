---
layout: post
title: "How I transformed this dirty script into a full module distribution"
date: 2014-02-26 11:27
comments: true
categories: [module, perl]
featured: "/images/posts/cpan_start_build_space_rockets.jpg"
published: false
---

# Part 1 : converting the script to a module
*This post is for beginners developpers who wants to start a project from scratch but also want to use as much as possible of things from CPAN. It's supposed to be **one possible way** that defends modern pratices. I do not try to marketing my own project, that is only used as an exemple and a personnal experience on modern Perl.*

Once, I needed a script for a simple but very repetitive task. Even if this task was pretty simple to do by hand (listing dependencies used in a project, but not dependencies of dependencies), it was error proof if done by hand. So, I picked some useful modules on [MetaCPAN](https://metacpan.org), [started to write some stuff](https://github.com/smonff/Shurf/commit/a5e7aeb2fab7ef6fa5dd9cf81aa34c49860037ce), and as each time with Perl, things get done kickly than you can imagine. Work was done.

I started to write this script, considering it as a helper for a bigger project. Period. Then I realized that it has a lot of serious failings :

* I've written something because I could'nt find what I need on the CPAN. So my stuff should be accessible to others
* I haven't tested anything so each time I will make a change to the script, I wasn't shure the full script keeps working as expected
* It has been used only on one machine, running Ubuntu, I wasn't even shure it could run on Debian, Windows or on another Perl version
* The stuff starts to give me headaches
* It has no documentation
* It was very slow
* It was dirty (use system commands with backquotes and other horrors)
* It's not modular, AKA, no subroutines/methods that allow a clean API : the script is fully executed, you can't call the part that is interesting in a specific context.

Now, I'm going to tell you the story of a one week side-work that has been transformed to a three month (and more) of a Perl module development full of fun and adventure. It's more about culture and ease of use,  about performance and optimization.

## Looking at what others have already done on MetaCPAN
First of all, you really should search for what you need on [MetaCPAN](https://metacpan.org). CPAN **is** one of the main Perl's strengh so if you don't use it to make your own life simplier, all the following will be a pain. Installing modules from CPAN is [simple as getting `cpanm`](http://www.cpan.org/modules/INSTALL.html). Most of the time, a module already exist for everything you want to do, it's almost decouraging if you decide that you need to create something new.

The interresting way is to use some existing modules to create something new. For example, if you combine [App::Ack](http://beyondgrep.com), [Module::CoreList](https://metacpan.org/pod/Module::CoreList) and [Version::Compare](https://metacpan.org/pod/Version::Compare), you can easily build a tool that :

* search for declared module versions patterns (`use Foo::Bar 0.01;`)
* know if this module version is part of your Perl core modules
* know if the installed version of this module would not be a recent version (not the core one), because you may have installed it to get new functionnality or bugfixes for exemple. 

As we will see in part 2, things are not that easy, but using the tools written by others will makes things easier, because most of the tools you will find on CPAN have been developed by a community of developers, debuged, patched and step by step designed as better and better by each of this community. It's now time to proceed as them, and make your work available to the world.

## Starting not exactly from scratch
You will first need to 
Module::Starter

## Testing

## Documentatation
Tests ARE documentation

## Deploying 

* Brian d Foy slides

## Cpantesters

## Bug reports
This is what learns me the most of things. Even if I get very few bug reports at the moment (not because my module is a good quality one), the very kind people who reports bugs makes me think about things I didn't know before. For a Perl beginner like me, receiving a bug report is almost deranging, because sometimes, what the report describes is hardly understandable, I mean, it requires some hard concentration.



## Start to meet the community

* questhub.io
* stackoverflow
* cpantesters
* RT bug tracking

I would like to thank [Neil Bowers](https://twitter.com/neilbowers) who suggests me to write this blog post by answering to the two questions :

* Converting a script to a module
* What makes a "good module?

Next post will be answer the question *what makes a **good module***.

# What makes a good module ( Part 2 notes )
## The moment of depresssion
This is the moment when experimented hackers got one liners that are supposed to do the same stuff than your 2-months-long-work-and-1000-lines-module.

<blockquote class="twitter-tweet" lang="fr"><p>grep -rh ^use --include=&quot;*.pl&quot; --include=&quot;*.pm&quot; . | sort | uniq -c # Not perfect, but gives you a start on the Perl modules in use.</p>&mdash; Command Line Magic (@climagic) <a href="https://twitter.com/climagic/statuses/404992356513902592">25 Novembre 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

One liners are great... Sometimes. Problems with one liners is that you can't remember them, so you have to bookmark it, but you've got so many bookmarks that you will even not found it back when neccessary. More, how can you trust oneliners ? 

After weeks of work and the one-liner, you will surely find some [smart code that do the job in an incredibly simpliest way that you did](https://github.com/book/Act/blob/master/t/01dependencies.t). But meeeew, TIMTOWTDI !

## Things are not that easy 
When development progress, things that you don't even think of before occurs and create problems. When you are a beginner, it can be a pain, but if you have a strong motivation, you can learn very quick. That's why I believe a large part of programming today is not fully about optimized code and performance by about culture, conventions and readability.


Other interesting questions

* What I've learned from others and common best practices
* What I've learned by myself and how I became a better Perl developer

