
<img width="584" height="369" alt="135722831-39856c2c-ffde-42b7-a023-ebcb636c609f" src="https://github.com/user-attachments/assets/a24a93c2-82f1-4768-b6ee-b2bb78015027" />

# Synopsis
 “Cap” is marked as easy difficulty machine which features Gunicorn web server running a security dashboard. The dashboard gives access 
 to Pcap files to analyze locally, (Insecure Direct Object Reference (IDOR) vulnerability) one of the Pcap files has clear text password 
 used to access FTP service. Because of password reuse we can able to login via SSH service and we can read user.txt. 
 Through automated, local enumeration, we learn a system package (python) is incorrectly configured to allow setuid. 
 Using this knowledge, we are able to exploit the package to get a shell as root – gaining access to root.txt.

**Let's jump in and start to hack !:).
# Enumeration

We are using network map scanner with aggressive mode recon for service and version number.

Nmap scan printscreen result :

<img width="1245" height="521" alt="nmap" src="https://github.com/user-attachments/assets/83fba693-666c-4a9c-bbc6-e5170cf2b654" />


Next, we enumerate (fuzzing) the port 80 using tools like gobuster and ffuf for directory listing.

We are using both fuzzing scanner with same fuzzing list to compare the result.
First we are using FFUF scanner for directory fuzzing with raft-medium-directories list.
Fuzzing with FFuf printscreen result :

<img width="1102" height="641" alt="fuffdirscan" src="https://github.com/user-attachments/assets/a206c685-778a-4f54-a1c4-5f30ffa7702d" />


In second stage we are using Gobuster scanner for directory fuzzing with raft-medium-directories list.
Fuzzing gobuster print screen result :

<img width="1159" height="402" alt="gobuster" src="https://github.com/user-attachments/assets/8e30d7c3-f0fc-4d0d-94e3-4341d47c3e1e" />

Both fuzzing scanner give a same result .Since we did not find any further results from our automated tools.We decide to dig deeper into the web pages. 

<img width="1900" height="557" alt="website" src="https://github.com/user-attachments/assets/b84b4e27-e964-421a-b703-0bc8601fa456" />

# Gaining Access

We see a security page, we can access to IP Config, we will the output of ifconfig, if we enter to netstat we will se the output of netstat. 
But if we enter to security snapshot, which points to http://10.129.8.16/capture, we generated a pcap, the URL path was /data/1. This suggests a possible Insecure Direct Object Reference (IDOR) vulnerability may exist and there is no authorization checks around the request. 
To test this, we can change this number, and monitor the results. In the case the object is invalid, we are redirected to the dashboard, however, 
when we set the number to “0”, we are given a valid capture file that we are able to download.

<img width="1464" height="848" alt="wireshark" src="https://github.com/user-attachments/assets/762fa80d-b420-4df1-94cb-e47e8007efdf" />

It shows us the captured packets information. Let’s download the Pcap file and open in wireshark. 

<img width="1900" height="557" alt="website" src="https://github.com/user-attachments/assets/1a569526-c9f0-46a0-84ed-b784967a76a0" />

To help hunt for interesting information, we open the “Statistics” menu, and launch the Protocol Hierarchy window. In it, we see there are FTP packets that were captured. 
Since FTP is a clear-text protocol, we know there may be credentials captured that we can get. To check this, we set the display filter to “ftp”. 
Once we set the filter, within the first few packets, we see credentials for the nathan user. 
Credentials: Nathan : Buck3tH4TF0RM3!

This Pcap file has username and password for FTP service. Let’s use these credentials to login via SSH and read the user flag.

<img width="537" height="59" alt="flag1" src="https://github.com/user-attachments/assets/365b3153-ff22-4a38-90dc-68ba93ffbec3" />


```nathan@cap:~$ cat user.txt
ce3f90fe646c14a033f2cff997f3b5c4
```

# Privilege Escalation






