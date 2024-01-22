---
description: This page contains access information for ECE security lab.
---

# 🖥 Lab Access

### IN-PERSON Access

In Spring 2024, students are encouraged to go to security lab to perform the lab contents.

~~In FALL2022, students are suggested to go to security lab to perform lab contents.~~

LOCATION: 22B Riggs Hall ( Level B, double door, door windows covered by shutters)

Access Code: See Canvas

Machines Available: \
192.168.10.8 - 192.168.10.22\
Login Credentials: Distributed in lecture.

DO NOT UNPLUG ANY POWER PLUG.\
DO NOT SHUTDOWN MACHINES.

### Remote Access

Student can access security lab via Clemson Network.

* **Off-Campus Network:**\
  Follow [CCIT-VPN-Guide](https://ccit.clemson.edu/services/network-phones-cable/network/vpn/) and [ClemsonVPN](https://cuvpn.clemson.edu) to setup VPN to connect to Clemson Network.
* **Within Clemson Network:** \

  1.  Add following content to ssh configuration(default location: /home/username/.ssh/config)\
      [\*reference](https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client)\
      \
      Host eceddos\
      &#x20; HostName 130.127.248.232\
      &#x20; User ddos

      \
      Host sniffer\
      &#x20; HostName 192.168.10.9\
      &#x20; User \<your username here>\
      &#x20; ForwardX11 yes\
      &#x20; ProxyCommand ssh -W %h:%p eceddos

      \
      Host node1\
      &#x20; User \<your username here>\
      &#x20; HostName 192.168.10.10\
      &#x20; ForwardX11 yes\
      &#x20; ProxyCommand ssh -W %h:%p eceddos

      \
      Host node2\
      &#x20; User \<your username here>\
      &#x20; HostName 192.168.10.11\
      &#x20; ForwardX11 yes\
      &#x20; ProxyCommand ssh -W %h:%p eceddos
  2. Open two terminals, connect to node1/2 and sniffer respectively. \
     &#x20;`$ssh node1`

## Login and change password

Start computer and login with username and initial password given at first class. You can login to any machine in the lab.

Once login, please open a terminal and change your password at first

<figure><img src=".gitbook/assets/WeChat Screenshot_20180826145052.png" alt=""><figcaption></figcaption></figure>

```
[username@clemson11 ~]$ passwd
Changing password for user username.
Current Password: 
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
[username@clemson11 ~]$ 
```

##
