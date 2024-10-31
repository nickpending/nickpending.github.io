---
layout: post
title: "censyspy: FQDN Discovery via Censys"
date: 2024-10-29
category: [security,tools]
---

Built a tool called censyspy that simplifies FQDN discovery via Censys. While you can absolutely do all of this with the Censys CLI and their query syntax (which isn't particularly complex), this wrapper makes it a bit more straightforward if you're specifically hunting for FQDNs. Since Censys has such a massive certificate dataset, queries can take a few minutes to complete - the tool defaults to a 5-minute timeout which should be enough for most cases.

## What's it actually do?

censyspy pulls FQDNs from two Censys data sources:


1. DNS Records: Grabs FQDNs from forward/reverse DNS lookups
2. SSL/TLS Certificates: Extracts additional FQDNs from certificate SANs


Here's where it gets interesting - while Censys is great at showing you what's live right now, there's value in tracking what you find over time. The Censys API doesn't have a "first seen" parameter, so we have to work around that limitation by maintaining our own dataset of discoveries.

## Use

Basic usage is straightforward:
```bash
# Initial data grab
censyspy --data-type both --domain example.com --output initial_scan.json --days all

# Daily updates (catches both new and current assets)
censyspy --data-type both --domain example.com --output daily_scan.json --days 1
```


The real power comes from what you do with this data over time. By keeping historical records, you can:
- Build wordlists based on how your target actually names things (way better than generic lists)
- Spot DNS issues by comparing current vs historical records
- Track how infrastructure changes
- Find forgotten assets that might not show up in current scans


Processing the results for actual security work is pretty simple:
```bash
# Get a clean list of FQDNs
cat results.json | jq -r 'keys[] as $top | .[$top] | keys[]' | sort -u > fqdns.txt
```

Once you have your list of fqdns you can feed that into your favorite tools.


I typically pipe the output straight into httpx for an initial survey. It'll probe for HTTP/HTTPS services across a bunch of common ports, grab response headers, titles, and server info. Particularly useful for spotting vhosts and getting a feel for what's running where:

```bash
# Hunt for services with httpx
cat fqdns.txt | httpx -irh -bp -td -title -vhost -fhr -td -csp-probe -sc -server \
-ports http:80,8080,8081,8082,8088,https:443,4443,6443,7443,8089,8443,8449,8905,8910,9443
```


For TLS analysis, here's an example using tlsx to check for hosts that might be vulnerable to FREAK by testing for export ciphers:

```bash
# Check for weak TLS configs with tlsx
cat fqdns.txt | tlsx -ci TLS_RSA_EXPORT_WITH_RC4_40_MD5,TLS_RSA_EXPORT_WITH_RC2_CBC_40_MD5,\
TLS_RSA_EXPORT_WITH_DES40_CBC_SHA,TLS_DH_ANON_EXPORT_WITH_RC4_40_MD5,\
TLS_DH_ANON_EXPORT_WITH_DES40_CBC_SHA,TLS_KRB5_EXPORT_WITH_RC4_40_SHA,\
TLS_KRB5_EXPORT_WITH_RC4_40_MD5,TLS_KRB5_EXPORT_WITH_RC2_CBC_40_SHA,\
TLS_KRB5_EXPORT_WITH_RC2_CBC_40_MD5,TLS_KRB5_EXPORT_WITH_DES_CBC_40_SHA,\
TLS_KRB5_EXPORT_WITH_DES_CBC_40_MD5 \
-c 5 -sm ztls -tps -tv -p 433 -cipher
```

Quick note: learned the hard way that you NEED to use `-sm ztls` when using `-ci`. If you don't, it defaults to auto and completely ignores your cipher list, which can be... frustrating to debug.


## Why bother saving the data?

This is where it gets good. Since Censys only shows you what's active, maintaining your own dataset gives you a serious edge:


1. You can generate wordlists based on actual naming patterns. Way more effective than generic subdomain lists when you're targeting specific organizations.

2. Spot dangling CNAMEs by comparing what you see now vs what you've seen before. These are easy to miss if you're only looking at current state.

3. Sometimes old assets stick around longer than they should. Historical data helps you find forgotten dev environments, test systems that never got cleaned up, or old API endpoints that might still be alive.

4. Since the Censys API can't tell you when something first appeared, tracking changes over time requires keeping your own records. It's not perfect, but it's the best we can do with the API's limitations.

## Setting it up

```bash
git clone https://github.com/nickpending/censys-toolkit.git
cd censys-toolkit
pip install .
```

You'll need API creds from Censys (https://censys.io/api).

## What's next?

Working on:
- Better ways to analyze historical data
- More filtering options
- Support for other Censys search types


Code's up on [GitHub](https://github.com/nickpendings/censys-toolkit) if you want to check it out or contribute.


---

*Standard disclaimer: Check out [Censys's API terms](https://censys.io/api-terms-of-service) before you start hammering their API.*