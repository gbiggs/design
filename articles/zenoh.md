---
layout: default
title: Zenoh as an alternative communications middleware to DDS
permalink: articles/zenoh.html
abstract: 
author: '[Geoffrey Biggs](https://github.com/gbiggs) [Morgan Quigly](https://github.com/codebot) [Brandon Ong](https://github.com/???)'
published: true
---

{:toc}

# {{ page.title }}

<div class="abstract" markdown="1">
{{ page.abstract }}
</div>

## Overview

[Zenoh](http://zenoh.io/) is a new communications middleware created at ADLink Advanced Technology Laboratories.
It originated as a submission in response to the DDS-XRCE (Extremely Resource Contrained Environments) call for proposals
Although the DDS-XRCE specification ultimately went with another submission, ADLINK continued working on and improving their original proposal, producing the middleware now known as Zenoh.

Zenoh is an alternative to DDS.
It covers many of the DDS use cases, supports additional use cases not covered by DDS, but also misses some of the use cases that DDS covers.

## Motivation

DDS has proven to be a reliable way to transport data between different points in a variety of situations, from intra- and inter-process communication on a single computing node to communication between computing nodes distributed over a geographically large area.

However, our growing experience with using DDS in ROS has uncovered some areas where the design of DDS, as described in the specification, is not necessarily ideal.
The majority of these problems relate to the distributed discovery mechanism, which uses the Simple Discovery Protocol (SDP) (a part of the DDS specification) and UDP multicast.
When a new participant starts up, it broadcasts its presence using UDP multicast and SDP

- The use of multicast can degrade or outright fail in situations of lossy networks, particularly on Wi-Fi networks.
- Wi-Fi networks that use managed access points often interfere with the type of network traffic used by DDS to do discovery, leading to ROS nodes not being aware of each others' presence and incompletely-connected ROS graphs.
- ROS graphs are relatively sparse, with a relatively low number of connections between each node (each process containing ROS nodes is equal to a DDS participant in Foxy; prior to Foxy each node would have one DDS participant).
  DDS was designed for applications with a relatively small number of participants and where each participant is densely connected to all other participants.
  When a ROS graph of individually-launched nodes gets large (even just twenty nodes), and with each node being involved in ten topics _by default_ for the ROS infrastructure (before even counting application topics), the discovery traffic caused by the sudden startup of all these DDS participants at once during system startup can generate enough discovery traffic to bring down a Wi-Fi network.
- Even when discovery works correctly, it can be slow, leading to a slow system startup or tools that take time to launch.
  ROS includes countermeasures for this, such as a locally-running daemon to collect discovery information so that nodes and tools can bypass the usual DDS discovery mechanism and rapidly get a complete picture of the ROS graph.

Zenoh is...


## Use cases



## Requirements

Written properly.


## Design

### Relationship between Zenoh concepts and ROS concepts



## Evaluation

### Advantages of Zenoh

Advantages in a global context; don't try and play the DDS-vs-Zenoh game (we've already done that in the motivation, and DDS still has its useful places too)


### Disadvantages of Zenoh

An honest accounting of the disadvantages of Zenoh. Situations where it's not so great to use it, features its still missing, how long the roadmap is to getting those features, lack of a standard, etc.


## Alternative approaches

### Add a name server system to DDS

We should consider working with DDS vendors to incorporate non-multicast-based discovery (i.e. using a name server) into DDS.
A name server would revitalise the criticism of ROS 1 where the master was a single point of failure.
However, modern web applications have got fail-over redundancy and information redundancy down to a fine art.
Producing a redundant name server system should not fundamentally be difficult.
The difficulty lies in retrofitting a DDS implementation to use this instead of its discovery mechanism.
