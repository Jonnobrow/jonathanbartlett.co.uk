+++
title = "Malware detection using cloud-based online machine learning"
date = 2021-06-13
draft = true
description = "An approachable overview of my dissertation"
+++

The aim of this post is to provide an approachable overview of my dissertation. A copy of my
dissertation is available for download for those interested in reading the full document, be aware
it expects a knowledge of basic cloud concepts, machine learning and software development.

## The aims of the project

In October of 2020, when my project supervisor and I agreed on my project brief, the aims of my
project where to do two things:

1. Implement one or many machine learning algorithms with the aim of detecting malware
2. Build the underlying cloud infrastructure to support this and allow for scaling

*So what does that mean exactly?*

**Aim 1:** Well, machine learning is basically just a fancy buzzword for
maths (statistics) that has been applied to improve over time with the goal of predicting or
classifying something. In my case the maths is trying to determine whether a file is malware or not.

Malware is the name we give to malicious files and executables. This encompasses worms,
ransomware, botnets and most of the infamous "computer viruses" we hear about on the news.
Usually the aims are political or for financial gain. Malware is increasingly abundant and gets more
intelligent by the day. We used to be able to detect malware by simply looking at it closely, but
thanks to a technique called obfuscation[^fn:1] this is no longer the case. There are two approaches
to extracting data from a potentially malicious file and both result in a large quantity of things
we call features. These features can then be encoded into numbers (e.g. "malicious" -> 1) and then
used in the maths for machine learning.

**Aim 2:** The aforementioned abundance of malware means that in order to keep computers safe a huge
quantity of files must be checked. Think about how many files, apps and programs you interact with
on a day to day basis - huge right? In order to meet this demand a system that scales as more and
more files come in is required.

```
 ======================= One Analysis at a time (Slow and therefore bad) =========================
                                          +------------+                                           
                                          |            |                                              
              Unknown File--------------->|  Analyser  |------>Is it malicious?
                                          |            |                                              
                                          +------------+                                              
 ===================== Many Analyses at a time (Fast and therefore good) =========================
                                          +------------+                                   
                                          |            |                                   
                                         ->  Analyser  -\                                  
                                      --/ |            | -\                                
                                   --/    +------------+   --\                             
                               ---/       +------------+      --\                          
                            --/           |            |         -\                        
              Unknown File---------------->  Analyser  -------------> Is it malicious?     
                            --\           |            |         -/                        
                               ---\       +------------+      --/                          
                                   --\    +------------+   --/                             
                                      --\ |            | -/                                
                                         ->  Analyser  -/                                  
                                          |            |                                   
                                          +------------+                                   
```
This scaling can be achieved by using the cloud[^fn:2].

## How did I go about it?

```
 ================================ An overview of the stages of my project ========================== 

   October - December        Jan - Feb                February - April                  April     
 +--------------------+ +----------------+ +-----------------------------------+ +-----------------+
 | Background Reading | | Implementing a | | Implementing the learning         | |  Analysis       |
 | Literature Review  | | simple malware | | algorithm I chose and the elastic | |  Evaluation     |
 | Initial Design     | | analyser       | | cloud-based malware analyser      | |  Report Writing |
 +--------------------+ +----------------+ +-----------------------------------+ +-----------------+
                        +--------------------------------------------------------------------------+
                        |                                                                          |
                        |                       Testing and Project Management                     |
                        |                                                                          |
                        +--------------------------------------------------------------------------+
```

### Project Management

### Design 

### Implementation

## The results and analysis

[^fn:1]: **Obfuscation**: a technique by which code is transformed into a form that is semantically
  the same as the original program but is difficult to extract meaning from.
[^fn:2]: **Cloud**: a term describing a collection of servers (powerful computers) that are managed
  by someone else which can be used, usually for a cost, by anyone over the internet.



