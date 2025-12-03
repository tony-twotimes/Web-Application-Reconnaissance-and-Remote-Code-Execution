# Web Application Reconnaissance and Remote Code Execution

## Objective

The goal of this lab is to move from environment setup to active engagement. We will first map the attack surface of the DVWA web application using directory brute-forcing to identify hidden resources. Once the structure is mapped, we will exploit a Command Injection vulnerability to achieve Remote Code Execution (RCE) on the underlying Docker container.

### Skills Learned

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

### 1 The blueprints 
We found /php.ini with a status 200 (it's accessible). This is a massive security failure for the target. This file contains the configuration settings for the PHP environment running the server. It tells us exactly what the server and and can't do. This is like we walked into the building and we found the blueprints of the building laying out in plainsite. 

### 2 The Treasure Map

We found the /robots.txt also with a status 200. This is a ile that tells search engines which parts of the site NOT to look at. This is like a treasure map. 

### 3 The Keys 

We found /config with a status 301 (Redirect). This directory likely contains configuration files for the web app. 

*This scan moved us from blindly throwing exploits into the dark, to targeted attacking.* 




























