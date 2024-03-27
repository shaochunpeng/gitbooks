# 💻 Lab 4 Deceiving DDoS Detection (1)

## Instroduction

This document is the DDoS Attack Entropy Detection Spoofing lab guide.

There are several data formats you need to be aware of before doing this lab. They are “pcap” files, end with “.pcap”; transcript files, end with “.trns”; histogram files, end with “hist”; and entropy files, end with “.entr”. The attack record file is simply a text file. You should be familiar with some of them.

The entropy spoofing is performed based on the histogram files in Dr.Ilker’s scripts. That means the spoofing is not happening in real-time. The “entropy\_spoofV3.py” script takes a histogram file and an attack record time file as input, and calculates the desired packets that need to be injected during the attacks, and generates a spoofed histogram file.

So the work flow for entropy spoofing would be:

1. Perform some flood attack
2. Capture the traffic and atack times
3. Use pcap files to generate transcript files
4. Use transcript files to generate histogram files
5. Use the histogram files and attack record files to generate spoofed histogram files.
6. Use the spoofed histogram files to generate spoofed entropy files
7. Plot the spoofed entropy files.

The entropy spoofing script was implemented by Ilker during his Ph.D study at Clemson. The code has been tested in the security lab.

## Questions

1. Execute ./entropy\_spoofV3.py script for three different values of the scale(but should be close to 1) and see if you can get better results.
2. Discuss the pros and cons of using entropy to detect DDoS attacks.



Lab Report



## Lab Guide

1. Add ssh configuration, add “ForwardX11 yes” to host spoof\
   We are using spoof machine (cnc) for this lab.\
   Reference: \
   https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client\

2. In CNC machine, goto DDoS\_Lab6 folder.\

3. \[OPTIONAL, not suggested]\
   **Generate histogram files from pcap files** \
   This step takes pcap files in the autoTest folder as input and generates an “autoTest.hist” file.\
   \
   Before proceeding, you need to have some pcap files in the \
   “/root/DDoS\_Lab6/Appendix/autoTest/” directory.\
   \
   Follow instructions in the previous labs to perform attacks and get pcap files.\
   \
   Generate histogram files: root@cnc:\~/DDoS\_Lab6/Appendix# ./pcap2hist.py\
   \
   This step takes about 1 hour or more to finish depending on the size of the pcap file.\

4. NOTE: The “outputTime0604.hist” is provided so you don’t need to perform step 5. You are supposed to replace the input file name “autoTest.hist” everywhere in step 6 with “outputTime0604.hist”.\

5.  Generate Spoofed histogram file.

    This step uses the file” autoTest.hist” as the input file to generate the “spoofed\_1\_autoTest.hist” file. \
    Following command will spoof the given histogram file. Modify the scale value to whatever other value close to 1. This script will keep running and generate several output files and eventually crash. This script will take around 10 to 20 minutes to finish(crash):

    | root@cnc:\~/DDoS\_Lab6/Appendix# ./entropy\_spoofV3.py autoTest.hist autoTest.hist.out complete\_attack\_times 1.25 -c 1 |
    | ------------------------------------------------------------------------------------------------------------------------ |

    \
    You will see output similar to this: \
    \
    `Namespace(FA=None, attack_times='complete_attack_times', columns=[], ent=False, file_name='autoTest.hist', margin=[0.15], output='autoTest.hist.out', scale='1.25', spoofPac ketCount=False)` \
    `Copying autoTest.hist to temp.txt` \
    `Performing attack number 1 Average cycle: 5.95511450382 Average time: 0.000528254351145` \
    `Performing attack number 2 Average cycle: 5.81375166889 Average time: 0.000568560080107` \
    `Performing attack number 3 Average cycle: 5.98514517218 Average time: 0.000573065158677` \
    `('It took ', 339, ' seconds')` \
    `End of process!!` \
    `Copying spoofed_1_autoTest.hist to temp.txt` \
    `Performing attack number 1 Average cycle: 5.62683823529 Average time: 0.000446215992647` \
    `Performing attack number 2 Average cycle: 5.49799062291 Average time: 0.000533518084394` \
    `Performing attack number 3 Average cycle: 5.7324270557 Average time: 0.000679098474801` \
    `('It took ', 333, ' seconds')` \
    `End of process!!`\
    \
    This step may takes about 15 minutes to finish(crash). Roughly 5 minutes for each sub-step. After this step finishes, you would have a new histogram file(spoofed).\

6.  Generate entropy files from histogram files:\
    This step uses file “outputTime0604.hist” as input to generate the “one.entr” file and “outputTime06041.hist” as input to generate the “two.entr” file. These scripts take about 10 minutes each to generate the entropy files. Please be patient. \
    root@cnc:\~/DDoS\_Lab6/Appendix# ./calculate\_entropyV2.py outputTime0604.hist one

    root@cnc:\~/DDoS\_Lab6/Appendix# ./calculate\_entropyV2.py outputTime06041.hist two\
    \
    The .entr files are the entropy files. One.entr is the original entropy, two.entr is the spoofed entropy. Then you can move the generated entropy files to DDoS\_Lab6 dir. root@cnc:\~/DDoS\_Lab6/Appendix# move one.entr .. \
    root@cnc:\~/DDoS\_Lab6/Appendix# move two.entr ..\

7. Compare entropy files: These commands are used to plot the entropy files. \
   root@cnc:\~/DDoS\_Lab6# ./plot.py -d outputTime0604.entr 1\
   root@cnc:\~/DDoS\_Lab6# ./plot.py -d spoofed\_1\_outputTime0604.entr 1\


