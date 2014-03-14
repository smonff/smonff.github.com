---
layout: post
title: "How I transformed this dirty script into a full module distribution"
date: 2014-02-26 11:27
comments: true
categories: [module, perl]
featured: "/images/posts/CPAN_start_building_space_rockets.jpg"
published: false
---

# Part 1 : converting the script to a module
*This post is for beginners developers who wants to start a project from scratch but also want to use as much as possible of things from CPAN. It's supposed to be **one possible way** that defends modern practices. I do not try to do marketing over my own projects, that is only used as an example and a personal experience on modern Perl.*

Once, I needed a script for very repetitive task. Even if this task was pretty simple to do by hand (listing dependencies used in a project, but not dependencies of dependencies and generating a report), it was error proof if done by hand. So, I picked some useful modules on [MetaCPAN](https://metacpan.org), [started to write some stuff](https://github.com/smonff/Shurf/commit/a5e7aeb2fab7ef6fa5dd9cf81aa34c49860037ce), and as each time with Perl, work was done quicker than you can imagine.

I started to write this script as a helper for a bigger project. Period. Then I realized that it has a lot of serious failings :

* The script wasn't easily accessible to others on CPAN
* I haven't tested anything so each time I will make a change to the script, I wasn't sure the full script keeps working as expected
* It has been used only on one machine, running Ubuntu, it wasn't supposed to be abble run on Debian, Windows or on another Perl version
* The stuff starts to give me headaches
* It has no documentation
* It was very slow
* It was dirty (use system commands with back-quotes and other horrors)
* It was not modular, AKA, no subroutines/methods that allow a clean API : the script is fully executed, you can't call the part that is interesting in a specific context.

That's why at this point, the solution to fix all these issues was to start a module distribution. Now, I'm going to tell you the story of a one week side-work that has been transformed to a three month (and more) of a Perl module development full of fun and adventure. It's more about culture and ease of use, than about performance and optimization.

## Looking at what others have already done on MetaCPAN
First of all, you really should search for what you need on [MetaCPAN](https://metacpan.org). CPAN **is** one of the main Perl's strength so if you don't use it to make your own life simpler, all the following will be a pain. Installing modules from CPAN is as [simple as getting `cpanm`](https://metacpan.org/pod/App::cpanminus). It's almost encouraging if you decide that you want to create something new because most of the time, a module already exist for everything you want to do.

The interesting fact is that you can use some existing modules to create something new. For example, if you combine [App::Ack](http://beyondgrep.com), [Module::CoreList](https://metacpan.org/pod/Module::CoreList), [Module::Version]() and [Version::Compare](https://metacpan.org/pod/Version::Compare), you can easily build a tool that :

* Search for declared module *used* (`use Foo::Bar;`)
* Know if this module version is part of your Perl core modules
* Retrieve your installed module version
* In case this module is in Perl core but you would use a more recent one, know it (you may have installed it to get new functionality or bug-fixes for example). 

Using the tools written by others will makes your life easier, because most of the tools you will find on CPAN have been developed by a community of developers, debugged, patched and step by step designed as better and better by the community. It's now time to proceed as them, and make your work possibly available to the world.

## Starting not exactly from scratch
To start building a module, you will need a distribution architecture. Typical distribution looks like the following : 

    Foo-Bar/
	├── Changes
	├── lib
	│   └── Foo
	|       ├── Bar.pm
	│       └── Bat.pm
	├── Makefile.PL
	├── MANIFEST
	├── README
	└── t
	    ├── 00-load.t
		├── boilerplate.t
		├── manifest.t
		├── pod-coverage.t
		└── pod.t
						

The full distribution directories and files tree, a lot of boilerplate code, and the module squeleton, containing documentation and licences templates can be generated automatically using `Module::Starter` (not part of a Perl core - but why?). 

    $ cpanm Module::Starter Module::Starter::Smart
    $ module-starter --module=Foo::Bar,Foo::Bat --author="Sébastien Feugère" --email=smonf@cpan.org
	
Much more arguments can be used, so please consider reading `perldoc Module::Starter`.
	
	# First argument is the module to add, second is the existing distro (don't use `::` separator)
    $ module-starter --module=Foo::Bar::Me --distro=Foo-Bar  

`Module::Starter::Smart` allows to *add new modules into an existing distribution*, a thing that is not permitted by `Module::Starter`. It will be useful if your distribution grows up all along your development process. You are not supposed to know all your distribution architecture at it's beginning.

This is definitively a way to get a common architecture for your distribution and start to speak the common language of CPAN. Remember that if a module don't look right for you, most of the time, you can find many one that do almost the same thing in a different way, so feel free to explore.

## Testing

## Documentatation
* Tests ARE documentation
* README matters (if README is just like shit, there is more chances that your module smells like shit too)

## Deploying / sharing
[Setting up a Github repository](https://help.github.com/articles/create-a-repo)

Deploying to CPAN open new fields in Perl development :

* you can test your module on a large variety of Perl versions (Perltesters)
* others can install your module through `cpanm` easily

Brian d Foy said :

> Upload early and often
>
> * You don't have to be perfect, or even good
> * Other people can help as soon as possible
> * CPAN Testers can send you feedback

## Cpantesters

## Bug reports
This is what learns me the most of things. Even if I get very few bug reports at the moment (not because my module is a good quality one), the very kind people who reports bugs makes me think about things I didn't know before. For an intermediate Perl beginner like me, receiving a bug report is almost deranging, because sometimes, what the report describes is hardly understandable, I mean, it requires some hard concentration. So this is a good way to practice and go further.



## Start to meet the community

* questhub.io
* stackoverflow
* cpantesters
* RT bug tracking

I would like to thank [Neil Bowers](https://twitter.com/neilbowers) who suggests me to write this blog post by answering to these two questions :

* Converting a script to a module
* What makes a "good module"?

Next post will answer the question *what makes a **good module***.

# References

* *Uploading Your First Module to CPAN, brian d foy, The Perl Review, YAPC::EU 2011, Riga* is a valuable document, quick to read and full of wonderful tools

# What makes a good module ( Part 2 notes )
## The depression time
This is the moment when experimented hackers got one liners that are supposed to do the same stuff than your 2-months-long-work-and-1000-lines-module.

<blockquote class="twitter-tweet" lang="fr"><p>grep -rh ^use --include=&quot;*.pl&quot; --include=&quot;*.pm&quot; . | sort | uniq -c # Not perfect, but gives you a start on the Perl modules in use.</p>&mdash; Command Line Magic (@climagic) <a href="https://twitter.com/climagic/statuses/404992356513902592">25 Novembre 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

One liners are great... Sometimes. Problems with one liners is that you can't remember them, so you have to bookmark it, but you've got so many bookmarks that you will even not found it back when neccessary. More, how can you trust one liners ? 

After weeks of work and the one-liner, you will surely find some [smart code that do the job in an incredibly simpliest way that you did](https://github.com/book/Act/blob/master/t/01dependencies.t). But meeeew, TIMTOWTDI !

## Things are not that easy 
When development progress, things that you don't even think of before occurs and create problems. When you are a beginner, it can be a pain, but if you have a strong motivation, you can learn very quick. That's why I believe a large part of programming today is not fully about optimized code and performance by about culture, conventions and readability.


Other interesting questions

* What I've learned from others and common best practices
* What I've learned by myself and how I became a better Perl developer

