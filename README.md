# Web Application Reconnaissance and Remote Code Execution

## Objective

The goal of this lab is to move from environment setup to active engagement. We will first map the attack surface of the DVWA web application using directory brute-forcing to identify hidden resources. Once the structure is mapped, we will exploit a Command Injection vulnerability to achieve Remote Code Execution (RCE) on the underlying Docker container.

### Skills Learned

This lab develops the ability to perform active web application reconnaissance using Gobuster to brute-force directory paths and identify hidden resources. The process involves analyzing HTTP status codes and server responses to locate sensitive configuration files like php.ini and robots.txt. It also covers exploiting a Command Injection vulnerability by manipulating unsanitized input fields to execute arbitrary system commands. The exercise concludes with achieving Remote Code Execution (RCE), verified by retrieving the current user context and conducting post-exploitation enumeration of the Docker container's file system.

### Tools Used 

- Kali Linux: Attacker machine
- Gobuster: Directory enumeration tool
- Browser: Firefox (for manual exploitation)
- Target: Windows 10 VM hosting Docker (DVWA container)



### Steps 

### Prerequisite: Install Gobuster on Kali Linux Machine 

### <img width="758" height="363" alt="image" src="https://github.com/user-attachments/assets/b77b10c1-9a64-4aac-a68f-564f766f3599" />

### Phase 1: Reconnaissance (Forced Browsing)

### <img width="788" height="525" alt="image" src="https://github.com/user-attachments/assets/16c9bf91-a0e7-4e2f-b6fd-22333eeb382c" />

In an earlier lab, we conducted Nmap scans. Think of this as finding a building you're looking for and you can see which doors are open (ports). The Gobuster scan is like walking inside and finding the floor plan, the safe, and the maintenance manuals that were left on someone's desk. 

### Our Findings 

### 1.) The blueprints 
We found /php.ini with a status 200 (it's accessible). This is a massive security failure for the target. This file contains the configuration settings for the PHP environment running the server. It tells us exactly what the server and and can't do. This is like we walked into the building and we found the blueprints of the building laying out in plainsite. 

### 2.) The Treasure Map

We found the /robots.txt also with a status 200. This is a ile that tells search engines which parts of the site NOT to look at. This is like a treasure map. 

### 3.) The Keys 

We found /config with a status 301 (Redirect). This directory likely contains configuration files for the web app. 

*This scan moved us from blindly throwing exploits into the dark, to targeted attacking.* 

### Phase 2: Manual Verification 

Automated tools give us leads, but we must verify them manually to confirm the vulnerability and gather actionable intelligence. We used the browser on our Kali machine to inspect the findings.

### 1. Verify /robots.txt 

### <img width="916" height="402" alt="image" src="https://github.com/user-attachments/assets/cb8b3737-8b3c-45c1-a260-d37d1aa3a628" />

We browse to robots.txt and find "Disallow: /" which confirms that the administrator is actively trying to hide the entire site strucutre from search engines. 

### 2. Verify /php.ini

### <img width="905" height="352" alt="image" src="https://github.com/user-attachments/assets/e0bb4478-5251-4f14-a96a-755083b9a1de" />

We browse to /php.ini and discover that the file is readable and reveals critical server findings. Namely, that "allow_url_include" is set to "On" which is a dangerous misconfiguration that allows us to potentially execute code hosted on our attacker machine. 

### 3. Verify /config

### <img width="1019" height="518" alt="image" src="https://github.com/user-attachments/assets/e2fa191a-7abe-4104-955d-3afe3c055eae" />

Normally, a server should show a blank page or a 403 Forbidden error, but ours shows a list of every file in that folder. We identified a backup file named config.inc.php.bak. This is a high-value target; attackers can often download backup files to view the application's source code and hardcoded database credentials in plain text.




































