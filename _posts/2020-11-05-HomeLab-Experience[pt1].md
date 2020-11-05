---
layout: post
title: Becoming the next Google - HomeLab Experience [Part 1]
date: 2020-11-04 00:00:00
tags: [projects, homelab, networking]
last_modified_at: 
---
-----
Ah so it's been an extremely long time since I've lasted posted BECAUSE I have this thing where I would start a project... and then move onto a different one despite not finishing the previous project... 

Now some of you might be wondering, oh why don't you just finish the first project before you move on blah blah. 

Well here's my 20000IQ answer: if you are working on one project but get bored of it, move onto a second project; then, when you get bored of that second project, just work on the first project. Boom! 

![smort]({{ site.baseurl }}/assets/images/2020-11-05-HomeLab-Experience[pt1]/smort.jpg)

Now the tiny issue that I ran into was that I went from project one to project two and got bored of project two, I would move onto project three... and then onto project four... and then onto project five... So yeah, that really didn't work out for me aha

![soundsgood]({{ site.baseurl }}/assets/images/2020-11-05-HomeLab-Experience[pt1]/soundsgooddoesntwork.jpeg)

Anyhow, here we are so let's get started!

-----
# What's a HomeLab????
Well from my perspective, there are different spins on how people approach their own homelab set up. 

- On one hand, some people host services to provide a way of securing and maintaining their own data instead of having their data on a third-party server. 

- On the another hand, some people use it to torture themselves by testing and playing with different types of hardware and software solutions while having little-to-no impact on anything 'major' (i.e. on a company server). 

![linuxisfree]({{ site.baseurl }}/assets/images/2020-11-05-HomeLab-Experience[pt1]/linuxisfree.jpg)

- Now some people will use both hands by setting up and using their homelabs for both purposes as stated before, which is my ultimate goal. 

-----
# Okay, so what's your set up like?
That's a very good question that I don't even have an answer to yet. This is mostly because I'm trying to reconfigure my home network by separating my 'homelab network' from my 'rest-of-the-house' network. By doing so, whenever I need to make some network configurations to my homelab network, it won't affect the network stability for the rest of my family.

-----
# Alright, then what about hardware?
Oh this I can answer with some confidence! I have:
- 3 Dell PowerEdge Servers
    - PowerEdge R320 
    - PowerEdge R510
    - PowerEdge R710
- 1 Custom Built Virtualization Server
- Bunch of small computers

I'm too lazy to pull out the specs for each server so I won't. For the three PowerEdge servers, I bought them in 2019 (so about a year ago) on [eBay] using a very dangerous and tempting website called [LabGopher]. For the custom built virtualization server, I bought the components also from [eBay]; I will probably create a separate post that talks more about the virtualization server and the components I bought. For the bunch of small computers, my dad used to administer the TOEFL exam so we have a bunch of small computers that I now use for my homelab:) They're good enough to do some lightwork server stuff. 

![custombuild]({{ site.baseurl }}/assets/images/2020-11-05-HomeLab-Experience[pt1]/custombuild.jpg)

-----
# So now what?
Alright so here's the situation: I own two domains (hson.dev, sonbyj01.xyz) that currently don't have much functionality. That's because most of my services haven't been configured yet so I can't reverse proxy those domains to the proper services quite yet.

As of now, I have a general idea of how I want to configure my network:

![netdiagram]({{ site.baseurl }}/assets/images/2020-11-05-HomeLab-Experience[pt1]/netdiagram.jpg)

Now despite the saying 'A picture is worth a thousand words,' you probably don't know what's going on here and moreover why I have this particular set up in mind. 

So as I've mentioned before, I wanted to separate my home network into two parts, sort of like an onion. The 'first' layer is in white and that's from the first [NAT]-ing that my Asus router does. All the access points, computers, laptops, phones, etc. in the house, for the most part, will connect to this 'layer' of the network. <u>From here-on-out, I will call this layer 'NAT-1'.</u> Now there is a second layer of [NAT]-ing that occurs, and pfSense will handle [LAN] network traffic that's represented in mint green. <u>From here-on-out, I will call this layer 'NAT-2'.<u>

***My homelab will essentially live in that second 'layer', where it's colored in mint green.***

Now looking back at the diagram, you can clearly see that I have put two of my small computers on NAT-1, keller and balboa (please excuse my handwriting). Keller, which is running [CentOS 7], has the [Docker] engine running and will be hosting my very important [Minecraft server container] because I want to still be able to play [Minecraft] despite my entire homelab being offline. Balboa, which is running [Debian 10], also has the [Docker] engine running but instead will be hosting the [UniFi Controller container] for my access points. 

Moving onto NAT-2, this is where the fun stuff begins and ends... Mostly because I have't figured out how I want to approach it... Just off the top of my head, I know I want to get [link aggregation] to work so I can increase the bandwidth between servers, but right now I can't even access the [pfSense] UI so I have to first figure that out before I even move on. Once I can access the UI for my router, then I will most likely set it as my [DMZ] host from my Asus router. 

So, in all, as of now, none of my services are up and running, but I hope that during winter break I can spend more time with my servers so I can actually properly set them up. 

Tentatively, I have a couple of services that I want to get up and running once I figure out my entire network situation.
- [SWAG] - to handle all my http/https requests ([reverse proxying])
- [WireGuard]/[OpenVPN] - VPN access to my internal homelab network and also to secure my traffic when I'm on a public network
- [DokuWiki] - wiki page for my family whenever there's some technical issue... so they don't always have to call me...
- [NextCloud] - I don't want to pay for Google Drive storage anymore>:(
- [OpenLDAP] - enable authentication to different self-hosted services
- [KeyCloak] - enable SSO in conjunction with OpenLDAP

But that's all the update I have so far in regards to my homelab set up!

Thanks for reading - 

sonbyj01

-----

[OpenLDAP]:https://github.com/osixia/docker-openldap
[KeyCloak]:https://github.com/keycloak/keycloak
[NextCloud]:https://github.com/linuxserver/docker-nextcloud
[DokuWiki]:https://github.com/linuxserver/docker-dokuwiki
[OpenVPN]:https://github.com/linuxserver/docker-openvpn-as
[WireGuard]:https://github.com/linuxserver/docker-wireguard
[SWAG]:https://hub.docker.com/r/linuxserver/swag
[reverse proxying]:https://en.wikipedia.org/wiki/Reverse_proxy
[DMZ]:https://en.wikipedia.org/wiki/DMZ_%28computing%29
[pfSense]:https://www.pfsense.org/
[link aggregation]:https://en.wikipedia.org/wiki/Link_aggregation
[Minecraft server container]:https://github.com/itzg/docker-minecraft-server
[UniFi Controller container]:https://docs.linuxserver.io/images/docker-unifi-controller
[Docker]:https://www.docker.com/
[Debian 10]:https://www.debian.org/
[Minecraft]:https://www.minecraft.net/en-us
[CentOS 7]:https://www.centos.org/download/
[LAN]:https://en.wikipedia.org/wiki/Local_area_network
[NAT]:https://en.wikipedia.org/wiki/Network_address_translation
[eBay]:https://www.ebay.com/
[LabGopher]:https://www.labgopher.com/