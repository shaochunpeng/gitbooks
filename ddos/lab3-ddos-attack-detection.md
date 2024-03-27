# 🖥️ Lab3 DDoS Attack Detection

#### Introduction

Different DDoS detection methods have different performance.  In this lab, we are going to detect DDoS attacks using CUSUM, wavelet and entropy methods.&#x20;

Because some of the detection methods, e.g., wavelet detection and entropy detection, requires several hours to capture traffic and preprocess the pcap files. We preprocessed previously captured data and created the intermediate file for you to save some time. Since you already learnt how to capture traffic and perform attacks, there is no need to do it again for this lab.&#x20;

The CUSUM, wavelet and entropy detection script were implemented by Ilker during his Ph.D study at Clemson.&#x20;

In this lab, you will:

* Perform the four types of detection using preprocessed intermediate data files.

#### Instructions

* Connect to Security Lab machines(CnC machine).
* Run detection scripts on preprocessed data .

#### Questions

1. What detection methods work well? Why
2. How would you try to avoid being detected?
3. Which method detects more quickly?
4. How often do you get false alarms from your results?

#### Lab Report

1. Discuss the questions above.
2. Show your steps and results in this lab. If you try to attach a screenshot, make sure you explained it adequately.&#x20;
3. Due date given on Canvas.

GUIDE

1. Setup and connect to CUVPN on your pc.\
   \

2. Add SSH configuration: (add “ForwardX11 yes” to host spoof)\
   \

3.  In CnC machine, goto DDoS\_Lab5 folder.

    $ssh -X cnc&#x20;

    \#cd DDoS\_Lab5

    \
    \*if there isn’t a DDoS\_Lab5 folder, use “unzip lab5.zip” to create a new DDoS\_lab5 folder. Otherwise do not touch this zip file.\
    \

4. Traffic Volume Detection\
   This method simply uses a threshold on the traffic time series to decide whether there is a DDoS attack. The file “timeseires.txt” is generated from previous captured pcap files.
   1. Packets Count Thresholding\
      First plot the packets count time series and get a rough idea about the traffic. Following command plot the “rx\_packet” column of the file “timeseries.txt”.\
      \
      [root@cnc:\~/DDoS\_Lab5# ./plot.py -d timeseries.txt 1](#user-content-fn-1)[^1]\
      \
      ![](https://lh7-us.googleusercontent.com/k57FZMgJ3jVTSy7xuFaNVEz-LGe7hst-mjn2LSmyhIuiSN77vSNvFJQ3PxLjc3bPFl5XCCnoBuZIt4T3vv9xofYhQ7vz4Ohg5sT4Ob7PhoYFRs\_lsQKNkuY4NZdcdLp5iYsL47LFfdOoEY46pgqdlQ)\
      \
      The attack time tag for this data set is logged in file “complete\_attack\_times”. \
      Use option “-r” and “-t” to determine a good threshold. For example:\
      root@cnc:\~/DDoS\_Lab5# ./plot.py -d timeseries.txt 1 -r complete\_attack\_times -t 30000\
      \
      Use option “-c” to plot the ROC curve automatically. For this attack log, you can get a perfect ROC curve. \
      root@cnc:\~/DDoS\_Lab5# ./plot.py -d timeseries.txt 1 -r complete\_attack\_times -c\
      \
      Note that the automated ROC function assumes that the filter is a high pass filter. In another word, any data point that is higher than a threshold is considered as “positive”. This function does not work for cases where a low pass filter or a band filter is needed.\

   2. Data Volume Thresholding\
      Repeat all steps in (a), use the “rx\_byte” column instead.\
      root@cnc:\~/DDoS\_Lab5# ./plot.py -d timeseries.txt 2\
      \

5. CUSUM Detection\
   To perform CUSUM analysis, execute “vda.py” with the “-c” option on the time series data file. \
   \
   Following command perform CUSUM analysis on the “rx\_byte” column in file “timeseries.txt” with the default parameters and generates an intermediate file “Csm\_20130604\_timeseries.txt”:\
   \# ./vda.py -c timeseries.txt 2 -alpha 0.22 -epsilon 0.98811 -ce 0.13\
   \
   Then perform the steps in 5(a) to process “Csm\_20130604\_timeseries.txt”.\
   \# ./plot.py -d “Csm\_20130604\_timeseries.txt 1\
   \

6. Wavelet of the CUSUM detection\
   To perform Wavelet analysis, execute “vda.py” with the “-w1” option on the time series data file.\
   \
   Following command perform wavelet analysis on the “rx\_byte” column in file “timeseries.txt” with the default parameters and generates an intermediate file “Wvl\_20130604\_timeseries.txt”\
   \# ./vda.py -w1 timeseries.txt 2 -alpha 0.22 -epsilon 0.98811 -ce 0.13 -depth 3\
   \
   Then perform the steps in 5(a) to process “Wvl\_20130604\_timeseries.txt”.\
   \# ./plot.py -d “Wvl\_20130604\_timeseries.txt 1\
   \
   You can use the “-u” option instead of “-t” to detect the attack with a low pass filter. Every node below the given value is considered as “positive”.\
   \# ./plot.py -d Wvl\_20130604\_timeseries.txt 1 -r complete\_attack\_times -u -50000000\
   \

7. CUSUM of the Wavelet Detection\
   To perform Wave-CUSUM analysis, execute “vda.py” with the “-w2” option on the time series data file. \
   Following command perform CUSUM analysis on the “rx\_byte” column in file “timeseries.txt” with the default parameters and generates an intermediate file Csm\_salem\_20130604\_timeseries.txt”\
   \# ./vda.py -w2 timeseries.txt 2 -alpha 0.0 -ce 0.5\
   \
   Then repeat the steps in 5(a):\
   \#./plot.py -d Csm\_salem\_20130604\_timeseries.txt 1 \
   \

8. Entropy Based Detection\
   The preprocessing of the intermediate file for the entropy detection takes a very long time. In this step, we preprocessed the pcap files and generated the intermediate “fileoutputTime0604.entr” for you. \
   \
   There are multiple columns in the “outputTime0604.entr” file. Check the available columns by:\
   \# head outputTime0604.entr\
   \
   Try with different columns, e.g. plot the “srcIP” column of the entropy file with:\
   \# ./plot.py -d outputTime0604.entr 1\
   \
   Or plot the “destPort” column of the entropy file with:\
   \# ./plot.py -d outputTime0604.entr 4\
   \
   Then you can use the plot.py scripts to check the detection rate like those in the previous steps.\

9. Finish lab report.(Pdf format ONLY)
10. FAQ: Email [chunpes@g.clemson.edu](mailto:chunpes@g.clemson.edu) if you have any questions regarding this guide.

\
References:

\[1] Carl, Glenn, Richard R. Brooks, and Suresh Rai. "Wavelet based denial-of-service detection." Computers & Security 25.8 (2006): 600-615.

\[2] Callegari, Christian, et al. "WAVE-CUSUM: Improving CUSUM performance in network anomaly detection by means of wavelet analysis." computers & security 31.5 (2012): 727-735.

\[3] Ozcelik, Ilker, Yu Fu, and Richard R. Brooks. "DoS detection is easier now." 2013 Second GENI Research and Educational Experiment Workshop. IEEE, 2013.

\


[^1]: 
