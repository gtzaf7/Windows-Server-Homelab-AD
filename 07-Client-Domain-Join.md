# Phase 7: Client Network Configuration & Domain Join

**Objective:** Configure a Windows client machine (Workstation/Laptop) to communicate with the Domain Controller and officially join it to the `homelab.local` Active Directory domain. This process establishes trust between the client and the server, allowing for centralized authentication and policy management.

---

## Part 1: Network Configuration 

Before a computer can join a domain, it must be able to "find" the Domain Controller on the network. By default, client computers use the router's DNS or public DNS (like Google's `8.8.8.8`), which have no idea what `homelab.local` is. We must point the client's DNS directly to our server.

**Step 1:** On the Client Windows machine, open **Network Connections** (`ncpa.cpl`), right-click the active network adapter, and select **Properties**.
**Step 2:** Double-click **Internet Protocol Version 4 (TCP/IPv4)**.
**Step 3:** You can leave the IP address as Automatic (DHCP) or set a Static IP, but you **must** change the **Preferred DNS server** to the IP address of `DC01`:
* **Preferred DNS Server:** `192.168.2.9`
Click **OK** to save.

<br>
<img width="450" alt="Client DNS Configuration" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_ΜΕ_ΤΗΝ_IP_ΤΟΥ_CLIENT" />
<br>

**Step 4:** Open **Command Prompt** on the client and verify connectivity by typing: `ping homelab.local`. It should successfully reply with the server's IP (`192.168.2.9`), proving that DNS resolution is working.

> **Technical Rationale:** Active Directory is critically dependent on DNS. The Domain Controller (`DC01`) holds the specific DNS records (SRV records) that tell the client exactly where to send authentication requests. If the client's DNS points anywhere else, the domain join will fail with a "Domain Controller cannot be found" error.

---

## Part 2: Joining the Domain

**Step 5:** On the Client machine, open **Settings > System > About** and click on **Advanced system settings** (or **Domain or workgroup** depending on the Windows version).
**Step 6:** In the System Properties window, go to the **Computer Name** tab and click **Change...**
**Step 7:** First, ensure the **Computer name** is something logical (e.g., `DESKTOP-IT` or `LAPTOP-01`). Then, under "Member of", select **Domain** and type: `homelab.local`. Click **OK**.

<br>
<img width="450" alt="Domain Join Screen" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_ΤΟΥ_DOMAIN_JOIN" />
<br>

**Step 8:** A Windows Security prompt will appear. You must enter the credentials of an account with permission to join computers to the domain. Enter the Domain Admin credentials we created in Phase 6:
* **Username:** `homelab\admin` (or `admin@homelab.local`)
* **Password:** *[Your Admin Password]*
Click **OK**.

**Step 9:** A welcome message saying *"Welcome to the homelab.local domain."* will appear. Click **OK**, and **Restart** the client computer.

<br>
<img width="450" alt="Welcome to Domain" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_WELCOME" />
<br>

---

## Part 3: Verification & Organization

**Step 10:** After the client restarts, notice the login screen. Instead of the local user, select **Other user**. You can now log in using the Domain Admin account or one of the standard user accounts we created earlier (e.g., the Accounting or Management user).

**Step 11:** Switch back to the Server (`DC01`), open **Active Directory Users and Computers**, and click on the default **Computers** container. The newly joined client machine will appear here.

**Step 12:** Right-click the client computer object, select **Move...**, and place it into its appropriate Organizational Unit according to our Phase 6 design (e.g., `_Corp \ IT \ Computers`).

<br>
<img width="800" alt="Moving Computer to OU" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_ΜΕ_ΤΟ_MOVE_ΣΤΟ_ADUC" />
