+++
title = "Homelab - Part 5: Reset, Renovate, Rebuild"
date = "2022-01-13"
description = """A quick update to start the new year."""
toc = true
tags = ["homelab", "devops", "self-hosting", "ansible", "k8s"]
[series]
    name = "homelab"
    number = 5
+++

_Checkout my homelab GitOps here: [https://github.com/Jonnobrow/coffee-shop](https://github.com/Jonnobrow/coffee-shop)_

Since [my previous post]({{< ref "/blog/homelab-part-4" >}}) not a great deal
has changed in my homelab. I have been busy with work and had some other
projects that have interested me more.

Here is a quick rundown of what I have added/changed since part 4.

## Reset

I wiped out a bunch of services that I wasn't using and now just have my core,
daily use services:

- **Nextcloud**[^fn:1]: File storage and photo ingress for me and my partner
- **PhotoPrism**[^fn:2]: Photo management and sharing
- **Paperless**[^fn:3]: Document Manager
- **Mealie**[^fn:4]: Recipe Manager and Meal Planner
- **Jellyfin**[^fn:5]: Media System
- I also have a bunch of utilities and background services

## Renovate

I now self-host [`renovate`](https://docs.renovatebot.com/) and have
[a bot user](https://github.com/JonnobrowRenovateBot) dedicated to operating that.

The rationale behind self-hosting was to learn more about the bot rather than
using the Github action in which I would only have had to do the repository
specific config rather than the bot config. I also wanted to learn more about
[cron jobs in
k8s](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/).

## Rebuild

I went back to the roots of why I started a homelab and made space for learning again.

A lot of my services were barely being used and had run their course in terms
of teachable moments. They went in the bin and will hopefully get replaced with
new things, some possiblities are:

- **Dendrite**[^fn:6]: A matrix homeserver
- **Drone**[^fn:7]: A CI/CD server
- **Gitea**[^fn:8]: A git server
- **Vault**[^fn:9]: A secrets management tool

[^fn:1]: https://nextcloud.com/
[^fn:2]: https://photoprism.app/
[^fn:3]: https://github.com/jonaswinkler/paperless-ng
[^fn:4]: https://hay-kot.github.io/mealie/
[^fn:5]: https://jellyfin.org/
[^fn:6]: https://github.com/matrix-org/dendrite
[^fn:7]: https://www.drone.io/
[^fn:8]: http://gitea.com/
[^fn:9]: https://www.vaultproject.io/
