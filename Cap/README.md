
<img width="584" height="369" alt="135722831-39856c2c-ffde-42b7-a023-ebcb636c609f" src="https://github.com/user-attachments/assets/a24a93c2-82f1-4768-b6ee-b2bb78015027" />

# Easy Hackthebox challange machine.

The To solve this machine, we begin by enumerating open ports – finding ports 21, 22, and 80 open. On the webserver, we are able to exploit an Insecure Direct Object Reference (IDOR) vulnerability to obtain system credentials as the nathan user. Using the credentials, we are able to SSH into the machine, and read user.txt. Through automated, local enumeration, we learn a system package is incorrectly configured to allow setuid. Using this knowledge, we are able to exploit the package to get a shell as root – gaining access to root.txt.

**Let's jump in and start to hack !:).
