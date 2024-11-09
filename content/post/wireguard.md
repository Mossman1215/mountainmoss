---
title: "Wireguard"
date: 2024-11-10T11:14:49+13:00
draft: false
---

## Implementing wireguard manually

To allow more fine grained control of my network I have decided to implement wireguard as a site to site VPN.

Prior to this I was using tailscale to advertise routes from catalyst cloud into my homelab LAN. The issue I experienced was the traffic going all the way to sydney to Tailscales relay server. Part of the featureset for tailscale is to perform NAT punchthrough and that requires a public endpoint to provide coordination.

Most companies host in sydney because it has cloud regions for the big three vendors (google, microsoft, amazon) but that increases the charges for my hosting because international bandwidth is more expensive than national bandwidth.

To remove the relay server problem I decided to try building a wireguard tunnel with networkmanager

### Factors that allowed me to implement wireguard  
  
- Both of my machines are running Fedora CoreOS with network manager
- the existing subnet router is responsible for the tunnel
- I have a public static ip which I can use to allowlist on the catalyst cloud security group
- port forwarding rules translate my public address to the internal address of the subnet router
- the main node is the prodesk G2 hosting both the kubernetes api server and wireguard tunnel

I've added a specific network to transit between catalyst and home 10.9.9.0/24.
That allows me to assign gateway addresses and static routes in order to forward the set of addresses allocated to metallb running inside the cluster over the new tunnel.

My hope is that it will resolve routing issues and so far on twodegrees I have been routed to my cloud server via auckland with 38ms of latency which for a game server is within my acceptable range.

Implementation all worked well thannks to the redhat guides on configuring network manager.
The only issue I had was not ticking the automatically connect tickbox when creating the connections.
Which lead to a couple of automatic patching related outages.

In future I'm considering nmstate-operator for the kube cluster to allow the configuration to be commited to my homelab code repository
