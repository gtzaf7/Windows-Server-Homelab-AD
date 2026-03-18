# Windows Server Homelab: Active Directory Deployment

A comprehensive, step-by-step technical guide on deploying a Windows Server infrastructure within a virtualized environment. This project demonstrates the installation, configuration, and management of Active Directory Domain Services (AD DS) and Group Policy Objects (GPO).

---

## Project Overview

The goal of this project was to build a functional corporate-style network from scratch using **VMware Workstation Pro**. The infrastructure simulates a real-world business environment with dedicated Organizational Units (OUs), Administrative accounts, and managed Windows Workstations.

### Key Features Implemented:
* **Infrastructure:** Virtual Networking (Bridged Mode) for seamless host-to-VM communication.
* **Core Services:** Active Directory Domain Services (AD DS) and DNS Configuration.
* **Security:** Password Complexity policies and administrative account separation.
* **Management:** Automated environment control via Group Policy Objects (GPOs).
* **Troubleshooting:** Resolving IPv6 priority conflicts and virtualization hardware errors.

---

## Lab Architecture & Topology

The network consists of a primary Domain Controller and multiple client workstations, organized by department.

* **Domain Controller (DC01):** Windows Server 2025 (IP: `192.168.2.9`)
* **Domain Name:** `homelab.local`
* **Network Topology:** Bridged Adapter (VMnet0)
* **Organizational Units:** IT, Accounting, Management.

---

## Project Roadmap (Phases)

This project is documented in 8 detailed phases. Each file contains technical explanations and step-by-step visual guidance.

| Phase | Description | Link |
| :--- | :--- | :--- |
| **Phase 1** | Virtual Networking (Bridged Mode) | [View Phase 1](./01-Virtual-Networking.md) |
| **Phase 2** | VM Provisioning (Hardware Specs) | [View Phase 2](./02-VM-Provisioning.md) |
| **Phase 3** | Windows Server OS Installation | [View Phase 3](./03-OS-Installation.md) |
| **Phase 4** | Post-Installation Configuration (Static IPs, NTP) | [View Phase 4](./04-Post-Installation-Config.md) |
| **Phase 5** | AD DS Installation & Forest Promotion | [View Phase 5](./05-ADDS-Configuration.md) |
| **Phase 6** | Logical Structure (OUs & Users) | [View Phase 6](./06-Logical-Structure.md) |
| **Phase 7** | Client Network Config & Domain Join | [View Phase 7](./07-Client-Domain-Join.md) |
| **Phase 8** | Group Policy Management (GPOs) | [View Phase 8](./08-Group-Policy-Management.md) |

---

## Technical Skills Demonstrated

* **Server Administration:** Windows Server 2025 Deployment.
* **Identity Management:** Active Directory Users & Computers (ADUC).
* **Networking:** IPv4 Subnetting, DNS Resolution, NTP Synchronization.
* **Policy Enforcement:** Group Policy Management Console (GPMC).
* **Virtualization:** VMware Workstation Pro Advanced Configuration.

---

## Technical Explanation: Why this Lab?

This homelab was built to simulate an enterprise environment where security and efficiency are prioritized. By using a **Bridged Network**, the lab mimics a physical office where the Server is a reachable resource on the wire. The implementation of **GPOs** demonstrates the power of centralized management—changing a setting once on the server and having it automatically apply to hundreds of workstations instantly.

---

## Contributions & Feedback

Feel free to fork this repository, open an **Issue** if you find a bug, or submit a **Pull Request** with improvements. Any feedback on the network configuration or GPO logic is highly appreciated!

*Project developed by [George Tzaferis/gtzaf7]*
