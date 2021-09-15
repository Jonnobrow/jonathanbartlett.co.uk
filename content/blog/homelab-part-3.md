+++
title = "Homelab - Part 3: Backups"
date = "2021-09-30"
description= "A slightly deeper dive into my backup strategy"
toc = true
tags = ["homelab", "devops", "self-hosting", "backups"]
draft = true
[series]
    name = "homelab"
    number = 3
+++

As mentioned in [Part 2 of this series]({{< ref "/blog/homelab-part-2" >}}),
backups were a priority of mine over the last few weeks. Although I was backing
up some data when I wrote that article, not everything was being backed up
properly and nothing was being backed up offsite.

That's all changed now, and this post will explain the process I went through to
get a reasonable backup strategy for my homelab in place.

## My Existing Backup "Strategy" and the Goal

My existing backup strategy consisted of a cronjob running a simple script 8
times a day (every 3 hours). That script runs a Borg[^fn:1] backup which
includes most of the key directories on my homelab, including all the data and
database directories. The script also prunes the backup repository so that I
only have 7 daily backups (one for each weekday), 4 weekly backups (one for each
week of the month) and 6 monthly backups.

This works well and gives a me a range of points to restore from if I make a
mistake or something bad happens. However, the backup repository is local,
meaning it sits on the same drive as the data originally came from, so in the
case of hard drive failure, I'd lose everything.

The 3-2-1 Backup Strategy[^fn:2] is widely regarded as being pretty air-tight
and is used by a lot of the self-hosting and homelab community, not to mention
in business. The 3-2-1 strategy has 3 parts:
- 3 copies of your data
- 2 different mediums/devices
- 1 copy off-site

So to quickly compare my old strategy to 3-2-1:
- I have: *2* copies of my data and need *3*
- I have: *1* medium and need *2*
- I have: *0* copies off-site[^fn:3] and need *at least 1*

**The goal** is therefore to sort out some off-site location for my most
important data and continue doing local backups, but change the destination to a
different medium/device.

## Exploring strange new worlds

One thing I wanted to be certain of was that my choice of backup software was
sound. Therefore, before putting work into everything else I looked into some
other incremental backup solutions.

Aside from Borg[^fn:1] there are several backup solutions such as
Duplicati[^fn:4] and Restic[^fn:5]. Both initially appeared worth a shot but
after a little research it became clear Duplicati wouldn't meet my needs since I
wanted to do local backups too.

Restic offers much the same as Borg, and even uses similar terminology and
commands, making it quite easy to try out. I got everything setup and working
nicely but unfortunately ran into some resilience issues with the backup
process. My poor home broadband means that disconnections and timeouts are a
regular occurrence and I need something that can handle this nicely. 

I persevered with Restic for a little over a week, but after failing to find a
solution to my problem I returned to Borg. Frequent cut outs happened with Borg
too, which I was able to resolve with some SSH options[^fn:10], but partial
backups are stored with Borg so after restarting the backup, everything just
continues from where it left off. 

I found that there isn't much to choose between Borg and Restic, but this little
difference was quite important for me. Restic is worth a look and I will
certainly revisit it in the future, but for now *Restic is futile* and Borg
shall live on.

## Where do off-site backups live?

The logistics of the off-site part was probably the biggest challenge of the
whole backup overhaul. As much as possible I want to be in control of my data
and that means not using big cloud services, hence I'm self-hosting everything.
Therefore, using a cloud service as a backup destination is not ideal. My ideals
coupled with the cost of storage in the cloud is probably why it took so long to
get this sorted.

Here were the options I considered:
- **Amazon S3 (or similar)** due to the relatively low cost of storage and my
  familiarity with S3 from an internship. Sadly individual requests have an
  associated cost in Amazon's billing model and that means doing frequent
  backups, as I do, could cost a fortune in the long run. It is also difficult
  to predict costs, especially at this early stage in my homelab.
- **IDrive/Backblaze/Crashplan...** since they seem to be at the top of every
  list entitled "Best Backup Service 2021". These are very nice solutions and
  allow for huge amounts of data, but I don't need more than 100GB at the moment
  for all of my most important files. Additionally I want a place I can host a
  Borg repository, but most of these backup solutions have some proprietary
  software or a lot of features I'll never use and that is reflected in the
  price.
- **Dropbox/Google Drive (or similar)** is another option that seems popular.
  Dropbox would let me keep 2TB of stuff in the cloud for £7.99 a month[^fn:6],
  but the same issues that I have with those dedicated backup solutions still
  exist. I'm a DevOps engineer, I'm not afraid to get my hands dirty and I want
  something fairly basic that I can build on not something that provides
  everything imaginable.
- **Hetzner Storage Box** is a much simpler solution with a wide range of price
  points and ways to access the box. Hetzner also seems to be highly thought of
  in the cloud hosting space. They offer everything from 100GB to 10TB and at
  reasonable prices.

For my current usage, a 100GB BX10[^fn:7] storage box from Hetzer, at a little
less than £3 a month, is the best choice. It was easy to setup and I can use
SFTP/SSH to access the data on the box. There are no overcomplicated extras, its
simple and that's all it needs to be. Another solution might be the better for
me in the future though as the quantity of data I'm backing up increases.

## Ansible Roles and Automation

I have a few different machines that I'd like to start backing up including my
personal laptop, desktop and homelab server. Therefore I would have to set up a
backup on each of the machines and any time a change in my backup setup occurs
I'd have to propagate that change to each machine. This caused me to revisit an
old friend by the name of Ansible to try and automate the installation (and also
the running) of my backup setup.

Restic was still being tested when I started playing with Ansible and I
discovered [a pretty decent role on Github](https://github.com/angristan/ansible-restic)
that I was able to use as a starting point for my own backup role.

I wanted a few things from this role:
- To be as platform agnostic as possible (especially Linux/MacOS)
- To handle general backups but also more service specific backups
- To be very configurable as my use on different machines varies

My initial task was to adapt a backup script I have been playing around with to
a Jinja2 template so that Ansible could populate it with the details of
whichever repository was configured. Then I added a Nextcloud backup script
which I will detail more in the next section. Finally I added in a playbook,
default configuration and inventory file and was able to start testing.

My playbook installs an SSH key and configuration for the remote Hetzner storage
box. It then creates backup scripts for each configured repository and a
separate script for the Nextcloud backup. Next a cronjob is added for each of
the scripts at a configurable interval. I have been using this for a few days
and everything has gone smoothly so far.

In addition to the basics, both Restic and Borg behaviour can be used and if
Borg is used a set of `init` scripts are generated to create the initial
repository if it does not already exist.

More detail on the configuration and usage of this Ansible role can be found on
my Github[^fn:8].

It is definitely not perfect and still a work in progress, but it will do for
now and as I learn more about Ansible and my needs it will improve.

## Nextcloud Specific Backups

Nextcloud is probably tied first for the most important thing to backup for me -
photos being the joint most important. Until this point Nextcloud had been
backed up as a folder like everything else, which isn't good as it can lead to
inconsistencies in files which are then part of my backups. Additionally the
database was being backed up as a folder too, which again is far from the
recommended way to do that. 

In order to safely backup Nextcloud you must enable maintenance mode, then
backup the config, data and theme folders (or more if you want) and finally dump
the database[^fn:9]. Then you can disable maintenance mode and continue using
Nextcloud.

To enable maintenance mode on my Nextcloud instance I must exec into the
Kubernetes pod and run a command, something like

```bash
kubectl exec -n nextcloud $POD -- \
    su - -s '/bin/sh' www-data -c \
    "php -d memory_limit=-1 /var/www/html/occ maintenance:mode --on"
```

which runs the command on the remote pod as the `www-data` user with php memory
limit disabled (this was necessary at time of writing).

Once this is done I can backup the data folders and then run another command on
the postgres pod to dump the database

```bash
kubectl exec -n nextcloud $POD -- \
    /bin/bash -c "/opt/bitnami/postgresql/bin/pg_dump -Unextcloud nextcloud" \
    > nextcloud.bak.sql
```

I combined these commands into a Nextcloud backup script and piped the output of
the database dump to Borg/Restic, as both support backups from `stdin`. Now my
data from Nextcloud is backed up safely.

## Next Steps

- [ ] Make service specific scripts for Jellyfin and Photoprism
- [ ] Tidy up the Ansible role 
- [ ] Add some resilience to the scripts
- [ ] Add general purpose database dumping to the ansible role

I will update this list as I tick off these items :)


[^fn:1]: https://www.borgbackup.org/
[^fn:2]: [A decent article about 3-2-1](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/)
[^fn:3]: Not quite true as most of my data is still in the cloud somewhere until
  I get this self-hosting stuff sorted.
[^fn:4]: https://www.duplicati.com/
[^fn:5]: https://restic.net/
[^fn:6]: https://www.dropbox.com/buy
[^fn:7]: https://www.hetzner.com/storage/storage-box
[^fn:8]: https://github.com/Jonnobrow/ansible-backup
[^fn:9]: https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html
[^fn:10]: See this issue: https://github.com/borgbackup/borg/issues/636

