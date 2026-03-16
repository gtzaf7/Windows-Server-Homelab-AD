# Phase 6: Active Directory Logical Structure (OUs & Users)

**Objective:** Design and implement the organizational hierarchy within Active Directory using Organizational Units (OUs). This logical structure reflects the company's actual departments (IT, Accounting, Management) and prepares the environment to properly categorize our physical hardware (Host PC, Laptops, Tablet).

---

## Part 1: Logical Design 

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

> **Technical Explanation:** Creating a custom Root OU (like `_Corp`) isolates our actual company assets from the built-in Windows containers. This makes applying global security policies (GPOs) to our entire company much safer and easier. *Note: The Domain Controller (`DC01`) will remain in the default `Domain Controllers` container, which is an enterprise security best practice.*

---

## Part 2: Implementation Steps

**Step 1:** On `DC01`, open **Server Manager**, click **Tools**, and select **Active Directory Users and Computers (ADUC)**.

<br>
<img width="1226" height="420" alt="Image" src="https://github.com/user-attachments/assets/4a5fe7be-1c64-48fe-851c-a4d7d2fbef14" />
<br>

**Step 2:** Create the custom Root OU. Right-click the domain name (e.g., `homelab.local`), select **New > Organizational Unit**, name it `_Corp`, and ensure **"Protect container from accidental deletion"** is checked.

<br>
<img width="902" height="614" alt="Image" src="https://github.com/user-attachments/assets/670334ac-4cfa-4fad-8810-092c65c595bc" />
<img width="515" height="449" alt="Image" src="https://github.com/user-attachments/assets/3fe45cb2-b593-43b2-acf6-0d1c185c4068" />
<img width="258" height="234" alt="Image" src="https://github.com/user-attachments/assets/ffc9c9f5-a465-460d-b5d3-22a26a3f2cbc" />
<br>

**Step 3:** Create the departmental sub-OUs. Right-click the newly created `_Corp` OU, select **New > Organizational Unit**, and create `IT`, `Accounting`, and `Management`. 

<br>
<img width="200" height="100" alt="Image" src="https://github.com/user-attachments/assets/f8e5aad9-4229-4546-b74b-e899998bef1b" />
<br>

**Step 4:** Inside *each* of these three departmental OUs, create two sub-OUs: `Users` and `Computers`. This separates human accounts from machine accounts, allowing for precise policy targeting.

<br>
<img width="199" height="263" alt="Image" src="https://github.com/user-attachments/assets/ca8d55a2-161b-4433-bf85-fc453a5defe7" />
<br>

**Step 5:** Create a dedicated System Administrator account. Right-click the `_Corp \ IT \ Users` OU, select **New > User**. 
* **First Name:** `Admin`
* **Last Name:** `YourName` (e.g., Doe)
* **User logon name:** `admin`

<br>
<img width="522" height="442" alt="Image" src="https://github.com/user-attachments/assets/9fdb0cce-9df5-4b15-b6f9-4474e20801b4" />
<br>

Click **Next**, assign a strong password, check **"Password never expires"** (for lab purposes), and click **Finish**.

<br>
<img width="512" height="440" alt="Image" src="https://github.com/user-attachments/assets/33c78bcf-2336-404a-9a70-02b4c750f7b9" />
<img width="514" height="434" alt="Image" src="https://github.com/user-attachments/assets/6e470260-9a49-4868-9eaf-e02defd1db19" />
<br>

**Something Important:** When I clicked 'Finish', I got this error: 

<br>
<img width="505" height="230" alt="Image" src="https://github.com/user-attachments/assets/9ed762c7-8fa2-489c-ae4c-6abc73e955ec" />
<br>

This error indicates that the password for the new user does not meet the default Active Directory password complexity requirements. For the sake of this Lab, we are going to disable the password complexity requirements so we can use simpler passwords for testing.

**How to solve it:**
**Step 1:** On `DC01`, open **Server Manager**, click **Tools**, and select **Group Policy Management**.
**Step 2:** Expand the tree on the left: `Forest: homelab.local` > `Domains` > `homelab.local`.
**Step 3:** Right-click the **Default Domain Policy** and select **Edit...**. This opens the Group Policy Management Editor.

<br>
<img width="774" height="546" alt="Image" src="https://github.com/user-attachments/assets/e91022f3-e131-4ccb-8528-48c237081d05" />
<br>

**Step 4:** Navigate to the exact policy path: `Computer Configuration` > `Policies` > `Windows Settings` > `Security Settings` > `Account Policies` > `Password Policy`.
**Step 5:** On the right pane, double-click **Password must meet complexity requirements**, set it to **Disabled**, and click OK. 

<br>
<img width="815" height="593" alt="Image" src="https://github.com/user-attachments/assets/d255ff66-ea56-4897-8f36-8131bf3e09a7" />
<br>

**Step 6:** Close the Group Policy Editor. 
**Step 7:** To apply the changes immediately, open **Command Prompt** and type the following command: `gpupdate /force`

<br>
<img width="510" height="208" alt="Image" src="https://github.com/user-attachments/assets/922e5154-4c5c-4ecb-88d6-48f9869cbefb" />
<br>

Now with the password complexity requirements disabled, we can follow the previous steps to create the admin account.

<br>
<img width="716" height="447" alt="Image" src="https://github.com/user-attachments/assets/90ce77e7-1322-4820-8862-a508d624667a" />
<br>

**Step 6:** Grant the new user administrative privileges. Right-click the new user, select **Properties**, go to the **Member Of** tab, click **Add**, type `Domain Admins`, click **Check Names**, and then **OK**.

<br>
<img width="897" height="619" alt="Image" src="https://github.com/user-attachments/assets/33048959-f569-473f-9baf-4e2544b3b988" />
<br>

> **Technical Explanation & Security Groups:** > * **How Permissions Work:** In Active Directory, permissions are rarely assigned directly to individual users. Instead, we use **Security Groups**. By adding our user to the built-in **`Domain Admins`** group, they instantly inherit full administrative rights over the entire network. 
> * **Principle of Least Privilege:** It is a critical security violation to use the default built-in `Administrator` account for daily tasks. Creating a personalized admin account provides auditing accountability (knowing exactly *who* did *what*) and follows standard enterprise security frameworks.

**Step 7:** Populate the remaining departments with Standard Users. Following the same creation procedure (but **without** granting Domain Admin privileges), create standard employee accounts in their respective OUs:
* Navigate to `_Corp \ Accounting \ Users` and create a standard user (e.g., First Name: `Eve`, Last Name: `Doe`, Logon name: `eve`).
* Navigate to `_Corp \ Management \ Users` and create a standard user (e.g., First Name: `Jimmy`, Last Name: `Smith`, Logon name: `jimmy`).

<br>
<img width="631" height="134" alt="Image" src="https://github.com/user-attachments/assets/689e5c6a-1263-426b-8239-cbd51b5bf725" />
<img width="652" height="209" alt="Image" src="https://github.com/user-attachments/assets/8db7ed45-edae-47c4-9d16-5ba41c679c40" />
<br>

> **Technical Explanation:** Creating standard user accounts for everyday employees is a core security principle. These accounts have restricted privileges. If an employee's laptop or tablet is compromised by malware, the attacker cannot make domain-wide changes or access sensitive server configurations, which protects the entire network.

---

## Verification

The Active Directory Users and Computers console now perfectly reflects our logical design. The custom hierarchy is established for our departments, and a dedicated, trackable Domain Admin account is ready for use.

<br>
<img width="188" height="208" alt="Image" src="https://github.com/user-attachments/assets/a25c8ff1-e808-4d6f-a875-472f7df65a3d" />
<br>
