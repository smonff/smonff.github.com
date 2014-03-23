---
layout: post
title: "Using Perlbrew on Ubuntu"
date: 2014-03-23 13:33
comments: true
categories: perl, perlbrew
featured: "/images/posts/h1-beer.png"
published: true
---

Today I discovered that I was using [Perl 5, version 14.2](https://metacpan.org/pod/release/FLORA/perl-5.14.2/pod/perl.pod) from the Ubuntu 12.04 packages (v5.14.2). It includes 57 registered patches. As it is a 2011 version, I've decided to switch from the Ubuntu packages that to a more recent one (and considered as stable by the Perl community).

As I love to use `local::lib` and `cpanm`, a possible way to install this 5.18.2 last stable version, I've choosed to use [`Perlbrew`](http://perlbrew.pl/) to achieve this. The tool seems as convenient as `cpanm` and `local::lib`.

`Perlbrew` is :

* *a tool to install different verisons of perls*
* *an admin-free installation management* system, means, as for local::lib, you don't even need to have sudo rights to use it, so you don't have to suck the admin's ass
* just like `RVM` for those who knows Ruby

The reason I write this post is that I encountered a tiny problem when installing `Perlbrew` on an Ubuntu box, so I hope it could be useful for you.

I assume that you already have `cpanm` and `local::lib` installed (no `root`).

We install `Perlbrew` :

    $ cpanm perlbrew

Then, we initialize it's environment. Don't forget this part or it won't work :

    $ perlbrew init
	
	perlbrew root (~/perl5/perlbrew) is initialized.
	
	Append the following piece of code to the end of your ~/.bash_profile and start a
	new shell, perlbrew should be up and fully functional from there:
	
	    source ~/perl5/perlbrew/etc/bashrc
		
	Simply run `perlbrew` for usage details.
		
	Happy brewing!
		

It create some files in `~/.perlbrew`. You need to add the one that fit your environment in your configuration file (`.bashrc` or other) : 

    # In .bashrc
    source ~/perl5/perlbrew/etc/bashrc
	
Now start a new shell or `source` your configuration file, that's it ! You can check wich Perls are available :

    $ perlbrew available
	  perl-5.19.10
	  perl-5.18.2
	  perl-5.16.3
	  perl-5.14.4
	  perl-5.12.5
	  perl-5.10.1
	  perl-5.8.9
	  perl-5.6.2
	  perl5.005_04
	  perl5.004_05
	  perl5.003_07
						  

So pick a version, or ask for the last *stable*. In fact, that's the moment I encountered an issue with `Perlbrew` and `Devel::PatchPerl.pm`. I asked for the installation of last stable Perl but it failed :

    $ perlbrew install stable
	Installing /home/smonff/perl5/perlbrew/build/perl-5.18.2 into
	~/perl5/perlbrew/perls/perl-5.18.2
	
	This could take a while. You can run the following command on
	another shell to track the status:
	
	  tail -f ~/perl5/perlbrew/build.perl-5.18.2.log
	  
	Installation process failed. To spot any issues, check
	  
	  /home/smonff/perl5/perlbrew/build.perl-5.18.2.log
		
	If some perl tests failed and you still want install this
	distribution anyway, do:
		
	  (cd /home/smonff/perl5/perlbrew/build/perl-5.18.2; make install)
		  
	You might also want to try upgrading patchperl before trying again:
		  
	  perlbrew install-patchperl
			
	Generally, if you need to install a perl distribution known to
	have minor test failures, do one of these command to avoid seeing this message
			
	  perlbrew --notest install stable
	  perlbrew --force install stable
				
As the process was failing, I've tailed the logs :

    tail -f ~/perl5/perlbrew/build.perl-5.18.2.log
	Can't locate Devel/PatchPerl.pm in @INC (@INC contains: /etc/perl 
	/usr/local/lib/perl/5.14.2 /usr/local/share/perl/5.14.2
	/usr/lib/perl5 /usr/share/perl5 /usr/lib/perl/5.14 
	/usr/share/perl/5.14 /usr/local/lib/site_perl .) at 
	/home/smonff/perl5/bin/patchperl line 12.
	BEGIN failed--compilation aborted at /home/smonff/perl5/bin/patchperl line 12.
	
Finally, I found the answer on [StackOverflow](http://stackoverflow.com/questions/16113467/perlbrew-fails-with-cant-locate-devel-patchperl-pm), even if it was clearly mentioned in the `Perlbrew` logs, you should better read sentences to the end :

    $ perlbrew install-patchperl  

I've tryied to update a `Devel::PatchPerl` before but updating stupidely without understanding or before reading the doc is useless.Perlbrew seems to embark it's own `Devel::PatchPerl` in `~/perl5/perlbrew/bin`, that's why it conflicts with the core version.

Now you got this enviroment ready, you can install Perl very very easily :

    $ perlbrew install stable 
	perl-5.18.2 is successfully installed.
	

