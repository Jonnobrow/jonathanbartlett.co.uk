+++
title = "Emacs Fatigue"
author = ["Jonathan Bartlett"]
date = 2020-10-28
draft = false
+++

As users of Emacs and Arch linux will be aware, you can't simply perfect your setup.
There is almost always something that needs tweaking, or something new a like-minded
FOSS-head is working on that you want to try instead. This is all good fun until you
reach the point where it all feels a bit excessive and overly specific to one tool
and you enter a depressive funk about your setup.

This hasn't happened to me before, but it's happening to me now and the following is
my journey to this point and some thoughts on the issue.


## From `vim` to `VimWiki` {#from-vim-to-vimwiki}

When I first switched from ~~Windows~~ to Linux I started using **vim** to take notes.
They were simple and written in Markdown. It was very pure and frankly I do miss the
simplicity of it. **vim** is very lightweight and this approach required little to no
plugins to be effective.

However as my notes started to grow in quantity and the complexity of each note
evolved (I wanted LaTeX maths and code snippets), I started to look for something
a little bit more. Enter **VimWiki**, a "Personal Wiki for Vim".

**VimWiki** lets you organise your notes and thoughts into a more structure knowledge base
and this worked for me. I could create different files for everything and navigate
through a file structure I have defined in my markup. I could also maintain to-do lists
and write diary entries.

Sadly, **VimWiki** uses a custom markup language, and although Markdown is supported,
I always felt like it just wasn't quite right. So once again I set out to find something
a little bit more. It wasn't hard to find **Emacs**, but I wasn't ready for that yet so
I kept on with **VimWiki** until the summer, when I had far too much time on my hands and
took the plunge into **Spacemacs**.


## Slowly getting more EVIL (The Spacemacs Era) {#slowly-getting-more-evil--the-spacemacs-era}

The transition from **vim** to **Emacs** wasn't particularly hard for me, EVIL mode and some
of the easy beginner friendly features of **Spacemacs** like layers made it a smooth
transition. I was also willing to sink time into it because I had read about the supposed
benefits of **org-mode** and wasn't going to miss out on the opportunity.

**Spacemacs** had the potential to be so much more that **vim** but I didn't want to overdo it and
reach the stage where the quote "a great operating system, lacking only a decent editor"
applied to me. I made myself somewhat of a promise that I would use it only for things I
could do in something like VS-Code or Atom, that included:

-   Note taking in plain text
-   Fully featured IDE for most if not all language I used
-   A terminal and file-tree
-   Syntax Highlighting
-   Some other nice-to-haves

I pulled this off quite easily, Spacemacs lets you drop in a list of layers and, due to its
relatively wide adoption, a lot of these things already existed in a layer. Perhaps the most
important thing on the list was note taking since I am mainly a student and note-taking forms
a dominant portion of my week.


## `org-mode` - your life in plain text {#org-mode-your-life-in-plain-text}

First off, I highly recommend org-mode, it is flexible, simple and versatile and offers something
for everyone if you are willing to put in the work. You can read more about it on [their shiny new
website](https://orgmode.org/).

When I first started using **org-mode** my setup consisted of a relatively small portion of the what
was possible. I took what I was comfortable with in **VimWiki** and replicated it in **org-mode** but I
also started using things like `TODO` keywords, scheduling and deadlines which helped me to keep
track of work I had to do for Uni.

Another feature I really liked was `org-babel` because it let me keep bits of code in my notes, and
execute them, and if necessary, export them to a source file. This was a little out of my initial
scope for what a text editor needed to be, but wasn't too far off, so I let it happen.

When you use **org-mode** every day, as I did and still do, you discover new things you can do and
if you have the same disposition as me you will start to explore these things when you get a little
time.
Next I want to discuss the broader topic of **Emacs** and how I started to stray from the idea of
a text editor.


## EMACS Makes Any Computer Slow {#emacs-makes-any-computer-slow}

There is a reason **Emacs** is more demanding than **vi** and that is because it can do so much more,
it can become the aforementioned operating system if you make it. A few features took my fancy
such as Email (**mu4e**) and PDF Browsing (**pdf-tools**).

I had been using **mutt** and then **neomutt** for a little while before I switched to **Emacs**, so mail
in the terminal was no stranger to me. When I switched the **Emacs** I stuck with using a dedicated
tool for mail for a couple of months, then finally caved - the first instance of wanting **Emacs**
to do more that I had originally set out. Along comes **mu4e** which does everything **neomutt** did, it
even uses the same tools in the background the sync mail, but it handles accounts in a cleaner
way and I can write emails in **org-mode** so they are prettier, plus I can have images in my **emacs**
buffers so all those modern emails don't look as bad.

I felt good about my switch to **mu4e** because I had streamlined my workflow, I had removed an entire
application from my day-to-day and replaced it with a package for **emacs**. I wanted more of this
feeling so I started to add other little bits and pieces into my day-to-day until **Emacs** was more
than I had agreed for it to be, but that was okay, I was streamlining my workflow, I felt more
productive, I had to learn fewer specific tools, I knew **Emacs**.

I found that, if anything, **Emacs** made my computer faster because I could switch buffers in **Emacs**
quicker than I could switch applications, or start a new application in Arch.


## Social Networking for Thoughts (`org-roam`) {#social-networking-for-thoughts--org-roam}

We are now at October 2020 in my story and I am using **Emacs** for a lot of things, I refuse to
browse the internet in **Emacs** and a few other things, but most things happen in **Emacs**. A friend,
[@sseneca](https://ssene.ca), mentioned to me that something called `org-roam` existed, they hadn't had the chance
to explore it yet so I was the guinea pig and installed in on my setup.

`org-roam` is an `org-mode` remake of the popular **Roam Research** tool, which employs the Zettelkasten
method of note taking. Basically each note forms a node in the network of your knowledge, you
can then form links between these notes and relate ideas together. Building your knowledge in
this way makes it very easy to jump into your notes and find everything you have on a topic.

Since it was the start of a new year at Uni, now was the time to adopt it, so I did, I created
a basic structure for all of my notes, and then started from scratch, and repeated that process
probably three to four times before I got it just right. Now I am happy with the way I take notes
and I feel that I am not just taking notes for a university module, I am recording my knowledge
on a topic and its not just my "uni notes", its my notes forever.

I even found something that lets you view all of you notes in a big interactive graph called
`org-roam-server` and had that open in my browser most of the time when I was taking notes so
I could see how my thoughts were building up, and get a kick of saying "wow, look at all then
stuff I know!".


## We are all DOOM-ed {#we-are-all-doom-ed}

DOOM Emacs was the next thing, Spacemacs started to feel sluggish and wasn't handling the masses
of **org-mode** files I was accruing. So one afternoon in early October I ditched Spacemacs and said
hello to DOOM, which is faster, more barebones and still lets me use **vi** bindings and easily manage
packages.

Moving my configurations across meant I was looking into my configuration files for the first time
in a little while, so a few things started to bug me and I started to go down the rabbit hole, I
removed a lot of stuff, then added it back again, and then I decided to commit a weekend to
making the `org-agenda` more useful to me, so that I could manage projects and schedule my uni work
for my final year.


## The Agenda Rabbit Hole {#the-agenda-rabbit-hole}

This is the last step before it all went wrong. I found some very well maintained dotfiles by a few
different people. They had all adopted the **Get Things Done** way of staying organised and had really
perfected the way they managed that in **org-mode**. One feature of **org-mode** that I hadn't really spent
any time on was the `org-agenda` and that seemed to be a focus of the **Get Things Done** way of life.

Another thing that came up was the ability to log your time against heading in **org-mode**, which I
fancied doing after logging time for an internship all summer and finding it useful in terms of
predicting what I could do in a day.

So it began, I sat down on a Saturday to setup the following things:

-   A tagging system
-   A file structure for my **org-mode** files
-   New **TODO** keywords and associated task flows
-   Custom agenda views for different purposes
-   Time clocking

It took all weekend but the result was amazing, I could see all the things I had to do next in one
neat little view, I could see all the projects I was working on, I could see the things I had
scheduled for today, I could see my deadlines for the next 30 days, and so much more. It was ...
perfect?


## So where did it all go wrong? {#so-where-did-it-all-go-wrong}

I wish I understood why I feel like I do, but I don't, I can only guess and try to solve the problem
as I always do. So my number one theory at the time of writing this is: "**emacs fatigue**" which to me
means that I have done so much with **emacs** that whenever I can't do something with **emacs** I feel like
all that work trying to make it **perfect** has been for nothing. It also means that I've run out of the
easy wins, there aren't many more untapped sources of pristine dotfiles out there. I have started
writing my own functions and learning `elisp`, and it means the return of my investment is diminishing.

I think the trigger for all of these feelings came from the realisation I had strayed from my goal
of keeping Emacs as a text editor and nothing more. I immediately decided that switching back to
**vim** was the only option, and going `cold turkey` was the only way. I quickly realised how much I would
miss **org-mode** and scurried back to the comfort of **emacs** but now with the feeling that even though
I loved **emacs** it would never be `perfect` and there will probably never be the perfect tool for all
the things I do on a computer.

I am too invested to just ditch **emacs** but I also know I have gone too far, but now I don't have the
time of the energy to dig myself out of the **emacs** hole I have dug for myself. The best I can do
is to remove the things that aren't "supposed" to be in **emacs** and hope the fatigue passes and one
day in the future I build up the courage to streamline even further, so **emacs** will be nothing more
than a note-taking tool and maybe a code editor.


## Closing Thoughts {#closing-thoughts}

I love computers, I love Linux and I love **Emacs** but I was exposed to too much fun and exciting stuff
in a short period of time and now there isn't anything else for me to do. I also feel like what I
have done isn't enough because I am a perfectionist, but I know I have really done too much.

Therefore my only closing thought is that people like me need to spend time actually enjoying a new
thing before adding something else to the mix, the Agenda Rabbit hole might have just been one step
too far or maybe it was even **org-roam**. I was just as happy before **org-roam** as I was after it, perhaps
a little more organised and productive but the same level of happy, so I could have relished that
longer before working **org-roam** into my day-to-day and maybe I wouldn't have crashed as hard, if at
all.

So, slow down and enjoy the things you make, or, like me, you will experience **Emacs fatigue.**
