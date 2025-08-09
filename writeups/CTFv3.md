
# Cybersecurity CTF Writeups - Abhi Kaushik

## CTF 1: InfiltrateX - Basic Recon Challenge

### Objective
Perform reconnaissance, scanning, and enumeration on the target to collect all available flags without privilege escalation.



### Step 1: Discovering Hidden Paths

**Command:**
curl http://192.168.x.x/robots.txt


**Output:**
User-agent: *
Disallow: /.hidden/


Discovered hidden directory from `robots.txt`.

**Flag:**

curl http://192.168.x.x/.hidden/flag1.txt
# FLAG{recon_complete}


### Step 2: Port Scanning

**Command:**
nmap -sV <192.168.x.x>

Found open SSH service with a custom banner.

**Banner Output:**

SSH Banner: Welcome. Here's a clue... FLAG{port_scan_wins}


### Step 3: FTP Enumeration

**Command:**

ftp <192.168.x.x>

Login as `anonymous`, then:
ftp
ls
get flag2.txt
bye
cat flag2.txt
# FLAG{ftp_enum_success}


### Step 4: SSH Access

Used provided credentials:

- Username: `ctfplayer`
- Password: `player123`

**Command:**

ssh ctfplayer@192.168.x.x
cat ~/flag3.txt
# FLAG{user_login_success}



### Summary

| Step | Flag |
|------|------|
| Recon (robots.txt) | `FLAG{recon_complete}` |
| Nmap Scan (SSH)    | `FLAG{port_scan_wins}` |
| FTP Access         | `FLAG{ftp_enum_success}` |
| SSH Login          | `FLAG{user_login_success}` |



