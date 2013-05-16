---
comments: true
date: 2010-04-22 10:01:16
layout: post
slug: networking-on-the-xo-with-realbasic
title: Networking on the XO with REALBasic
wordpress_id: 252
categories:
- OCRI
tags:
- networking
- olpc
- realbasic
- xo
---

One of the main things that I personally wanted to get across to the  students here in Ottawa was that the XO is not just another laptop.  There are certain key features that make it unique compared to the  desktops and notebooks that they may have seen before.

![3 XO laptops on a desk](http://farm3.static.flickr.com/2062/2265749376_d3e1f3fba7_m.jpg)


Chief amongst these is the mesh networking capabilities of the XO.  Whilst it can connect to a regular wireless network just like any other  computer, the XO can also form its own peer-to-peer network with others in the vicinity. This is really important as many of the  applications that ship with the XO are social or collaborative in nature  and the mesh allows them to be used in situations where a centralized  network is unavailable.


With this in mind, I started thinking about how to harness the powerful  capabilities of the mesh for my own applications. As far as I could  tell, the whole thing just worked by magic. This was great, but didn't  give me any idea on how to send and receive messages from other machines  on the network.

The teacher of the particular grade 12 class I'm mentoring (along  with my good friend [Dan Menard](http://danmenard.com)) is teaching his students  [REAL Basic](http://www.realsoftware.com/realbasic/). I'd  not seen this environment before, and I can't remember the last time I  did any Basic programming. Fortunately, RB comes with extensive  documentation.

RB comes with a set of pre-defined 'modules',  roughly equivalent to the way that Java provides default  packages/objects for common tasks. There were a couple of interesting  modules that I wanted to take a closer look at: EasyTCPSocket and  EastUDPSocket.

Both of these allow simple interaction over the network. You bind  them to a port, then send information to a specified IP on that port.  That's all well and good, but what I really wanted was a way of  discovering other machines on the network automatically. I knew that  each XO on the mesh was somehow assigned an IP address, but neither  socket provided a means searching for addresses automatically. I dug a  little deeper, and struck gold.

RB has a nice little module called AutoDiscovery. Subclassed from the  EasyUDPSocket, it allows you to bind to a port and then join a named  group within the subnet. Furthermore, it can pull back a list of IPs in  that same group. Perfect! After a little bit of trial and error, I  managed to get two desktop computers on my home network to join the same  group and exchange IP addresses. For proof-of-concept, that was enough.  It was time to move to the XOs.

Fortunately, RB can cross-compile applications for Windows, Mac and  Linux. The XO runs a modified version of Fedora, so I built a linux  version of my simple app and transferred it to my pair of laptops via a  USB key. I was slightly worried that they wouldn't have the required GTK  libraries installed, but both applications started up fine. A couple of  seconds later, and success! Each XO was proudly displaying the other's  IP in it's group list.

![Two XO laptops demonstrating mesh networking](/a/2010-04-22-networking-on-the-xo-with-realbasic/IMAG0205-300x200.jpg)](http://theflyingdeveloper.com/blog/wp-content/uploads/2010/04/IMAG0205.jpg)

From this starting point, it is easy to see that more complex network-enabled apps can be built: Either by using the discovery mechanism as a way to exchange IP addresses before forming dedicated one-to-one connections with other machines (using the EasyTCPSocket, for example) or communicating with the group directly using a broadcast function. Both options have their pros and cons, but the main hurdle of harnessing the underlying mesh has been solved.

I'm really excited to see what the students do with the starting point I've given them. They're still at the design stage right now, but by the end of term I expect that they'll have produced some really phenomenal stuff.
