+++
title = "A brief history of my dotfile management"
author = ["Jonathan Bartlett"]
date = 2021-04-05
draft = true
+++

<details>
<summary>
tl;dr
</summary>
<p class="details">
Over the past 2-3 years I have managed my dotfiles in various different ways.
This post covers some of the highlights of that journey, and some tips for those just
starting out on their configuration journey.
</p>
</details>

{{<mermaid align="left">}}
graph LR;
	A[Symlinking] --> B[Git at Home]
	B --> C[Chezmoi]
	C --> D[Ansible]
	D --> C
{{</mermaid>}}

## So what is a dotfile and why do I need to manage them?

For the uninitiated amongst you, a "dotfile" is a configuration file for a tool, program or service
on your computer. They usually refer to the user specific configuration and often on a Unix based 
operating system like MacOS or Linux. The name comes from the `.`(dot) usually at the start of the 
filename which makes the file "hidden" in most operating systems.

---

For example `~/.config/nvim/init.lua` is the "dotfile" for my NeoVim configuration since it lives
in the hidden ".config" directory.

---


As to why you should manage them, well you don't have to and if you are using a flashy distribution
of Linux where everything is configured with graphical user interfaces then chances are you will
have very few dotfiles to manage even if you want to. In my situation, I run a more customisable
distribution of Linux called Arch Linux[^fn:4] so there are more than a handful of dotfiles to keep
track of. Furthermore my configuration changes on a regular basis and I might want to revert certain
parts so version control if a really nice thing to have.

So here are my top reasons for managing dotfiles:
- You can revert things that don't go to plan
- You can easily move dotfiles between machines (I run at least two machines)
- You can keep your configuration in text format and not have to deal with configuration GUIs

So you may now be thinking "so how do I manage my dotfiles?" and here are some approaches I have
experience with that will hopefully answer that question.

## Symlinking {#symlinking}

Symlinking (short for symbolic linking) creates a relationship between the real file and a linked
file, allowing the same file to appear multiple times in your filesystem. This can be really useful
for dotfiles since you can keep all your configurations in one place, say `~/dotfiles` and then symlink
those files to their required locations.

For example:
```shell
# ZSH Configuration
ln -sf ~/dotfiles/.zshrc ~/.zshrc
```

Now when we update the file in `~/dotfiles` the changes will propagate to the relevant places and your
config will be updated.

There are even tools to manage links like this, the most notable is Gnu Stow[^fn:5].

---

My personal experience is limited, I never got into Stow[^fn:5] and only briefly used the symlink method.
Since I use linux and almost every tool in my day to day has a config file that lives in its own unique
location I ended up confusing myself with symlinks and therefore I quickly started looking for something
else.

Now I am not saying don't use Stow[^fn:5] or plain old symlinks, but it wasn't for me when I tried it.

Another downside I noticed is that you either have to script the linking or do it manually, which again
I didn't get along with.

## `.git` at `~` {#git-at-home}

My second management tool involved creating a git repository in my home directory (`~/`).

This is a fairly simple concept. You create a git repository, add most things to .gitignore
and then add your dotfiles relative to the home directory.

Another similar approach involves creating a bare repository, again in somewhere like `~/dotfiles`,
and then changing the work tree for the git operations to be `~/` or `$HOME`.
There is a pretty good [blog post by Marcel Krčah](https://marcel.is/managing-dotfiles-with-git-bare-repo/)
that explains it better than I can.

---

I have taken both approachs, for similar periods of time and neither stuck.

The first approach meant everything below my home directory was inside a git repository and this was
far from ideal and meant that maintaining the `.gitignore` file was more than half the battle here.
Furthermore, the process of using this repo when setting up a new machine was a little confused.

The second approach was arguably better, however the work tree still covered all of the home
directory and there were special aliases required for calling git. In my mind, git should be git
and not my dotfile manager. Git can play a valuable part in managing dotfiles and it essential
to each of the strategies in this post, but it just isn't what a configuration management tool
should be (in my mind) on its own.

## chezmoi part 1 {#chezmoi-1}

Chezmoi[^fn:10] solves some problems with previous setups. It is a tool that on the
surface appears to be a wrapper for `git`, and in some ways it is, but it can 
manage secrets, run scripts and conditionally apply configuration.
These features pulled me in because the state of my laptop and PC were largely separate
apart from the basic building blocks.

Chezmoi allowed me to create a simple setup (which I ran on my laptop) and then add
to that to create the setup I used on my PC. Further to this, I was able to use 
my same Chezmoi repo to manage the dotfiles on my Windows VM when I was working
at my summer internship.

Although possibly biased, there is a comparison between Chezmoi and other dotfile
managers [that you can find here.](https://www.chezmoi.io/docs/comparison/)

The reason I left Chezmoi for a period was that some features got a little abstract.
My least favourite part was when managing dotfiles that were edited externally 
(not with `chezmoi edit <filename>`) since these would have to be added to
chezmoi again or you would risk losing them next time you applied your changes.
As a user of Emacs, where I would edit my dotfiles daily and from within Emacs,
this was a pain.

## The Ansible Era {#the-ansible-era}

During a boredom driven spiral through YouTube I stumbled across a [series of videos](https://www.youtube.com/watch?list=PL2%5FOBreMn7FqZkvMYt6ATmgC0KAGGJNAN&v=goclfp6a2IQ) by [Jeff Geerling](https://www.jeffgeerling.com/)
which go through basically everything you need to know about Ansible[^fn:1] , especially for a hobbyist like me.
I had heard of Ansible[^fn:1] prior to this point but had never really taken the time to learn about it,
and Jeff seemed like a reputable source so I started to watch.

During the series he mentions at least once about [managing his personal Mac with an Ansible playbook](https://github.com/geerlingguy/mac-dev-playbook). So I set
about looking for ways to do that myself. This is when I found myself clicking through a repository by `kespinola` on
Github&nbsp;[^fn:2]. These seemed a little more my speed as they were targeted towards linux, so I used the
layout of his repository and copied the `bin` directory for the bootstrapping script (will explain more about this later).

Now why do I need this? I run two machines, a desktop PC and a laptop. Typically their configurations have diverged
and I have used something like Chezmoi[^fn:3] to manage my dotfiles, with some success. However Chezmoi[^fn:3] is limited
when it comes to performing system operations, like installing packages, which I do on a regular basis when I install
a fresh copy of Arch Linux&nbsp;[^fn:4] on my machines. Therefore I have historically scripted this and had Chezmoi[^fn:3]
run those scripts. The appeal of Ansible[^fn:1] in this area is the idempotent nature of its operations, meaning
it won't try to install stuff that's already installed, it won't copy files if they exist or, more generally, it won't
do something it's already done.

## chezmoi part 2 electric boogaloo

## The Takeaway {#the-takeaway}

I am by no means an expert in dotfile management and have certainly not used all the possible
approaches. There is a much more comprehensive list of tools at <https://dotfiles.github.io/>
which I recommend looking at if you don't see something in this post you like the look of.

For me, Chezmoi is king at the moment. It offers me the things I need for my current situation
and the near future. Ansible is a close second but is overkill for my personal dotfiles and
I don't set up new machines from scratch often enough to warrant it. A bare git repository
is a cool, minimalist approach but only works if you have a single machine with the same
structure, or you will be managing a bunch of branches and it gets needlessly complicated.


[^fn:1]: <https://www.ansible.com/>
[^fn:2]: <https://github.com/kespinola/dotfiles>
[^fn:3]: <https://www.chezmoi.io/>
[^fn:4]: <https://www.archlinux.org/>
[^fn:5]: <https://www.gnu.org/software/stow/>
[^fn:10]: <https://www.chezmoi.io/>
