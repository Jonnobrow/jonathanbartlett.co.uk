+++
title = "Homelab - Part 2"
date = "2021-09-02"
description= "An Update on my Homelab"
toc = true
tags = ["homelab", "devops", "self-hosting"]
[series] 
    name = "homelab"
    number = 2
+++

Since [my previous post]({{< ref "/blog/homelab-part-1" >}}) I have
spent a fair amount of time tinkering with my homelab. In part 1 I
hadn't even built a server yet and now I have a number of services
running that my partner and I use on a daily basis. This post includes
some of the ups and downs of that tinkering and details the services I
am hosting and my plans for the future.

## The Good, the Bad and the Ugly

**The Good**: Shortly after I released the first post in this series the
components for my server arrived. I have built a few PCs before and enjoyed the
build process, which, in the world of plastic constructions toys, I would equate
to Duplo as opposed to Lego, although the stakes can be a little higher. The
entire build took a little under two hours, and the system booted straight into
the PVE live USB I had plugged in.

**The Bad**: In my eagerness to get started I neglected to plan as thoroughly as
perhaps I should have. I sped through the guided installer for Proxmox[^fn:1]
and set up my first virtual machine. Once Ubuntu 21.04[^fn:2] had installed, I
SSH-ed into the VM and installed the required applications to get a Kubernetes
cluster up and running. This may all sound as though it was going well, but in
the setup I failed to set up static IP addressing for both the Proxmox host and
the Kubernetes VM. This would later result in me having to basically start the
installation from scratch, with a much more rigidly defined IP addressing
strategy for my home network. The re-installation of Kubernetes went smoothly,
as it had before, and I had a cluster up and running.

**The Ugly**: At this point in the story, my experience goes from limited to
non-existent. I knew that I wanted to use GitOps[^fn:4] where possible and had a
pretty decent idea of what I wanted to host, but claiming I understood more than
that would be straight up lying to you. I would like to stress the fact that
this is exactly the reason why I was doing any of this in the first place - to
learn. My lack of knowledge did mean that what follows isn't the best and my
homelab is still nowhere near perfect or even good.

- I chose FluxCD (v2)[^fn:3] as my GitOps[^fn:4] provider and used a friends
  manifests as the starting point for my own. The problem here arose from me
  adding in stuff for my own particular use case. I added in files and created
  Persistent Volumes and Persistent Volume Claims that were not needed. I had to
  install a local-path provisioner[^fn:5] in order for volumes to be
  automatically created if I hadn't explicitly defined them. Neeedless to say,
  it got quite messy, and my manifests are still rather disorganised and my
  volume management is far from ideal.
- Data storage and backups was the next issue. Until this point my solution for
  backups left a lot to be desired. I had multiple copies of the really
  important stuff but a lot of those copies hadn't been updated in a while and
  had therefore grown out of sync with the original, defeating the point of a
  backup. All of my data lived (and sadly still lives) on a 2TB Portable HDD.
  This gets mounted to the VM on boot and uses BTRFS (wanted to learn a bit
  about this too) subvolumes for each main category of data. I now run
  borg-backup[^fn:6] 4 times a day to backup my photos and any essential
  documents and data from my cluster. The ugliness comes from the fact my
  backups live on the same portable HDD and that my database backups just copy
  the files rather than using the intended backup functions for postgres.
  Additionally, I make no use of BTRFS. 
- As for how the data is served to my services, I use NFS. NFS is perfect for my
  usage as I can mount exports on multiple services and have everything play
  nicely. The issue here is permissions, and this is such an issue that I cannot
  yet explain what is going on and has affected so much that it warrants it own
  bullet. I'm sure a future post will address my solution to this. However, for
  now it remains an issue that prevents Jellyfin from updating metadata,
  prevents me from directly managing my photo library or prevents Nextcloud from
  backing up photos from my devices.

## The End Result

So after all of this about the highs and lows of my experience, you may be
wondering what the end result is like. I changed my plans for what to host a
little during the process since I discovered certain things that I needed sooner
and found that a few services would not be required if I used something else
(\*cough cough\* nextcloud). 

So here is a bit about what I'm hosting, my thoughts on the service as a whole
and how the setup and maintenance has gone.

- **Nextcloud**[^fn:7] is probably the most crucial service I host (except
  services I regard as being system services such as a load balancer and reverse
  proxy). It is a self-hosted cloud with an abundance of features.

  <details>
  I rely on Nextcloud for: storage of my documents, backup and sync of
  my photos, my calendar, my todo-list, my contacts, my quick notes, my RSS feed
  aggregation, my bookmarks and probably some more things that I can't remember
  right now. 

  I am very pleased with Nextcloud, the community surrounding it and the ongoing
  development and would highly reccomend taking a look at it if you are thinking
  of removing a cloud provider, like Google, from your day to day. And as far as
  maintenance is concerned, after you have done the initial setup the day to day
  is minimal and larger updates are generally pain-free too.
  </details>

- **PhotoPrism**[^fn:8] is an app for managing and viewing your photo
  collection. For years my photos have lived on Google Photos or on my portable
  HDD. This worked for me and my family, but when Google announced changes to
  the Google Photos storage policy[^fn:9] I knew it was time for a change.

  <details>
  I use photoprism to manage my photo library. I can put photos into albums,
  manage their metadata, archive lower quality snaps, share albums with family
  and have full size images forever with not 15GB limit or crazy compression.

  PhotoPrism is okay, it's not perfect but is still in the early stages of life
  and many features that would make it a really good Google Photos replacement
  are in the roadmap or under development. These include multiple user accounts,
  feeds (a bit like a personal Instagram/PixelFed) and more. It's quite easy to
  setup and maintain, indexing takes a while and so does the initial photo
  organisation, but once that is done its trivial to maintain your library as a
  user.
  </details>

- **Paperless-ng**[^fn:10] is a self-hosted document manager with a nice
  Django[^fn:12] web interface. I spun it up to have a play and its here to
  stay. I can scan documents with my phone or a dedicated scanner and then add
  the PDFs to a directory on Nextcloud which gets consumed by paperless. 

  <details>
  I can tag documents, give them a correspondent, document type and date and
  also inform paperless of the keywords or phrases in documents that relate to
  the preceding things and paperless with use OCR[^fn:11] to work it out
  automatically in the future. It has meant I don't need to keep paper copies of
  a lot of things and can take all my documents with me wherever I go - super
  useful.

  As for the maintenance, it requires very little and I am yet to encounter any
  issues with paperless whatsoever.
  </details>

- **Jellyfin**[^fn:13] is a free-software media system and was one of the few
  services I self-hosted prior to this new setup. My library was already setup
  and its something I use a lot so was probably the second thing I setup after
  Nextcloud.

  <details>
  I use it for Movies, Shows, Music, Podcasts and Audiobooks, but Jellyfin has
  support for some other things as well. It can also grab metadata from many
  sources, handle subtitles, stream to most devices and platforms and handle
  multiple users.

  This is unfortunately a service I am experiencing issues with. Since setting
  it up on my new server I have yet to get metadata fetching working which
  leaves my library ugly and uninformative (but functional). I have experimented
  with several different fixes but I believe the issue lies with the permissions
  error mentioned earlier.
  </details>

## Tackling the Ugliness

Backups are my next priority. My services, from a user perspective, are working
nicely and I have started relying upon them. Therefore, backing up data is super
important to keeping the self-hosting dream alive. I need to work out a solution
to creating offsite backups and will likely be looking into this over the next
few weeks and months - so watch this space. 

Once backups are sorted I will likely start tidying my manifests and organising
my services a little better. Then I will tackle the permissions issues I am
experiencing and try to get Jellyfin back to where it was before. I also want to
set up a recipe manager (with shopping list support) in the near future. My
personal website is also still on Gitlab pages, and while I don't think it is
necessary to self-host this, it would be nice if [](https://jonnobrow.co.uk)
redirected to this site rather than just being a dead end.

I also want to start creating Ansible roles for certain things I setup. For
example, a role to setup whatever my backup solution is and a role to setup
wireguard. These are things I need on 4+ different machines (a couple of
servers and a couple of personal devices) and therefore the time and effort of
creating an Ansible role will pay off quickly.

Thats all for now, but I will hopefully have some more useful and interesting
posts to share in the near future.


[^fn:1]: https://www.proxmox.com/en/proxmox-ve
[^fn:2]: https://releases.ubuntu.com/21.04/
[^fn:3]: https://fluxcd.io/docs/
[^fn:4]: GitOps: versioned CI/CD on top of declarative infrastructure. Stop scripting and start shipping. - [Kelsey Hightower](https://twitter.com/kelseyhightower/status/953638870888849408)
[^fn:5]: https://github.com/rancher/local-path-provisioner
[^fn:6]: https://www.borgbackup.org/
[^fn:7]: https://nextcloud.com/
[^fn:8]: https://photoprism.app
[^fn:9]: https://blog.google/products/photos/storage-changes/
[^fn:10]: https://github.com/jonaswinkler/paperless-ng
[^fn:11]: Optical Character Recognition
[^fn:12]: https://www.djangoproject.com/
[^fn:13]: https://jellyfin.org/

