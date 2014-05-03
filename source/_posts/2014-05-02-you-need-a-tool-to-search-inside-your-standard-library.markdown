---
layout: post
title: "You need a tool to search inside your standard library"
date: 2014-05-02 14:40
categories: [perl,modules,java,library]
comments: true
---
Today, programming languages have so much big standard libraries that it's a pain to know everything: one of my programming teacher (a C and C++ addict) told me that the thing that hurts him with Java and .Net was the huge standard library and the difficulty of having a good knowledge of these *toolkits*. Maybe he was not wrong, and that documentation is sometimes a kind of unusable garbage, or even disappearing (for example when Oracle moved everything about Java to random locations).

However, it can be a bigger pain to re-write something that already exists than to learn and build easy tools to search for informations inside your standard library. A tool that allow to search for common modules names (like *URI* manipulation or *JSON* or *IO*, anything), it's sometime too long to search on metacpan.org and on perl.org when you just need to `use` something in your code. This problem comes also from my bad (human) memory, I can't remember all these tools that I used only a few times. I can even less remember oneliners whith more than 20 chars...

<!--more-->

``` perl
perl -MExtUtils::Installed -MData::Dumper -e  'my ($inst) = ExtUtils::Installed->new(); print Dumper($inst->modules());'
````
... is maybe a possible solution to search for modules, but it is hard to remember.

I wondered how I could display all the modules installed on my machine (most of the time, you don't need a *new* module). So after a search, [I found something that makes possible to display all the modules on my machine](stackoverflow.com/questions/115425/how-do-i-get-a-list-of-installed-cpan-modules#answer-117386) (standard or user-installed modules, AKA `@INC` content).

The only thing that were missing was a kind of searchability. As I was too lazy and shy to try to extend `App::Module::Lister`, I've just wrote some configuration in my `.bashrc`.

First install App::Module::Lister...

``` bash
$ cpanm App::Module::Lister	
Searching App::Module::Lister on cpanmetadb ...
Fetching http://www.cpan.org/authors/id/B/BD/BDFOY/App-Module-Lister-0.15.tar.gz
Building and testing App-Module-Lister-0.15
All tests successful.
Successfully installed App-Module-Lister-0.15
1 distribution installed
```

...then in .bashrc, add the following...


``` bash Allow to search inside Perl @INC and find installed modules. Note that it can take one argument (search string)
function plister () {
  # Get the current position to go back into it  after the script run
  INITIAL_POSITION=$(PWD)
  PLISTER_HOME=$HOME/perl5/lib/perl5/App/Module/
  # Have to move because of limitation of the module
  # See http://search.cpan.org/dist/App-Module-Lister/lib/App/Module/Lister.pm#TO_DO
  cd ${PLISTER_HOME}

  # Search with a pattern
  if [ $1 ] ; then
    # \grep "unalias" grep if you got some
	  perl Lister.pm | \grep --ignore-case $1
	  echo "Searched with App::Module::Lister with pattern $1"
  # Search all modules installed
  else
	  perl Lister.pm
	  echo "Searched with App::Module::Lister with no pattern"
  fi
  cd ${INITIAL_POSITION}
}
```

`source` your `.bashrc`, and you now got a nice modules *local search-engine* based on `App::Module::Lister` abilities.

``` bash
$ plister Module::Starter
Module::Starter	1.60
Module::Starter::Smart	0.0.4
Module::Starter::Simple	1.60
Module::Starter::App	1.60
Module::Starter::BuilderSet	1.60
Module::Starter::Plugin::Template	1.60
Searched with App::Module::Lister with pattern Module::Starter
```
