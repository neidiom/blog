---
layout: post
title:  "Shrink ext4 partition"
date:   2017-12-15 12:34:07 +0000
categories: e2fsck resize2fs cfdisk
---
We want a 10G partition therefore we need to resize the filesystem to a smaller size to something like 9G.


# e2fsck -f /dev/sda2
# resize2fs /dev/sda2 9G

Delete the partition and create a new one with 10 G size.


# cfdisk /dev/sda

Run resize2fs without size parameter


# resize2fs /dev/sda2
