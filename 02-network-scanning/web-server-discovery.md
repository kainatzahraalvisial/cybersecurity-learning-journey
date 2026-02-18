# Lab 02b – Web Server Discovery on Metasploitable

**Date:** February 18, 2026  
**Target IP:** 10.0.2.5  
**Port:** 80/tcp (http)  
**Discovered via:** Nmap scan (Apache 2.2.8 on port 80) – see [nmap-metasploitable.md](./nmap-metasploitable.md)  
**Tool:** Firefox browser on Kali

## Objective
Confirm the web server is running and identify any exposed web applications.

## Steps Performed

1. Opened Firefox on Kali Linux
2. Navigated to:  
   http://10.0.2.5
3. Observed the main page 
4. Noticed multiple links to intentionally vulnerable web applications:
   - DVWA 
   - phpMyAdmin
   - Mutillidae
   - TWiki
   - WebDAV
   - Others
5. Clicked on DVWA and saw the login page

## Screenshots

![Main web server page at http://10.0.2.5](./screenshots/01-web-main-page.png)  
![Example vulnerable app page (e.g. DVWA)](./screenshots/02-vulnerable-app-page..png)

## Key Learnings

- Nmap showed port 80 open it led me to manually check the web server
- Metasploitable deliberately exposes several vulnerable web apps on port 80
- This creates a very large attack surface for web hacking practice
- No login or exploitation was attempted yet, this was pure discovery

## Next Plan

- Try default credentials on one of the apps 
- Run a web vulnerability scanner (Nikto or dirb/gobuster) on http://10.0.2.5
- Attempt actual exploitation (e.g. SQL injection in DVWA) in a future lab

This is part of my cybersecurity learning journey:  
https://github.com/kainatzahraalvisial/cybersecurity-learning-journey