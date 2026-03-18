# Phase 4: Initial Server Configuration (Post-Installation)

**Objective:** Execute the standard IT Server Provisioning Checklist to prepare the Windows Server for its role as a Domain Controller. This includes establishing a permanent identity, assigning a static IP address, enabling Remote Desktop (RDP) for remote management, configuring the firewall, ensuring accurate time synchronization, and applying critical Windows Updates.

---

## Configuration Steps

**Step 1: Hostname Configuration** Open **Server Manager**, navigate to **Local Server**, and click on the randomly generated Computer name. Click **Change...**, set the computer name to **`DC01`**, and ensure the **Workgroup** option remains selected (default: WORKGROUP). *(Do not restart yet)*.

<br>
<img width="1228" height="916" alt="image" src="https://github.com/user-attachments/assets/24c5b094-afd1-4209-93b6-ea78a96fa878" />

<img width="1194" height="604" alt="image" src="https://github.com/user-attachments/assets/c4a01329-64e3-4673-90ba-66a8d6452e4c" />

<img width="469" height="394" alt="image" src="https://github.com/user-attachments/assets/eec6b5a0-a738-4ece-b1cb-60fdfa581ee9" />

<img width="376" height="454" alt="image" src="https://github.com/user-attachments/assets/0cfa626c-1767-4e69-9b98-2a361518d753" />


<br>

> **Technical Rationale:** > * **Hostname (`DC01`):** A Domain Controller must have a fixed, identifiable hostname before promotion. Renaming a server *after* Active Directory is installed is a complex process that can easily corrupt the domain database.
> **Workgroup Selection:** We deliberately leave the server in a "Workgroup" at this stage because our target Domain does not exist yet. This specific server will be the one to create the new domain from scratch in the next phase (Active Directory Promotion). Attempting to select "Domain" here would result in an error.


**Step 2: Network Configuration (Static IP)**
First, open **Command Prompt** and type `ipconfig`. We do this to identify the current IPv4 address, Subnet Mask, and Default Gateway automatically assigned to our server by the local DHCP server (the router). We will use this dynamically assigned IP and convert it into our permanent static IP to ensure it's valid for our subnet.

<br>
<img width="999" height="505" alt="image" src="https://github.com/user-attachments/assets/876c34ce-1974-4cb9-8490-cda4d3ec9bd1" />

<br>

Next, open **Network Connections**, right-click the network adapter (`Ethernet0`), and select **Properties**. Double-click **Internet Protocol Version 4 (TCP/IPv4)** and manually apply the configuration we gathered:
* **IP Address:** `192.168.2.9`
* **Subnet Mask:** `255.255.255.0`
* **Default Gateway:** `192.168.2.1`
* **Preferred DNS Server:** `127.0.0.1`

<br>
<img width="464" height="537" alt="image" src="https://github.com/user-attachments/assets/653e1b6f-8a88-4830-92b0-c5197711017b" />

<br>

> **Technical Rationale:** Domain Controllers strictly require a static IP address, as clients need a permanent, unchanging address to query for DNS and authentication. The Preferred DNS is set to the loopback address (`127.0.0.1`) because this server will be promoted to a DNS Server itself during the Active Directory setup.

**Step 3: Enable Remote Desktop (RDP)**
In **Server Manager > Local Server**, click on **Remote Desktop** (currently Disabled) and check **"Allow remote connections to this computer"**.

<img width="1207" height="616" alt="image" src="https://github.com/user-attachments/assets/a0271ca4-4884-4223-a892-e2cc098088a5" />

<img width="476" height="549" alt="image" src="https://github.com/user-attachments/assets/5f6b5235-6f72-4dd6-9b8f-b1a13b0d6f3f" />


<img width="283" height="20" alt="image" src="https://github.com/user-attachments/assets/ead225de-1122-4c59-a55f-fb1cced31141" />

> **Technical Rationale:** Enabling RDP is an enterprise standard. It allows System Administrators to manage the server remotely via the network without requiring direct access to the hypervisor console.

**Step 4: Windows Firewall (Enable ICMPv4/Ping)**
Open **Windows Defender Firewall with Advanced Security**, navigate to **Inbound Rules**, locate the rule named **File and Printer Sharing (Echo Request - ICMPv4-In)**, right-click, and select **Enable Rule**.

<img width="420" height="122" alt="image" src="https://github.com/user-attachments/assets/bc95d159-5c02-40eb-a235-46517bb3ef7f" />

<img width="430" height="243" alt="image" src="https://github.com/user-attachments/assets/50587d05-5d67-4975-b3b5-14ce53c7ddb9" />

<img width="752" height="718" alt="image" src="https://github.com/user-attachments/assets/07d61e5b-c804-4ebd-a386-e3dbacf2ea3a" />

<img width="738" height="731" alt="image" src="https://github.com/user-attachments/assets/25842db1-9bd2-4c4a-8df1-52a34b7da619" />


> **Technical Rationale:** By default, Windows Server blocks incoming Ping requests for security purposes. Enabling this specific rule allows the physical host machine and future client VMs to test network connectivity to the Domain Controller during network troubleshooting.

**Step 5: Time Synchronization (NTP)**
First, we must ensure the virtual machine does not inherit time from the physical host. Right-click the `DC01` VM in VMware Workstation, go to **Settings > Options > VMware Tools**, and uncheck **"Synchronize guest time with host"**.

<br>
<img width="746" height="728" alt="image" src="https://github.com/user-attachments/assets/b835f71c-d57a-4d72-9750-f1c9d949eb82" />
<br>

Before synchronizing, ensure your server is set to the correct Timezone so that the fetched UTC time translates correctly to your local display time. Open **Command Prompt** as Administrator and set the timezone (e.g., for Greece):

`tzutil /s "GTB Standard Time"`

After, open **Command Prompt** as Administrator and configure the `w32time` service to sync with external Greek NTP servers using the following commands:

`w32tm /config /manualpeerlist:"0.gr.pool.ntp.org 1.gr.pool.ntp.org 2.gr.pool.ntp.org 3.gr.pool.ntp.org" /syncfromflags:manual /reliable:YES /update`

Restart **Windows Time Service**

`net stop w32time`
`net start w32time`

**Force Synchronization**

`w32tm /resync /force`

**Verify the Configuration**

`w32tm /query /status`

<img width="1205" height="794" alt="image" src="https://github.com/user-attachments/assets/34de8407-797e-41cd-ad72-bf731eb5f2ac" />


> **Technical Rationale:** Active Directory relies heavily on the **Kerberos protocol** for authentication, which strictly requires all machines in the domain to have synchronized clocks (typically within a 5-minute tolerance). Configuring a reliable external NTP server prevents catastrophic authentication failures.

**Step 6: Security and Patch Management (Windows Updates)**
Open **Settings > Update & Security > Windows Update** and click **Check for updates**. Install all available critical and security updates.

<img width="901" height="778" alt="image" src="https://github.com/user-attachments/assets/c4c3b3a6-1960-4ee6-8196-b43e502dfeea" />

<img width="263" height="62" alt="image" src="https://github.com/user-attachments/assets/f0118185-4372-4e15-969c-34db784aec26" />

<img width="185" height="81" alt="image" src="https://github.com/user-attachments/assets/99d8c590-fbf2-4dc3-8af6-dbbddc237dd3" />


> **Technical Rationale:** A Domain Controller is the most critical security asset in an infrastructure. Before promoting a server to a DC, it must be fully patched against known vulnerabilities to ensure a secure foundation.

**Step 7: System Reboot**
**Restart** the server to apply the updates, the new hostname, and finalize all network and system configurations.

---

## Verification

After rebooting, the Server Manager dashboard confirms the correct hostname (`DC01`), the assigned static IP (`192.168.2.9`), and that Remote Desktop is Enabled. Furthermore, opening a Command Prompt from the physical host machine and executing `ping 192.168.2.9` returns successful replies, confirming end-to-end network connectivity.

<img width="1058" height="642" alt="image" src="https://github.com/user-attachments/assets/17185136-e898-43d3-a8ee-6f9c1c7530c8" />

<img width="530" height="238" alt="image" src="https://github.com/user-attachments/assets/4cb5dd91-49a8-4722-8cd7-0a73ffd05863" />
