# Phase 7: Client Network Configuration & Domain Join

**Objective:** Configure a Windows client machine (Workstation/Laptop) to communicate with the Domain Controller and officially join it to the `homelab.local` Active Directory domain. This process establishes trust between the client and the server, allowing for centralized authentication and policy management.

---

## Part 1: Network Configuration 

Before a computer can join a domain, it must be able to "find" the Domain Controller on the network. By default, client computers use the router's DNS or public DNS (like Google's `8.8.8.8`), which have no idea what `homelab.local` is. We must point the client's DNS directly to our server.

**Step 1:** On the Client Windows machine, open **Network Connections** (`ncpa.cpl`), right-click the active network adapter, and select **Properties**.

<br>
<img width="400" height="204" alt="Image" src="https://github.com/user-attachments/assets/b280561e-50b8-4a14-bb08-942d8b1c3726" />
<img width="422" height="269" alt="Image" src="https://github.com/user-attachments/assets/95504e6a-3db3-4dba-b1f8-6e04f5b7b117" />
<br>

**Step 2:** Double-click **Internet Protocol Version 4 (TCP/IPv4)**.

**Step 3:** You can leave the IP address as Automatic (DHCP) or set a Static IP, but you **must** change the **Preferred DNS server** to the IP address of `DC01`:
* **Preferred DNS Server:** `192.168.2.9`

Click **OK** to save.

<br>
<img width="774" height="469" alt="Image" src="https://github.com/user-attachments/assets/a83f1e5e-d8e6-4e06-94ad-8c23210d5a15" />
<br>

**Step 4:** Open **Command Prompt** on the client and verify connectivity by typing: `ping homelab.local`. It should successfully reply with the server's IP (`192.168.2.9`), proving that DNS resolution is working.

**SOMETHING IMPORTANT:** So, when I changed the Preferred DNS to the server's IP I got a response. On the other hand, when I pinged the `homelab.local` I got this response: `Ping request could not find host homelab.local. Please check the name and try again.`

**WHAT IS THE CAUSE?** Modern Windows operating systems are programmed to prioritize IPv6 over IPv4. Because this Virtual Machine's network adapter is bridged to the host's home network, the physical router was silently assigning an IPv6 DNS server to the virtual network adapter. The VM prioritized this external IPv6 DNS (which knows nothing about our internal `homelab.local` domain) and completely bypassed our custom IPv4 DNS setting.

**HOW TO FIX IT?** 1. Open **Network Connections** (`ncpa.cpl`), right-click the active network adapter, and select **Properties**.
2. Find the **Internet Protocol Version 6 (TCP/IPv6)**, and **uncheck** the box to disable it.
3. Click **OK**.
4. Open CMD and type `ipconfig /flushdns` to clear the cached failed attempts.
5. Retrying `ping homelab.local` now successfully resolves to `192.168.2.9`.

<br>
<img width="548" height="237" alt="Image" src="https://github.com/user-attachments/assets/42b07e8a-e910-4a82-bdb5-dcc27c8705a8" />
<br>

> **Technical Explanation:** Active Directory is critically dependent on DNS. The Domain Controller (`DC01`) holds the specific DNS records (SRV records) that tell the client exactly where to send authentication requests. If the client's DNS points anywhere else, the domain join will fail with a "Domain Controller cannot be found" error.

---

## Part 2: Joining the Domain

**Step 5:** On the Client machine, open **Settings > System > About** and click on **Advanced system settings** (or **Domain or workgroup** depending on the Windows version).

**Step 6:** In the System Properties window, go to the **Computer Name** tab and click **Change...**

**Step 7:** First, ensure the **Computer name** is something logical (e.g., `DESKTOP-IT` or `LAPTOP-01`). Then, under "Member of", select **Domain** and type: `homelab.local`. Click **OK**.

<br>
<img width="641" height="384" alt="Image" src="https://github.com/user-attachments/assets/e103e929-e4d7-44af-b85b-96a7c77825c6" />
<img width="313" height="384" alt="Image" src="https://github.com/user-attachments/assets/7010210c-ccf0-4781-8344-bf30e5001e3f" />
<br>

**Step 8:** A Windows Security prompt will appear. You must enter the credentials of an account with permission to join computers to the domain. Enter the Domain Admin credentials we created in Phase 6:
* **Username:** `homelab\admin` (or `admin@homelab.local`)
* **Password:** *[Your Admin Password]*

Click **OK**.

<br>
<img width="446" height="415" alt="Image" src="https://github.com/user-attachments/assets/3c0ffec9-cf45-426e-9370-6ec620f87fdb" />
<br>

**Step 9:** A welcome message saying *"Welcome to the homelab.local domain."* will appear. Click **OK**, and **Restart** the client computer.

<br>
<img width="291" height="150" alt="Image" src="https://github.com/user-attachments/assets/73d037d4-8cbe-4304-a7c2-fdd657220c9f" />
<img width="351" height="182" alt="Image" src="https://github.com/user-attachments/assets/837badef-5618-4a45-882c-a50086a9ce80" />
<br>

---

## Part 3: Verification & Organization

**Step 10:** After the client restarts, notice the login screen. Instead of the local user, select **Other user**. You can now log in using the Domain Admin account or one of the standard user accounts we created earlier (e.g., the Accounting or Management user).

<br>
<img width="931" height="699" alt="Image" src="https://github.com/user-attachments/assets/1ece0ea4-d015-40da-8314-29bf0277625a" />
<img width="417" height="473" alt="Image" src="https://github.com/user-attachments/assets/fcd6dd5b-b767-4328-beff-76e8e5c6e983" />
<br>

**Step 11:** Now, let's switch back to the Server (`DC01`), open **Active Directory Users and Computers**, and click on the default **Computers** container. The newly joined client machine will appear here.

<br>
<img width="700" height="263" alt="Image" src="https://github.com/user-attachments/assets/3e4e5960-713b-4b20-ba66-c7679a936454" />
<br>

**Step 12:** Right-click the client computer object, select **Move...**, and place it into its appropriate Organizational Unit according to our Phase 6 design (e.g., `_Corp \ IT \ Computers`).

<br>
<img width="495" height="386" alt="Image" src="https://github.com/user-attachments/assets/a093ae1f-8277-43ee-bdb2-f1f28143317d" />
<br>

---

## Part 4: Scaling the Network (Adding the Remaining Departments)

To fully populate our simulated corporate environment and utilize the Organizational Units (OUs) created in Phase 6, the exact same Domain Join procedure is repeated for the remaining hardware.

**The Process for Additional Workstations:**
For each new client machine (e.g., the Accounting Laptop and the Management Tablet/VM), the following steps are executed:
1. **Network Config:** Point the client's IPv4 DNS to `DC01` (`192.168.2.9`) and disable IPv6 to prevent resolution conflicts.
2. **Domain Join:** Rename the PC (e.g., `LAPTOP-ACC`, `TABLET-MAN`) and join it to `homelab.local` using the Domain Admin credentials.
3. **Active Directory Organization:** On the Server, move the newly joined computer objects from the default `Computers` container into their respective OUs (`_Corp \ Accounting \ Computers` and `_Corp \ Management \ Computers`).
4. **User Login:** Upon restart, employees log in using their specific standard user accounts (e.g., `eve` for Accounting, `jimmy` for Management).

> **Technical Explanation:** By moving the newly joined computers into their dedicated departmental OUs, we prepare the environment for targeted Group Policy Objects (GPOs). For example, a security policy designed for the Accounting computers will not accidentally affect the Management devices.
