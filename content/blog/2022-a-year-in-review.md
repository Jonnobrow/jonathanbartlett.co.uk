+++
title = "A Year in Review"
date = "2022-10-23"
description = """It has been 10 months since my last update, here is what I have been up to..."""
toc = true
tags = ["homelab", "devops", "self-hosting", "wedding"]
+++

It has been 10 months since my last update, here is what I have been up to...

## October 2021 - Joining Sky 

[Sky](https://sky.com) was, and continues to be, a fantastic first workplace. It provided a great space
for me to learn and explore technologies with no real pressure to deliver results unless
I felt ready.

The first few weeks were occupied by a technology Bootcamp where we learned about everything
from basic Python programming through to AWS and configuration management in Puppet. I then
joined a small team with some other graduates to build an internal tool for capturing and
analysing the cost of incident investigations - this primarily used Golang and GCP.

## January 2022 - Dendrite Helm Chart

Towards the end of December 2021 I started working on a Helm chart to deploy Dendrite[^fn:2],
on Kubernetes. You can see the post where this was announced here:
https://matrix.org/blog/2022/01/07/this-week-in-matrix-2022-01-07#dendrite-helm-chart-website

## February 2022 - Joining the Software Defined Streaming Team

After completing the internal tool we were all assigned to teams within the Global Reliability
department at Sky. I joined the Software Defined Streaming (SDS) Site Reliability Engineering (SRE)
team - SDS SRE for short. The platform within the team is sizeable and split between on-premise
vsphere clusters and GCP. We manage all of this using tools like Puppet, Jenkins and Terraform and my
main responsibilities in the team are to improve, modernise and maintain the platform, whilst also
supporting the Streaming Engineers and Testers that make up the rest of the Streaming team.

I also released 
[another update for the Dendrite Helm Chart](https://matrix.org/blog/2022/02/11/this-week-in-matrix-2022-02-11#dendrite-helm-chart-website) 
which added the ability to do a Polylith deployment.

## May 2022 - Getting Engaged

My long-term partner, Izzy, and I got engaged after dating for five-and-a-half years! We have now
set a date (4th May 2024) and are enjoying planning the ins and outs of our wedding day.

At some point we hope to start sharing updates via [our wedding website](https://izzyandjonny.co.uk/wedding),
so save that if you want to stay in-the-know.

## July/August 2022 - Going abroad for the first time in a while

Before COVID-19 a trip abroad was pretty much an annual thing for me and my family, and sometimes
it was even more frequent since we love to travel and explore the world. My last trip abroad was towards
the end of 2019. 

We spent two weeks exploring the Loire Valley in France, drinking good wine and eating
good food. It was certainly a much needed and enjoyable break from carrying out production releases!

## September 2022 - Planing to buy a house

The next step for us to get on the property ladder and we are hoping to do that early in the new year.
It's early days at the moment but it has been fun to start nosing around on Rightmove and Zoopla.

## October 2022 - What's next?

*For the homelab*: Earlier this month I turned off my server for the first time since it went on in
[my first homelab post]({{< ref "/blog/homelab-part-1" >}}). My usage has decreased over the past few
months and now the cost of electricity doesn't really make it worthwhile. I have a couple of projects
and training goals that will make use of it again soon, but for now I'm happy to turn it on to backup
photos and files every once-in-a-while.

*For Dendrite*: The Dendrite chart repository was deprecated so has moved somewhere else and I haven't
followed. I felt it has reached a point where maintaining it is fairly straightforward and/or no longer
required so I am leaving it up to those who actually use it keep it going if they want.

*For Sky*: I am looking to get promoted to Site Reliability Engineer in the coming months and take on some
more responsibilities in my team. We have some exciting projects just around the corner that I am looking
to lead on and/or be a large part of. The platform is changing and modernising at speed, so its definitely a good
place to see a progression from the way it was 10 years ago to the shiny cloud-centric world we now live in.

*For Personal Projects*: I'm looking to start/join a project again soon to fill the space the Dendrite chart
has left. This will probably tie in nicely with the Homelab again and be largely focused on K8s and Golang,
so watch this space.

*Beyond Tech*: My focus outside of work is planning for my wedding, buying a house and generally looking
forward to the future. I'm enjoying that as my main hobby and its certainly time consuming enough to be one. 
Getting back into hiking is also quite high up on my list since I live relatively close to some lovely national
parks and can't remember the last time I spent any real time there. In December I will also be seeing The Cure live
for a second time which is awesome!




[^fn:1]: This is the most junior level assigned to Graduates and similar level joiners. It is also synonymous with Associate SRE. 
[^fn:2]: See https://github.com/matrix-org/dendrite. Dendrite is a Matrix Homeserver.
