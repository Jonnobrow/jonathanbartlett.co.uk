+++
title = "Homelab - Part 4: From the ground up"
date = "2021-10-30"
description = """I rebuilt my cluster from the ground up, created ansible roles for
                setting up my server(s) and learnt a lot in the process."""
toc = true
tags = ["homelab", "devops", "self-hosting", "ansible", "k8s"]
[series]
    name = "homelab"
    number = 4
+++

For the last month I have been working on restructuring my Kubernetes cluster to
make everything a bit more organised and sensible. During the process I also
scripted the setup of the host using Ansible and made some changes to the way I
do things.

If you're just interested in seeing the GitOps repository, you can check that
out here: [https://github.com/Jonnobrow/coffee-shop](https://github.com/Jonnobrow/coffee-shop).

## Motivation for change

Before I dive in to the new structure of my cluster, the things I have added and
the things I have changed, I want to explain why this was necessary (or why I
felt it was anyway).

Three main things prompted the changes:

- **Messy Manifests**: I mentioned this towards the end of
  [my second post]({{< ref "/blog/homelab-part-2" >}}) and although functionally
  everything was fine, having your manifests organised makes everything a little
  easier to maintain.
- **Backups**: Everything important is backed up so I can mess around with my
  cluster knowing a restore is **(a)** possible, and **(b)** quite
  straightforward thanks to the work I did in [part 3]({{<ref "/blog/homelab-part-3">}}).
- **Learning and Automation**: My initial cluster had been somewhat adapted from
  a friends cluster[^fn:1], so I hadn't really learnt all that much about what I
  was doing. Also, I had left a lot of manual processes creep into my
  maintenance and I wanted to learn how to remove as much of these as possible -
  _automate everything_!

## Some new tools

A couple of new tools were integrated into my workflow during this process. Some
replaced tools I was already using while others simply improved my quality of
life or were necessary for the automation I was trying to accomplish.

### Mozilla SOPS[^fn:2]

The Github repository describes SOPS (**S**ecrets **OP**eration**S**) as being a
"simple and flexible tool for managing secrets". It supports a wide range of
file formats and can encrypt using a few different mechanisms depending on your
needs.

For me it replaces Bitnami Sealed Secrets[^fn:3], but also gives me a way to
encrypt secrets that don't like within my cluster, like secrets used in Ansible
deployments.

Some examples of usage:

- To Encrypt a file in place: `sops -e -i /path/to/file`
- To Decrypt a file in place: `sops -d -i /path/to/file`
- To edit an encrypted file with $EDITOR: `sops /path/to/file`

FluxCD provides a guide[^fn:4] on using Mozilla SOPS, as does Ansible[^fn:5].

### Task[^fn:6]

> Task is a task runner ... that aims to be simpler and easier to use than ... GNU Make.

Instead on the Makefile format, Task uses yaml, which I am much more familiar
with. It also has a more natural feel to usage (for me) and I found the ability
to use templates, include other task files and define dynamic variables really
useful for the kinds of things I wanted to do with it.

I predominantly use Task for running small admin tasks like:

- Enabling/Disabling Nextcloud Maintenance Mode
- Running an Ansible playbook
- Running a flux reconciliation

The documentation[^fn:6] explains how to install and use Task and I recommend checking
it out for a little bit more automation in all of your projects. For an example
of multiple task files, see [my GitOps repository](https://github.com/Jonnobrow/coffee-shop/blob/main/Taskfile.yml).

### pre-commit[^fn:7]

This tool managed pre-commit hooks for Git. These hooks mean that on every
commit my code or manifests are run through a linter and any other tools I want.

So far I am only using this for a few checks for my manifests such as:

- Checking for trailing whitespace and line endings
- Checking for consistent yaml using yamllint[^fn:10]
- Checking for unencrypted secrets[^fn:8]

However, in the future I can see me using this to run linters and tests before
committing python code and much more.

A small configuration file is included in [my GitOps repository](https://github.com/Jonnobrow/coffee-shop/blob/cf9c93f755cfc63d020cc40bb3cabbdc0b551a51/.pre-commit-config.yaml) and simply
running `pre-commit install-hooks` will add the hooks to that repository, causing them
to run on every `git commit` command, stopping the commit if a check fails.

## Still Messy Manifests

My original plan was to just put the manifests in to a new directory structure,
perhaps move around some services into more sensible namespaces and then be
done. However, I am somewhat prone to diving deep into the rabbit hole[^fn:9]
and found myself looking at other GitOps Kubernetes Clusters on
[k8s-at-home/awesome-home-kubernetes](https://github.com/k8s-at-home/awesome-home-kubernetes).
This quickly led to me excitedly creating a new branch on my own GitOps
repository and the procrastination had officially begun.

### A new cluster is born

I run a single node cluster, so by "a new cluster" what I really mean is "a new
node". In order to test the re-done manifests I decided it was best to create a
separate environment because I wasn't sure how long it would take and I didn't
want to be without some of my services for an unknown amount of time.

For ease, I decided to look into using k3s[^fn:11], rather than doing everything
from scratch, and here begins the first descent into the rabbit hole.

Many of the aforementioned repositories contain a directory with deployments
using either Ansible, Terraform or a combination of the two. Of course, I liked
the idea of having a single command I could run that would install dependencies,
configuration files and setup k3s - so I made it happen. It was a chance to
learn some more about Ansible[^fn:12] and also make my life easier in the future
should I decide to move to new hardware or rebuild my cluster again.

So after about two days work I had a new cluster up an running, but still hadn't
done anything that I originally set out to do, oh well.

### Making use of Kustomize

Another thing that was common across most of the repositories linked in the
awesome-home-kubernetes project was the use of Kustomize[^fn:13] and the Flux
Kustomize Controller[^fn:14].

Kustomize is a powerful tool that can do some really cool stuff like generating
secrets and config maps from the actual files, so you aren't managing two copies
of something. It can also apply patches, perform replacements and much more.
When coupled with the Kustomize Controller for flux it allows me to have quite
fine-grained control over my manifests and I had to have that.

These are the things I mainly use Kustomize for:

- Decrypting secrets
- Substituting cluster wide values in all my manifests
- Applying the resulting manifests

Kustomizations also mean that I can specify exactly which manifests should be
applied, rather than the alternative which seemed quite flakey to me. I can
reconcile a whole kustomiztion in the knowledge that my volumes, secrets and
configs will also get applied whereas before only the helm release itself would
be updated.

Once again, this is all very cool but at this point I'm three days in an not a
lot has changed in the way of removing messy manifests.

### Setting up Tasks and SOPS

Of course, things must come in threes, so here we are with a third change before
the real work even begins. As mentioned in the
"[Some new tools]({{<ref"#some-new-tools" >}})" section,
I wanted to use SOPS to manage secrets and Task to run tasks. I set up both of
these tools when creating the Ansible deployments so that I could easily keep
secret variables and run playbooks. However I needed to add a couple more tasks
and generate some extra keys before I could use everything with Kubernetes.

I wrote a couple of tasks that simply ran Flux commands I use all the time, so
instead of running `flux reconcile source git flux-system` I could type
`task flux:sync` which saves me quite a few key presses over a day of
reconciliations.

Additionally, I followed the instructions on the flux website[^fn:4] for using
SOPS and generated a separate key that the cluster would use to decrypt secrets.
I then added some tasks to my task files for generating those secrets again in
the future, although hopefully I will never have to as that would be very bad!
I also created a task to generate the secret in the cluster that flux uses to
decrypt everything and that was that.

## Finally getting somewhere

Now that there is new cluster, I have decided to use Kustomize and I've set up
some new tools, I can finally start re-organising my services. I started by
copying a structure from the repos I was using as inspiration.

```
/coffee-shop
├── cluster
│  ├── apps
│  ├── base
│  ├── core
│  └── crds
├── server # Notes live elsewhere, see above
└── Taskfile.yml # Contains tasks
```

Let's break that down a bit:

- `cluster` contains all the manifests for my K8S cluster
  - `apps` contains the services I am running on the cluster and inside lives
    a directory for each namespace, which then holds a directory for each
    service. More on that later.
  - `base` is the directory that flux uses as a source. It contains the
    cluster settings and secrets, the core, apps and crd kustomizations and
    the gotk manifests that contain everything required by Flux. I also put
    other sources for my services, such as helm repositories, here.
  - `core` contains the namespaces themselves and the `cert-manager`
    manifests. It gets applied before anything else as the namespaces and
    `cert-manager` are crucial to the whole cluster.
  - `crds` contains custom resource definitions used by some of the services.
    Although all of the services have an option to manage crds with the helm
    chart, by having them here I can ensure they exist and have finer control
    over their versions.
- `server` contains the Ansible roles, playbooks and inventory files
- `Taskfile.yml` contains tasks for the project
  - There is also a `.taskfiles` directory with more task files that relate to
    specific things like nextcloud or ansible.

Okay, so now a nice new structure is in place, what does a typical service look
like in that structure. Here is an example using Jellyfin:

```
/coffee-shop
└── cluster                         # Top Level
   └── apps                         # Jellyfin is a service, so under apps
      └── media                     # Jellyfin is a "Free Software Media System"
         ├── _pvc                   # Namespace level Persistent Volume Claims
         └── jellyfin                # Jellyfin get's its own directory
            ├── helm-release.yaml   # The helm release itself
            ├── kustomization.yaml  # A kustomization to say what should be included
            └── config-pvc.yaml      # Service level Persistent Volume Claims
```

By grouping resources into common namespaces I avoid having separate persistent
volume claims and persistent volumes for each service. I have six services in
the `media` namespace that all use the same volumes, but I only have one
definition for each, not six like in my old setup.

The logical next step would be moving everything over, but of course the rabbit
hole re-opened and I couldn't help but jump in.

## Switching to Traefik[^fn:16]

My cluster had been running fine using ingress-nginx[^fn:15] for a few months,
so I had no real reason to change to a different proxy. However, the main aim of
all of this is to learn **AND** to follow trends, so naturally I took a look at
something new that goes by the name Traefik. Traefik is "The Cloud Native
Application Proxy" and provides a lot of nice features:

- Routing and load balancing
- Security: HTTPS, Let's Encrypt Support and More
- Configuration: Service Auto-Discovery and Middlewares
- Observability: Built-in dashboard, real-time metrics

Further to this, Traefik seems to be the in-thing so with the aim of staying
somewhat current with my homelab, I decided to move to it.

The process didn't go off without a hitch though. For most services it was as
adding some annotations for Traefik service discovery. Something
like:

```yaml
annotations:
  traefik.ingress.kubernetes.io/router.entrypoints: "websecure"
  traefik.ingress.kubernetes.io/router.middlewares: "list,of,middlewares"
```

However, for services like Nextcloud that required more configuration, I had to
define middlewares ([see my nextcloud middleware here](https://github.com/Jonnobrow/coffee-shop/blob/6c8b97e569b93e700f13591db30ea21b62126d9a/cluster/apps/networking/traefik/middlewares/nextcloud.yaml))
in order to replace inline nginx configuration snippets in my old setup.
This took a bit of working out and quite a few commits to get sorted.

As for some of the benefits, I could now define middlewares as an extra layer of
security. For example, I have a middleware that only allows traffic from
cloudflare IP addresses, for my external traffic, and another than only allows
RFC1918[^fn:18] IP addresses for my local traffic. I could also then define multiple
ingresses for some of my services, like those with a `/api` path so that only
things on my local network could access the API, and only traffic through
cloudflares proxy could access the rest[^fn:17].

[^fn:1]: https://github.com/sseneca/sserver
[^fn:2]: https://github.com/mozilla/sops
[^fn:3]: https://github.com/bitnami-labs/sealed-secrets
[^fn:4]: https://fluxcd.io/docs/guides/mozilla-sops/
[^fn:5]: https://docs.ansible.com/ansible/latest/collections/community/sops/docsite/guide.html
[^fn:6]: https://taskfile.dev/#/
[^fn:7]: https://pre-commit.com/
[^fn:8]: https://github.com/k8s-at-home/sops-pre-commit
[^fn:9]: a complexly bizarre or difficult state or situation conceived of as a hole into which one falls or descends, especially one in which the pursuit of something leads to other questions, problems or pursuits. --- [Merriam-Webster](https://www.merriam-webster.com/dictionary/rabbit%20hole)
[^fn:10]: https://yamllint.readthedocs.io/en/stable/index.html
[^fn:11]: https://k3s.io/
[^fn:12]: https://www.ansible.com/
[^fn:13]: https://kustomize.io/
[^fn:14]: https://fluxcd.io/docs/components/kustomize/
[^fn:15]: https://kubernetes.github.io/ingress-nginx/
[^fn:16]: https://traefik.io/traefik/
[^fn:17]:
    I appreciate that this might be possible with nginx but the simplicity
    and declarative approach with Traefik makes it much nicer for me.

[^fn:18]: https://en.wikipedia.org/wiki/Private_network
