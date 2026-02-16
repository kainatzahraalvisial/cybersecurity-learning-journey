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
```bash
nmap 10.0.2.5
2. More detailed scan (service version + OS detection)
Bash# (you can add this later when you run it)
nmap -sV -O 10.0.2.5
Scan Results – Basic Scan
textStarting Nmap 7.95 ( https://nmap.org ) at 2026-02-15 23:34 EST
Nmap scan report for 10.0.2.5
Host is up (0.000301s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
110/tcp  open  pop3
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: 08:00:27:F2:79:79 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in XX.XX seconds
Screenshot – Basic Nmap output
Basic Nmap scan – top ports
Key Observations & Learnings





















































PortServiceWhat it usually meansSecurity implication (Metasploitable)21ftpFile Transfer ProtocolOld vsftpd 2.3.4 with backdoor22sshSecure Shell (remote login)Old OpenSSH – weak keys / config possible23telnetPlain-text remote loginVery dangerous – passwords sent in clear80httpWeb server (Apache/httpd usually)Multiple vulnerable web apps installed445microsoft-dsSamba (SMB) – file sharingOld Samba versions – many exploits3306mysqlMySQL databaseOften root with no password / weak creds5900vncRemote desktop (VNC)Weak/no authentication in Metasploitable
Main takeaway from this scan:

23 open ports on a single machine = very bad security posture
Metasploitable deliberately runs many outdated / misconfigured services
Even a basic Nmap scan (no special options) already gives an attacker a lot of attack surface

Questions I Asked Myself After the Scan

Which of these services look easiest / most dangerous to attack first?
Can I visit http://10.0.2.5 in a browser? What do I see?
What happens if I try to connect to FTP anonymously?
Which service has a well-known public exploit?

Next Steps Planned

Visit the web server (http://10.0.2.5) and take screenshots
Run version detection scan (nmap -sV -O 10.0.2.5)
Try anonymous FTP login
Start Metasploit and look for exploits matching the versions found
Document first successful exploit (most likely vsftpd 2.3.4 backdoor)

References / Commands I Want to Remember
Bash# Save scan to file (good habit for reports)
nmap -oN nmap-basic-10.0.2.5.txt 10.0.2.5

# Scan all 65,535 ports (slow but complete)
nmap -p- 10.0.2.5

# Aggressive scan (OS, version, scripts, traceroute)
nmap -A 10.0.2.5

This document is part of my public learning portfolio:
https://github.com/kainatzahraalvisial/cybersecurity-learning-journey