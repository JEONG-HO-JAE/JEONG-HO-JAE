---
title:  OSI 7 Layer, Protocol
excerpt: concepts

toc: true
toc_label: About
toc_sticky: true

categories: NW

date: 2022-03-25
last_modified_at: 2022-03-25
---
<br>
<br>
**Concepts**
<br>
<br>
# 1. OSI 7 Layer
The Open Systems Interconnection (OSI) model is an abstract representation of how the Internet works. It contains 7 layers, with each layer representing a different category of networking functions.

![7Layer](/assets/images/OSI7layer.PNG)<br>

OSI 7 layer can devide to 2 layer. One is Data Flow layer, 1~4 layer. It has the role of transferring data well to the other party as the term implies. The other is Application layer, 5~7 layer. It is a layer that exists to provide useful services to users.<br>


## 1-1. physical layer
The first layer defines information related to physical connection as a physical layer. The focus is mainly on transmitting electrical signals (0 and 1). The main equipment of the first layer includes hub, repeater, cable, connector, transceiver, and tab.<br>

## 1-2. Datalink layer
The second layer collects electrical signals from the first layer and processes them in the form of data that we can recognize. In this layer, it check the departure and destination addresses, and also handles error correction from the physical layer. There are the Media Access Control (MAC) and the Logical Link Control (LLC) in this layer. In the networking world, most switches operate at Layer 2.<br>

## 1-3. Network layer
The third layer is wh<br>

## 1-4. Transport layer
<br>

## 1-5. physical layer
<br>

## 2-6. physical layer
<br>

# 2. Protocol
In networking, a protocol is a standardized way of doing certain actions and formatting data so that two or more devices are able to communicate with and understand each other.<br>

## 2-1. IP - Internet protocal
The Internet Protocol (IP) is a protocol, or set of rules, for routing and addressing packets of data so that they can travel across networks and arrive at the correct destination. Data traversing the Internet is divided into smaller pieces, called packets. IP information is attached to each packet, and this information helps routers to send packets to the right place.<br>

## 2-2. IP Packet
IP packets are created by adding an IP header to each packet of data before it is sent on its way. An IP header is just a series of bits (ones and zeros), and it records several pieces of information about the packet, including the sending and receiving IP address. <br>

## 2-3. IP routing




글 그림 출처
https://www.cloudflare.com/ko-kr/learning/network-layer/what-is-a-protocol/
