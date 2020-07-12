---
layout: post
author: Michael Tsikerdekis
permalink: /:categories/:year/:month/:day/:title/
title: Hiding your real IP from the Internet using a proxy and securing it through your firewall
---

<p>
	This article is not about setting up a proxy on your browser, skype or torrents. This is easy! Just go on your application's options and setup your proxy. You should of course have access to a proxy server that ideally does not retain any log files. If they do, then if those log files can be accessed by someone then your traffic history would be available to them. So, you got your proxy server (paid or free) with no log files and you have its port and authentication (if any). You set everything up on your browser or torrent application and that's it, right? All your traffic parsed through the proxy, others see the proxy's IP address while you are hidden behind the proxy. Well, that is not always the case and the reason boils down to he programming of the application that you are using. This is a guide on how to put an additional layer of protection through applying firewall.<br />
	&nbsp;</p>

<h2>
	<a href="http://tsikerdekis.wuwcorp.com/HidingYourIpThroughProxyAndFirewall#hn_Proxies">Proxies</a></h2>

<p>
	<br />
	Before I go into more details, I need to clarify the obvious. Proxies (such as SOCKS5) do not encrypt traffic from your ISP. If you were to look at the packets transmitted from your computer on the way to the proxy server, you could see what is the final destination of the packet. After packets leave the proxy server then your IP cannot be discovered unless it is in the information of the actual packet sent to the server and it is not encrypted. Notice I am using the term packet loosely here. So if you don't want a website, a torrent swarm or peers and skype knowing what is your IP address, proxies will do the trick. Your ISP will still be able to see what you are doing. The only workaround for this is an SSH tunnel or a VPN which is not going to be discussed in this post.<br />
	&nbsp;</p>

<h2>
	<a href="http://tsikerdekis.wuwcorp.com/HidingYourIpThroughProxyAndFirewall#hn_How_does_your_IP_leak">How does your IP leak?</a></h2>

<p>
	<br />
	This could happen in a number of ways. It all boils down to bad programming on the side of applications. Some are designed in such a way that when your proxy server is down, they just redirect all traffic through the normal route. Other times, some of the traffic is sent through the proxy while some packets may be sent outside the proxy. All it takes is one packet to leak and basically you failed to do what you were attempting to do (hiding your IP from the destination server).<br />
	&nbsp;</p>

<h2>
	<a href="http://tsikerdekis.wuwcorp.com/HidingYourIpThroughProxyAndFirewall#hn_Solution">Solution</a></h2>

<p>
	I am providing a solution for Ubuntu but a Windows solution would work in the same way. Also, Mac users may be able to follow this guide but instead use the ipfw command which is similar to iptables (linux's firewall).<br />
	<br />
	The problem is divided into two solutions: a) block all outgoing leaking traffic and b) don't answer to any calls that don't come from a proxy. The latter is not necessary with browsing but with torrents it is if you really want to appear that you don't have a torrent client on to the outside world.<br />
	<br />
	&nbsp;</p>

<h2>
	<a href="http://tsikerdekis.wuwcorp.com/HidingYourIpThroughProxyAndFirewall#hn_Blocking_incoming">Blocking incoming</a></h2>

<p>
	<br />
	Blocking an incoming connection is relatively easy. I am assuming that if you are behind a router you already port forwarded the relevant port for your application to the computer running the application. Sometimes,&nbsp;<a href="http://tsikerdekis.wuwcorp.com/UPnP">UPnP</a>&nbsp;takes care of that. So let's say that port 8000 is the one for your application. All you need to do is tell your firewall to accept packets to this port only when they come from your proxy and drop the rest. Let's say that your proxy's ip is 10.10.10.10. As root you just run:<br />
	&nbsp;</p>

<p>
	iptables -F<br />
	iptables -A INPUT -p tcp -s 10.10.10.10 --dport 8000 -j ACCEPT<br />
	iptables -A INPUT -p udp -s 10.10.10.10 --dport 8000 -j ACCEPT<br />
	iptables -A INPUT -p udp --dport 8000 -j DROP&nbsp;<br />
	iptables -A INPUT -p tcp --dport 8000 -j DROP</p>

<p>
	<br />
	<br />
	The first command deletes all previous rules on the firewall which by default there aren't any.<br />
	&nbsp;</p>

<h2>
	<a href="http://tsikerdekis.wuwcorp.com/HidingYourIpThroughProxyAndFirewall#hn_Blocking_outgoing_packets">Blocking outgoing packets</a></h2>

<p>
	<br />
	Windows are a bit easier at restricting rules for one application. Linux isn't. My solution for this is to run an application as another user and apply rules to that user. It is definitely safer this way but it takes a bit of work. I won't go into details on how to create a new user and run that application as that user but you can find guides online. Assuming you have this ready and verified using ps -faux that your application runs through that user (IMPORTANT since rules will apply only for that user) you can type the following as root.<br />
	&nbsp;</p>

<p>
	iptables -A OUTPUT -p tcp -m owner --uid-owner testing -d 10.10.10.10 -j ACCEPT<br />
	iptables -A OUTPUT -p udp -m owner --uid-owner testing -d 10.10.10.10 -j ACCEPT<br />
	iptables -A OUTPUT -p udp -m owner --uid-owner testing -d 192.168.0.0/24 -j ACCEPT<br />
	iptables -A OUTPUT -p tcp -m owner --uid-owner testing -d 192.168.0.0/24 -j ACCEPT<br />
	iptables -A OUTPUT -p tcp -m owner --uid-owner testing -d 127.0.0.1 -j ACCEPT<br />
	iptables -A OUTPUT -m owner --uid-owner deluge -j DROP</p>

<p>
	<br />
	<br />
	Basically accept outgoing traffic from this user to 10.10.10.10, all ips in the LAN (you don't have to do this though) and packets sent to localhost. The last option is used by some programs to communicate with others. You have to adjust your settings but the important part is that you DROP packets sent to any IP that you don't like. If you try to do anything with that user, you will find that no websites will open without a proxy on your browser.<br />
	<br />
	<br />
	If you combine all of the incoming and outgoing rules into one file, make it executable and place it here: /etc/network/if-pre-up.d/ then your firewall settings will not be deleted after a reboot.<br />
	<br />
	&nbsp;</p>

<h2>
	<a href="http://tsikerdekis.wuwcorp.com/HidingYourIpThroughProxyAndFirewall#hn_Verifying_that_it_works">Verifying that it works</a></h2>

<p>
	<br />
	A way to see what packets are hitting your interface is to use tcpdump. This shows incoming packets before they pass through the firewall and outgoing packets that already passed through the firewall.<br />
	&nbsp;</p>

<p>
	sudo tcpdump port 8000 -i wlan0</p>

<p>
	<br />
	<br />
	Here is a sample of what you would expect to see:</p>

<p>
	17:30:57.219187 IP michael-netbook.local.8000 &gt; 10.10.10.10.42869: UDP, length 111<br />
	17:30:57.430905 IP 10.10.10.10.42869 &gt; michael-netbook.local.8000: UDP, length 30<br />
	17:30:57.461266 IP 10.10.10.10.42869 &gt; michael-netbook.local.8000: UDP, length 380<br />
	17:30:57.461473 IP michael-netbook.local.8000 &gt; 10.10.10.10.42869: UDP, length 30<br />
	17:30:57.492072 IP 10.10.10.10.42869 &gt; michael-netbook.local.8000: UDP, length 380<br />
	17:30:57.492286 IP michael-netbook.local.8000 &gt; 10.10.10.10.42869: UDP, length 30<br />
	17:30:57.502889 IP 10.10.10.10.42869 &gt; michael-netbook.local.8000: UDP, length 380<br />
	17:30:57.503056 IP michael-netbook.local.8000 &gt; 10.10.10.10.42869: UDP, length 30<br />
	17:30:57.517659 IP 10.10.10.10.42869 &gt; michael-netbook.local.8000: UDP, length 380<br />
	17:30:57.517858 IP michael-netbook.local.8000 &gt; 10.10.10.10.42869: UDP, length 33</p>

<p>
	<br />
	<br />
	It is likely that you would still see incoming traffic. This can be due to a) you had an open connection before applying the rules and activating the proxy (this will persist for a while) and b) machines on the internet initiated port scans for whatever reason. If your IP is dynamic it is likely to see a (b) traffic mainly due to other users that used your IP before you got it.<br />
	<br />
	But is the firewall working? Well, let's see:<br />
	&nbsp;</p>

<p>
	michael@michael-netbook:~$ sudo iptables -nvx -L INPUT<br />
	Chain INPUT (policy ACCEPT 5814 packets, 2633217 bytes)<br />
	&nbsp; &nbsp; pkts &nbsp; &nbsp; &nbsp;bytes target &nbsp; &nbsp; prot opt in &nbsp; &nbsp; out &nbsp; &nbsp; source &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; destination &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<br />
	&nbsp; &nbsp; &nbsp; &nbsp;0 &nbsp; &nbsp; &nbsp; &nbsp;0 ACCEPT &nbsp; &nbsp; tcp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 10.10.10.10 &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;tcp dpt:8000<br />
	&nbsp; &nbsp; 4210 &nbsp;3556857 ACCEPT &nbsp; &nbsp; udp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 10.10.10.10 &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;udp dpt:8000<br />
	&nbsp; &nbsp; &nbsp; &nbsp;0 &nbsp; &nbsp; &nbsp; &nbsp;0 DROP &nbsp; &nbsp; &nbsp; udp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;udp dpt:8000<br />
	&nbsp; &nbsp; &nbsp; &nbsp;0 &nbsp; &nbsp; &nbsp; &nbsp;0 DROP &nbsp; &nbsp; &nbsp; tcp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;tcp dpt:8000</p>

<p>
	<br />
	<br />
	Ideally, you will not see DROPPED packages in the counters but even if you see that is a good thing. It means that people tried to sent you stuff from 8000 directly to your IP and you firewall blocked them. For the rest of the world, the port appears closed as if you don't have an application listening.<br />
	<br />
	How about your outgoing traffic?<br />
	&nbsp;</p>

<p>
	michael@michael-netbook:~$ sudo iptables -nvx -L OUTPUT<br />
	Chain OUTPUT (policy ACCEPT 1028 packets, 388980 bytes)<br />
	&nbsp; &nbsp; pkts &nbsp; &nbsp; &nbsp;bytes target &nbsp; &nbsp; prot opt in &nbsp; &nbsp; out &nbsp; &nbsp; source &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; destination &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<br />
	&nbsp; &nbsp; 2585 &nbsp; 652144 ACCEPT &nbsp; &nbsp; tcp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;10.10.10.10 &nbsp; &nbsp; &nbsp; owner UID match 130<br />
	&nbsp; &nbsp; 4074 &nbsp; 336713 ACCEPT &nbsp; &nbsp; udp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;10.10.10.10 &nbsp; &nbsp; &nbsp; owner UID match 130<br />
	&nbsp; &nbsp; &nbsp; &nbsp;0 &nbsp; &nbsp; &nbsp; &nbsp;0 ACCEPT &nbsp; &nbsp; udp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;192.168.0.0/24 &nbsp; &nbsp; &nbsp; owner UID match 130<br />
	&nbsp; &nbsp; &nbsp;552 &nbsp; 314414 ACCEPT &nbsp; &nbsp; tcp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;192.168.0.0/24 &nbsp; &nbsp; &nbsp; owner UID match 130<br />
	&nbsp; &nbsp; &nbsp;826 &nbsp; 423050 ACCEPT &nbsp; &nbsp; tcp &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;127.0.0.1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;owner UID match 130<br />
	&nbsp; &nbsp; &nbsp; &nbsp;0 &nbsp; &nbsp; &nbsp; &nbsp;0 DROP &nbsp; &nbsp; &nbsp; all &nbsp;-- &nbsp;* &nbsp; &nbsp; &nbsp;* &nbsp; &nbsp; &nbsp; 0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;0.0.0.0/0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;owner UID match 130</p>

<p>
	<br />
	<br />
	Ideally, this should also show no DROPPED packages but even if it does it just means that everything is working. It also means that your application attempted to send something by bypassing the proxy but your firewall crashed its attempts.<br />
	<br />
	But don't take my word for it. Setup these rules and then remove the proxy. Try using your application and monitor traffic. Does anything work? If not then your firewall is doing its job allowing traffic only through proxy even if programs attempt to bypass your settings.</p>
