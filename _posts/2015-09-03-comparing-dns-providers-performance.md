---
layout: default
title: Comparing DNS providers performance
comments: true
---

An interesting topic came up at work today regarding the performance and security features of DNS servers. A colleague of mine pointed me to an interesting [new service](http://www.mnemonic.no/en/Blog/Free-and-secure-public-DNS-service/) launched by Norwegian Internet security firm [Mnemonic](http://www.mnemonic.no). The service is a type of free and secure DNS service that protects users from malicious sites by resolving addresses to them to their "sinkhole" site (a web server with a blocking page). Sounds like a good idea, especially for users who needs that kind of protection agains themselves (e.g. your average Internet citizen).

Now, I've been a long time user of Google free public DNS offering and been really happy with the service, and as I'm not a huge fan of filtering DNS servers I probably won't switch - unless the performance of Mnemonic's service is better. I've always "felt" that Google DNS performed better than my ISP's own DNS servers. Feeling, is as we know, a very, very dangerous thing to do in the IT world, which brings me to the point of this article. Let's measure the performance and bring some raw, qualified data to the table!

![Measure all the things meme](/images/measureallthethings.jpg)

Let's compare some public DNS servers with some ISP DNS servers! I'm using a tool called [namebench](https://code.google.com/p/namebench/) and the following three are the once I'm interested in measuring:

* Google public DNS servers (8.8.8.8, 8.8.4.4)
* Mnemonic (54.76.198.100 52.28.79.1)
* Get ISP (84.208.20.110 84.208.20.111)

So let's run namebench with these nameserver IP's as arguments (I'm leaving out Google's name servers since those are my default). Namebench will automatically determine some other name servers to test too:

{% highlight bash %}
$ namebench 84.208.20.110 84.208.20.111 54.76.198.100 52.28.79.14
{% endhighlight %}

This outputs some numbers and graphs to stdout, but more importantly it also generates a [HTML report with some numbers and graphs](/assets/namebench_2015-09-03_1343.html).

![DNS performance](http://chart.apis.google.com/chart?chxt=y%2Cx%2Cx&chd=e%3AWsXecacbc5ekfGpdviyb.e&chxp=0%7C2%2C258&chxr=1%2C0%2C580%7C2%2C-29.0%2C609.0&chxtc=1%2C-720&chco=0000ff&chbh=a&chs=720x195&cht=bhg&chxl=0%3A%7CDynGuide%7C52.28.79.14%7C54.76.198.100%7CPortalane%20SE%7CPowerTech%20NO%7CCable%20%26%20Wireless%20DE-3%7CTelio-2%20NO%7CInternal%20192-1-1%7CUltraDNS-2%7C84.208.20.111%7COpenDNS-2%7C1%3A%7C0%7C40%7C80%7C120%7C160%7C200%7C240%7C280%7C320%7C360%7C400%7C440%7C480%7C520%7C560%7C580%7C2%3A%7CDuration%20in%20ms.)

The above graph is the highlight of the report and there's a couple of interesting things to note from this. First, clearly my ISP's DNS servers are faster than Google's (shown in the graph as Internal 192-1-1-). In fact, they are only beaten by [OpenDNS](https://www.opendns.com/) (although by a very small margin). They are both about 25% faster than Google's. These results clearly highlights the importance of measuring things, instead of just trusting your "feelings" or "senses"!

The second interesting thing to note is that Mnemonic's DNS servers are among the slowest in this test, only "beaten" by [DynGuide](http://dyn.com/labs/dyn-internet-guide/). They are in fact more than twice as slow as OpenDNS and [Get](https://www.get.no/). One might be tempted to attribute this to the fact that they are doing content filtering, but so does OpenDNS too. Of course I don't know the implementation details of either provider's content filtering technology, but I find it hard to believe that Mnemonic's implementation is so much better than OpenDNS' that it's worth the sacrifice in performance.

As of today I'm switching my home network DNS servers to OpenDNS - I'll consider the filtering a bonus for non-technical users on the network.
