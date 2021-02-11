---
title: CS 224 - Computer Networks
layout: post
---

## Multi Access Networks

Till now we have only discussed the protocols used in a P2P (Point-to-Point) network. A P2P network can only have 2 computers participating in the network, while a multi-access network, as suggested by it's name, can have multiple computers accessing the network. The most common example of a multi-access network is Ethernet.

In multi-access networks the transmission channels are not dedicated i.e. any computer in the network can use the channel to transmit data to other computers in the network. This causes data overlapping and lose of data due to collisions between signal sent by different computers. Certain protocols are used in multi-access networks to handle data transmission on non dedicated channels
