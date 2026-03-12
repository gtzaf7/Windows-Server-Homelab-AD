# Phase 5: Active Directory Domain Services (AD DS) Configuration

**Objective:** Install the Active Directory Domain Services role and promote the standalone server to the primary Domain Controller of a brand new forest. This establishes the centralized identity and access management system for the entire network.

---

## Part 1: Installing the AD DS Role

**Step 1:** Open **Server Manager**, click on the **Manage** menu at the top right, and select **Add Roles and Features**. Click **Next** on the initial Before you begin screen.

<img width="1223" height="261" alt="image" src="https://github.com/user-attachments/assets/f7ed247d-e86f-46f2-9c08-5d31ea3da635" />

<img width="1229" height="324" alt="image" src="https://github.com/user-attachments/assets/75fd9df6-905f-46da-bd8a-e1196e3c80eb" />

**Step 2:** On the **Installation Type** screen, leave the default selection: **Role-based or feature-based installation** and click **Next**.

<br>
<img width="681" height="226" alt="image" src="https://github.com/user-attachments/assets/aade3909-ad3d-439d-8246-f9ee935c1455" />
<br>

> **Technical Rationale:** We choose "Role-based" because we are configuring foundational infrastructure services (in this case, Active Directory) on a single, specific server. The alternative option, "Remote Desktop Services installation," launches a specialized wizard used exclusively for deploying Virtual Desktop Infrastructure (VDI) or session-based desktop farms across multiple servers, which is entirely unrelated to building a Domain Controller.

**Step 3:** On the **Server Selection** screen, ensure `DC01` is highlighted in the server pool and click **Next**. 

<img width="689" height="467" alt="image" src="https://github.com/user-attachments/assets/fe0c1f5c-ccd2-4270-8f1e-4c05c2df1f42" />

**Step 4:** On the **Server Roles** screen, check the box for **Active Directory Domain Services**. A prompt will automatically appear asking to add required management features (like the AD DS Tools). Click **Add Features** and then click **Next**.

<br>
<img width="444" height="450" alt="image" src="https://github.com/user-attachments/assets/85cbaacb-9a83-44e4-839e-0aaafd4fe825" />

<img width="475" height="457" alt="image" src="https://github.com/user-attachments/assets/1848ce9d-6888-4f7e-bf3a-f8e8ec740c48" />

<br>

> **Technical Rationale:** The AD DS role installs the necessary underlying database engine (`NTDS.dit`) and management MMC snap-ins (like Active Directory Users and Computers) onto the server. However, simply installing the role does not make the server a Domain Controller; it merely prepares the operating system binaries for the promotion process.

**Step 5:** Continue clicking **Next** through the Features and AD DS informational screens without making any changes, and finally click **Install**. Wait for the installation progress bar to complete.

<br>
<img width="691" height="479" alt="image" src="https://github.com/user-attachments/assets/656b027c-c838-4354-bc5c-9f58b0835d03" />
<br>
---

## Part 2: Promoting to a Domain Controller

**Step 4:** Once the role installation finishes, a yellow warning flag will appear at the top of the Server Manager. Click it and select **Promote this server to a domain controller**.

<br>
<img width="1221" height="532" alt="image" src="https://github.com/user-attachments/assets/c812f4b7-d040-45e6-9505-dbcf532c70bf" />

<img width="1051" height="525" alt="image" src="https://github.com/user-attachments/assets/33c2a2d6-38c8-4fcb-a408-1dc9a919ce1e" />
<br>

**Step 5: Deployment Configuration**
Select **Add a new forest**. In the **Root domain name** field, type your desired internal domain name (e.g., `homelab.local` or `ad.yourname.com`) and click **Next**.

<br>
<img width="564" height="244" alt="image" src="https://github.com/user-attachments/assets/874690b3-3d9e-40d0-8d3d-3390ce7d557c" />
<br>

> **Technical Rationale:** We select "Add a new forest" because this is the very first Domain Controller in our environment. Using a `.local` or an internal-only top-level domain is a common practice for isolated lab environments to prevent DNS conflicts with public internet domains.

**Step 6: Domain Controller Options**
Ensure both **Domain Name System (DNS) server** and **Global Catalog (GC)** are checked. Enter a strong **Directory Services Restore Mode (DSRM)** password and confirm it.

<br>
<img width="549" height="371" alt="image" src="https://github.com/user-attachments/assets/45eb4510-3604-4e36-bc74-48a939b68c17" />
<br>

> **Technical Rationale:** > * **DNS Server:** Active Directory cannot function without DNS. Because no other DNS server exists in our lab to handle AD records, this DC must host the DNS zone.
> * **DSRM Password:** This is a critical "safe mode" password for Active Directory. It is uniquely used by administrators to log into the database offline in case of catastrophic corruption or disaster recovery scenarios.

**Step 7: DNS Options & Additional Options**
Ignore the warning stating that a delegation for this DNS server cannot be created and click **Next**. Verify the NetBIOS domain name assigned automatically (e.g., `HOMELAB`) and click **Next**.
<img width="574" height="122" alt="image" src="https://github.com/user-attachments/assets/bbb62a29-aa75-4f3d-8e49-eb8c314146b3" />

> **Technical Rationale:** The DNS delegation warning is completely normal when creating the first DNS server in a new root forest, as there is no parent DNS zone to delegate authority from.

**Step 8: Paths**
Leave the database (`NTDS.dit`), log files, and SYSVOL folders in their default `C:\Windows\...` locations and click **Next**.
<br>
<img width="656" height="229" alt="image" src="https://github.com/user-attachments/assets/757502e8-1561-47ae-a2d3-a5498fd9cdfa" />
<br>

**Step 9: Prerequisites Check & Install**
Review the selections. Once the prerequisites check passes successfully (indicated by a green checkmark), click **Install**.

<br>
<img width="661" height="488" alt="image" src="https://github.com/user-attachments/assets/92d40e9a-e07e-4e4f-abe7-fd6b868017cb" />
<br>

**The server will automatically reboot upon completion.**

---

## Verification

After the reboot, press `Ctrl + Alt + Delete`. Notice that the login screen has changed. Instead of a local administrator, you must now log in with domain credentials (e.g., `HOMELAB\Administrator`). 

<br>
<img width="1194" height="730" alt="image" src="https://github.com/user-attachments/assets/741365ba-6b9f-4ab2-a23f-05a8e2c5bdbd" />

<br>
Open **Server Manager** > **Tools** and verify that the Active Directory management snap-ins (such as *Active Directory Users and Computers*) are now available, confirming the successful promotion.

<img width="1225" height="867" alt="image" src="https://github.com/user-attachments/assets/619155db-90bf-4385-b977-333e163f60b8" />

---

---

## 💡 Summary: What Did We Just Build? 

If you are completely new to IT infrastructure, the technical terms above might seem overwhelming. Here is the simplest way to understand exactly what we achieved in this phase, using the analogy of a massive, exclusive **VIP Resort**:

* **The Forest (The Outer Wall):** Think of the Forest as the entire VIP Resort property. It is the absolute highest security boundary. A resort can contain multiple different clubs, hotels, or restaurants inside it, but everything within the property walls follows the same ultimate rules. We just laid the foundation for this entire resort.
* **The Domain (`homelab.local`):** This is one specific, highly exclusive VIP Club *inside* our new resort. Right now, the building is empty, but soon it will be full of people (Users) and equipment (Computers).
* **Active Directory (AD DS):** This is the club's Master Guest List. It is a highly secure database containing the names, passwords, and access levels of every single person allowed inside.
* **The Domain Controller (`DC01`):** Before today, our server was just a regular guy standing outside. By "promoting" him, we officially gave him the clipboard with the Master Guest List and made him the Head Bouncer of the domain.

**The End Result:** From now on, whenever someone tries to log into *any* computer connected to this network, they are essentially walking up to the door of the VIP Club. The computer will silently ask `DC01` (the Head Bouncer): *"Is this person on the list? Did they type the correct password? Are they allowed to enter?"* We have successfully built the ultimate gatekeeper for our entire network.

<img width="1408" height="768" alt="image" src="https://github.com/user-attachments/assets/535996dc-339a-49fd-8e28-db536b439534" />

