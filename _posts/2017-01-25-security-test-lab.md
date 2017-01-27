---
layout: post
title:  "Planning my test lab"
date:   2017-01-25 12:00:00 -0600
categories: learning lab
---
I finally took action on my long-standing intention to build a security
testing lab. Tomorrow I will get my hardware, a PowerEdge R900 with 128GB RAM,
8 NICs and a moderate bunch of disk. Since I haven't done this before, I'm
putting a lot of time into planning.

I'm using a design and architecture developed by da_667 to get insight into
IPS behavior when monitoring attack traffic. So the test range contains
a victim VM (metasploitable 2), a Kali box to mount attacks from, a pfSense
firewall, and Snort/Suricata and Splunk boxes.

Update: got my Dell R900. For some reason I failed to get my ESXi install bits
onto the media I thought I copied them to. Minor setback. This thing weighs
95 pounds. o_O
