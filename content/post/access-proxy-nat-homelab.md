---
title: "Bypassing NAT issues with Cloudflare Argo"
date: 2019-10-08T18:39:39+13:00
draft: true
---

# Bypassing NAT issues with Cloudflare Argo Tunneling

My homelab environment is my way of cost effectively trialling diferent types of tools and run it like a production service.
Getting a home environment go out to the wider internet nicely when ISP's are expecting you to only consume content make this quite difficult.
I have some trouble getting port forwarding working with my new ISP  who are using CGNAT and providing some router hardware that I can't configure so all my attempts were failing even though I've been doing DDNS with this [script](https://github.com/adrienbrignon/cloudflare-ddns) for a while.

## Enter Argo Tunnel

So researching how to do reverse proxy setups and SSL configuration was getting tedious I thought I'd try this Cloudflare system called argo tunnel
basically you point it at something running http/https and then it forwards traffic to cloudflare bypassing NAT and the need to setup TLS on your server because the tunnel software handles secure transmission to cloudflare and cloudflare provides TLS termination.

Systemd templated services

For multiple services you need multiple config files and systemd units. Systemd templates allow a single unit file define multiple units under an umbrella service
I found this helpful tip on the [github issues page](https://github.com/cloudflare/cloudflared/issues/59) to define as many services via config files as I needed

SSH

You can also do the same process for SSH!