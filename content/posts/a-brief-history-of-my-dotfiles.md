+++
title = "A brief history of my dotfiles"
author = ["Jonathan Bartlett"]
date = 2021-04-01
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

## chezmoi part 2 elctric boogaloo

## The Takeaway {#the-takeaway}

[^fn:1]: <https://www.ansible.com/>
[^fn:2]: <https://github.com/kespinola/dotfiles>
[^fn:3]: <https://www.chezmoi.io/>
[^fn:4]: <https://www.archlinux.org/>
[^fn:5]: <https://www.gnu.org/software/stow/>
