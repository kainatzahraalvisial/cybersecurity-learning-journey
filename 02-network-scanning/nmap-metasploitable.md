# Lab 01 – First Nmap Scan on Metasploitable 2

**Date:** February 16, 2026  
**Student:** Zahra  
**Environment:**  
- Kali Linux 2025.x (VirtualBox VM – NAT Network)  
- Target: Metasploitable 2 (vulnerable training VM)  
- Target IP: 10.0.2.5  
- Tool: Nmap version 7.95

## Objective
Perform basic network reconnaissance on a deliberately vulnerable machine to:

1. Confirm the host is alive
2. Discover open TCP ports
3. Identify running services and (possibly) versions
4. Understand what information an attacker could gather in the reconnaissance phase

## Commands Used

### 1. Basic port scan (default top 1000 ports)
nmap 10.0.2.5
### 2. More detailed scan (service version + OS detection)
nmap -sV -O 10.0.2.5
## Scan Results – Basic Scan
Nmap discovered **23 open TCP ports** on 10.0.2.5.

| PORT     | STATE | SERVICE       | Common Use / Notes                                      |
|----------|-------|---------------|---------------------------------------------------------|
| 21/tcp   | open  | ftp           | File Transfer Protocol (vsftpd 2.3.4 – backdoor known)  |
| 22/tcp   | open  | ssh           | Secure Shell remote login                               |
| 23/tcp   | open  | telnet        | Insecure plain-text remote login                        |
| 25/tcp   | open  | smtp          | Simple Mail Transfer Protocol (email sending)           |
| 53/tcp   | open  | domain        | DNS server                                              |
| 80/tcp   | open  | http          | Web server (Apache + vulnerable apps)                   |
| 110/tcp  | open  | pop3          | Post Office Protocol v3 (email retrieval)               |
| 111/tcp  | open  | rpcbind       | RPC portmapper                                          |
| 139/tcp  | open  | netbios-ssn   | NetBIOS Session Service (Samba)                         |
| 445/tcp  | open  | microsoft-ds  | Microsoft Directory Services / Samba                   |
| 512/tcp  | open  | exec          | rexec (remote execution)                                |
| 513/tcp  | open  | login         | rlogin (remote login)                                   |
| 514/tcp  | open  | shell         | rsh / remsh                                             |
| 1099/tcp | open  | rmiregistry   | Java RMI Registry                                       |
| 1524/tcp | open  | ingreslock    | ingres (old database service)                           |
| 2049/tcp | open  | nfs           | Network File System                                     |
| 2121/tcp | open  | ccproxy-ftp   | Proxy FTP                                               |
| 3306/tcp | open  | mysql         | MySQL database                                          |
| 5432/tcp | open  | postgresql    | PostgreSQL database                                     |
| 5900/tcp | open  | vnc           | VNC remote desktop                                      |
| 6000/tcp | open  | X11           | X Window System                                         |
| 6667/tcp | open  | irc           | Internet Relay Chat                                     |
| 8009/tcp | open  | ajp13         | Apache JServ Protocol (Tomcat)                          |
| 8180/tcp | open  | unknown       | Often Tomcat or proxy service                           |

**Summary:**
- Total open ports: **23**
- Not shown: 977 closed ports
- MAC Address: 08:00:27:F2:79:79 (Oracle VirtualBox virtual NIC)
## Key Observations & Learnings

| Port   | Service      | What it usually means                  | Security implication (Metasploitable)              |
|--------|--------------|----------------------------------------|----------------------------------------------------|
| 21     | ftp          | File Transfer Protocol                 | Old vsftpd 2.3.4 with **backdoor**                 |
| 22     | ssh          | Secure Shell (remote login)            | Old OpenSSH – weak keys / config possible          |
| 23     | telnet       | Plain-text remote login                | **Very dangerous** – passwords sent in clear       |
| 25     | smtp         | Email sending server                   | Possible relay abuse                               |
| 53     | domain       | DNS server                             | Zone transfer possible                             |
| 80     | http         | Web server                             | Multiple vulnerable web apps installed             |
| 110    | pop3         | Email retrieval                        | Clear-text passwords                               |
| 111    | rpcbind      | RPC portmapper                         | Many old RPC vulnerabilities                       |
| 139    | netbios-ssn  | NetBIOS session service                | Samba-related exploits                             |
| 445    | microsoft-ds | Microsoft Directory Services (SMB)     | Old Samba versions – many exploits                 |
| 3306   | mysql        | MySQL database                         | Often root with no password / weak creds           |
| 5900   | vnc          | VNC remote desktop                     | Weak/no authentication                             |

**Main takeaway from this scan:**
- 23 open ports on a single machine = **very bad security posture**
- Metasploitable deliberately runs many outdated / misconfigured services
- Even a basic Nmap scan (no special options) already gives an attacker a lot of attack surface

**Questions I Asked Myself After the Scan**

- Which of these services look easiest / most dangerous to attack first?
- Can I visit http://10.0.2.5 in a browser? What do I see?
- What happens if I try to connect to FTP anonymously?
- Which service has a well-known public exploit?

## Detailed Scan Results (Service Version + OS Detection)

**Command used**
nmap -sV -O 10.0.2.5

## Output

| PORT     | STATE | SERVICE       | VERSION                                      |
|----------|-------|---------------|----------------------------------------------|
| 21/tcp   | open  | ftp           | vsftpd 2.3.4                                 |
| 22/tcp   | open  | ssh           | OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) |
| 23/tcp   | open  | telnet        | Linux telnetd                                |
| 25/tcp   | open  | smtp          | Postfix smtpd                                |
| 53/tcp   | open  | domain        | ISC BIND 9.4.2                               |
| 80/tcp   | open  | http          | Apache httpd 2.2.8 ((Ubuntu) DAV/2)          |
| 512/tcp  | open  | exec          | Pnetkit-rsh rexecd                           |
| 111/tcp  | open  | rpcbind       | 2 (RPC #100000)                            |
| 139/tcp  | open  | netbios-ssn   | Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  |

- Device type: general purpose
- Running: Linux 2.6.X
- OS CPE: cpe:/o:linux:linux_kernel:2.6
- OS CPE: cpe:/o:linux:linux_kernel:2.6
- Network Distance: 1 hop

**Key new information I noticed:**
- FTP vsftpd 2.3.4 version has a well-known backdoor 
- HTTP (port 80) Apache 2.2.8 (Ubuntu) with DAV/2 runs multiple vulnerable web apps


**Next Steps Planned**

- Visit the web server (http://10.0.2.5) and take screenshots
- Try anonymous FTP login
- Document first successful exploit (most likely vsftpd 2.3.4 backdoor)

**Commands I Want to Remember**
<!--Save scan to file-->
nmap -oN nmap-basic-10.0.2.5.txt 10.0.2.5

<!--Scan all 65,535 ports-->
nmap -p- 10.0.2.5

<!--Aggressive scan (OS, version, scripts, traceroute)-->
nmap -A 10.0.2.5






This document is part of my public learning portfolio:
https://github.com/kainatzahraalvisial/cybersecurity-learning-journey
