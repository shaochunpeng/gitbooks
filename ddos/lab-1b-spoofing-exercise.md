# 🖥 Lab 1B: Spoofing Exercise

## Purpose

This lab is designed to let students do spoofing exercise. The spoofing game is where students are meant to spoof the IP addresses of the other students, detect if their IP address was spoofed, and who did the spoofing.

In this lab, you will:

* Learn common network tools such as Nmap and scapy.
* Understand methods used in spoofing and spoofing detection.
* Be able to decide proper data collection points on a network.
* Have a better understanding of low level network communication and the structure of a network packet.

## Instructions

* Connect to Security Lab machines.
* Discover all the active hosts in the subnet of the security lab.
* Sniffing using Wireshark/Tshark.
* Send out spoofed packets and capture them.

## Discuss Questions

1. How can you evade detection when spoofing others? Is it possible to ascertain the identity of the sender?&#x20;
2. Take a look at the RFC for the Internet Protocol, RFC791(https://www.ietf.org/rfc/rfc791.txt)\
   Explain what ip address spoofing is, and what a host on the network must do to spoof its ip address
3. Take a look at the RFC for the User Datagram Protocol, RFC768\
   [https://www.ietf.org/rfc/rfc7](https://www.ietf.org/rfc/rfc7) \
   And the RFC for the Transmission Control Protocol, RFC 793\
   [https://www.ietf.org/rfc/rfc793.t](https://www.ietf.org/rfc/rfc793.t) \
   Explain why an attacker cannot just grab any existing IP packet carrying UDP or TCP, change only the IP addresses in there, and expect the target host to accept the packet. Especially for TCP, you don’t have to read the entire RFC but focus on the header(pages 1519).

## Lab Report

Lab report should contain both Lab 1A and Lab 1B, due date given in Canvas.

Lab report should contain :

1. discussions of above questions.
2. Practice send normal packets and send spoofed packets.
3. Write capture/display filters for Wireshark/Tshark, try to intercept potential spoofing attempts.



## Guide

1.  configure ssh config like:

    Host spoof\
    &#x20; user root\
    &#x20; Hostname 192.168.10.125\
    &#x20; ProxyCommand ssh -W %h:%p eceddos\

2. Open 2 terminals\
   spoof and sniff
3.  In the terminal connected to the sniffer, open Wireshark(or Tshark). The interface to capture traffic is enp2s0. Start capture.&#x20;

    Then on the other terminal that is connected to node1 or node2, ping 192.168.30.2.&#x20;

    Reference: [https://www.google.com/search?q=wireshark](https://www.google.com/search?q=wireshark)



    (This step is the same as Lab1, you can just skip this if you know how to do such captures.)

    Then you should have captured these packets on sniffer. Terminate the capture.&#x20;


4.  In the terminal connected to the spoof machine, use nmap to discover all the active hosts in your subnet by using following command:\
    \>$ nmap -sn 192.168.10.0/24

    This will give you the MAC addresses and IP address for the hosts in your subnet.

    (Read the manual page for more options of this command.)
5. Open another terminal and connect to the spoof machine. Then run scapy command to open a scapy interactive session.
6.  In scapy, use:

    send(IP(dst=’192.168.30.x’,src=’192.168.10.y’)/’quick brown fox jumps over the lazy dog’)

    sendp(Ether(src=’12:34:56:78:9a:ab’)/IP(dst=’192.168.20.x’,src=’8.8.8.8’)/’quick brown fox jumps over the lazy cat’)

    This sends a DNS query with a spoofed source IP address. Remember to fill in x and y in IPs appropriately.
7. Spoof at least three ip addresses on your subnet. Send different payload messages for each destination IP you are trying to spoof.
8. Write filters in wireshark/Tshark to detect spoofing.(take screenshots)
9. Continuously sniff traffic on wireshark to try and intercept potential spoofing attempts.\
   Scapy: [https://scapy.readthedocs.io/en/latest/usage.html](https://scapy.readthedocs.io/en/latest/usage.html)
10. Finish lab report.(Pdf format ONLY)

    \


    \


    \
