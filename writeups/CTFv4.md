Phantom CTF – Write-Up
Target: 192.168.136.132
Services: DNS, SMB, LDAP
Objective: Find the hidden flag in the environment!

Overview
You’re given just the target IP. Your challenge is to enumerate the network, follow clues through DNS, SMB, and LDAP, and ultimately unlock a password-protected ZIP containing the final flag.

1. Reconnaissance
Start with a port scan to discover available services:

bash
nmap -sV -p- 192.168.136.132
Service highlights:

53/tcp: DNS

445/tcp: SMB

389/tcp: LDAP

2. DNS Enumeration
Check for interesting TXT records:

bash
dig @192.168.136.132 phantom.lan TXT
Result:

text
phantom.lan. TXT "Look under \\smbshare\hint.txt"
Try zone transfer:

bash
dig @192.168.136.132 phantom.lan AXFR
3. SMB Enumeration
List available SMB shares:

bash
smbclient -L //192.168.136.132 -N -m SMB3
Shares found:

smbshare

print$

IPC$

Connect to the share and list files:

bash
smbclient //192.168.136.132/smbshare -N -m SMB3
smb: \> ls
smb: \> get hint.txt
smb: \> get secret_files.zip
smb: \> exit
4. Analyze the Clue
View hint.txt:

bash
cat hint.txt
Content:

text
Company founded in 1998
5. LDAP Enumeration
Query LDAP for flag or password hints:

bash
ldapsearch -x -H ldap://192.168.136.132 -b "dc=phantom,dc=lan"
Find user JohnDoe:

bash
ldapsearch -x -H ldap://192.168.136.132 -b "cn=JohnDoe,ou=CTF,dc=phantom,dc=lan"
Output:

text
description: The password is the year the company was founded
6. Crack the ZIP
Based on previous hints, try password 1998:

bash
unzip -P 1998 secret_files.zip
Extracted file: flag.txt

7. Find the Flag
Read the flag:

bash
cat flag.txt
FLAG:

text
CTF{congratulations_you_found_the_flag}
