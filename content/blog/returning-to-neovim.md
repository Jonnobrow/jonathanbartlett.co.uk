+++
title = "Returning to neovim"
date = "2021-05-30"
lastmod = "2021-05-30"
published = "false"
description = "What's new in NeoVim and why is it my editor of choice going forward?"
tags = ["editors", "neovim", "emacs"]
+++

<details>
<summary>
tl;dr
</summary>
<p class="details">
I've moved away from Emacs and back to (neo)vim.
This post details the reasons why and some of the noteable conclusions
from my move.
</p>
</details>

## My Emacs Experience

I have written about Emacs&nbsp;[^fn:1] in the past and the motivation for that
post was a negative one.
Overall I like Emacs and a lot that it has to offer, but there are also a list
of issues that I either can't overcome or can't be bothered to overcome because a better
solution exists elsewhere.

### Features of Emacs I love(d)

Emacs automatically launched on startup of my PC, there were days when I would
do nothing except use Emacs and a web browser (Firefox) and there are some key features
that I loved in emacs.

- **org-mode (#1 feature)**[^fn:2]: "Your life in plaintext"
	- note-taking (along with org-roam[^fn:3])
	- my agenda
	- reports and documents
	- literate programming

- **IDE Features**
- **File Tree**

### The Main Issues

Emacs is pretty heavy and that is okay most of the time, especially on my custom
build desktop PC. However, I don't strive to have bloat on my PC and therefore I
welcome alternatives. A huge amount of the stuff in my Emacs configuration was rarely
used and a big chunk I only added because of temptation. I need something that works
for me and does just the stuff I use.

Emacs is not really supported anymore and some of my favourite thing is really difficult
to use across multiple devices. I used Orgzly[^fn:4] but syncing files wasn't easy
and I spent a huge amount of time resolving conflicts. I know I might not be able to use
Linux all the time 😦 and Emacs on Windows is truly awful and I can't justify to myself
spending any more time in an editor I can't use everywhere I need an editor.

Emacs eats my time. My post about "emacs fatigue"[^fn:1] mentions this. Although
I briefly kicked the fatigue when I switched to DOOM[^fn:5], it returned shortly
after I my configuration fell behind what was current and there was constantly something
that needed tweaking. I found myself in an endless loop of changing my agenda or 
note-taking strategy in order to use a cool new thing I had found and then wasting
time learning the new system and perfecting the config before starting again.

## Why now?

I have been considering the move away from Emacs for several months and my main reasons for waiting
were:

- The effort required to switch
- The time required to set up new tools
- My dependence on org-mode

And here are why those reasons don't exist at the moment:

- The negatives of Emacs have built up on me and I have stopped Emacs for most things.
	I even foung myself using vim for a lot of editing, so switching is less effort than perhaps
	it once was.
- I am on holiday and although my workload is still relatively high, I have a bit more time than
  I have had over the last few months.
- My time at University has come to a close and I don't see me making anywhere near as many notes
  or using a plaintext agenda when I start my professional life in October 2021. There are alternatives
  to the things I need in org-mode, and although I won't forget it, I don't feel it will be missed.

## Why (neo)vim?

This is an easy one. 

I have used Emacs for the last 18 months and enable vim bindings wherever I am given the
opportunity. I am trying to escape bloat and proprietary software so something like VSCode[^fn:8]
or Atom[^fn:9] is out of the question. I live in the terminal and (neo)vim lives there too.

## vim vs neovim

vim[^fn:6] is "the ubiquitous text editor" according to <https://www.vim.org/> and it is included in most
distributions I work with. This is the tool I will use when I need to make a quick config change
on the fly in a remote server and for that I don't need anything more than the basics.

vim is ideal for the basics but for my main text editor I want to achieve the features something
like Visual Studio Code[^fn:8] or Atom[^fn:9]. vim *can* do this and *is* extremely configurable,
but it uses Vim Script as its config language and isn't really meant to do some of the things I
would be asking of it.

neovim[^fn:7] is a vim-based editor and is built to be extensible, it has lua built-in and
since version 0.5 supports lua user config, the treesitter syntax engine and has a built-in lsp 
client. Therefore for my new day-to-day editor neovim is the choice.

I will still use vim, but the situations I will be using it in are limited enough that I don't 
need anything other than defaults.

## Making neovim my own

As time of writing I am using neovim nightly:v0.5.0 since this is the first version to add some
features that make neovim really attractive choice.

### Package Management

### Configuration in Lua

### Treesitter

### LSP

## Conclusions


[^fn:1]: [My post about Emacs](/blog/emacs-fatigue)
[^fn:2]: <https://orgmode.org/>
[^fn:3]: <https://www.orgroam.com/>
[^fn:4]: <http://www.orgzly.com/>
[^fn:5]: <https://github.com/hlissner/doom-emacs>
[^fn:6]: <https://www.vim.org/>
[^fn:7]: <https://neovim.io/>
[^fn:8]: <https://code.visualstudio.com/>
[^fn:9]: <https://atom.io/>
