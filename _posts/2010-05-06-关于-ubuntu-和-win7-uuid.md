---
layout: post
title: "关于 ubuntu 和 win7 uuid"
subtitle: ""
author: "yangchw"
catalog: false
date: 2010-05-06 16:22:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - ubuntu
  - win7
---

今天重装win7之后，grub菜单没了，这是意料之中的事,不过用livecd把菜单弄回来后，进入选择菜单后出现开机画面后就停在那了，不能进ubuntu,然后我又试了下win7也进不去了，根据提示信息是 说 `can't find dev` 了，找不到设备了，我想了一下，重装win7之后都是什么发生了变化，无非就是硬盘分区了，而且也就只是重装win7的那个分区，影响到别的分区的可能性不大，于是我就到网上找ubuntu的硬盘分区管理方法，才知道uuid的事。

我在ubuntu上为了方便把所有的分区设置成了自动挂载，包括重装系统的这个分区， 用livecd进了系统，用命令

    sudo ls -l /dev/disk/by-uuid

查看现在实际的各个分区的uuid 然后在打开`/etc/fstab`文件看里面的内容是否对应，果然win7所在的uuid不一样了，把它改回来后，然后在把`grub.cfg`也改了一下，重启之后就全好了，其实如果不设置自动挂载的话，这事就不会发生了，不过话又说回来了，因为一个分区无法自动挂载就进不去系统，ubuntu还是很脆弱的，还有待改进，呵。