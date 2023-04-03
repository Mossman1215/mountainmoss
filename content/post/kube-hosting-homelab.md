---
title: "Homelab Diagrams"
date: 2023-04-03T21:11:35+13:00
draft: false
---

## Neato diagram

Here's a diagram of my homelab setup
16 cores and 64GB of RAM on the cloud would be ~$500/month in ap-southeast-2
I'm only expecting local traffic so I'm gonna give catalyst cloud a go for ingress

```goat
                                       +-----------------------------------------------+----------------------------+
                                       |    Kubernetes nodes (k3s)                     |                            |
                                       |                                               |                            |
                                       |   +--------------------+   +----------------+ |                            |
                                       |   |                    |   |                | |                            |
+---------------+                      |   | thinkcentre m700   |   | prodesk g400   | |                            |
|               |                      |   | 4CPU               |   | 4CPU           | |                            |
|               |   +--------------+   |   | 16GB               |   | 16GB RAM       | |                            |
| CatalystGw    |   |              |   |   | 160GB sata ssd     |   | 120GB sata ssd | |                            |
|               <---+ edgerouter x <-----+ | 240GB sata ssd m.2 |   | 240GB nvme ssd | |                            |
|  1vCPU        |   |              |   | | |                    |   |                | |  +------------------------ |
|  1GB RAM      |   +--------------+   | | +----------+---------+   +-------+--------+ |  |                       | |
|  10GB         |                      | |            |                     |          |  | netgear 212 NAS       | |
|               |                      | +------------------------------------------------+ 500 GB sata ssd tier0 | |
+---------------+                      |              |                     |          |  | 2TB SATA HDD tier1    | |
                                       |  +-----------+--------+    +-------+--------+ |  |                       | |
                                       |  |                    |    |                | |  |                       | |
                                       |  | thinkcentre m720q  |    | NUC i5         | |  |                       | |
                                       |  | 6CPU               |    | 2CPU           | |  +-----------------------+ |
                                       |  | 16GB               |    | 16GB RAM       | |                            |
                                       |  | 240GB sata ssd     |    | 240GB sata ssd | |                            |
                                       |  | 240GB nvme ssd     |    | 240GB nvme ssd | |                            |
                                       |  |                    |    |                | |                            |
                                       |  +--------------------+    +----------------+ |                            |
                                       +-----------------------------------------------+----------------------------+
```

## Reasoning

1. TINY PUTERS
2. 16GB of ram is really expensive in the cloud and this hardware is being discarded frequently
3. Each tiny pc comes with an SSD

catalyst edge node provides a secure public ip and attaches to the tailnet to get access to kubernetes resources

ACLs would be good.