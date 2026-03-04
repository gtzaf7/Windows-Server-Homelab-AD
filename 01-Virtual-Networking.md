# Phase 1: Virtual Networking Setup

**Objective:** Establish the foundational network bridge between the physical host and the virtual environment. The goal is to allow Virtual Machines to reside on the same Local Area Network (LAN) as physical devices. 

---

## Configuration Steps

**Step 1:** Open VMware Workstation and navigate to **Edit > Virtual Network Editor**.

<img width="999" height="610" alt="Virtual Network Editor Menu" src="https://github.com/user-attachments/assets/15cbb599-2d6d-42ba-9e7b-0d1d5568f669" />

**Step 2:** Click **Change Settings** in the bottom right corner and accept the Windows User Access Control (UAC) prompt to grant administrator privileges.

<img width="707" height="655" alt="UAC Prompt" src="https://github.com/user-attachments/assets/519d57d7-55a6-4283-8127-a726ccfabf15" />

**Step 3:** Select **VMnet0** from the list. Choose **Bridged (connect VMs directly to the external network)**, and explicitly set "Bridged to" to your specific, active network adapter (avoiding the "Automatic" option). Click **Apply** and then **OK**.

<img width="745" height="664" alt="VMnet0 Configuration" src="https://github.com/user-attachments/assets/38eb67e0-22b9-4fde-ab96-fc61290a93e8" />

<img width="692" height="597" alt="Network Adapter Selection" src="https://github.com/user-attachments/assets/1311d89a-99fc-40e8-ab9a-0286d55b885f" />

---

## Network Topology Overview

To understand what we just configured, here is a simple breakdown of our Bridged network topology:

<img width="804" height="428" alt="Network Topology Diagram" src="https://github.com/user-attachments/assets/d5c82769-6344-4572-8565-f1d107c9598d" />

* **The Default Gateway:** Our physical router (from the ISP) serves as the default gateway for the entire network.
* **The Virtual Bridge (VMnet0):** Think of VMnet0 as an invisible, virtual Layer 2 switch inside our PC. It acts as a direct cable connecting our physical network card to the virtual world.
* **Seamless Communication:** Because of this bridge, our Virtual Machine will act exactly like a real, physical computer plugged into our home router. It will receive an IP address on the exact same local subnet (e.g., `192.168.x.x`) as our physical PC, laptop, or smartphone, making cross-device communication seamless.
