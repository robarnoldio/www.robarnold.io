---
layout: post
title:  "Conclusions vs. Observations"
date:   2020-01-27 16:00:00 -0600
categories: IT troubleshooting
---

This is the first post in a short series about troubleshooting--the skill everyone in tech
is expected to have, but no one gets trained to do. 

# Observations vs. Conclusions

**Bottom line up front:** less experienced troubleshooters often hinder their progress toward a solution by treating conclusions like observations. 

What's the real difference? An observation should be repeatable, and relatively resistant to differences 
due to the observer. You can and should use both while troubleshooting, but be acutely aware when you are making a conclusion.

| conclusion | observation |
| :------------- | :------------- |
| All the servers are down. | I can't ping host *foo* from outside the network. |
| The Internet connection failed. | DNS resolution fails for three hostnames I tested from my machine. |
| The web server crashed. | Host *foo* is not accepting connections on port 80 or 443. |

You may be asking yourself right now "why does this matter? Aren't we supposed to conclude what's wrong in troubleshooting?"

In the end, yes. But each of us has something known as [**confirmation bias**][cbias] that affects how we gather information and use it to update our beliefs about the world. It's a powerful effect, and no one is immune. The best any of us can do is be aware of the bias and try to act in ways that minimize the impact. This is one technique for that.

**Confirmation bias** is the human tendency to give information that confirms our beliefs about the world more significance than information that disconfirms those beliefs.

Oftentimes it's helpful to explicitly qualify your assumptions in the statement of an observation:

| ok observation | detailed observation |
| :------------- | :------------- |
| I can't ping the outside interface. | I can't ping the outside interface by IP from my workstation iside the firewall. |
| I can't ping the web server. | I can't ping host *foo* by hostname or by IP address. |
| The engineering subnet is down. | I can see packets getting forwarded to the engineering subnet, but reply packets are getting dropped. |

**Recommendation:** if you are working an issue, or part of a team that is working an issue, be alert to 
your tendency to state conclusions as facts, adn intentionally recast your statements as observations. 
Doing so will lead to quicker issue resolution and avoid testing that retraces steps already taken.
Resist the temptation to explain, and just diligently observe. The explanation will emerge from a set of 
well-stated observations.


# A true story

This story happened to me, but I'm changing some names and details to protect privacy.

While enjoying a nice dinner at home, my phone rang. The senior leader over applications 
got right to the point: "all the servers are down." I asked if we had a bridge open for 
the team that would be working the issue (as specified in our incident management procedure.)
Apparently no one had read that procedure recently, so I asked if we could hang up, and get the 
Network Operations Center (NOC) to send us all details of the "war room" bridge after they open it. 

Within a few minutes, we had the response team (still pulling their hair out) on the conference bridge, 
and NOC staff riding along to assist. The team consisted of pretty much the CIO's direct reports: head of applications, head of infrastructure, head of communications[^1], head of support services, and head 
of security (that's me). It wasn't too clear at first that we were not under attack, so I got two of my
senior analysts on a separate bridge to gather data, and answer questions with facts wherever possible.

Dear reader, you may be asking yourself right now why staff were not handling this incident. Apparently
the genesis of the incident was a call about "all" the business apps not working, which got escalated to 
the head of apps. What testing that the app team performed is what resulted in the "all the servers are down" proclamation.

It didn't take long to disconfirm the conclusion--by carefully choosing vantage points in different
networks to observe from, my team found that while multiple servers were "down" or not responding,
at least some servers were "up." This was the beginning of narrowing the possible solution space and 
converging on a cause for the issue.

By observing that reachable hosts had a network in common, we formed a hypothesis that a router was not 
forwarding traffic to or from at least one network, causing the interruption in service. I shared that
hypothesis on the conference bridge, had the NOC analyst enter our observations into the ticket (to 
preserve a timeline of actions taken), and some discussion ensued.

The network engineers quickly came back with "the core router is functioning normally" after looking at
their monitoring tools. Alert readers will spot another conclusion dressed as an observation. Even more alert readers will grasp how confirmation bias would make any evidence their belief was wrong look less credible than information that confirmed the router was working normally. They were interested in ruling out their service as a problem, so quickly concluded that it was functioning normally
after failing to find monitoring or log evidence that it was failing.

We eventually performed a series of tests designed to disprove their hypothesis ("core router normal.") 
By designing experiments that isolated that piece of infrastructure, we were able to show traffic getting 
dropped with no indication in the logs that anything was amiss. (more on experiment design in a future article in this series).

In the end, the fault was a supervisor module in a core router. It failed, but did not produce the expected indications of failure that would have triggered the redundant module in the chassis to take over. Normal service was restored quickly by pulling out the active supervisor card, allowing the backup to take over.

This action (intentionally triggering a failover) called for trust on the part of the network engineers, but the process we used to isolate the problem was straightforward, repeatable, clear, and documentd (we had NOC write all test output into the incident ticket).

We spent about 30 minutes to get to resolution. Emotions were high because of the impact, even though this was outside working hours. Without a disciplined process, users could have felt much greater impact.

[^1]: Protip: "We are aware of the issue and actively working toward resolving it. Updates will follow as we gather more information." makes a pretty good template...

[cbias]: https://youarenotsosmart.com/2010/06/23/confirmation-bias/