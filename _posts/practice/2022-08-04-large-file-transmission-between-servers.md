---
title: Large file transmission between severs
category: practice
tags: copy ssh compress transfer pigz tar
toc: false
date: 2022-08-04 19:46:00 +0800
---

When it comes to the server management, especially involving multiple servers, sometime the large file transmission is needed. For example, you need to backup or transmit a directory with gigabytes or even terabytes of data to different servers.

Of course, we can use `scp` or `rsync` to perform the copying task. However, for large amount of files in the initial copy, `rsync` is not so efficient because it will do many checks while transmission. Here, I share a faster and more efficient method to transmit a large number of files.

The commands we will use are `tar` and `ssh`. We can create an archive by `tar` and then transmit the tared file to a remote host on the fly by `ssh`. So the command will be this: 
```console
$ tar -c </path/to/directory_or_file> | ssh <remote> 'tar -xvf - -C </path/to/extracted>'
```
You will need to type your password to `ssh` to permit the transmission. The transmission rate is much higher than using `rsync` and `scp`. In my testing, the transmission rate reached 100MB/s.

In addition, we can compress the tared file before sending it to the remote host and then decompress at the other side. For that purpose, we can use `gzip` or `pigz`. By `gzip`, the command becomes this:
```console
$ tar -c </path/to/directory_or_file> | gzip -c | ssh <remote> 'gzip -d | tar -xvf - -C </path/to/extracted>'
```
or by `pigz`
```console
$ tar -c </path/to/directory_or_file> | pigz | ssh <remote> 'pigz -d | tar -xvf - -C </path/to/extracted>'
```
I strongly recommend to use `pigz` for compression because it is a *parallel* compressor and rather efficient by taking advantage of available CPUs.

Finally, if you want to see a progress status you can also use `pv` command to achieve that and just put `pv` after `pigz` at the sender side.
```console
$ tar -c </path/to/directory_or_file> | pigz | pv | ssh <remote> 'pigz -d | tar -xvf - -C </path/to/extracted>'
```