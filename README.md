# PwnDrive - Full System Compromise Walkthrough

**Target:** 10.150.150.11  
**Environment:** Windows / Apache (XAMPP)  
**Objective:** Gain RCE, escalate to SYSTEM, and retrieve FLAG1.txt.

---

## 1. Phase 1: Reconnaissance & Network Mapping

The engagement began by establishing a connection to the lab network and identifying the target host.

* **VPN Initialization:** Connection established via OpenVPN.
    ![VPN Connection](./screenshot/vpnconnection.png)
    ![VPN Connection](./screenshot/initializationsequencecomplete.png)
* **Host Discovery:** Nmap scan confirmed the target at `10.150.150.11`.
    ![Nmap Discovery](./screenshot/nmapscanning.png)
* **Service Enumeration:** A full port scan identified HTTP (80) and HTTPS (443) services running on an Apache server.
    ![Service Scan](./screenshot/image_6633c5.png)

---

## 2. Phase 2: Vulnerability Analysis

Upon visiting the web application, I identified a file upload portal named **PwnDrive**. 

* **Initial Access:** Navigated to the site to confirm it was live.
    ![Opening Web Portal](./Screenshots/openipwebsite.png)
* **Authentication:** Located the administrative login page to reach the upload interface.
    ![Login Interface](./Screenshots/loginpage.png)
* **Exploit Research:** `searchsploit` was used to check for known vulnerabilities in the PwnDrive software; however, no public exploits were found.
    ![Searchsploit Results](./Screenshots/searchsploit.png)

---

## 3. Phase 3: Exploitation (File Upload Bypass)

The server employed a blacklist filter to prevent the upload of `.php` files. 

* **The Bypass:** Uploaded a web shell using a case-sensitivity bypass by changing the extension to `.PhP`.
    ![Uploading Shell](./Screenshots/uploadshellfile.png)
* **Execution:** The server accepted the renamed file and processed it as executable code.
* **Access Verification:** Running the `whoami` command confirmed the shell was running as **`nt authority\system`**.
    ![RCE and Whoami Proof](./Screenshots/image_663429.png)

---

## 4. Phase 4: Post-Exploitation & Flag Retrieval

With SYSTEM privileges, I performed local enumeration to find the target data.

* **Directory Discovery:** Searched the Administrator's Desktop and found the flag file.
    ![Searching for Flag](./Screenshots/foundflagname.png)
* **Flag Location:** Confirmed the file path and presence of `FLAG1.txt`.
    ![Flag Path Confirmation](./Screenshots/locatedflag.png)
* **Flag Capture:** Used the `type` command to extract the secret key.
    ![Flag Captured](./Screenshots/image_5ac03d.png)

**Flag:** `PwnTillDawnAcademyIsAwesome!!!`

---

## 5. Phase 5: Clearing Tracks

To conclude the engagement, the web shell was deleted from the server to minimize the forensic footprint. 

* **Cleanup Command:** `del C:\xampp\htdocs\upload\2\shell.PhP`
    ![Executing Cleanup](./Screenshots/clearfile.png)
* **Verification:** A 403 Forbidden error confirmed the shell was successfully removed and the directory was secured.
    ![Cleanup Verification](./Screenshots/image_5acec6.png)

---
*Developed for educational purposes in a controlled lab environment.*
