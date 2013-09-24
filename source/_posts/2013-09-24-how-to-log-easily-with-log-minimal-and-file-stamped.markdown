---
layout: post
title: "How to log easily with Log::Minimal and File::Stamped ?"
date: 2013-09-24 18:00
comments: true
categories: [perl, log, debug]
---
We love logs because it helps us to debug during development. We also need a quick way to enable / disable logs because it's not necessary for everybody :

  * you maybe want logs during the development of your module
  * people who will use it don't *necessary* want to watch it

When testing a module and building a distribution, you maybe don't want to inject *dirt* in your tests output. In the case of debuging, I think it's a good thing to write it to a file and `tail` on it.

We're gonna use two CPAN's modules, [Log::Minimal](https://metacpan.org/module/Log::Minimal), a *minimal but custimizable logger* and [File::Stamped](https://metacpan.org/module/File::Stamped), that allows *stamped log file*. They are not supposed to be build together, but Tokuhiro Matsuno(File::Stamped creator) recommend Log::Minimal in a code example.

My aim was to create a log system for the debug moment, because I was boring of using `p()` function from [Data::Printer](https://metacpan.org/module/Data::Printer) during my development and tests, because it was a bad thing: I was constantly adding and moving debug-print code, that is a bad practice. I took a look at CPAN and Stack Overflow for some logging solutions, and [Log::Log4Perl](https://metacpan.org/module/Log::Log4perl) seems to be a recommended solution for most of serious apps. But it seems to much complicated to me, I don't want to manage a new conf file, etc.

As `Log::Minimal` seems to be really simple to use, so I've installed it and was very satisfacted very quickly. Now, we are going to see how to use it to create a temporary file that will contain the traces.


``` bash Install the modules :D
$ cpanm Log::Minimal File::Stamped
```

``` perl Log some stuff to a file
use Log::Minimal;
use File::Stamped;

$ENV{LM_DEBUG} = 1;
my $tmp = '/tmp';

my $log_fh = File::Stamped->new(
  pattern => "$tmp/log.%Y-%m-%d.out",
);
	
# Overrides Log::Minimal PRINT method
$Log::Minimal::PRINT = sub {
  my ( $time, $type, $message, $trace) = @_;
  
  # Removed $trace because it was too long in my environment
  print {$log_fh} "$time [$type] $message\n";
};

debugf("My::Module debugger init.");
```
If you `tail` to `/tmp/log.2013-09-24.out` from your terminal during your module tests...

``` bash tail
$ tail -vf /tmp/log.2013-09-24.out
```

... it will log the following :

``` plain out
2013-09-24T19:28:38 [DEBUG] My::Module debugger init.
2013-09-24T19:28:39 [DEBUG] Other
2013-09-24T19:28:39 [DEBUG] interestant
2013-09-24T19:28:39 [DEBUG] things...
```

That was a very *minimal* blog post, but I hope it will be usefull.
