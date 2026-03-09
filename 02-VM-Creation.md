# Phase 2: Virtual Machine Provisioning

**Objective:** Create and configure the virtual hardware parameters for the primary server (`DC01`) using VMware Workstation Pro, preparing it for the Windows Server operating system installation.

---

## Configuration Steps

**Step 1:** In VMware Workstation, click **Create a New Virtual Machine** and select **Custom (advanced)** to ensure precise control over hardware allocation.

<img width="1207" height="389" alt="image" src="https://github.com/user-attachments/assets/a9da3e85-56f8-43ff-97f0-5e4b4243f203" />

<img width="424" height="425" alt="image" src="https://github.com/user-attachments/assets/d8e903ca-fe6f-4120-8adb-699d006911b4" />


**Step 2:** On the Hardware Compatibility screen, select **Workstation 25H2** (or the highest version available in your installation) from the dropdown menu.

<br>
<img width="419" height="424" alt="image" src="https://github.com/user-attachments/assets/4589652a-609c-4116-81be-81ae311566cd" />
<br>

> **Technical Explanation:** Selecting the latest hardware compatibility version ensures the virtual machine has access to the newest virtualization features, maximum supported hardware limits (such as higher RAM/CPU ceilings), and the best performance optimizations for modern operating systems like Windows Server 2022/2025.

**Step 3:** On the Guest Operating System Installation screen, select **"I will install the operating system later"**.

<img width="422" height="422" alt="image" src="https://github.com/user-attachments/assets/6cd5a05f-7578-48e1-a502-31f6f6883afa" />


> **Technical Explanation:** This option is deliberately selected to bypass VMware's automated "Easy Install" feature. It ensures the administrator retains 100% manual control over the Windows setup wizard (preventing auto-generated hostnames or default user accounts).

**Step 4:** Select **Microsoft Windows** and set the version to **Windows Server 2025**. Name the virtual machine `DC01`.

<img width="421" height="425" alt="image" src="https://github.com/user-attachments/assets/1ddbaed8-fa5e-48e0-9bfd-e2bb9540d4f7" />

<img width="422" height="425" alt="image" src="https://github.com/user-attachments/assets/42c21a78-50c7-49b5-bd38-ceaba83698ab" />

**Step 5:** For the Firmware Type, select **UEFI** and check the **Secure Boot** option.

<img width="420" height="420" alt="image" src="https://github.com/user-attachments/assets/397caab0-73cb-4a80-8bba-905dc697d479" />

> **Technical Explanation:** Aligns with modern enterprise security baselines. Secure Boot prevents unauthorized, malicious pre-boot software (like rootkits) from compromising the Domain Controller before the OS even loads.

**Step 6:** Assign **2 Processors** with **2 Cores** per processor (Total: 4 Cores) and allocate **4096 MB** (4 GB) of RAM.

<img width="424" height="421" alt="image" src="https://github.com/user-attachments/assets/97763104-ae1e-4973-bd82-be16fb18c929" />

<img width="419" height="421" alt="image" src="https://github.com/user-attachments/assets/3787ccfc-73e2-45fc-ae2b-7df48d823ea4" />

**Step 7:** For the Network Type, select **Use bridged networking**. This attaches the VM directly to the `VMnet0` switch we configured in Phase 1.

<img width="418" height="419" alt="image" src="https://github.com/user-attachments/assets/16e51c24-0f15-4d8c-ad73-b09c37cbaf3a" />

**Step 8:** Configure the storage controllers.
* **I/O Controller Type:** Leave the default recommended option: **LSI Logic SAS**.
* **Virtual Disk Type:** Select **NVMe** (or leave as recommended).

<br>
<img width="424" height="425" alt="image" src="https://github.com/user-attachments/assets/25439a75-0975-4b86-9fe1-9f42f3682308" />

<img width="415" height="422" alt="image" src="https://github.com/user-attachments/assets/22211e28-4180-4cd8-9353-170b59eeadd8" />


<br>

> **Technical Explanation:**
> * **LSI Logic SAS:** This controller is chosen because it offers broad OS compatibility natively within Windows Server, proven stability, and excellent performance for enterprise workloads. It is the industry standard for virtualized Windows environments.
> * **NVMe:** Selecting NVMe over traditional SATA/SCSI provides significantly higher IOPS (Input/Output Operations Per Second) and drastically lower storage latency, directly utilizing the host's SSD architecture. This is highly beneficial for a Domain Controller, as the Active Directory database (`NTDS.dit`) and log files perform frequent, rapid read/write operations.

**Step 9:** Select **Create a new virtual disk** with a maximum capacity of **50 GB** and choose **Store virtual disk as a single file**.
<img width="418" height="422" alt="image" src="https://github.com/user-attachments/assets/1641065f-0547-409e-9474-b9d8be9fc919" />

<img width="416" height="423" alt="image" src="https://github.com/user-attachments/assets/447d0392-9638-4309-9cdb-d2745d8fee68" />

<img width="420" height="420" alt="image" src="https://github.com/user-attachments/assets/fa493c5e-e683-4050-9c6f-d9517ae8efd7" />


> **Technical Explanation:** Storing the 50GB `.vmdk` as a single file optimizes local disk I/O performance on the host's SSD, as the hypervisor doesn't need to manage data spanning across multiple split files.
> 
<img width="419" height="423" alt="image" src="https://github.com/user-attachments/assets/ee383b66-8bd8-4059-ba80-5454864cf8a5" />

---

## Verification

The following screenshot shows the successful creation of the Virtual Machine.
<br>
<img width="343" height="421" alt="image" src="https://github.com/user-attachments/assets/994af5d4-5bdf-42cd-b446-8f10682fb45b" />
