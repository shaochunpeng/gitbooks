# 🖥 Lab 1A: Traffic Sniffing

## Purpose

Data collection from a network is an important step in network traffic analysis. Depending on the volume of traffic, it might be a challenging task. One should be able to collect only desired packets and choose proper places on a network to perform an efficient data collection.

In this lab, you will:

* Learn common tools used to collect network packets.
* Understand challenges in packet capturing and learn how to use capture and display filters.
* Be able to decide proper data collection points on a network.
* Have a better understanding of low level network communication and the structure of a network packet.

## Instructions

* Setup networks listed in the next page.
* Generate traffic between Node1 and Node2.
* Collect and save packets from the marked places using Wireshark/Tshark.
* Constrain your data collection/presentation for a specific packet type(IP address, protocol type, etc.) using proper capture/display filters.

## Network Setup

Sniffing experiment using router. Red dots are the sniffing points.

(Network already set up for use at the Security Lab located in Riggs Hall.)

<figure><img src="https://lh7-us.googleusercontent.com/_YB0xtHyCQrLBS9j5WOTBveQ2PZSBNF-41ogg_dNHkhsqVAcVPk2EKV6J4GQDu0BoAKtgZ-hfOCqG0YqdPqTfi61WuFrHcrMqhXVe-utruP2dABWWU3k6nOoe_DxmKX6IYoLCE9d7Qj1ErmekMtXqw" alt=""><figcaption></figcaption></figure>

## Questions:

1. Describe the layers and fields of a packet captured from your network.
2. Explain the characteristics and functioning of a hub, switch and router in network science.
3. Why is the use of Software Defined Network(SDN) as a tool necessary for DDoS?

## Lab Report

1. Discuss the questions above.
2. Write capture/display filters for Wireshark/Tshark to collect packets coming from/going to the selected host on the network.&#x20;
3. Write capture/display filters for Wireshark/Tshark to collect packets with specific protocol type on the network. (For example: collect only TCP packets)
4. Generate some traffic on the network. Try to collect all packets on the network group using tcpdump. Investigate the tcpdump report after terminating the capture. Did you drop any packets? If yes, explain why.\
   (transferring large files can generate the necessary traffic between the nodes.)
5. Plot time-series graph for captured packets which is a graph of number of packets vs time to demonstrate your data. (You may use the scripts from the provided zip file.)
6. Due date given on Canvas (syllabus).

## Guide:

Step by step guide for this lab.

1. (Optional - for remote access use )\
   Open _**TWO**_ terminals, connect to node1/2 and sniffer respectively. \
   `$ssh node1` \
   `$ssh sniffer`
2. In the terminal connected to the sniffer, open Wireshark(or Tshark). The interface to capture traffic is enp2s0. Start capture. \
   Then on the other terminal that is connected to node1 or node2, ping 192.168.30.2 \
   Then you should have captured these packets on the sniffer. Terminate the capture. \
   Reference: [https://www.google.com/search?q=wireshark](https://www.google.com/search?q=wireshark)
3. Write filters to show the packets you want to capture. Take screenshots or results.
4. Transfer a 50MB\~200MB file from one node to the other.  Capture the result. Save the result to file. And download the pcap file to your pc. \
   ( scp sniffer:/home/your\_user\_name/your\_pcap\_file \~/)\
   \
   Reference: \
   To generate a 100Mb file we would do:\
   `dd if=/dev/urandom of=file.txt bs=1048576 count=100`\
   [https://skorks.com/2010/03/how-to-quickly-generate-a-large-file-on-the-command-line-with-linux/](https://skorks.com/2010/03/how-to-quickly-generate-a-large-file-on-the-command-line-with-linux/)
5. Delete the trash large file. (to save storage at lab machines)
6. Use the scripts in the provided zip file to plot the required graph. (You have to read the script to figure it out.)
7. Finish lab report.(PDF format ONLY)
8. FAQ: Email [chunpes@g.clemson.edu](mailto:chunpes@g.clemson.edu) if you have any questions regarding this guide.

\
