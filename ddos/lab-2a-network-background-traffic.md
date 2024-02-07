# 🖥 Lab 2A: Network Background Traffic

## Introduction

Network background traffic is very important in DDoS attack detection studies. The amount of traffic generated in a network depends on the time of the day, the allowed network services (e.g., email, cloud services, VoIP, downloading/uploading large files), and the average number of users. It constitutes a ground truth during a detection. While signature based detection approaches the search for known patterns in background traffic, anomaly based detection approaches use the sudden deviation in the observed feature (such as the number of packets received in a second) of the background traffic for attack detection. Therefore it is crucial to use operational network background traffic in these studies. However, it is not always possible to have access to an operational network for testing. Researchers overcome this issue by using simulated network background traffic, generating traffic in a computer cluster or replaying packet traces collected from an operational network in their studies.

In this lab, you will:

* Learn how to replay captured network traffic.
* Observe and understand the difference between operational network background traffic and a simulated background traffic.

## Instructions

* Connect to Security Lab machines.
* Capture campus traffic.
* Replay the captured traffic.
* Execute a python script to compare the real background traffic versus simulated background traffic.

## Questions

1. Plot the background traffic datasets generated/used in three scenarios(different time slots?) where x-axis shows time, and the y-axis shows the number of packets received. Compare the figures and discuss their differences.
2. Network background traffic statistics generation script (plot\_time\_series\_example.py) generates datasets using different probability distribution functions (PDF). Compare datasets generated using different PDFs. Discuss the difference between datasets and the data-set converted from operational network data.
3. What kind of traffic did you run between hosts in the second scenario (Replay a pcap file with tcpreplay). Justify why you think it can represent operational network traffic.
4.  You can replay and forward captured pcap files on a link in a controlled network

    environment to use as background traffic. If you perform a DDoS attack on this link, you can observe the effects of the DDoS attack without jeopardizing the operational network. However, some of the effects cannot be observed with replayed/forwarded background traffic. List some of these effects and explain the reason why they can not be observed.
5. Number of packets received by a node on the network is one of the popular metrics used in DDoS detection applications. In this assignment, we focused on packet count statistics. Discuss what other metrics that can be used for DDoS detection.

## Lab Report

Submission together with Lab 2B.

1. Discuss the questions above.
2. Show your steps doing this lab.
3. Discuss your plot result.



## Lab Guide

1. Host Sniffer: 192.168.10.9\
   Host Spoof: 192.168.10.125\
   see previous lab guide  & canvas files/announcement for credentials.
2. login to sniffer machine. In the terminal connected to the sniffer, use tshark to collect campus network traffic data.(use interface enp3s1). Capture traffic at your choice.
3.  Open another terminal and connect to the spoof machine.

    $ssh spoof

    \
    At spoof machine, enter lab3scripts folder:

    \#cd lab3scripts



    Get the pcap file you captured on the sniffer machine:

    \#scp your\_user\_name@192.168.10.9:\~/campus\_traffic.pcap .



    Open wireshark/tshark and listen to interface lo, and then you can use the tcpreplay command to replay the traffic.

    \#tcpreplay --intf1=lo campus\_traffic.pcap



    Save the replay traffic and compare it to the real one.\

4. You have your pcap file in the lab3scripts directory, then you can plot the time series using the python script. Make sure that the captured pcap file is the only pcap file in that folder.\
   \#python plot\_time\_series\_example.py\
   \
   This command will generate two figures. The first one is the time series from the real campus background traffic. After closing the first one, the second one will appear, which is the time series of simulated background traffic.\

5.  Make a copy of plot\_time\_series\_example.py:

    \# cp plot\_time\_series\_example.py your\_user\_name.py



    Use a text editor to edit your\_user\_name.py. The line 28 of the file has the options to change different arriving time algorithms. The available values are integers between 0 to 6. The number\_of\_packets refers to the total number of time stamps and expected\_duration modifies the total duration from the first time stamp to the last time stamp.&#x20;



    Modify those options and see if you get a figure that is closer to the real background traffic.

    \


\
