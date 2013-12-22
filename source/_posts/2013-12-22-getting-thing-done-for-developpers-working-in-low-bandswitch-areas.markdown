---
layout: post
title: "Getting things done for developers working in low-band-switch areas"
date: 2013-12-22 20:01
comments: true
categories: [travel, mobile, data-plan, development, telecommuting, work, GTD, SSH]
featured: "/images/posts/getting_things_done.jpg"
published: true
---

Now I have landing in [Lao PDR](https://en.wikipedia.org/wiki/Lao_PDR) and I don't got my lovely high-speed Internet connection. All I have is a cool smartphone without a SIM card for this country. Thought, it don't seem to be a problem to find it because just before the immigration point in airport, the application form already featured an ad for [one of the main telecoms company](http://www.beeline.la/) that explain that  *to really communicate like a local*, I should apply to it.

## Get an unlimited data plan
Anyway, I have to work for my [french company](http://etyssa.fr) (that kindly grant me some holidays, just in time to empty my french flat and have some more time to find electricity and Internet connection in Lao PDR) just before to be back to work.

Finding a new SIM card is not a problem (you can literally find credit sellers for mobile phones at *each street corner*), and you don't even need to give an address or have a bank account. The main problem is you don't speak Lao, and Lao people don't speak English that much. So maybe they will not understand you need this killer-unlimited-data-plan and give you some *re-loadable* cards. Anyway, you don't speak Lao either...

You will understand very soon that unlimited data plan, including phone calls and SMS like the one I previously know with [Free](http://free.fr) in France is not that easy to find. 5 x 50 000 kips (5€) cards later and two days of surfing has been enough to empty my cards :( , that was supposed to allow 2.5G of data.

## First sensations
Once I have asked to the nice couple who sell me the cards where I can get a bigger data-plan, they redirect me to 
the nearest [Lao Telecoms](http://laotel.com/home_Lao.html) office where people have been able to advice me a decent unlimited data-plan (still in alpha-test for me). Some impressions about it :

* SSH is available through the phone hotspot but pretty slow
* band-switch is pretty good
* no outgoing SMTP (or don't find it for my current ISP)
* some obtained IPs seems to be considered as potentially dangerous (open proxies, low reputation for spam, e.g. blacklisted on Wikipedia)
* band-switch speed is bipolar
* Some websites are hardly accessible, I don't know if they are *locked* or *censored*, or only hosted on too far away servers (?)
* network and / or electricity can be down

### Solutions to problems
#### Mosh
![Never let you abuse by stuff that shouldn't happen](/images/posts/broken_pipe.jpg)

First, I was very worried about SSH *availability*. A friend tried it for me from Laos before my departure, and the stuff seems to be OK. But I've forgot something: SSH is not that lightweight, mainly because of it's strong encryption.

Another friend advices me to use [Mosh](http://mosh.mit.edu) :
{% blockquote Mosh contributors, http://mosh.mit.edu %}
Remote terminal application that allows roaming, supports intermittent connectivity, and provides intelligent local echo and line editing of user keystrokes.

Mosh is a replacement for SSH. It's more robust and responsive, especially over Wi-Fi, cellular, and long-distance links.
{% endblockquote %}

And Mosh is absolutely incredible...

SSH was almost unusable without it, and even dangerous because of the latency time. I've experimented that it could increase typing errors in case you need some reactivity. Mosh seems to be the best solution for mobile workers that need a connection to a remote SSH server because of two of it's main features :

* **roaming connection available**. You can literally switch to a connection to another and the connection between the local and distant Mosh servers will be maintained.
* **predictive typing**. As the connection will be slow, typing through SSH will have a long latency (this mades me very angry). Predictive typing will reduce the latency in an awesome way, you can't almost feel it. In their own words, the stuff *get rid of network lag*.

My life has been changed the day I have discovered [Tmux](https://en.wikipedia.org/wiki/Tmux). I believe the use of Tmux + Mosh will make your life easier too  if you telecommute in uncommon places, have to switch connections easily or simply hibernate your computer without experiment some *broken pipe* horrors.

#### Tor
I don't know how the Web could be controlled in Lao PDR. I've encountered some problems to access different technical websites :

* I have been forced to switch from French Ubuntu repositories to Malaysian one,
* http://mosh.mit.edu and http://octopress.org weren't available,
* a lot of website stress me with the accusation I'm a bot, or spammer, and I am sometimes blacklisted (especially on Wikipedia, but I've also typed too many CAPTCHA in one week)
* some more things are *weird*

I say weird because I can access to these sites through [Tor](https://www.torproject.org/).

![Connection to some websites are better through Tor](/images/posts/good_luck_7_proxies.png)

{% blockquote Tor Project Wikipedia page, https://en.wikipedia.org/wiki/Tor_%28anonymity_network%29 %}
Tor is free software network for enabling on-line anonymity. Tor directs Internet traffic through a free, worldwide, volunteer network consisting of more than four thousand relays to conceal a user's location or usage from anyone conducting network surveillance or traffic analysis. Using Tor makes it more difficult to trace Internet activity)
{% endblockquote %}


Now each time I got troubles for accessing a part of the web I try through Tor: when it was impossible, it solves the problem most of the time, and when it's too slow, it sometimes help. I've never felt the need of using an onion relay network (or *proxy*) in France, but it is necessary here.

#### Electricity and network disruptions
It's not a daily thing, but for the first time in one week, we got a total district electricity failure that has begun in the morning and lasts in the afternoon. 

Electricity and network disruptions can be very localized : find a nice place to work in another district in case of emergency.
