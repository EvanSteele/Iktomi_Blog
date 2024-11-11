---
layout: post
author: Evan
---
The game Cyberpunk 2077 is set in a fictional future after the development of general intelligence AI. These AI were able to find their way onto the wider internet, and essentially started World War 3. To mitigate their damage, the internet became fragmented into a series of subnets, and sitting between those subnets was something called the Blackwall, humanity's last line of defense keeping the rogue AI's out of our fragile networks. 

A few weeks ago I spent a day working on digitizing a board game I like. I got a functional copy of the board and a few unit tests for different features working, and I felt pretty good about it. I wanted to demo it to a friend at work the next day, but I didn't want to go through the effort of setting up a full webserver. Feeling clever, I spun up a python SimpleHTTP server on my desktop, opened port 80 on my firewall to point to it, and set up NoIP to have a subdomain point to my home IP (The last part wasn't technically needed, I just wanted to be stylish). With this all set up for tomorrow I went off to bed. 

What I didn't realize is that the moment I opened that port on my network, I was moving my computer out beyond our real-life blackwall. I had invited the world to connect to a small part of my home network, and the would wasted no time in letting themselves in. 

Looking at the server logs, at 12:27AM I had port forwarding working and I was able to connect from the internet. At 12:46AM, 19 minutes after it first opened, we see a connection being made from Holland.  Another from Holland at 1:01AM, but no malicious activity so far. Maybe their just games enthusiasts? 

Our first malicious payload came in at 1:33AM, just over an hour since the server went public. From there the floodgates came open, and you can see a snippet of all the requests that came in below: 

```
::ffff:5.8.11.202 - - [24/Sep/2024 01:33:08] code 400, message Bad request version ("¯nãY»bhlÿ(=':©\\x82ÙoÈ¢×\\x93\\x98´ï\\x80å¹\\x90\\x00(À")
::ffff:5.8.11.202 - - [24/Sep/2024 01:33:08] "\x16\x03\x02\x01o\x01\x00\x01k\x03\x02RHÅ\x1a#÷:Nßâ´\x82/ÿ\x09T\x9f§Äy°hÆ\x13\x8c¤\x1c="á\x1a\x98 \x84´,\x85¯nãY»bhlÿ(=':©\x82ÙoÈ¢×\x93\x98´ï\x80å¹\x90\x00(À" 400 -
::ffff:83.97.73.245 - - [24/Sep/2024 01:57:58] "GET / HTTP/1.1" 200 -
::ffff:83.97.73.245 - - [24/Sep/2024 02:05:18] "GET / HTTP/1.1" 200 -
::ffff:149.50.103.48 - - [24/Sep/2024 02:09:34] "GET / HTTP/1.1" 200 -
::ffff:83.97.73.245 - - [24/Sep/2024 02:12:27] code 501, message Unsupported method ('POST')
::ffff:83.97.73.245 - - [24/Sep/2024 02:12:27] "POST /Autodiscover/Autodiscover.xml HTTP/1.1" 501 -
::ffff:149.50.103.48 - - [24/Sep/2024 02:18:44] "GET / HTTP/1.1" 200 -
::ffff:83.97.73.245 - - [24/Sep/2024 02:22:10] code 501, message Unsupported method ('POST')
::ffff:83.97.73.245 - - [24/Sep/2024 02:22:10] "POST /vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php HTTP/1.1" 501 -
::ffff:185.191.126.213 - - [24/Sep/2024 02:24:44] "GET / HTTP/1.1" 200 -
::ffff:83.97.73.245 - - [24/Sep/2024 02:30:44] code 404, message File not found
::ffff:83.97.73.245 - - [24/Sep/2024 02:30:44] "GET /vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php HTTP/1.1" 404 -
::ffff:83.97.73.245 - - [24/Sep/2024 02:37:55] code 404, message File not found
::ffff:83.97.73.245 - - [24/Sep/2024 02:37:55] "GET /solr/admin/info/system?wt=json HTTP/1.1" 404 -
::ffff:95.214.55.138 - - [24/Sep/2024 02:40:25] "GET / HTTP/1.1" 200 -
::ffff:185.16.39.118 - - [24/Sep/2024 03:00:37] "GET / HTTP/1.1" 200 -
```

When I woke up I realized just how much of a problem this may be. Your average web server is going to have some protections in place to limit access to some degree, and even then if someone did gain access it would only be to the contents of the server. This is just a bare-bones python script that serves out the directory it's run from, and if someone were to find an exploit for that they'd have access to my entire computer. After a few hours of sleep to mull it over, I realized how much of a Bad Idea this was and killed the server. 

In the six hours the server was online, 70 requests from unknown groups came in from 21 IPs. I won't go into specific payloads for each one, but it looks like a few were scraping by IP and some others were looking for a specific vulnerability on a wide-range of IPs that I happened to be a part of. I should consider myself fortunate that there was no serious attack against me, because as it turns out there is [A known CVE for this module](https://nvd.nist.gov/vuln/detail/CVE-2021-28861) that would have allowed for open redirects. oops. 

Treat this as a warning for anyone else looking for shortcuts, your IP is likely getting hit by a couple hundred malicious requests each day. These attackers are using known vulnerabilities to target misconfigured equipment. If you put anything on the internet without securing it, it's just a matter of time until someone sends the exploit that matters for your system. 
