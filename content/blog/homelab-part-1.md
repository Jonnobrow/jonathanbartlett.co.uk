+++
title = "Homelab - Part 1"
date = "2021-05-18"
description= "The beginnings of my homelab"
toc = true
tags = ["homelab", "devops", "self-hosting"]
[series]
    name = "homelab"
    number = 1
+++

Now that I have finished university I have nearly 5 months of
unemployment ahead of me before I start my graduate role at Sky.
Therefore I need a project to keep my entertained. I have a huge list
of things to get done, some of which will end up immortalised as blog
posts on here. However, if you are reading this you are probably here
to hear about the beginnings of my Homelab.

## What is a Homelab

**r/homelab** provides the following definition[^fn:1]:

> Homelab \[hom-læb\]\(n\): a laboratory of (usually slightly outdated)
> awesome in the domicile

So essentially, it is some hardware that you set up a home in order to:

1. Experiment with "enterprise" kit in your home
2. Learn about network infrastructure
3. Self-host your own apps and services

For me it's the predominantly the last one, with a little of number 2 in
there as well. I will get to play with the enterprise stuff at work and
my degree has taught me about the underlying infrastructure, therefore
its mainly the benefits of self-hosting and the DevOps experience I get
along the way that I am interested in.

_So to quickly summarise, a homelab is some tech you have at home that
you can play around with, learn about and have fun with all at the same
time._

### The benefits of self-hosting

- **You own your data**. If it's on your hardware, then you own it. You
  aren't giving your memories to Google or relying on Spotify and
  Netflix to have the music and tv you want to watch.
- **You learn a lot**. Setting up some of this stuff isn't easy and
  isn't for beginners (there is a lot that is though) and it _can_ be a
  bit of a steep learning curve. However the community is great and will
  probably help you if you ask 😜
- **You can make it your own**. This may seem obvious, it may not, but
  self-hosting gives you that all important freedom to do whatever you
  like and with that comes the ability to tweak pretty much everything
  to your liking. There is a big overlap between free-software[^fn:2] and
  self-hosting so if you are so inclined you are almost certainly able
  to contribute to or fork a project so that a feature you have always
  wanted is included. And failing that, ask nicely and someone might get
  to it eventually.

## My current experience

So you might be able to tell that this isn't exactly new to me.
Although this is first time I am inclined to call it a Homelab, I have
had some form of self-hosting going for a little while. Mainly on
Raspberry Pis and as virtual machines on my desktop PC, so I have been
quite limited in what I can do, but its a great place to start if you
aren't sure it's for you, or if you can't afford to build or buy a
server.

My _credentials_ in the Homelabbing space are:

- Self-hosted Nextcloud[^fn:4], Minecraft, Jellyfin[^fn:5] (Just for me
  on a RPi)
- Deployed K8s[^fn:6] clusters the hard way[^fn:3] and with various scripts
- Work experience with cloud technologies, docker and networking
- My dissertation was half DevOps
- I have a computer science degree in which I took some advanced
  networking modules

I definitely do not know everything, far from it and I would still call
myself a Homelab beginner. A lot of the things I have set up were
relatively easy and my practical experience is limited, hence Homelab -
Part 1 and not Homelab - Part x.

## The sad state of affairs that is my home network

I wish I could tell you my home network was dreamy with gigabit fibre
broadband, high quality switches and access points, ethernet everywhere
and all the bells and whistles.

However, the real state of my network is dire. I am a student still
(technically) when writing this and I live in a flat, which I rent,
therefore my ability to install ethernet, access points and the rest
is limited.

I also had the misfortune of falling in love with a flat that receives
a maximum of 10Mbps download and 1Mbps upload speed.

![My abismal speedtest](/speedtest.png)

My home network actual consists of a Sky Hub with a single 25m ethernet
cable clipped to the skirting board all the way to the office. In there
it goes in to a 5 port gigabit switch and provides the upstream for my
PC, a range extender (because we have concrete walls too 😦), the
printer and my Raspberry Pi.

I do plan to upgrade all this in the future but I don't see the point in
getting good network hardware when the highest speed it will see is
10Mbps.

## Building a "Server"

From my research it seems that there are four main choices when
it comes to getting in to homelabbing. These are listed below in order
of cost (both initial and ongoing, descending):

1. The first is to buy enterprise grade kit second or third hand from
   somewhere like eBay and deal with the incessant whining of high rpm
   fans and the power bill that will kill your WAF[^fn:7].
2. _(My choice)_ Buy components, either new or used, and build a server
   PC yourself.
3. Buy some kit like Raspberry Pis and have a play since
   they are cheap and low power. One day you might even get to a crazy
   setup that dwarfs enterprise solutions[^fn:8].
4. The last is that you just had some old kit lying around like an old
   PC and you install an operating system on it an you're good to
   go. Unfortunately I am not in that position and all the kit I have is
   either in use or useless.

So I decided to use consumer parts and build my own server PC. Although
for similar money I could have purchased a rack mountable, ex-enterprise
server, this would be extremely overkill for my needs and in the long
run probably cost me an arm and a leg whilst killing baby seals in the
process.

### The Parts

| Part Type       | Part Choice                           | Rationale                              |
| --------------- | ------------------------------------- | -------------------------------------- |
| **CPU**         | Ryzen 3600                            | 65W TDP and 12 Threads                 |
| **Memory**      | 16GB DDR4 3200Mhz (KLEVV BOLT X)      | 16GB seems like the minimum these days |
| **Storage**     | 500GB WD Blue SSD (For now)           | Boot Drive and VM/OS Storage           |
| **PSU**         | AeroCool Integrator 500 W, 80+ Bronze | Cheap and reasonably efficient         |
| **Motherboard** | Gigabyte B450M DS3H                   | 64GB RAM, Good expandability           |
| **Case**        | Antec VSK-3000-Elite                  | 4 3.5" Bays, Not Ugly and MicroAtx     |

## What services will I be deploying?

I can divide the software portion into a few different categories. These
being the host operating system, main virtual machines and then the
services I will use internally and the services I will use externally.

### Host Operating System

For the host operating system I will be using Proxmox Virtualization
Environment (PVE)[^fn:9] which is AGPLv3[^fn:10] and is a distribution
of linux based on Debian. It is virtualization management system that is
based on QEMU/KVM, with which I am familiar, and will therefore allow me
to create virtual machines easily. It also provides some nice management
features and supports clustering with other PVE nodes for
redundancy - something I may explore in the future.

My reasoning for choosing a virtualization environment over a server OS
like Ubuntu 20.4 LTS is that I hope to run several virtual machines in
the future including TrueNAS Core for data storage, pfsense for routing
and K8s.

### Main Virtual Machines

For now there will be only a single virtual machine, however as I
explained in my rationale for the host operating system, I plan to add
more in the future.

The one virtual machine will basically use all of the system resources
not consumed by Proxmox and will house a K8s cluster. I have toyed with
splitting my cluster into a few separate virtual machines but given that
they will all be on the same hardware, the redundancy and
high-availability benefits are void - it's also just me using it and if
it goes down, I just have to deal with it.

K8s is a container orchestration tool that will allow me to neatly run
all of my services with much less overhead than if I provisioned virtual
machines for them all. It is also a great technology to learn in 2021.

### Utilities and Services for Internal Use

**High Priority**:

- NGINX Proxy[^fn:17]: Access to services
- Wireguard[^fn:22]: Remote access to home network
- Prometheus[^fn:13]: Monitoring
- Grafana[^fn:14]: Dashboard for metrics
- Syncthing[^fn:20]: File synchronisation between devices

**Nice to Have**:

- Transmission[^fn:23]: BitTorrent client
- Sonarr[^fn:11]: TV Show Management
- Radarr[^fn:11]: Movie Management
- Lidarr[^fn:11]: Music Management
- Jackett[^fn:12]: Query aggregator for \*arr apps above
- Organizr[^fn:21]: Dashboard for services

### Services for External Use

**High Priority**:

- Jellyfin[^fn:5]: Movies, TV, Music
- Nextcloud[^fn:4]
- Tiny Tiny RSS[^fn:19]: RSS Aggregator
- My Personal Website

**Medium Priority**:

- Photos: Photoprism (Synced with Nextcloud)
- Prosody IM[^fn:15]: XMPP
- Paperless-ng: Document manager

**Nice to Have**:

- Recipe Sage[^fn:16]: Recipe management and food planner
- Sourcehut[^fn:18]: Software Development

## What's next?

Well, the first thing is to build the PC. The parts are on the way and
as soon as they arrive I will get started. Then I need to setup Proxmox
and Kubernetes. After that I will get started on moving over some of the
services I already self-host and the things that I have labelled as High
Priority. Once all that is done I will move on to the other services,
and eventually I will start adding in more VMs for things like TrueNAS
Core and pfsense, but thats a while away yet.

Subscribe to my RSS feed so you don't miss Homelab - Part 2.

[^fn:1]: <https://www.reddit.com/r/homelab/wiki/introduction>
[^fn:2]:
    Free as in freedom. Free Software gives everybody the rights to
    use, understand, adapt and share software. <https://fsfe.org/>

[^fn:3]: <https://github.com/kelseyhightower/kubernetes-the-hard-way>
[^fn:4]: <https://nextcloud.com/>
[^fn:5]: <https://jellyfin.org/>
[^fn:6]: K8s == Kubernetes <https://kubernetes.io/>
[^fn:7]:
    Wife Acceptance Factor <https://openhomelab.org/index.php/WAF>
    or <https://en.wikipedia.org/wiki/Wife_acceptance_factor>

[^fn:8]: <https://www.reddit.com/r/homelab/comments/jqh6m8/raspberry_pi_k8s_cluster>
[^fn:9]: <https://pve.proxmox.com/wiki/Main_Page>
[^fn:10]: <https://www.gnu.org/licenses/agpl-3.0.en.html>
[^fn:11]: <https://wiki.servarr.com/Main_Page>
[^fn:12]: <https://github.com/Jackett/Jackett>
[^fn:13]: <https://prometheus.io/docs/introduction/overview/>
[^fn:14]: <https://grafana.com/>
[^fn:15]: <https://prosody.im/>
[^fn:16]: <https://recipesage.com/#/welcome>
[^fn:17]: <https://nginx.org/en/>
[^fn:18]: <https://sr.ht/>
[^fn:19]: <https://tt-rss.org/>
[^fn:20]: <https://syncthing.net/>
[^fn:21]: <https://github.com/causefx/Organizr/>
[^fn:22]: <https://www.wireguard.com/>
[^fn:23]: <https://transmissionbt.com/>
[^fn:24]: <https://github.com/jonaswinkler/paperless-ng>
