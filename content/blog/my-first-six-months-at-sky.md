+++
title = "My First Six Months at Sky"
date = "2022-04-11"
description = """
I've been at [Sky](https://sky.com) for six months. Here's what I've been up to ...
"""
tags = ["work", "sky", "devops", "learning"]
draft = true
+++

At the time of writing this (April 2022) I am working as an **Associate DevOps Engineer**
for [**Sky**](https://sky.com). I started almost six months ago and a lot has happened.
This post is meant to immortalize my journey, mainly for my sake, but hopefully you
will get something out of this too.

## Bootcamp - The first six weeks

{{< utils/daterange from="October 2021" to="November 2021" >}}

The beginning of my graduate scheme with Sky started with a six week period of
training that was referred to as **Bootcamp**. The point of this was to bring
everyone in our cohort up to speed and give us all a reasonable grounding in
software engineering and devops.

Towards the end of the training we were divided into teams and asked to build a
pipeline with the aim of deploying an application (either one we had made or
one we had been given).

My team used the following tools and technologies:

- **Amazon ECS** for hosting our application
- **Jenkins** as our build server
- **Github** as our remote source code repository
  - Jenkins builds were also triggered by webhooks from Github
- **Dockerhub** for our container image hosting
- **Terraform** for deployment of our infrastructure
- **pre-commit** as a quality of life tool

After completing the pipelines we presented our projects to the wider department
and graduated bootcamp.

## An Educational Project - The next 3 months

{{< utils/daterange from="November 2021" to="February 2021" >}}

The next step was an educational project. The purpose of this project was to
give us exposure to Sky's way of working and give us a project to complete that
had real business value, but was low priority so that we could learn as much as
possible whilst on the job.

Our team (myself and three others) was tasked with building and internal tool from
the ground up that would aggregate data from a number of sources to help with reporting.

### Technology Index

{{< utils/grid >}}
{{< utils/icon src="/logos/jenkins.png" alt="Jenkins" more="https://jenkins.io">}}
{{< utils/icon src="/logos/JCasC.png" alt="JCasC">}}
{{< utils/icon src="/logos/terraform.png" alt="Terraform">}}
{{</ utils/grid >}}

## Into a Team - From then until now
