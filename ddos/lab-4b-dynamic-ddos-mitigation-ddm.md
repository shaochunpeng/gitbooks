# 🖥️ Lab 4B Dynamic DDos Mitigation(DDM)

## Introduction

This document is the Dynamic DDoS Mitigation lab guide.

The Dynamic DDoS Mitigation(DDM) system extends a standard Content Delivery Network(CDN) network with the ability to scale up and down upon the system status.

For this lab, the basic idea is to use more machines to undertake the DDoS attacks to keep service availability.&#x20;

## Questions

1\. Can this system withstand a DDoS attack? Why or why not?\
2\. In what cases would you use a DDM system? Why do you think it’s worth it in that case?\
3\. Discuss what would you change if you were to implement this system on the Internet?

## Lab Report

1\. Discuss the questions above.\
2\. Show and Explain your steps and results in this lab.\
3\. Due 4/24/2024 (see Canvas Submission)\


## Do not wait until the last minute to finish this lab. You may find others struggling with this and all machines are as slow as being DDoS attacked.



## Steps

1. \--(ALREADY DONE, SKIP THIS)--Setup a standard CDN. (ALREADY DONE, SKIP THIS)
   1. Configure several HTTP reverse proxy server to cache the victim's website.
   2. Configure DDoS Mitigation DNS server to have 2 domain names pointing to victim and reverse proxy servers.
2. \--(ALREADY DONE, SKIP THIS)--Implement DDM with CDN--(ALREADY DONE, SKIP THIS)
   1. Configure passwordless SSH from DDM DNS server to HTTP reverse proxy servers(vms) and vm host machines.
   2. Setup pssh command.
3. Modify ddm.py scripts and put it under DNS machine lab7 folder, with your name.\
   [https://github.com/sonusz/dynamic-cdn/blob/master/ddm.py](https://github.com/sonusz/dynamic-cdn/blob/master/ddm.py)\
   READ THE SCRIPT, Make sure you understand what this script do.
4. Add the DNS server's IP to the bot vms' /etc/resolv.conf file
5. Launch DDoS Attack on Victim's domain name. Explore the effects of the DDM system

## Guide

### Machines:

CNC: ip - 192.168.10.125

BOT: ip - 192.168.10.109 - 192.168.10.132

Victim: ip - 192.168.10.112, hosted at 192.168.10.22

DDM DNS machine: 192.168.10.100 - same credential as CNC machine\
\=======INFORM ME WHEN THIS MACHINE IS NOT WORKING=====

HTTP reverse proxy machines: root : root\
192.168.10.143 - hosted at 192.168.10.19\
192.168.10.118 - hosted at 192.168.10.20\
192.168.10.116 - hosted at 192.168.10.21\
\=====DO NOT TURN OFF/RESTART .19 .20 .21 MACHINES=====



1. On CNC machine, use curl command or ping\_web.sh to access http reverse proxy machine address:\
   curl http://edge.ddm.lan\
   or\
   ./ping\_web.sh http://edge.ddm.lan\
   use Tshark on 192.168.10.22(the host of the victim) to compare the query count, and explore how cache works.
2. Download ddm.py from previous link, put it to DNS machine lab7 folder(you can modify this on your machine or on DNS machine) \
   host\_username set to 'ddm'\
   guest\_username set to 'root'\
   hosts\_file set to '/root/lab7/hosts.txt'\
   other settings follow the script comments.\
   \
   _**Note, when using vboxmanage command in the script, the vm name shall be CentOS7, not CentOS.**_\
   _**vboxmanage guestproperty will not work. You are to modify the script to skip this part.**_\

3. On DNS machine, run /root/lab7/ddm.py (your modified script.)
4. On CNC machine, se ping\_web.sh to evaluate the mitigation effects:\
   compare response time with same type/amount of attack on this two domains:\
   http://edge.ddm.lan\
   http://www.victim.lan\
   You can use different types of DDoS Attacks to show your understanding.\

5. Analyze your result. Discuss if there are changes, why? What’s happening behind the scenes?\
   How is ddm.py working? How can you know it?(tshark logs, respond times, ping response)
6. Finish lab report. It is due on 4/24/2024. Late submission requires prior approval by Dr.Brooks.
7. My email: chunpes@g.clemson.edu

* Old reference (previous semester guide) available in Canvas: Files->LabMaterials->OLD\_REF\_lab7....pdf\
  &#x20;&#x20;
