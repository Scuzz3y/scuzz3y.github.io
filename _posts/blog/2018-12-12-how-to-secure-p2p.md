---
title: "How to Secure P2P"
excerpt: "Blog Post for CSEC-472: Authentication and Security Models"
---

# Authors
- [Brandon Adler](https://www.scuzz3y.space)
- [Russell Babarsky](https://www.russellmbabarsky.com)
- Kristen Tumacder
- Chase Van Duyne

# Research Questions
- How can today’s various technologies such as, but not limited to, cryptography, time decay models, and PKI be used in securing P2P networks while maintaining the characteristics of P2P
  - Such as decentralization, anonymity, and anarchy trust systems?
- What protocols today can be implemented to achieve this effect with P2P networks?

<img src="/assets/images/how-to-secure-p2p/p2p-example.png" style="display:block;width:50%;margin-left:auto;margin-right:auto">


# Background
From [RFC 5694](https://tools.ietf.org/html/rfc5694), a system is considered to be P2P if “the elements that form the system share their resources in order to provide the service the system has been designed to provide.  The elements in the system both provide services to other elements and request services from other elements.” The main two functions in all P2P systems are enrollment function and peer discovery function. The enrollment function manages node authentication and authorization while the peer discovery function “allows nodes to discover peers in the system in order to connect to them” (RFC 5694) and to become a peer.

# Current Methods
## Securing P2P
There are several types of attacks on P2P networks mainly because of the decentralization nature of resources and because the lack of control. The weaknesses in P2P include, but are not limited to, lack of fixed or centralized server [[5]](#5), query over-flooding [[5]](#5), ineffectiveness of distributed hash tables (DHT) [[5]](#5), and ability for attackers to spoof peer devices [[8]](#8). The main feature of P2P networks is the decentralization nature; because of this, there can be abnormal peers and there is no mechanism that would correct this. 

To correct these weaknesses, several papers propose different solutions. The first solution is to encrypt P2P traffic [[5]](#5). By encrypting P2P traffic, the traffic is harder to be “detected, … attacked, blocked, or throttled” [[5]](#5). An example of this solution is in BitTorrent, where the P2P traffic is encrypted so that the data stream is obfuscated without diminishing performance. 

Another solution is to anonymize peers [[5]](#5). “An anonymous P2P provides enough anonymity such that it is extremely difficult to find the source or destination of a data stream” [[5]](#5). Using anonymization and encryption would be the “most secure P2P usage experience available today” [[5]](#5). 

A final solution would be the pricing mechanism discussed in [[3]](#3). This model “promises to ‘grow’ P2P networks that are more resistant to virus and worm propagation by integrating recent developments in complex network theory with incentive compatible pricing of network links” [[3]](#3). Basically, the main idea of this model is to increase the decision-making power for nodes so that they can carefully choose which nodes can be linked to the network. 

## Authentication and Trust
### TODO - Not done in paper yet

## Public Key Infrastructure
Implementation of PKI-based functions can also be used to mitigate the pitfalls of P2P. As stated in the Introduction, P2P systems have two functions for peers: enrollment function and peer discovery function. A PKI-based concept that can be applied to these two functions is implementing a Kerberos-like authentication mechanism or a Super Peer concept [[2]](#2). Although both the Kerberos-like and Super Peer mechanisms may be useful, there is a downfall: these mechanisms can be exploited. As soon as a malicious peer is authenticated it can report legit peers while allowing for the authentication of other malicious peers.

Other PKI-based implementations are:
- Chord-PKI [[1]](#1), where there will be multiple steps used to certify and validate a peer.
- Issuance of peers’ own X.509 certificate [[4]](#4). Each peer will issue its own X.509 and will also be assigned certificates by an external Certificate Authority. The certificates can be given special attributes for each peer such as a “Common Name” for other peers to validate against and attributes to determine a peer’s role.

# Results - TODO: Should probably be renamed
## From Current Literature
Time decay models, which are used to evaluate a peer trust value, have been shown through simulations to be able to detect malicious peers. Continued research may yield promising results and more efficient detection mechanisms, but for current times the time decay model proposed in [#] works well. 

Chord PKI, if designed for P2P, may prove beneficial to aid with the authentication process of peers within the P2P network. (May lead to PKI development for P2P if not designed for P2P, but can be modified for P2P)

## Our Answers to the Research Questions
Regarding the first research question, from all of the source summaries that we have done, we discovered that current modern technologies are simply good enough to secure P2P networks. As stated in the previous section, the time decay and chord PKI models seem to be the most beneficial in securing P2P networks. We could not find a general technology, standard or framework for securing P2P; therefore, this could be future work. 

For the second research question, there are indeed several trade-offs when securing P2P networks. From RFC 5694, a P2P network can still be considered P2P if it has few centralized peers, i.e. servers; however, having these centralized servers would possibly decrease the anonymity aspect between peers. 

# Proposed Method
## General Security Solutions for P2P Networks
- TODO: Section NOT FINISHED
First, we propose to encrypt the P2P traffic. To do this, several key authority servers/peers need to be established and designated. In the configuration file for these servers, the following will be stated: the algorithm used to generate the symmetric key, a list of the key authority servers/peers, and the algorithm for the key exchange. By default, AES-128 will be used to generate the symmetric key and Diffie-Hellman will be used to distribute the symmetric key.  

For key management and for the host to prove their authenticity, public key management will be used as it is the standard for key management technology. PKI allows us to manage certificates in such a way, that even if a machine is compromised and the certification is also compromised, we can easily revoke the certification, which forces clients on our network to not accept traffic or communicate with it in any way. 

The main challenges with implementing PKI into a P2P network are distributing keys and ...

## Trust Mechanisms
In order to establish trust on the P2P network peers will supply feedback to the various super peers on the network.  This feedback will be used to calculate trust metrics assigned to each of the peers.  The trust metric will be a numeric value which will represent the trustworthiness of a peer.  In order to calculate this metric multiple sub-metrics must been evaluated. These sub-metrics are: Satisfaction, Direct Trust, Indirect Trust, Global Trust, Feedback credibility, Decay Reliability, and Final trust.  This trust model is based of the work of Das, Islam and Sorwar [[12]](#12).

## Satisfaction
Satisfaction is determined by total transaction history of a agent and the satisfaction levels of the peers that the agent has interacted with.  Instead of recording  all of the transactions of all of the agents within the network, satisfaction will be determined overtime and re-calculated after each transaction per an agent.  Agents who complete tasks as expected will have an increase in their satisfaction metric.  For malicious agents, who do tasks incorrectly or unsatisfactory will have a significant reduction in their satisfaction levels. 

## Direct Trust
Direct trust is determined by the interactions that one agent has with a specific peer.  This number would start as neither trustworthy or untrustworthy.  As the agent connects with the same peer multiple time this direct trust value with change based on the satisfaction between these two peers.  This allows for individual agents to evaluate peers which they have previously connected to.  If a malicious peer were to continuously try to exploit the agent their direct trust would decrease. If the direct trust value between two specific peers then one of the peers can deny the connection to prevent abuse.

## Indirect Trust
Indirect trust is the based on transactions of an agent and other peers.  Indirect trust is calculated by the recommendations, or direct trust, from other agents.  The indirect trust is used when direct trust can not be used.  One example of this is if two peers have never connected before, then they would use the indirect trust metric to determine their connection.  This allows for peers evaluate and decide if they should connect to a peer even if they have not established a direct trust metric.  Trustworthy peers with develop good indirect trust by completing satisfactory transactions with many other peers. 

## Global Trust
Global Trust is a combination of direct and indirect trust.  In global trust, direct trust is more heavily weighted as the transaction history increases between peers and they become more confident with the tasks they perform.  This global trust metric allows for peers to compare both their and other’s experiences with a peer.  If a peer has a very low indirect trust metric, however, has a high direct trust metric, the global trust metric will reflect this poor satisfaction rates of others by decreasing global trust, even though direct trust is high.

## Feedback Credibility

## Decay Model
The decay model metric shows that as a significant amount of time passed by then the trust of the absent peer will decrease.  This is in place to counter dynamic malicious peers.  Dynamic malicious peers will slow their transaction to hide if they believe they are close to detection. Therefore, if a peer doesn’t make any transactions for a period of time it could be suspect for malicious activity.  Also it is hard to determine if the peer was able to maintain the trust level during the time of no transactions. Due to this, a significant gap between transaction is considered a negative and will result in a decline in trust relative to time.  In addition to this if a low transaction host suddenly starts making many significantly more transactions it will also be suspect for malicious activity, because the peer may have been compromises and it acting unusual.  In summary, the time decay metric measures the time between transactions and based on transaction history determines if a peer is trustworthy.

## Deviation Reliability

## Final Trust

# Future Work
## For Current Literature - TODO: Will be updated in paper
From current literature, time decay models, chord PKI and similar methods seem to be the most beneficial in securing P2P networks. In the time decay model, < Chase help pls > 

Methods which allow trust values to be assigned to hosts to determine if a transaction should be made, similar to a time-decay model, could help secure P2P by enabling hosts to make better decisions with whom they connect to. Also continued research in adapting PKI to work with the desired properties of P2P networks may prove to be another method of which to secure P2P networks.

## For Our Proposed Solution
The methodology stated in the Proposed Method section is purely theoretical. For future work, code, test cases and simulations can be written to see if the methods are feasible and if they actually work well when used in different applications and/or situations. If tests go well, we would modify the code so that it can be scalable for large P2P networks.

### Futher Development of Trust Model
- Math and numerical calculation for simulation

### Code and Simulation - TODO: Will be updated in paper
- Simulation effectiveness of trust model and authentication/ security mechanisms for P2P networks

# Conclusion - TODO: Might be updated before submission
P2P is said to be inspired by Napster (1999) and the concept of peers and decentralization has grown into what it is today. Even though at its most basic form P2P is naturally insecure, it can still be developed and made to be more secure and more up to date with current security standards and practices. Using modern security techniques such as PKI and trust mechanisms, makes PKI more practical for everyday use. PKI would allow for a hierarchy of trust and a way for hosts to validate traffic in the network that they are communicating within, while trust models allow hosts to make smarter decisions on their own from both past experiences and from information that is fed to them. Torrent tools, video games, and chat platforms show us that with the correct security in place, P2P can be used to develop great platforms that provide a variety of uses and functionality. With goals that were described in our future work, it may be possible that communication could be standardized to be performed solely through P2P.


# References
1. <a style="text-decoration:none" name="1">A. Avramidis , P. Kotzanikolaou, C. Douligeris and M. Burmester, “Chord-PKI: A distributed trust infrastructure based on P2P networks,” Comput. Netw., vol. 56, no. 1, pp. 378–398, 2012</a>
2. <a style="text-decoration:none" name="2">B.-T. Oh, S.-B. Lee, and H.-J. Park, “A Peer Mutual Authentication Method using PKI on Super Peer based Peer-to-Peer Systems,” 2008 10th International Conference on Advanced Communication Technology, 2008.</a>
3. <a style="text-decoration:none" name="3">D. O. Rice, "A Proposal for the Security of Peer-to-Peer (P2P) Networks: a pricing model inspired by the theory of complex networks," 2007 41st Annual Conference on Information Sciences and Systems, Baltimore, MD, 2007, pp. 812-813, from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=4298420&isnumber=4298257></a>
4. <a style="text-decoration:none" name="4">Franklin, M. (2004). PKI for P2P Authentication Without All the CA Hassles: A Proposal by the Dartmouth PKI Lab (Tech.). Hanover, New Hampshire. <http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=990434&isnumber=21356></a>
5. <a style="text-decoration:none" name="5">Li, J. (2007, December). A Survey of Peer-to-Peer Network Security Issues. Retrieved September 15, 2018, from <https://www.cse.wustl.edu/~jain/cse571-07/ftp/p2p/></a>
6. <a style="text-decoration:none" name="6">P. Vijayakumar, R. Naresh, L. J. Deborah, and S. H. Islam, “An efficient group key agreement protocol for secure P2P communication,” Security and Communication Networks, vol. 9, no. 17, pp. 3952–3965, 2016.</a>
7. <a style="text-decoration:none" name="7">R. Schollmeier, "A definition of peer-to-peer networking for the classification of peer-to-peer architectures and applications," Proceedings First International Conference on Peer-to-Peer Computing, Linkoping, Sweden, 2001, pp. 101-102.</a>
8. <a style="text-decoration:none" name="8">S. Balfe, A. D. Lakhani and K. G. Paterson, "Trusted computing: providing security for peer-to-peer networks," Fifth IEEE International Conference on Peer-to-Peer Computing (P2P'05), Konstanz, 2005, pp. 117-124, from <http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=1551027&isnumber=33049></a>
9. <a style="text-decoration:none" name="9">Y. Zhang, K. Wang, K. Li, W. Qu and Y. Xiang, "A Time-decay Based P2P Trust Model," 2009 International Conference on Networks Security, Wireless Communications and Trusted Computing, Wuhan, Hubei, 2009, pp. 235-238, from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=4908448&isnumber=4908381></a>
10. <a style="text-decoration:none" name="10">Z. Jiang and R. Xu, "A P2P Network Authentication Method Based on CPK," 2009 Second International Symposium on Electronic Commerce and Security, Nanchang, 2009, pp. 3-6., from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=5209775&isnumber=5209640></a>
11.  <a style="text-decoration:none" name="11">Picture: <https://en.wikipedia.org/wiki/Peer-to-peer#/media/File:Unstructured_peer-to-peer_network_diagram.png></a>
12.  <a style="text-decoration:none" name="12">A. Das, M. M. Islam and G. Sorwar, "Dynamic trust model for reliable transactions in multi-agent systems," 13th International Conference on Advanced Communication Technology (ICACT2011), Seoul, 2011, pp. 1101-1106 from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=5746000&isnumber=5745722></a>
13.  <a style="text-decoration:none" name="13">J. Chen, X. Guo and Z. Li, "Research on trust model in the mobile P2P network," 2017 4th International Conference on Information, Cybernetics and Computational Social Systems (ICCSS), Dalian, 2017, pp. 528-532. doi: 10.1109/ICCSS.2017.8091472 from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=8091472&isnumber=8091367></a>