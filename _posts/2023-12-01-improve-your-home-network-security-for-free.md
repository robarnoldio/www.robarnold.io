---
layout: post
title:  "Improve your home network security for free"
date:   2023-12-01 13:00:00 -0600
categories: free
---

Security is a problem that we all face. At work, there may be trained professionals leading the efforts to reduce harm and keep everything trustworthy. But what about on your home network? Couldn't we all use a little professional help with that? Read on for steps you can take today that will cost you zero.

## One service, three levels of difficulty

Today we'll cover how to use free services from Cloudflare to manage the risks to your home network and the devices on it. Cloudflare is amazing because they really care about creating a more secure Internet--so much so that they will let you consume some of their best services at no cost.

### Level 1: secure Domain Name Service with 1.1.1.1 (or .2 or .3)

Cloudflare operates a set of public DNS servers that are very fast and give you choices about how to deal with malicious sites and adult content sites. Their brand name for these services is 1.1.1.1, and you can read more about how it works.

What Domain Name Service (DNS) does is quite fundamental to the Internet operating correctly. When you use a name, like www.robarnold.io, the DNS resolves that to an address (or addresses) that are registered with that name to provide some useful service, like a website or email.

How Cloudflare works to keep your network more secure is actually really simple and powerful: any name that you ask it to resolve which is identified as serving malicious content, Cloudflare will return an address of 0.0.0.0 for that request. So you are prevented from visiting any compromised sites or malicious services, and it's free and fast to set up.

To use this protection, all you need to do is configure your router to use 1.1.1.2 and 1.0.0.2 as the DNS servers that it forwards requests to. In your config, this may be labeled several ways, but you'll recognize that there will already be an IP address in the configuration, probably provided by your Internet Provider.

Once you're done setting that up, it's easy to test with this address that points at a site which is safe, but categorized as malware for testing: [https://malware.testcategory.com/][malware].

If you would also like to protect people using your network from adult content, that is also simple and free. Configure your router to use 1.1.1.3 and 1.0.0.3 as the DNS servers to get protection from malware and adult content.

Once you're done setting that up, it's easy to test with this address that points at a site which is safe, but categorized as adult for testing: [https://nudity.testcategory.com/][adult].

You can read quite a bit more about this in Cloudflare's [documentation][docs], but this is the quickest way to achieve malware protection for every device using your home network.

Level 1 is amazing, and so quick to set up. But there are more free services that will help extend these protections to situations where your device is using someone else's network. It would be great to have that protection on any network. And you can, for no cost, with the Warp client from Cloudflare.

### Level 2: Warp client

Besides making sure your devices don't access malicious sites, the Warp client offers you the protection of a Virtual Private Network (VPN) as well. This VPN is not targeted at keeping you anonymous--it is there to ensure your traffic to the Internet is not modified by someone who carries that traffic (like a coffee shop network that has been compromised).

*As a side note, most of the traffic on the web is now encrypted, so this protection may seem redundant. But your devices are making a lot of connections all the time that aren't web traffic, and this approach gives all that traffic a trustworthy connection to the Internet too.*

Installing the Warp client is easy. Go to [this page][warpdl] and find the appropriate link for your devices, then install. You can read quite a bit more about how it protects you [in the documentation][warp], but you can also just turn it on and forget it.

Two caveats about using this approach:

- In my testing, running this client will drain your battery faster. That's probably a decent tradeoff for most of us. If you have an older device or are currently battery-challenged, you may want to skip installing this.
- A few services are not compatible. One that I ran into in testing was trying to play Apple Music through Sonos. Luckily, it's easy to toggle the Warp client off in situations where it breaks something you need to work.

### Level 3: Warp client with a free Cloudflare account and teams

Let's say you crave control over the precise categories of sites to block. Or that you need a log of actions taken to protect your network. Or that you need to fine tune the protection of the [Warp client][warpdesktop] based on who's using it. You can do all those things by signing up (for free) for a Cloudflare account.

Cloudflare generously provides almost all the features their enterprise customers pay for to everyone, for no cost. To access this, you do need to [register as a customer][signup].

![screenshot](i/cloudflarepricing.png)

When you do this, you can use *a lot more features* than most home users will have a need for. To help you imagine that, here's a short list of things I do with my account:

- I exempt certain application traffic from being sent through the Warp client, which helps me avoid the compatibility problems in Level 2.
- I look at logs to more fully understand the policy actions Cloudflare can take on traffic.
- I use a content filtering category that was developed to make network access school-friendly.
- I exempt my work machine from some protections my home devices get, because it comes with some robust protections that may conflict.

I (a giant nerd) particularly enjoy having enterprise-grade protection for my home network. But you may choose differently, and the good news is you have several options to pick from, all free.

[malware]: https://malware.testcategory.com/
[adult]: https://nudity.testcategory.com/
[docs]: https://developers.cloudflare.com/1.1.1.1/setup/
[warp]: https://developers.cloudflare.com/warp-client/
[warpdl]: https://1.1.1.1/
[warpdesktop]: https://blog.cloudflare.com/warp-for-desktop/
[signup]: https://dash.cloudflare.com/sign-up