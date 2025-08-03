# 🧪 Ubuntu-Vuln01 - Writeup (Aug 3, 2025)

## 🔍 Reconnaissance

- **Target IP:** `192.168.136.132`
- **Tools Used:** `nmap`, `nc`, `gobuster`, `curl`

###  Port Scan
```bash
nmap -sC -sV -T4 192.168.136.132
Result:

Port 21 (FTP) → vsFTPd 3.0.5

Port 80 (HTTP) → Apache 2.4.29

**FTP Enumeration**
Used Netcat to connect manually:

code:
nc -n 192.168.136.132 21
Got the banner:
code:
220 (vsFTPd 3.0.5)
I also ran some NSE scripts with Nmap:

code:
nmap -p 21 --script ftp* 192.168.136.132
The server responded with:

code:
500 Illegal PORT command
So the FTP server wasn’t vulnerable to a bounce attack, but the outdated version is still suspicious. Didn't find a way in yet, so I shifted to HTTP.

**Web Server Discovery**
First, I checked for a robots.txt file — and bingo.

code:
curl http://192.168.136.132/robots.txt
It revealed:
User-agent: *
Disallow: /admin/
Disallow: /backup/
Nice. That gave me two potentially sensitive directories to investigate.

**- Directory Bruteforce with Gobuster**
Ran a directory scan to confirm what else might be there.

code:
gobuster dir -u http://192.168.136.132 -w /usr/share/wordlists/dirb/common.txt
As expected, /admin/ and /backup/ showed up in the results. Let’s check them out.
backup File Loot
Visited /backup/ and found a file called old.zip. Downloaded it:

code
curl http://192.168.136.132/backup/old.zip -O
unzip old.zip
got: Old backup archive (maually created by me in vuln machine)

- Admin dir
Visited admin/ and found a file called config.php
opened it and got the message:do not touch this


