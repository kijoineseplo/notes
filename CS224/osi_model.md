---
title: CS 224 - Computer Networks
layout: post
---

## The OSI Model

The **Open Systems Interconnection** (OSI) model is a conceptual model, created by the International Organization for Standardization. It characterize the communication functions of a computer system irrespective of the system's underlying internal structure and technology.

TL;DR - The OSI model provides a standard for different computer systems to communicate using standard protocols.

### 7 Layers of the OSI Model

The following table from this [wikipedia article](https://en.wikipedia.org/wiki/OSI_model) summarizes the functions of each layer.

<table class="wikitable" style="margin: 1em auto 1em auto;">
<caption>OSI model
</caption>
<tbody><tr>
<th colspan="3">Layer
</th>
<th><a href="https://en.wikipedia.org/wiki/Protocol_data_unit" title="Protocol data unit">Protocol data unit</a> (PDU)
</th>
<th>Function
</th></tr>
<tr>
<th rowspan="4">Host<br>layers
</th>
<td style="background:#d8ec9b;">7
</td>
<td style="background:#d8ec9b;"><a href="https://en.wikipedia.org/wiki/Application_layer" title="Application layer">Application</a>
</td>
<td style="background:#d8ec9c;" rowspan="3"><a href="https://en.wikipedia.org/wiki/Data_(computing)" title="Data (computing)">Data</a>
</td>
<td style="background:#d8ec9c;">High-level <a href="https://en.wikipedia.org/wiki/API" title="API">APIs</a>, including resource sharing, remote file access
</td></tr>
<tr>
<td style="background:#d8ec9b;">6
</td>
<td style="background:#d8ec9b;"><a href="https://en.wikipedia.org/wiki/Presentation_layer" title="Presentation layer">Presentation</a>
</td>
<td style="background:#d8ec9b;">Translation of data between a networking service and an application; including <a href="https://en.wikipedia.org/wiki/Character_encoding" title="Character encoding">character encoding</a>, <a href="https://en.wikipedia.org/wiki/Data_compression" title="Data compression">data compression</a> and <a href="https://en.wikipedia.org/wiki/Encryption" title="Encryption">encryption/decryption</a>
</td></tr>
<tr>
<td style="background:#d8ec9b;">5
</td>
<td style="background:#d8ec9b;"><a href="https://en.wikipedia.org/wiki/Session_layer" title="Session layer">Session</a>
</td>
<td style="background:#d8ec9b;">Managing communication <a href="https://en.wikipedia.org/wiki/Session_(computer_science)" title="Session (computer science)">sessions</a>, i.e., continuous exchange of information in the form of multiple back-and-forth transmissions between two nodes
</td></tr>
<tr>
<td style="background:#e7ed9c;">4
</td>
<td style="background:#e7ed9c;"><a href="https://en.wikipedia.org/wiki/Transport_layer" title="Transport layer">Transport</a>
</td>
<td style="background:#e7ed9c;"><a href="https://en.wikipedia.org/wiki/Packet_segmentation" title="Packet segmentation">Segment</a>, <a href="https://en.wikipedia.org/wiki/Datagram" title="Datagram">Datagram</a>
</td>
<td style="background:#e7ed9c;">Reliable transmission of data segments between points on a network, including <a href="https://en.wikipedia.org/wiki/Packet_segmentation" title="Packet segmentation">segmentation</a>, <a href="https://en.wikipedia.org/wiki/Acknowledgement_(data_networks)" title="Acknowledgement (data networks)">acknowledgement</a> and <a href="https://en.wikipedia.org/wiki/Multiplexing" title="Multiplexing">multiplexing</a>
</td></tr>
<tr>
<th rowspan="3">Media<br>layers
</th>
<td style="background:#eddc9c;">3
</td>
<td style="background:#eddc9c;"><a href="https://en.wikipedia.org/wiki/Network_layer" title="Network layer">Network</a>
</td>
<td style="background:#eddc9c;"><a href="https://en.wikipedia.org/wiki/Network_packet" title="Network packet">Packet</a>
</td>
<td style="background:#eddc9c;">Structuring and managing a multi-node network, including <a href="https://en.wikipedia.org/wiki/Address_space" title="Address space">addressing</a>, <a href="https://en.wikipedia.org/wiki/Routing" title="Routing">routing</a> and <a href="https://en.wikipedia.org/wiki/Network_traffic_control" title="Network traffic control">traffic control</a>
</td></tr>
<tr>
<td style="background:#e9c189;">2
</td>
<td style="background:#e9c189;"><a href="https://en.wikipedia.org/wiki/Data_link_layer" title="Data link layer">Data link</a>
</td>
<td style="background:#e9c189;"><a href="https://en.wikipedia.org/wiki/Frame_(networking)" title="Frame (networking)">Frame</a>
</td>
<td style="background:#e9c189;">Reliable transmission of data frames between two nodes connected by a physical layer
</td></tr>
<tr>
<td style="background:#e9988a;">1
</td>
<td style="background:#e9988a;"><a href="https://en.wikipedia.org/wiki/Physical_layer" title="Physical layer">Physical</a>
</td>
<td style="background:#e9988a;"><a href="https://en.wikipedia.org/wiki/Bit" title="Bit">Bit</a>, <a href="https://en.wikipedia.org/wiki/Symbol_rate#Symbols" title="Symbol rate">Symbol</a>
</td>
<td style="background:#e9988a;">Transmission and reception of raw bit streams over a physical medium
</td></tr></tbody></table>

#### Layer 1: Physical Layer

This layer is responsible for the transmission and reception of raw data between a device and a physical transmission medium, such as cables and switches. It also converts the digital data into electrical, radio or optical signals to make it transferable via a physical medium.

#### Layer 2: Data Link Layer

The data link layer provides node-to-node data transfer. In this layer data is transferred between two devices on the SAME network. DLL takes packet from the network layer and breaks them into smaller pieces called frames. DLL detects and possibly corrects errors that may occur in the physical layer.

#### Layer 3: Network Layer

The network layer is responsible for transferring data packets between two different networks. It receives frames from the DL layer, and delivers them to their intended destinations based on the node's logical address, such as IP address. It also breaks segments from the transport layer into packers on the sending device and reassembles these packets on the receiving device. This layer is also responsible for finding the best physical path for the data to reach its destination, this is done by special nodes in the layer known as routers and this process is called routing.

#### Layer 4: Transport Layer

The transport layer manages the delivery and error checking of data packages. It controls the size, sequencing and the transfer of data between systems and hosts. It also provides the acknowledgement of the successful data transmission and sends the next data if no error occurred. Although not developed under the OSI Model, the Transmission Control Protocol (TCP) and the User Datagram Protocol (UDP) are commonly categorized as layer-4 protocols within OSI. Transport Layer Security (TLS) provide security at this layer.

#### Layer 5: Session Layer

The session layer controls the connections between computers. It establishes, manages and terminates the connections between two computers. This layer ensures that the session stays open long enough to transfer all the data being exchanged, and then promptly closes the session in order to avoid wasting resources. This layer is also responsible for session checkpointing and recovery in case the systems get disconnected in the middle of a data transfer. Session layer also include authentication and reconnection.

#### Layer 6: Presentation Layer

The presentation layer prepares the data so that it can be used by the application layer. It formats, translates data for the application layer based on the syntax or semantics that the application accepts. Because of this, it is also called syntax layer. It can also handle encryption and decryption of data. It also compress the data received from the application layer before sending it to Session Layer.

#### Layer 7: Application Layer

This layer is closet to the end user and both the user and the application layer interact directly with the application. Just for clarification, the applications are not part of the application layer, rather this layer is responsible for the protocols and data manipulation that the software uses to present data to the user. Some of the protocols in Application layer are HTTP, FTP, SMPT, Telnet, DNS etc.
