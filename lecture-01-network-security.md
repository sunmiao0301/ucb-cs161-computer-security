##### web security

从OSI网络模型中拿出关键的五层

- 7.Application. This is the human-readable content you want to send, such as the HTML of a webpage or the text of an email. The actual structure of this content depends on what exactly your application is.
- 4.Transport. This layer creates an end-to-end connection between your server and the destination server you want to communicate with. The two main options here are to use TCP, which guarantees that messages are sent in order, or UDP, which doesn’t. 
- 3.Network. This layer finds routes through the Internet in order to actually send messages. The IP address protocol is used here to give a global address to every location on the network. 
- 2.Link. This layer breaks down the routes in the network layer into individual hops between local subnetworks. It takes many hops at the link layer in order to route a message. 
- 1.Physical. This is the lowest layer, where individual bits are encoded with physical protocols such as voltage levels to send them over a link.

##### ARP Protocol

From unpacking the layer 3 (network) header, we have the global IP address of the destination. However, at the link layer, everything is addressed with local MAC addresses. Thus we need a way to translate global IP addresses into local MAC addresses. The protocol that does this is ARP (the Address Resolution Protocol) 主要是为了将全局IP转换为本地MAC地址的办法。

Say Alice wants to send a message to Bob, and knows Bob’s IP address is 1.1.1.1. The ARP protocol would follow three steps: 

1. Alice would broadcast to everyone else on the LAN “What is the MAC address of 1.1.1.1?”
2. Bob responds by sending a message only to Alice “My IP is 1.1.1.1 and my MAC address is ca:fe:f0:0d:be:ef.” 
3. Alice caches the IP address to MAC address mapping for Bob.

If Bob is outside of the LAN, then the gateway would make response in step 2 with its MAC address. Any received ARP replies are always cached, even if no broadcast request (step 1) was ever made. 但是这种基于询问的ARP协议容易受到中间人攻击man-in-the-middle

Modern wired Ethernet networks defend against ARP spoofing by using switches rather than hubs. Switches have a MAC cache, which keeps track of the IP address to MAC address pairings. If the packet’s IP address has a known MAC in the cache, the switch just sends it to the MAC. Otherwise, it broadcasts the packet to everyone. Smarter switches can filter requests so that not every request is broadcast to everyone. 现代网络基础设施选择用switch交换机来防御对ARP协议的攻击，switch会存储IP-MAC地址对并且广播给网络中的所有节点。

##### DHCP Protocol

DHCP (Dynamic Host Configuration Protocol) handles the setup when a computer first joins a network. 主要是为新加入网络的设备配置IP以及DNS

The DHCP handshake follows four steps, between you (the client) and the server (who can give you the needed IP addresses) 

1. Client Discover: The client broadcasts a request for a configuration.
1. Server Offer: Any server able to offer IP addresses responds with some configuration settings. (In practice, usually only one server replies here.) 
1. Client Request: The client broadcasts which configuration it has chosen. 
1. Server Acknowledge: The chosen server confirms that its configuration has been chosen

同样的，DHCP也会受到攻击。攻击者会伪装成新连入的节点，并发送广播说自己是新节点，IP是xxxxxx。

Defending against low-layer attacks like DHCP spoofing is hard, because we lack a trusted foundation to build upon when we’re first connecting to the network. 要抵御DHCP攻击比较困难，一般会借助网络架构中更高层来防御，比如对消息进行加密，网络中低层很难防御，因为当我们第一次加入网络的时候，we lack a trusted foundation to build upon.

##### IP

The biggest difference between v4 and v6 is the size of addresses and therefore the # of unique destinations available. For IPv4 the address is a 32b number, usually written as 4 integers between 0 and 255, such as 128.32.131.10. IPv6 instead supports 128b addresses, and they are generally written as 8, 4-byte hex values, such as cafe:f00d:d00d:1401:2414:1248:1281:8712. A single long run of 0 bytes in an IPv6 address can be replaced by two colons, so ::1 is really 0000:0000:0000:0000:0000:0000:0000:0001
IPv4和IPv6的区别，以及简单的例子

路由器（Router）通常根据目标 IP 地址的子网来决定如何转发数据包。这种按子网进行路由的方式有助于更高效地管理和组织 IP 地址空间。

有一些特殊的IP地址和网络块。其中，127.0.0/24 和 ::1 是“localhost”，用于在系统内创建与自己的系统的‘网络’连接。10/8、172.16/12 和 192.168/16 是私有的IPv4地址。它们不在互联网上进行路由，而是可以用于内部目的，如进行网络地址转换（NAT）。最后，255.255.255.255 是IPv4的广播地址，用于发送到本地网络内的所有计算机。

##### TCP = Transmission control protocol

TCP connections themselves are identified by a 5-tuple of (Client IP, Client Port, Server IP, Server Port, proto=TCP). A server listens for requests, usually on a set of “known ports”. Examples include port 22 (ssh), port 80 (http), port 443 (https). Ports below 1024 are “reserved” ports and only a program running as root can listen on those ports, but anyone can send to those ports. The client tends to use “ephemeral” ports, just the next available port or even just a random port. 

TCP连接通过五个值进行，客户端的IP和端口号，服务器的IP和端口号，以及协议名TCP。几个端口一般默认被用于常见的服务，如22ssh，80http，443https，1024号端口之下的一般是保留端口，只有以root权限运行的程序可以运行在这些端口上，但是发送信息到这些端口是不需要root权限的。

TCP建立连接是通过“三次握手”实现的。

TCP结束连接一般是通过”两次挥手“完成，但特殊情况下也可以通过RST包直接单方面结束。(p.s. TCP的RST复位也被用于 - TCP重置攻击.)

Notice that TCP by itself provides no confidentiality nor integrity guarantees. To prevent attacks like these, we look to TLS, which uses the cryptography you have learned in the last unit to provide a secure channel of communication. TCP本身不保证机密性和完整性，为了防御攻击，我们需要TLS。

##### UDP - user datagram protocol

与TCP相比，UDP不保证稳定性，UDP常用于需要考虑延迟的场景，比如与DNS这种很快的协议相配合时，或是视频游戏，语音通话等场景 (此时丢失几个包比一直等待好的多)

##### TLS



##### DNS

DNS用于翻译:人类可阅读的域名 --> 计算机可识别的ip地址.

举例而言,一次查询如下:

Every DNS query starts by asking one of the root servers: “Where is eecs.berkeley.edu?” Instead of answering directly, the root server will reply by redirecting you to the appropriate name server: “I don’t know, but you can ask the .edu name server, located at 192.5.6.30.” 注意此处给出的是.edu name server以及它的IP地址, 之所以要给出IP地址,就是避免查询人再次去请求.edu name server的位置.也就是说DNS name server给出回应的时候,必须附上反馈的域名的IP地址.

Since you don’t have an answer yet, your next step is to ask the .edu name server: “Where is eecs.berkeley.edu?” The reply will take you one level further down the tree: “I don’t know, but you can ask the berkeley.edu name server. located at 192.4.6.30.” 

You still don’t have an answer, so you ask the berkeley.edu name server: “Where is eecs.berkeley.edu?” Because you have reached the bottom of the tree, the berkeley.edu name server will respond: “eecs.berkeley.edu is located at 23.195.69.108,” completing the recursive DNS query. 

##### DNS安全

1.name server被劫持,返回给查询者危险的IP
2.name server中的cache被控制,返回给查询者危险的IP
3.卡明斯基攻击,基于查询到不存在的域的时候,由于不会查询成功,所以是没有DNS缓存的,这允许攻击者一直与合法回复race,直到成功,而不必等待缓存过期. -- 至于如何引诱被攻击者访问不存在的域名,This can be achieved by tricking the victim into visiting a website that tries to load lots of nonexistent domains:

![image-20240124100837147](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401241008198.png)

##### DNS污染

qiang除了DNS污染,还会进行IP污染,所以就算拿到了正确的IP也没用了.

##### Firewall

