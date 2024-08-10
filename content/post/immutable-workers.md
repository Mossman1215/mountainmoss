---
title: "Immutable Workers"
date: 2024-07-10T16:05:49+13:00
draft: false
---

## status

I have four hosts that run my homelab stuff and I want fewer things to patch by switching from a traditional rpm based OS to rpm-ostree.

## problems

centos8-stream went EOL July 2023
Over time config drifts between hosts with manual package selections
base filesystem config could be automated
saving system configuration as code

## goals

- new base os layer
- understand update process
- create unattended installer for server and workers
- configure metallb from the start
- configure safe kublet shutdown

## resulting design

Fedora core os base layer (fcos39)

butane configuration file and ignition installer put into iso files for automatic provisioning

systemd scripts for installation of k3s and tailscale installed via file directives in butane

## outcomes

seamless os upgrade process to fedora 40

users are consistient across the fleet using the same butane user keys

Overall happy with core os because it leverages existing rpm support for packages like k3s without needing a specialist OS