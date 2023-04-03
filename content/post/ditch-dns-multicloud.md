---
title: "Ditch DNS Go Multicloud"
date: 2022-09-02T21:11:35+13:00
draft: false
---

## Ditch Cloudflare

Cloudflare doesn't have good content moderation policies and refuses to remove a hate speech site.
So I must move my hosting and money from them.

Context:

https://www.dropkiwifarms.net/

> #DropKiwifarms works to end the relationship between far-right hate forum Kiwi Farms and the digital service providers that keep Kiwi Farms active online. We 
> started this campaign after members on the website published private information on Clara Sorrenti, including sexually explicit photos and videos, phone 
> numbers, addresses, her deadname and the private information of her friends and family. Publishing that information led to threats on her life, both implicit 
> and explicit, as well as attempts to end her life through false reports to the police about imminent violence -- a practice that has ended the lives of other
> people.

## Plan

1. copy dns to a file from cloudflare
2. add zone to google cloud with existing dns entries
3. move the name servers to be google via metaname

at this point the blog is migrated 😄

for the remaining things I used cloudflare for (reverse proxy and automated TLS)

4. start a VPS with decent bandwidth
5. connect VPS to tailscale network
6. configure caddy as a reverse proxy with tls certs
7. replicate the app hostname configurations with caddy + tailscale
