# Phase 3: Operating System Installation

**Objective:** Deploy the Windows Server OS on the newly provisioned virtual machine, ensure the correct Graphical User Interface (GUI) is active, and install virtualization integration drivers for optimal performance.

---

## Installation Steps

**Step 1:** In VMware Workstation, right-click the `DC01` virtual machine and select **Settings**. Navigate to **CD/DVD (SATA)**, check **Connect at power on**, select **Use ISO image file**, and browse to your downloaded Windows Server Evaluation ISO.

<br>
<img width="405" height="203" alt="image" src="https://github.com/user-attachments/assets/1397f02d-a1ab-4e06-83d8-15537e920f50" />

<img width="756" height="726" alt="image" src="https://github.com/user-attachments/assets/84376071-2b92-4326-87da-d894df5e22a0" />

<br>

> **Technical Explanation:** By manually mounting the ISO after the VM creation (instead of during), we maintain full control over the boot process and prevent the hypervisor from executing an automated, unattended installation.

**Step 2:** Power on the virtual machine and immediately click inside the console. Press any key when prompted with *“Press any key to boot from CD or DVD…”* to initialize the Windows Setup.
<img width="631" height="490" alt="image" src="https://github.com/user-attachments/assets/d57e02da-c6b4-4851-80ac-2d4efda3bfb7" />

<img width="1177" height="343" alt="image" src="https://github.com/user-attachments/assets/a81e1a59-9d10-430b-b447-af2672827640" />

<img width="394" height="456" alt="image" src="https://github.com/user-attachments/assets/17f3a3d8-8eae-41ea-b0bb-1bee200d042b" />


**Step 3:** Proceed through the initial language selection screen and click **Install Now**.
<img width="697" height="550" alt="image" src="https://github.com/user-attachments/assets/883b8c7f-ff01-4b15-b17a-f375af2f27ea" />

<img width="695" height="545" alt="image" src="https://github.com/user-attachments/assets/a7a0e8ab-f4b5-4fb7-a602-e11acaf2f8af" />

<img width="694" height="542" alt="image" src="https://github.com/user-attachments/assets/db1d0646-0b12-41f3-8d01-2a862b0567ec" />


**Step 4:** On the Operating System selection screen, explicitly select **Windows Server Standard Evaluation (Desktop Experience)**.

<br>
<img width="698" height="545" alt="image" src="https://github.com/user-attachments/assets/53782760-968b-44b0-993d-b0ef6b2cdd93" />

<br>

> **Technical Explanation:** Selecting the standard option defaults to a "Server Core" installation (command-line only). The **Desktop Experience** installs the full GUI, which is essential for our lab environment to visually manage Active Directory using Microsoft Management Console (MMC) snap-ins like *Active Directory Users and Computers* and *Server Manager*.

**Step 5:** Accept the licensing terms.
<img width="696" height="539" alt="image" src="https://github.com/user-attachments/assets/87bad484-9d8d-4b59-93a5-c52685de422e" />

**Step 6:** Select the 50 GB unallocated drive created in Phase 2 and click **Next** to begin the installation.
<img width="707" height="543" alt="image" src="https://github.com/user-attachments/assets/c2e4b6bd-88b7-488c-9bfd-17c81049dd72" />
<img width="706" height="546" alt="image" src="https://github.com/user-attachments/assets/62aa763c-c9d4-4b8f-a467-35adafad7fbb" />

**Step 7:** Once the installation completes and the VM reboots, establish a strong, secure password for the built-in local `Administrator` account.

<img width="935" height="605" alt="image" src="https://github.com/user-attachments/assets/24ec8dc0-5657-4ca5-8c52-3ddac6cf8e7c" />


**Step 8:** Log into the Windows Server environment. 
<img width="1024" height="769" alt="image" src="https://github.com/user-attachments/assets/91763cd7-1bed-4597-a8cb-2757879339a5" />

## Verification

The following screenshot shows the successful login on the Windows Server.
<br>
<img width="1021" height="766" alt="image" src="https://github.com/user-attachments/assets/2d4a4012-c567-4e80-ae20-73a817937eb1" />

