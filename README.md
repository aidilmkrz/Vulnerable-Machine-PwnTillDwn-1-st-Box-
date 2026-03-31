# PwnDrive - System Hacking Walkthrough
**Target IP:** 10.150.150.11  
**Objective:** Gain System-level access and retrieve the root flag.

## 1. Reconnaissance & Scanning
Initial observation of the web application revealed a file upload feature at `/upload/2/`. 

## 2. Gaining Access (Exploitation)
The server had a filter preventing `.php` files from being uploaded. 
* **Bypass Technique:** Renamed the web shell from `shell.php` to `shell.PhP`.
* **Result:** The server accepted the file and executed it as PHP code.

## 3. Enumeration & Privilege Escalation
Using the command execution shell, I verified my permissions:
- **Command:** `whoami`
- **Privilege Level:** `nt authority\system` (Full Administrative Access).

## 4. Post-Exploitation (Flag Retrieval)
I searched the Administrator's desktop for the flag file.
- **Search Command:** `dir C:\Users\Administrator\Desktop`
- **File Found:** `FLAG1.txt`
- **Read Command:** `type C:\Users\Administrator\Desktop\FLAG1.txt`
- **Flag Captured:** `PwnTillDawnAcademyIsAwesome!!!`

## 5. Clearing Tracks
To remove evidence of the compromise, the web shell was deleted from the server.
- **Command:** `del C:\xampp\htdocs\upload\2\shell.PhP`
- **Verification:** Navigating to the shell URL returned a **403 Forbidden** or **404 Not Found** error.

---
*Disclaimer: This walkthrough is for educational purposes only.*
