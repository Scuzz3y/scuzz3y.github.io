---
title: "How to Secure P2P"
excerpt: "Blog Post for CSEC-472: Authentication and Security Models"
---

This research was done for the class "Authentication and Security Models" in Fall 2018.

# Authors
- [Brandon Adler](https://www.scuzz3y.space)
- [Russell Babarsky](https://www.russellmbabarsky.com)
- Kristen Tumacder
- Chase Van Duyne

# Research Questions
1. How can modern technology conquer the security pitfalls of P2P?
2. Are there trade-offs when securing P2P?
    - Are certain core features of P2P (decentralization anonymity, etc.) lost when security is implemented?
3. How can peer authentication and trust be secured?

<img src="/assets/images/how-to-secure-p2p/p2p-example.png" style="display:block;width:50%;margin-left:auto;margin-right:auto">

# Background
From [[6]](#camarillo), a system is considered to be P2P if “the elements that form the system share their resources in order to provide the service the system has been designed to provide. The elements in the system both provide services to other elements and request services from other elements [[6]](#camarillo).” The main two functions in all P2P systems are enrollment function and peer discovery function. The enrollment function manages node authentication and authorization while the peer discovery function “allows nodes to discover peers in the system in order to connect to them” [[6]](#camarillo) and to become a peer. 

# Current Methods
## Securing P2P
There are several types of attacks on P2P networks mainly because of the decentralized nature of resources as well as the lack of control. The weaknesses in P2P include, but are not limited to: lack of fixed or centralized server, query over-flooding, ineffectiveness of distributed hash tables (DHT), and ability for attackers to spoof peer devices [[12]](#balfe). The main feature of P2P networks is the decentralization nature; therefore, there can be abnormal peers and there is no mechanism that would correct this [[8]](#li).

To correct these weaknesses, several papers propose different solutions. The first solution is to encrypt P2P traffic. By encrypting P2P traffic, the traffic is harder to be “detected, … attacked, blocked, or throttled”. An example of this solution is in BitTorrent, where the P2P traffic is encrypted so that the data stream is obfuscated without diminishing performance [[8]](#li).

Another solution is to anonymize peers. “An anonymous P2P provides enough anonymity such that it is extremely difficult to find the source or destination of a data stream” [[8]](#li). Using anonymization and encryption would be the “most secure P2P usage experience available today” [[8]](#li).

A final solution would be the pricing mechanism discussed in [[4]](#rice). This model “promises to ‘grow’ P2P networks that are more resistant to virus and worm propagation by integrating recent developments in complex network theory with incentive compatible pricing of network links” [[4]](#rice). The main idea of this model is to increase the decision-making power for nodes so that they can carefully choose which nodes can be linked to the network.

## Authentication and Trust
This section specifically focuses on current authentication and trust mechanisms that can be implemented in P2P networks. One solution is based on the use of an efficient distributed key agreement protocol [[9]](#vijayakumar).  A distributed group key agreement allows each node in a P2P network to generate and distribute an encryption key to communicate securely with other nodes.  Using the Chinese Remainder Theorem, an extension of the RSA cryptosystem, there is much less overhead in computational resources for group key operations when comparing it to similar solutions.  Each node must first generate a system of congruences to find a common group key.  Finding a common group key is only done once in the initial setup phase which is the primary difference from other protocols.  A Sponsor User then provides the public parameters to the key generation functions for each node.  Once the key generation is complete, the group encryption key is derived which is used to encrypt and decrypt data from other nodes.

Current methods of trust come in ways of evaluating hosts based on transaction history.  Some of the current trust models are PeerTrust, EigenTrust, Reco-Trust, SFTrust, UserTrust, MASTrust, and FCTrust [[2]](#das).  These trust models have certain criteria of trustworthiness in a host and come up with mathematical ways to evaluate the specified criteria.  Some the the criteria seen in other works are based on transaction history, peer recommendations, and time decay [2,13].  These criteria can be very specific to the nature of the the P2P network, or they can be universal and work for various types of P2P networks.  Overall, the most difficult thing about trust models that is the metrics are known by a malicious party then a dynamic malicious host can alter its trust metrics.

## Public Key Infrastructure
Implementation of PKI-based functions can also be used to mitigate the pitfalls of P2P. As stated in the Introduction, P2P systems have two functions for peers: enrollment function and peer discovery function. A PKI-based concept that can be applied to these two functions is implementing a Kerberos-like authentication mechanism or a Super Peer concept [[3]](#oh). Although both the Kerberos-like and Super Peer mechanisms may be useful, there is a downfall: these mechanisms can be exploited. As soon as a malicious peer is authenticated, it can report legitimate peers while allowing for the authentication of other malicious peers.

There are other PKI implementations that serve similar purposes but each have their own set of pros and cons. One common implementation is known as Chord-PKI, which focuses on removing external PKI systems due to their inefficiency [[1]](#avramidis). Instead, the Chord-PKI system has three distinct properties: scalability through a hash table to keep track of the participants, efficient method of storing information about the certificates themselves, and resilience to compromised nodes on the network. These goals are achieved through means of having certificate nodes serving specific areas. This way, if a certificate node is compromised, only that area is impacted and the rest of the network can function normally. This also reduces the amount of work the certificate nodes will have to do as they will not have to serve the entire network, just the one area, thus increasing efficiency.

Another common implementation is issuance of peers’ own X.509 certificate [[4]](#rice). This means that each peer will issue its own X.509 and will also be assigned certificates by an external Certificate Authority. The certificates can be given special attributes for each peer such as a “Common Name” for other peers to validate against and attributes to determine a peer’s role.

# Proposed Method
## General Security Solutions for P2P Networks
First, we propose to encrypt the P2P traffic. To do this, several key authority servers/peers need to be established and designated. In the configuration file for these servers, the following will be stated: the algorithm used to generate the symmetric key, a list of the key authority servers/peers, and the algorithm for the key exchange. By default, AES-128 will be used to generate the symmetric key and Diffie-Hellman will be used to distribute the symmetric key.  

For key management and for the host to prove their authenticity, public key management will be used since it is the standard for key management technology. PKI allows us to manage certificates in a way where even if a machine’s certificate is compromised, we can easily revoke the certificate.  This forces clients on our network to deny any communication signed by the revoked certificate.

The main challenges with implementing PKI into a P2P network are distributing keys and maintaining efficient communication between hosts on the network. With the nature of P2P networks, it can be hard to distribute keys to every node of the network since there is no central server. One possible solution is to have an external PKI server that serves all of the nodes in the network. While this lacks in efficiency, it provides simplicity as there is only a single server to setup and the implementation is straight forward. 

## Trust Mechanisms
In order to establish trust on the P2P network, peers will supply feedback to the various super peers on the network.  This feedback will be used to calculate trust metrics assigned to each of the peers.  The trust metric will be a numeric value which will represent the trustworthiness of a peer.  In order to calculate this metric, multiple sub-metrics must be evaluated. These sub-metrics are: Satisfaction, Direct Trust, Indirect Trust, Global Trust, Feedback credibility, Decay Reliability, and Final trust.  This trust model is based of the work of Das, Islam and Sorwar [[2]](#das).

## Satisfaction
Satisfaction is determined by the total transaction history of an agent and the satisfaction levels of the peers that the agent has interacted with.  Instead of recording all of the transactions of all of the agents within the network, satisfaction will be determined over time and re-calculated after each transaction per an agent.  Agents who complete tasks as expected will have an increase in their satisfaction metric.  For malicious agents, who do tasks incorrectly or unsatisfactory, their satisfaction levels will significantly decrease.

## Direct Trust
Direct trust is determined by the interactions that one agent has with a specific peer. Each peer would start as neither trustworthy or untrustworthy; this value depends on the scale that is being used. For example, if the scale is 1 to 10, where 1 is untrustworthy and 10 is trustworthy, the agent would initially start at 5.  As the agent connects with the same peer multiple times, this direct trust value will change based on the satisfaction between these two peers.  This allows for individual agents to evaluate peers which they have previously connected to.  If a malicious peer were to continuously try to exploit agents, the malicious peer’s direct trust would decrease. If the direct trust value between two specific peers is too low then one of the peers can deny the connection to prevent abuse.

## Indirect Trust
Indirect trust is based on transactions of an agent and other peers.  Indirect trust is calculated by the recommendations or direct trust from other agents.  The indirect trust is used when direct trust cannot be used.  One example of this is if two peers have never connected before, they can use the indirect trust metric to determine their connection.  This allows for peers to evaluate and decide if they should connect to a peer even if they have not established a direct trust metric.  Trustworthy peers will develop good indirect trust by completing satisfactory transactions with many other peers.  

## Global Trust
Global trust is a combination of direct and indirect trust.  In global trust, direct trust is more heavily weighted as the transaction history increases between peers and they become more confident with the tasks they perform.  This global trust metric allows for peers to compare both their and other’s experiences with a peer.  If a peer has a very low indirect trust metric but a high direct trust metric, the global trust metric will reflect this poor satisfaction rates of others by decreasing global trust, albeit direct trust is high.

## Feedback Credibility
Feedback credibility is a metric of combined feedback from all of the hosts that an agent has encountered while in the P2P network.  This metric is computed based on the recommendations of previous transactions of a host.  This metric would serve as an overall recommendation as to whether to trust a host based on transaction history and is updated over time.

## Decay Model
The decay model metric shows that as a significant amount of time passes by then the trust of the absent peer will decrease.  This is in place to counter dynamic malicious peers.  Dynamic malicious peers will slow their transaction to hide if they believe they are close to detection. Therefore, if a peer does not make any transactions for a period of time, the peer could be suspect for malicious activity.  Also it is hard to determine if the peer was able to maintain the trust level during the time of no transactions. Due to this, a significant gap between transaction is considered a negative and will result in a decline in trust relative to time.  In addition to this, if a low transaction host suddenly starts making many significantly more transactions it will also be suspect for malicious activity, because the peer may have been compromised and is acting unusual.  In summary, the decay metric measures the time between transactions and based on transaction history determines if a peer is trustworthy.

## Deviation Reliability
Deviation reliability is the the range of trust metrics we are willing to accept and connect to.  Having the deviation reliability allows a metric to counter dynamic hosts which raises or lowers trust scores in order to appear trustworthy.  This metric allows us to track these changes in order to see how a hosts trust changes over time and if it is with in our acceptable trust values.

## Final Trust
Using the expected trust and confidence values of a host we can calculate a final trust value which will be deterministic of whether a host is trustworthy overall.  This final trust metric wil take the other trust metrics into account, assuming that the values of the other metrics are valid. Overall, the final trust value should be within an acceptable range and will limit the number of malicious hosts that fabricate their trust levels.

# Results
From current literature, time decay models, chord PKI and similar methods seem to be the most beneficial in securing P2P networks. The time decay model, helps fight two vulnerabilities in P2P networks.  These vulnerabilities are dynamic hosts and shilling attacks [[9]](#vijayakumar). Dynamic hosts will change the way they attack if they think that they have been discovered.  These malicious hosts are often troublesome because they change in ways that some trust models do not anticipate.  The shilling attack is where malicious hosts make false transactions in order to increase the trust values of malicious hosts while decreasing the trust values of non-malicious hosts. Though these attacks exist, they can be countered with carefully crafted trust models.

## Answers to the Research Questions
Regarding the first research question, from all of the source summaries that we have done, we discovered that current modern technologies are simply “good enough” to secure P2P networks. As stated in the previous section, the time decay and chord PKI models seem to be the most beneficial in securing P2P networks. Despite all of our research over the semester, we could not find a general technology, standard or framework for securing P2P; therefore, this could be future work for this field.

For the second research question, there are indeed several trade-offs when securing P2P networks. From [[6]](#camarillo), a P2P network can still be considered P2P if it has few centralized peers, i.e. servers; however, having these centralized servers would possibly decrease the anonymity aspect between peers.

For the final research question, we found that a combination of the Super Peer [[3]](#oh) and feedback [[2]](#das) models would work best in securing authentication and trust mechanisms in P2P networks. Our model would be as followed: (1) several super peers would be designated then (2) peers would report feedback to each other and their designated super peer based on the criteria listed in the Trust Mechanisms section in Proposed Method. 

# Future Work
## For Current Literature
Methods which allow trust values to be assigned to hosts to determine if a transaction should be made, similar to a time-decay model, could help secure P2P by enabling hosts to make better decisions with whom they connect to. Furthermore, continued research in adapting PKI to work with the desired properties of P2P networks may prove to be another method of which to secure P2P networks.

## For Our Proposed Solution
The methodology stated in the Proposed Method section is purely theoretical. For future work, code, test cases and simulations can be written to see if the methods are feasible and if they actually work well when used in different applications and/or situations. If tests go well, we would modify the code so that it can be scalable for large P2P networks and applications.

### Futher Development of Trust Model
Based on the articles we have read, there are many ideas on how to evaluate hosts and create working trust models.  Many of these trust models involve mathematical calculations to create trust metrics.  In the future we plan on doing research to numerically evaluate hosts and to create boundaries in order to detect malicious hosts. This would involve more research into the specific ways malicious host bypass other trust metrics in order to stop these same flaws in our trust model. Finally, we could incorporate AI and deep learning techniques to automate malicious host detection using our trust model.

### Code and Simulation
After the development of the trust model and the proper authentication mechanisms, we would like to make to secure P2P network and would start development on a simulation.  The simulation would be a program that would implement the trust model equations and algorithm to evaluate the effectiveness of our model.  This simulation could then be compared to other trust models to find its effectiveness and identify malicious hosts within a P2P network.

# Conclusion
P2P is said to be inspired by Napster and the concept of peers and decentralization has grown into what it is today. Albeit at its most basic form P2P is naturally insecure, it can still be developed and made to be more secure and more up to date with current security standards and practices. Using modern security techniques, PKI can be more practical for everyday use. PKI would allow for a hierarchy of trust and a way for hosts to validate traffic in the network that they are communicating within, while trust models allow hosts to make smarter decisions on their own from both past experiences and from information that is fed to them. Torrent tools, video games, and chat platforms show us that with the correct security in place, P2P can be used to develop great platforms that provide a variety of uses and functionality. With goals that were described in our future work, it may be possible that communication could be standardized to be performed solely through P2P.

# References
1. <a style="text-decoration:none" name="avramidis">A. Avramidis, P. Kotzanikolaou, C. Douligeris and M. Burmester, “Chord-PKI: A distributed trust infrastructure based on P2P networks,” Comput. Netw., vol. 56, no. 1, pp. 378–398, 2012</a>
2. <a style="text-decoration:none" name="das">A. Das, M. M. Islam and G. Sorwar, "Dynamic trust model for reliable transactions in multi-agent systems," 13th International Conference on Advanced Communication Technology (ICACT2011), Seoul, 2011, pp. 1101-1106 from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=5746000&isnumber=5745722></a>
3. <a style="text-decoration:none" name="oh">B.-T. Oh, S.-B. Lee, and H.-J. Park, “A Peer Mutual Authentication Method using PKI on Super Peer based Peer-to-Peer Systems,” 2008 10th International Conference on Advanced Communication Technology, 2008.</a>
4. <a style="text-decoration:none" name="rice">D. O. Rice, "A Proposal for the Security of Peer-to-Peer (P2P) Networks: a pricing model inspired by the theory of complex networks," 2007 41st Annual Conference on Information Sciences and Systems, Baltimore, MD, 2007, pp. 812-813, from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=4298420&isnumber=4298257></a>
5. <a style="text-decoration:none" name="franklin">Franklin, M. (2004). PKI for P2P Authentication Without All the CA Hassles: A Proposal by the Dartmouth PKI Lab (Tech.). Hanover, New Hampshire. <http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=990434&isnumber=21356></a>
6. <a style="text-decoration:none" name="camarillo">G. Camarillo, Ed., IAB. “Peer-to-Peer (P2P) Architecture: Definition, Taxonomies, Examples, and Applicability,” November, 2009.</a>
7.  <a style="text-decoration:none" name="chen">J. Chen, X. Guo and Z. Li, "Research on trust model in the mobile P2P network," 2017 4th International Conference on Information, Cybernetics and Computational Social Systems (ICCSS), Dalian, 2017, pp. 528-532. doi: 10.1109/ICCSS.2017.8091472 from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=8091472&isnumber=8091367></a>
8. <a style="text-decoration:none" name="li">Li, J. (2007, December). A Survey of Peer-to-Peer Network Security Issues. Retrieved September 15, 2018, from <https://www.cse.wustl.edu/~jain/cse571-07/ftp/p2p/></a>
9. <a style="text-decoration:none" name="vijayakumar">P. Vijayakumar, R. Naresh, L. J. Deborah, and S. H. Islam, “An efficient group key agreement protocol for secure P2P communication,” Security and Communication Networks, vol. 9, no. 17, pp. 3952–3965, 2016.</a>
10. <a style="text-decoration:none" name="p2p-diagram">Picture: <https://en.wikipedia.org/wiki/Peer-to-peer#/media/File:Unstructured_peer-to-peer_network_diagram.png></a>
11. <a style="text-decoration:none" name="schollmeier">R. Schollmeier, "A definition of peer-to-peer networking for the classification of peer-to-peer architectures and applications," Proceedings First International Conference on Peer-to-Peer Computing, Linkoping, Sweden, 2001, pp. 101-102.</a>
12. <a style="text-decoration:none" name="balfe">S. Balfe, A. D. Lakhani and K. G. Paterson, "Trusted computing: providing security for peer-to-peer networks," Fifth IEEE International Conference on Peer-to-Peer Computing (P2P'05), Konstanz, 2005, pp. 117-124, from <http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=1551027&isnumber=33049></a>
13. <a style="text-decoration:none" name="zhang">Y. Zhang, K. Wang, K. Li, W. Qu and Y. Xiang, "A Time-decay Based P2P Trust Model," 2009 International Conference on Networks Security, Wireless Communications and Trusted Computing, Wuhan, Hubei, 2009, pp. 235-238, from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=4908448&isnumber=4908381></a>
14. <a style="text-decoration:none" name="jiang">Z. Jiang and R. Xu, "A P2P Network Authentication Method Based on CPK," 2009 Second International Symposium on Electronic Commerce and Security, Nanchang, 2009, pp. 3-6., from <http://ieeexplore.ieee.org.ezproxy.rit.edu/stamp/stamp.jsp?tp=&arnumber=5209775&isnumber=5209640></a>