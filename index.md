Request for Comments: 9297   April 2021

# **LAN Processing Power Share Protocol – LPPSP/1.0**

**Status of this Memo**

This document specifies the standard group protocol of the Internet community and needs discussion and suggestions for more perfection. This agreement is not restricted to circulate and publish.

**Abstract**

The LAN processing power share protocol is a distributed and collaborative system. It uses dynamic adjustment technology to coordinate the configuration of idle computing resources in the local area network to achieve the goal of long-term small processing power in exchange for short-term high processing power. Due to the risk of data transmission on the public network, this protocol is limited to the local area network, using TCP protocol to transmit data, and the data transmission process maintains end-to-end encryption.

**1 Introduction**

**1.1 Purpose**

Today, when Moore&#39;s Law is gradually failing, the growth of processing power purchased per unit of currency has slowed down, and it has begun to fail to meet the computing needs of users. This has made the call for multiple computers to cooperate in computing more and more. After years of development, distributed computing technology has become more mature and cloud computing technology has begun to be used commercially. However, we have neglected the computing resources owned by individuals. Although it is far from large computing platforms, it is in a community. In the intranet environment of China, gathering idle computing resources can have the effect of gathering sand into a tower.

At present, there have been some communities to conduct processing power trading, but the methods adopted are different, and the results are different. This is a great obstacle to the future popularization and implementation, which urgently needs a unified agreement for basic-level design.

**1.2 Requirements**

The key words &quot;MUST&quot;, &quot;MUST NOT&quot;, &quot;REQUIRED&quot;, &quot;SHALL&quot;, &quot;SHALL NOT&quot;, &quot;SHOULD&quot;, &quot;SHOULD NOT&quot;, &quot;RECOMMENDED&quot;, &quot;MAY&quot;, and &quot;OPTIONAL&quot; in this document are to be interpreted as described in RFC 2119.

An implementation is not compliant if it fails to satisfy one or more of the MUST or REQUIRED level requirements for the protocols it implements. An implementation that satisfies all the MUST or REQUIRED level and all the SHOULD level requirements for its protocols is said to be &quot;unconditionally compliant&quot;; one that satisfies all the MUST level requirements but not all the SHOULD level requirements for its protocols is said to be &quot;conditionally compliant.&quot;

**1.3 Terminology**

The following basic terms used in this agreement are used to indicate the respective roles and functions of the constituents of this computing network.

calculate node

The most basic member of the computing network, responsible for providing processing power

computing pool

Abstract virtualized computing node collection

client node

The most basic member of the computing network, publishes computing tasks

processing power

It is a unit measurement of processing power. In this agreement, 1 processing power=1MFLOPS

price

It is to provide the compensation that can be obtained per unit of time and unit of processing power, and it is also the price to be paid for requiring unit of time and unit of processing power

bounty

It is the pricing currency of a community network, bounty = unit processing power unit time market price \* processing power \* computing time

task queue

Data currently waiting to be calculated

task queue pressure

The total number of computing tasks in the computing pool divided by the total number of nodes

data path

Transport layer path through which data packets flow

raise hand beacon

Information to join the computing network, including the node&#39;s IP address, available processing power, processor architecture, and a Boolean value to indicate whether it is a computing node or a customer node. If it is a customer node, it should also include its own available reward amount

departure beacon

Information leaving the computing network

idle beacon

A computing node currently has no computing tasks

information book

Raise hand beacon and idle information of all nodes in the network recorded by each node

scheduler

A program that distributes data packets newly entered into the task queue according to rules

balancer

Procedures to measure the pressure of task queues and dynamically adjust public prices

**1.4 Overall Operation**

The local area network processing power protocol is a distributed protocol. Any computing node in the local area network runs a program that complies with the protocol. There is no node with the identity of the administrator to control the network data flow. After a new node joins the computing network, it broadcasts a hand-raise beacon to the entire network. At the same time, the node accepts the hand-raise beacon from other nodes in the network and caches the information book locally, which is ticked every 1 second.

When a node is a computing node, it must first broadcast idle beacons to the network, join the computing pool to accept the arrangements of other node schedulers in the network, accept computing tasks and work with its promised processing power, and finally send the computing results to the node indicated by the scheduler, the result is integrated by the node group and sent to the node that publishes the calculation task, and each node obtains its own bounty according to the algorithm. When the node exits the network, it broadcasts the departure beacon to the network, and each network node immediately clears the node&#39;s IP address in the information book.

When a node is a client node, it must apply for computing resources after broadcasting the raise-hand beacon. The scheduler will disassemble and configure the computing task according to the node&#39;s idle situation and calculate the price in the process. When the reward is insufficient When the scheduler sends a signal to stop the calculation.

The balancer is used to monitor the pressure of the task queue in the entire network. If there are too many computing tasks, it will broadcast a new market price modification proposal to the entire network. When all nodes in the market are confirmed and agreed by the algorithm, the new market price is generated and broadcast.

The balancer can use two modes for price adjustment, one is finite state machine, and the other is machine learning. This agreement stipulates that the adjustment of the finite state machine is necessary, and the adjustment of machine learning is optional. If the machine learning adjustment is selected, the finite state machine can be turned off.

a. Finite state machine When the balancer decides to modify the price, it will set the price as the current market price plus 1 for the first time and continue to monitor the task queue pressure. If the task queue pressure drops to 80% of the original state after 5 minutes, then Continue to increase by 1, if the task queue pressure drops to 60% of the original state after 5 minutes, continue to observe for 5 minutes, if the pressure drops to 50% of the previous state, stop the price increase, if it is greater than 50%, continue to increase the price by 1, state carry on.

b. Machine learning The community can choose another monitoring node to observe the elasticity of supply and demand, use machine learning to calculate the market price and broadcast it

**2 Protocol Parameters**

**2.1 LAN processing power share protocol header**

LPPSP = &quot;LPPSP&quot; &quot;/&quot; &quot;Current Price&quot;

The current price is the price obtained by the balancer through calculation that can balance the calculation task. This protocol header will be included in each information frame.

**2.2 Raise Hand Beacon**

Hand = &quot;Hand&quot; &quot;/&quot; &quot;IP&quot; &quot;/&quot; &quot;Processing\_Power&quot; &quot;/&quot; &quot;Architecture&quot; &quot;/&quot; &quot;Is\_Client&quot; &quot;/&quot; &quot;Remaining\_Bonty or NULL&quot;

**2.3 Departure Beacon**

Leave = &quot;Leave&quot; &quot;/&quot; &quot;IP&quot;

**2.4 Idle Beacon**

Idle = &quot;Idle&quot; &quot;/&quot; &quot;Is\_Idle&quot;

**2.5 Date and Time Format**

Implementation as defined in RFC 1123

**2.6 Character Set**

The definition of the term &quot;character set&quot; used in this agreement is the same as that described in MIME.

The term &quot;character set&quot; in this document refers to a method of converting an eight-byte sequence into a character sequence using one or more tables. Note that unconditional conversion in the other direction is not required. In this conversion, not all characters can be obtained in a given character set, and the character set may provide multiple octal sequences to represent a particular character. This definition will allow a variety of character encoding methods, from simple single table mapping such as US-ASCII to complex table exchange methods such as those used in ISO-2022 technology. However, the definition associated with the name of the MIME character set must fully explain the mapping from eight bytes to characters. In particular, it is not allowed to use external contour information to determine accurate mapping.

**2.7 Chunk Transfer Code**

chunked-Body=

chunk

chunk-size

last-chunk = \*chunk last-chunk

trailer

CRLF

= chunk-size [chunk-extension] CRLF

chunk-data CRLF

= 1\*HEX

= 1\*(&quot;0&quot;) [chunk-extension] CRLF

chunk-extension= \*( &quot;;&quot; chunk-ext-name [&quot;=&quot; chunk-ext-val] ) chunk-ext-name = token
 chunk-ext-val = token | quoted-string
 chunk-data = chunk-size(OCTET)

trailer = \*(entity-header CRLF)

**3 Compatibility**

This agreement is the initial version of the local area network processing power share agreement. Subsequent additions and deletions can maintain forward compatibility to maintain efficiency.

**4 Copyright Notice**

This document and its translation content can be copied and provided to others, and the document can be commented or explained in other ways. Derivative works can be prepared, copied, published and distributed in whole or in part, without any restrictions, premise All such copies and derivative works contain the above copyright notice and this paragraph. However, unless it is necessary for the development of Internet standards, this document itself may not be modified in any way, for example, to delete copyright notices and references.

The limited permissions granted above are permanent.
