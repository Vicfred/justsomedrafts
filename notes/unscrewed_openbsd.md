Unscrewed; a Story About OpenBSD

To put it in no uncertain terms, I was screwed. It was 5am, and the network was down. The network was down because the core routers had crashed. Yes, both of them, master and backup. And to make matters worse, this was only 12 or so hours after the network was down because the switches had crashed. Yes. All of them.

Have you ever even heard of something like that? 3 switches? core dumped?! Well, pull up a chair brother, and let me tell you the tale. I was THERE. Hell, I was even responsible.
I was well and truly screwed.

It all began in the year two thousand and seven, and I was in the midst of implementing a network design for the web-hosting company I work for, so they could migrate their 90’s era infrastructure to shiny new tech. Where before there were Alteons, there would now be application-layer balancers. While in the past they had bulky, power-hungry application servers, now they would have VM’s. Where before there was an inside and a DMZ, now there would be a proper four-tier multi-homed network with a dedicated static content tier. Can you see it brother? The promised land, where rainbows arch over free-flowing rivers of beer, and everything smells like bacon.

The only, teeny, tiny problem, was that we couldn’t quite afford to implement the network we needed on Cisco gear. It would have been upwards of 150k in network gadgets to get us there with the big C, and we had lots of other stuff to buy, so I began researching alternatives.

Now around that time, Juniper Networks was just beginning in earnest to condescend to make the little routers and security appliances that guys like I need. They had a slew of really interesting looking routers and switches that were perfect for our new environment, and a very new and shiny JunoOS, to go along with them. Especially given the new OS, the Juniper stuff was far more flexible compared to the Cisco gear, and would save us all kinds of money on hardware. And bonus, everything in Juniper-land ran on the same OS. Given their reputation with the MX series, I figured what could possibly go wrong?

Well let me tell you brother; a lot. And by “a lot”, I mean pretty damn-well everything. I tell you, I built those things up like the Burj Khalifa, and down they fell like Hephaestus from the heavens; down, down, and down… for days. The thing was, the new JunOS was just too different and too new. I couldn’t buy training at the time, none of the other network operators at any of the Colo’s we frequented had used it, and the books that were available were all written to earlier versions of the OS, which were quite different. It was a perfect storm of bad timing, ironic potential, and untried tech. So there I was, right out on the razors-edge, implementing production networks with an untested OS on brand new hardware. And oh how it failed.

But what choice had I? And indeed, now that the websites were down and every piece of network gear I’d used to implement this network had proved itself unusable, that was surely the question of the hour. What were my choices? Even if I could somehow conjure the hundred and a half thousand dollars we needed to acquire Cisco gear, it would be days before I could actually get my hands on it and configure it all. I needed uptime, and I needed it now.
Enter OpenBSD

If you’re in the packet delivery business, and you’ve never tired OpenBSD, then you’re really missing out. Pretty much everything you care about as a network guy on production networks is configured via a virtual interface. This includes CARP, IPSEC, and all manner of encapsulation and tunneling protocols. This is awesome because all the tools designed to work on interfaces, like tcpdump, work on these virtual interfaces too. So if I want to get a look at my VPN traffic, I can tcpdump enc0.

Which brings up another great point, with OpenBSD, your packet inspection and general network troubleshooting toolbox is way better. Nmap, Argus, sflow, tcpdump, snort, daemonlogger, and etc.. all the best tools are right there on your router if you want them. No need to use a packet tap, because your router is the packet tap.

OpenBSD has myriad built-in daemons for OSPF, BGP, and every other router protocol, as well as application-layer protocol proxies. OpenBSD is by far the fastest, easiest way to setup an ftp proxy that I know of. It also has a kernel-space packet filter called PF, which is crazy feature-rich and and easy to use. If you can console configure an ASA, or are an iptables user, you’ll pick up PF’s syntax in about 15 minutes. All the normal stuff like NAT, redirection, and forwarding are there. Further, PF can do things like policy routing, where you tag packets based on criteria you choose, and then make routing decisions later based on those tags. PF has packet queuing and prioritization built-in, so you can make some classes of traffic more important than others.

Bi-directional load balancing is available, so you can tell PF to distribute out-bound traffic across different upstream paths, or distribute in-bound traffic across a farm of web servers. PF can even use a special virtual interface to share it’s state table with other systems running PF, making possible seamless failover to up to 255 other PF hosts (limited by CARP).

But I digress. Anyway, at the time, I’d been playing with OpenBSD for a couple years and had some pet projects going with it, but I hadn’t honestly considered replacing ASA/Pix/j4350 class core router hardware with it. Well mired as I was, in the pit of molten exigency, the possibility dangled into view like a rope ladder. I knew it would be stable. I knew it was more than sufficiently functional. But I didn’t really know how well it’d perform.
So how did it perform?

So I grabbed a few Cisco 2950 switches that we had laying around and threw up a pair of OpenBSD boxes on 1u SuperMicro hardware that we also had laying around. That stabilized things, and bought me some time to ponder next steps.

While I looked for a permanent solution, that wouldn’t break the bank, I kept a nervous eye on those initial band-aid systems and while they kept up fine, (no jitter, latency equal to an ASA, less retransmission than we had on our Cisco-routed networks) they didn’t achieve the line speeds that my dedicated network gadgets did. The load-times on the sites we host weren’t noticeably higher, but it was noticible on large file transfers between internal systems.

That was interesing, so I Googled around, and came across this article from calomel.org on tuning (free|open)BSD for network performance. A few sysctl tweaks later, and my little duct-tape routers were indistinguishable from their posh green and/or blue colored counterparts.

Well, they weren’t totally indistinguishable, they were far more flexible and inexpensive than their posh green-colored counterparts, and far more stable, flexible, and inexpensive than their posh blue-colored counterparts. Was this a final answer? Was I really going to run production infrastructure – websites that were discussed on Oprah, and plugged in TV adds during the NCAA playoffs – on a general purpose OS with commodity hardware instead of industry standard network gear?
Brother, you’re damn right I was.

I liked them so much, I returned all of the Juniper gear, and used a teeny fraction of it to buy a pair of front I/O super micro boxes, with two 4-port EEPROs in the PCI slots, so they had a proper home. This has become a production standard for us. Anywhere you’d find an ASA, router or other mid-range network doohicky in somebody else’s network, you’ll find a happy little pair of SuperMicro 515’s in ours. We run OpenBSD on smaller, high-end Soekris-style hardware for dedicated tunneling stuff like OpenVPN and IPSEC (we maintain several IPSEC tunnels with 3rd parties where the other end is a Cisco ASA, and that works great). The only reason I use “real” network gear anymore is:

    In the switch layer (duh)
    When I need specific interface hardware for SIP, DS3, coax etc..
    If I ever need big-iron mx480’s or whatever for core routing 10gbps links.
    When my boss makes me.

And there you have the story of how I was unscrewed by OpenBSD. What’s wrong brother? I see you sitting there, arms crossed, with that skeptical look of disgust on your face. What’s that? I’m just a Unix-idiot who failed to plan? There’s no way OpenBSD can achieve the performance of dedicated hardware? Cisco and/or Juniper are for-the-win and I’m an ignorant windbag?

For the record, I didn’t write this post with the intention of dissing up on your preferred network hardware vendors. Personally I think Juniper and Cisco make great stuff, and while I agree in theory that there’s no way OpenBSD can fling packets as fast as a 2950 on a gigabit line, in practice, I can’t tell the difference with the tools at my disposal. In fact, I can tell you from experience that whatever latency I’m introducing is more than made up for by the introduction of application-layer solutions on the core routers themselves. For example, running a caching dns server on the end-user gateway device in HQ is faster for the users than running an ASA with a caching NS somewhere else on the network. And bonus, I get to run DJBDNS because the gateway is a Unix box.

Further, for the record, I ran the Juniper j4350’s and EX4200’s for a few months in that environment to make sure they were stable. Why and how they suddenly failed within 24 hours of each other I cannot guess, but I wasn’t doing anything fancy, and after that happened I just couldn’t risk any more down time. If I tried the same setup again today with current JunOS, I would almost certainly have a different experience.

Honestly though I’m glad it happened the way it did because while it didn’t meaningfully lower my estimation of either of those vendors, it substantially changed my perspective on the use of general purpose OS’s like OpenBSD instead of embedded systems to backbone real production networks. Where before I would have been hesitant, these days – for the preponderance of situations I normally encounter – I greatly prefer to run OpenBSD routers. They’re just more efficient.

    They save you hardware and hops because they can pull double duty as a router, firewall, FTP proxy, NAT gateway, DNS cache, VPN gateway, caching squid reverse-proxy, etc… ad-infinitum

    Your failover systems are fully functional (not nerfed-license versions of the real thing)

    No licenses. All of your routers have pretty much the same capabilities (no wondering if that regional 2800 has a crypto license)

    You choose your own hardware, and have real control over optimization.

    Better tools for everything, including troubleshooting and config backup/restore

    Run real monitoring agents (unless you actually enjoy SNMP, in which case, feel free to continue using it)

    Everything that comes with Unix (chef for the network layer!)

Anyway, the end.
