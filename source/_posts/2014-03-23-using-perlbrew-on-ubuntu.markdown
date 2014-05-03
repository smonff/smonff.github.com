---
layout: post
title: "Using Perlbrew on Ubuntu"
date: 2014-03-23 13:33
comments: true
categories: perl, perlbrew, ubuntu
featured: "/images/posts/h1-beer.png"
published: true
---

Today I discovered that I was using
[Perl 5, version 14.2](https://metacpan.org/pod/release/FLORA/perl-5.14.2/pod/perl.pod)
(v5.14.2) from the Ubuntu 12.04 packages. It includes 57 registered patches. As it is a 2011 version, I've decided to switch from the Ubuntu packages to a more recent one (and considered as stable by the Perl community although it is not by the Debian one).

As I love to use `local::lib` and `cpanm`, I've chosen to use [`Perlbrew`](http://perlbrew.pl/) to achieve the installation of this last stable version. The tool seems as convenient as `cpanm` and `local::lib`, it is known for making your life easier when dealing with Perl installations.

`Perlbrew` is :

* *a tool to install different versions of perls* and switch through them
* *an admin-free installation management* system, means, as for `local::lib`, you don't even need to have superuser rights to use it, so you don't have to suck the admin's ass
* it's just like `RVM` for those who know Ruby

<!--more-->

The reason I write this post is that I encountered a tiny problem when installing `Perlbrew` on an Ubuntu box, so I hope it could be useful for you.

I assume that you already have `cpanm` and `local::lib` installed (you don't use the `root` account nor sudo).

``` bash We install `Perlbrew`
$ cpanm perlbrew
```

``` bash Then, we initialize its environment. Don't forget this part or it won't work :
$ perlbrew init

perlbrew root (~/perl5/perlbrew) is initialized.
	
Append the following piece of code to the end of your ~/.bash_profile and
start a new shell, perlbrew should be up and fully functional from there:
	
    source ~/perl5/perlbrew/etc/bashrc
		
Simply run `perlbrew` for usage details.
		
Happy brewing!
````		

It has created some files in `~/.perlbrew`. You need to add the one that fit your environment in your configuration file (`.bashrc` or other) : 

``` bash In .bashrc
source ~/perl5/perlbrew/etc/bashrc
```

Now start a new shell or `source` your shell configuration file, that's it *Perlbrew is on*! You can check which Perls are available :
``` bash
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
```						  

So pick a version, or ask for the last *stable* one. In fact, that's the moment I encountered an issue with `Perlbrew` and `Devel::PatchPerl.pm`. I asked for last stable Perl's installation but it failed :
```
$ perlbrew install stable
Installing /home/smonff/perl5/perlbrew/build/perl-5.18.2 into
~/perl5/perlbrew/perls/perl-5.18.2
	
This could take a while. You can run the following command on
another shell to track the status:
	
	tail -f ~/perl5/perlbrew/build.perl-5.18.2.log
	  
Installation process failed. To spot any issues, check
	  
    /home/smonff/perl5/perlbrew/build.perl-5.18.2.log
		
You might also want to try upgrading patchperl before trying again:
		  
	perlbrew install-patchperl
```

As the process was failing, I tailed the logs :
``` bash
$ tail -f ~/perl5/perlbrew/build.perl-5.18.2.log
Can't locate Devel/PatchPerl.pm in @INC (@INC contains: /etc/perl 
/usr/local/lib/perl/5.14.2 /usr/local/share/perl/5.14.2
/usr/lib/perl5 /usr/share/perl5 /usr/lib/perl/5.14 
/usr/share/perl/5.14 /usr/local/lib/site_perl .) at 
/home/smonff/perl5/bin/patchperl line 12.
BEGIN failed--compilation aborted at /home/smonff/perl5/bin/patchperl line 12.
```
I tried to update a `Devel::PatchPerl`, but updating stupidely without understanding or without reading the doc is useless. Finally, I found the answer on [StackOverflow](http://stackoverflow.com/questions/16113467/perlbrew-fails-with-cant-locate-devel-patchperl-pm), even if it was clearly mentioned in the `Perlbrew` logs, you should better read sentences till the end and do what you're told. Perlbrew seems to embark its own `Devel::PatchPerl` in `~/perl5/perlbrew/bin`, and you should not use the core one. More about this [on Perlbrew website](http://perlbrew.pl/Perlbrew-and-Friends.html)... So now do what you're told : 
``` bash
$ perlbrew install-patchperl  
```
Ok, it seems everything is ok. I tried some `perl -v` and the version has changed. But when installing Dancer with `cpanm` and setup a test Dancer application, Dancer was reconizing the old version :

![Dancer don't use Perlbrew](/images/posts/dancer.png)

That's because, in the same spirit when need a dedicated `patchperl`, the `cpanm` installation need a special process too : 
``` bash
$ perlbrew install-cpanm
	
cpanm is installed to
	
	/home/smonff/perl5/perlbrew/bin/cpanm
```	

This fixes the issues, the environment is ready. I believe all previously installed modules through `cpanm` should be re-installed now.

Perls installations will be very very easy to achieve :
``` bash
$ perlbrew install stable 
perl-5.18.2 is successfully installed.
```
**Happy brewing !**

# References
* [perlbrew.pl/](http://perlbrew.pl/)
* [Perlbrew on MetaCPAN](https://metacpan.org/pod/distribution/App-perlbrew/bin/perlbrew)
* A talk about Perlbrew by Kang-min Liu

<iframe src="http://www.slideshare.net/slideshow/embed_code/9708071" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px 1px 0; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="https://fr.slideshare.net/gugod/perlbrew" title="Perlbrew" target="_blank">Perlbrew</a> </strong> from <strong><a href="http://www.slideshare.net/gugod" target="_blank">Kang-min Liu</a></strong> </div>
