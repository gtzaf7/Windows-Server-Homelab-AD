# Phase 6: Active Directory Logical Structure (OUs & Users)

**Objective:** Design and implement the organizational hierarchy within Active Directory using Organizational Units (OUs). This logical structure reflects the company's actual departments (IT, Accounting, Management) and prepares the environment to properly categorize our physical hardware (Host PC, Laptops, Tablet).

---

## Part 1: Logical Design (The Blueprint)

Before creating any objects, a System Administrator must map out the directory structure. Instead of dumping all users and computers into the default built-in containers (which cannot receive Group Policies natively), we create a custom Root OU.

**Our Proposed Hierarchy:**
* `homelab.local` (Domain Root)
  * `_Corp` (Custom Root OU - *The underscore ensures it stays at the top of the list*)
    * `IT`
      * `Users` (For IT personnel/Admins)
      * `Computers` (Target destination for the Host PC)
    * `Accounting`
      * `Users` (For standard employees)
      * `Computers` (Target destination for Laptop #1)
    * `Management`
      * `Users` (For Executives/Managers)
      * `Computers` (Target destination for Laptop #2 & the Windows Tablet)

> **Technical Rationale:** Creating a custom Root OU (like `_Corp`) isolates our actual company assets from the built-in Windows containers. This makes applying global security policies (GPOs) to our entire company much safer and easier. *Note: The Domain Controller (`DC01`) will remain in the default `Domain Controllers` container, which is an enterprise security best practice.*

---

## Part 2: Implementation Steps

**Step 1:** On `DC01`, open **Server Manager**, click **Tools**, and select **Active Directory Users and Computers (ADUC)**.

**Step 2:** Create the custom Root OU. Right-click the domain name (e.g., `homelab.local`), select **New > Organizational Unit**, name it `_Corp`, and ensure **"Protect container from accidental deletion"** is checked.

<br>
<img width="600" alt="Create Root OU" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_ΜΕ_ΤΗ_ΔΗΜΙΟΥΡΓΙΑ_ΤΟΥ_CORP" />
<br>

**Step 3:** Create the departmental sub-OUs. Right-click the newly created `_Corp` OU, select **New > Organizational Unit**, and create `IT`, `Accounting`, and `Management`. 

**Step 4:** Inside *each* of these three departmental OUs, create two sub-OUs: `Users` and `Computers`. This separates human accounts from machine accounts, allowing for precise policy targeting.

**Step 5:** Create a dedicated System Administrator account. Right-click the `_Corp \ IT \ Users` OU, select **New > User**. 
* **First Name:** `Admin`
* **Last Name:** `YourName` (e.g., Doe)
* **User logon name:** `admin.yourname`

Click **Next**, assign a strong password, check **"Password never expires"** (for lab purposes), and click **Finish**.

<br>
<img width="600" alt="Create Admin User" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_ΜΕ_ΤΟΝ_ΝΕΟ_ΧΡΗΣΤΗ" />
<br>

**Step 6:** Grant the new user administrative privileges. Right-click the new `admin.yourname` user, select **Properties**, go to the **Member Of** tab, click **Add**, type `Domain Admins`, click **Check Names**, and then **OK**.

> **Technical Rationale (Principle of Least Privilege):** It is a critical security violation to use the default `Administrator` account for daily tasks. Creating a personalized admin account provides auditing accountability (knowing *who* did *what*) and follows standard enterprise security frameworks.

---

## Verification

The Active Directory Users and Computers console now perfectly reflects our logical design. The custom hierarchy is established for our departments, and a dedicated, trackable Domain Admin account is ready for use.

<br>
<img width="800" alt="ADUC Final Structure" src="ΒΑΛΕ_ΕΔΩ_ΤΟ_LINK_ΤΗΣ_ΤΕΛΙΚΗΣ_ΦΩΤΟΓΡΑΦΙΑΣ_ΜΕ_ΟΛΑ_ΤΑ_OUS" />
