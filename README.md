# PwnDrive - Full System Compromise Walkthrough

**Target:** 10.150.150.11  
**Environment:** Windows / Apache (XAMPP)  
**Objective:** Gain RCE, escalate to SYSTEM, and retrieve FLAG1.txt.

## 1. Phase 1: Reconnaissance & Network Mapping

The engagement began by establishing a connection to the lab network and identifying the target host.

* **VPN Initialization:** Connection established via OpenVPN.
    ![VPN Connection](./Screenshots/image_66338a.png)
* **Host Discovery:** Nmap scan confirmed the target at `10.150.150.11`.
    ![Nmap Discovery](./Screenshots/image_6633a8.png)
* **Service Enumeration:** A full port scan identified HTTP (80) and HTTPS (443) services running on an Apache server.
    ![Service Scan](./Screenshots/image_6633c5.png)

## 2. Phase 2: Vulnerability Analysis

Upon visiting the web application, I identified a file upload portal named **PwnDrive**. 

* **Logic Testing:** I verified that the platform was active and reachable.
    ![PwnDrive Interface](./Screenshots/image_6633cc.png)
* **Exploit Research:** `searchsploit` was used to check for known vulnerabilities in the PwnDrive software; however, no public exploits were found, indicating a need for a manual bypass.
    ![Searchsploit Results](./Screenshots/image_663403.png)

## 3. Phase 3: Exploitation (File Upload Bypass)

The server employed a blacklist filter to prevent the upload of `.php` files. 

* **The Bypass:** By utilizing a case-sensitivity bypass, I renamed the payload to `shell.PhP`.
* **Execution:** The server accepted the renamed file and processed it as executable code.
* **Access Verification:** Running the `whoami` command confirmed the shell was running as **`nt authority\system`**.
    ![RCE and Whoami Proof](./Screenshots/image_663429.png)

## 4. Phase 4: Post-Exploitation & Flag Retrieval

With SYSTEM privileges, I performed local enumeration to find the target data.

* **Host Identification:** Confirmed the hostname of the compromised machine.
    ![Hostname Result](./Screenshots/image_663446.png)
* **File Discovery:** Located `FLAG1.txt` on the Administrator's Desktop.
    ![Directory Listing](./Screenshots/image_5abc18.png)
* **Flag Capture:** Used the `type` command to extract the secret key.
    ![Flag Captured](./Screenshots/image_5ac03d.png)

**Flag:** `PwnTillDawnAcademyIsAwesome!!!`

## 5. Phase 5: Clearing Tracks

To conclude the engagement, the web shell was deleted from the server to minimize the forensic footprint. 

* **Cleanup Command:** `del C:\xampp\htdocs\upload\2\shell.PhP`
* **Verification:** A 403 Forbidden error confirmed the shell was successfully removed.
    ![Cleanup Verification](./Screenshots/image_5acec6.png)

---
*Developed for educational purposes in a controlled lab environment.*
