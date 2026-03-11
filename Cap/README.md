
<img width="507" height="286" alt="banner" src="https://github.com/user-attachments/assets/34ec8a53-4c3d-42b5-971d-080401b7dd27" />

Easy Hackthebox challange machine.

To solve this machine, we begin by enumerating open ports – finding ports 21, 22, and 80 open. On the webserver, we are able to exploit an Insecure Direct Object Reference (IDOR) vulnerability to obtain system credentials as the nathan user. Using the credentials, we are able to SSH into the machine, and read user.txt. Through automated, local enumeration, we learn a system package is incorrectly configured to allow setuid. Using this knowledge, we are able to exploit the package to get a shell as root – gaining access to root.txt.

**Let's jump in and start to hack !:).
