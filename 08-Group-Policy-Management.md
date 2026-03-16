# Phase 8: Group Policy Management (The Grand Finale)

**Objective:** Demonstrate centralized administration by creating and linking Group Policy Objects (GPOs). We will implement three essential real-world corporate policies: restricting access to system settings, mapping a network drive for file sharing, and deploying a standard corporate wallpaper.

---

## Part 1: Infrastructure Preparation (The File Share)

Before deploying policies that require files (like wallpapers or shared documents), we must create a centralized Network Share on the Domain Controller that all clients can access.

**Step 1:** On `DC01`, open File Explorer, navigate to the `C:\` drive, and create a new folder named `CorpShare`.

<img width="670" height="306" alt="image" src="https://github.com/user-attachments/assets/1f05f971-8977-4c26-be4e-1c700eed59af" />

**Step 2:** Inside `CorpShare`, create two subfolders: `Public` (for the mapped drive) and `IT_Assets` (for the wallpaper).

<img width="770" height="199" alt="image" src="https://github.com/user-attachments/assets/d97b5be5-2b86-42a5-9414-82a63a97537e" />

**Step 3:** Place a sample image file (e.g., `wallpaper.jpg`) inside the `IT_Assets` folder.

<img width="776" height="160" alt="image" src="https://github.com/user-attachments/assets/ed41f1bf-bf3a-4181-9cec-2b0a602b74f3" />

**Step 4:** Right-click the `CorpShare` folder and select **Properties**. Go to the **Sharing** tab, click **Advanced Sharing**, check **"Share this folder"**, and ensure the share name is `CorpShare`.

<img width="424" height="576" alt="image" src="https://github.com/user-attachments/assets/ade6bbea-bd0b-41d8-ae40-918c038d9f9f" />

**Step 5:** Click **Permissions**, grant the `Everyone` group **Read** & **Change** access, and click **OK** on all windows.

<br>
<img width="427" height="531" alt="image" src="https://github.com/user-attachments/assets/88fa9f40-2c21-45e3-92e1-fa6bcdd72099" />
<br>

> **Technical Rationale:** A centralized network share allows us to host files that Group Policies will call upon. Instead of manually copying the wallpaper to 100 different laptops, the GPO simply tells the laptops: *"Look at `\\192.168.2.9\CorpShare\IT_Assets\wallpaper.jpg` and make it your background."*

---

## Part 2: Security GPO (Block Control Panel)

Our first policy will prevent standard users in the Accounting department from tampering with PC settings.

**Step 6:** On `DC01`, open **Server Manager > Tools > Group Policy Management**.

<img width="666" height="255" alt="image" src="https://github.com/user-attachments/assets/9964ad47-0e43-4f6f-9aac-da5102b7f17b" />

**Step 7:** Expand the forest and domain tree. Right-click the `Accounting` OU (under `_Corp`) and select **"Create a GPO in this domain, and Link it here..."**. Name it `Block_ControlPanel` and click OK.

<img width="454" height="204" alt="image" src="https://github.com/user-attachments/assets/fef0595f-1ecb-43c0-bd81-950507d883d3" />

**Step 8:** Right-click the new `SEC_Block_ControlPanel` GPO and select **Edit**.

<img width="550" height="330" alt="image" src="https://github.com/user-attachments/assets/1af86c1e-956b-4c07-a3fd-7d2c86db9a07" />

**Step 9:** In the Group Policy Management Editor, navigate to:
`User Configuration` > `Policies` > `Administrative Templates` > `Control Panel`
**Step 10:** On the right pane, double-click **"Prohibit access to Control Panel and PC settings"**, set it to **Enabled**. Apply the settings and then click OK. Close the editor.

<br>
<img width="1211" height="528" alt="image" src="https://github.com/user-attachments/assets/f1c24472-f58f-4adf-9210-656f206b4c36" />
<img width="809" height="755" alt="image" src="https://github.com/user-attachments/assets/81345f4c-a50e-4c86-811c-0ad58b4fbac4" />
<br>

---

## Part 3: Resource GPO (Mapped Network Drive)

Now, let's automatically provide the entire company with a shared network drive for easy file exchange.

**Step 11:** Back in **Group Policy Management**, right-click the parent `_Corp` OU (so all departments inherit it) and select **"Create a GPO in this domain, and Link it here..."**. Name it `CORP_Mapped_Drive`.
<img width="819" height="461" alt="image" src="https://github.com/user-attachments/assets/c7f3e2e7-41d1-421e-af6b-181b208f6e88" />
**Step 12:** Right-click `CORP_Mapped_Drive` and select **Edit**.
<img width="537" height="489" alt="image" src="https://github.com/user-attachments/assets/1aa61e15-3d9d-42f7-836d-4db2e76012c9" />
**Step 13:** Navigate to: `User Configuration` > `Preferences` > `Windows Settings` > `Drive Maps`.
<img width="925" height="675" alt="image" src="https://github.com/user-attachments/assets/9cc25dfd-c168-4f10-9301-e8c1ff8d7af6" />
**Step 14:** Right-click **Drive Maps**, select **New > Mapped Drive**.
<img width="564" height="276" alt="image" src="https://github.com/user-attachments/assets/bbde5c0f-531c-48d4-a025-0d1ce239f919" />
**Step 15:** Set the **Action** to `Update`. For the **Location**, type the exact network path to the shared folder we created: `\\192.168.2.9\CorpShare\Public`.
<img width="472" height="541" alt="image" src="https://github.com/user-attachments/assets/b1a88c9b-877a-4ca8-9d6b-921f12d070f7" />
**Step 16:** Check the box **"Reconnect"**. Label it as `Company Share`, and assign it a specific **Drive Letter** (e.g., `S:`). Click Apply, then OK and close the editor.

<br>
<img width="472" height="541" alt="image" src="https://github.com/user-attachments/assets/603f1f64-4b24-4e1d-9881-c7fc7f8027de" />
<br>

---

## Part 4: Corporate Branding GPO (Standard Wallpaper)



**Step 17:** In **Group Policy Management**, right-click the `_Corp` OU again, create a new GPO named `CORP_Wallpaper`, and Edit it.
<img width="544" height="241" alt="image" src="https://github.com/user-attachments/assets/ddaea78f-deba-41b1-a1a0-9d04b1d5060e" />
**Step 18:** Navigate to: `User Configuration` > `Policies` > `Administrative Templates` > `Desktop` > `Desktop`.
<img width="1065" height="438" alt="image" src="https://github.com/user-attachments/assets/cb23e974-45bf-44a4-9eeb-ff43ea7dcb43" />
**Step 19:** Double-click **"Desktop Wallpaper"**. Set it to **Enabled**.
<img width="794" height="258" alt="image" src="https://github.com/user-attachments/assets/e7a7620a-3c23-41b4-9600-8f81171a0d22" />
**Step 20:** In the **Wallpaper Name** field, type the exact network path to our image: `\\192.168.2.9\CorpShare\IT_Assets\comp_wallpaper.jpg`.
<img width="270" height="181" alt="image" src="https://github.com/user-attachments/assets/c6990e54-a27f-4d52-955c-27477ba91968" />
**Step 21:** Set the Wallpaper Style to **Fill** or **Stretch**. Click Apply, then OK and close the editor.
<img width="236" height="35" alt="image" src="https://github.com/user-attachments/assets/e6e481b7-50e5-439b-bc64-ca32bfebb967" />

---

## Part 5: Client Verification (The Magic of AD)

**Step 22:** Go to your Client VM (Windows 10 Pro) and log in as the standard user from the Accounting department (e.g., `eve`).
**Step 23:** Open Command Prompt and force the computer to immediately fetch the latest policies from the Server by typing:
`gpupdate /force`

<img width="513" height="124" alt="image" src="https://github.com/user-attachments/assets/6623b158-2826-4ba8-987f-0a09851bda30" />


*(Note: Sometimes a log off and log back in, or a restart, is required for the wallpaper to apply visually).*

**Step 24:** Witness the results of our centralized management:
* **The Desktop Background** has changed automatically to the corporate wallpaper.
* Open **File Explorer > This PC**, and you will see the **`S:` Company Share** network drive perfectly mapped and ready to use.
* Try to open the **Control Panel**. A restriction message will immediately pop up saying: *"This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator."*

<br>
<img width="1027" height="769" alt="image" src="https://github.com/user-attachments/assets/f82309df-e7ba-4a88-aa31-6676b9e04064" />
<img width="557" height="179" alt="image" src="https://github.com/user-attachments/assets/09a80155-1f9e-4d56-915a-0710021f8d61" />

<br>
<img width="556" height="131" alt="image" src="https://github.com/user-attachments/assets/4b9c9884-dec1-4c8d-bcb9-7e4ee824cb7e" />
<br>

> **Technical Rationale:** Group Policy Objects (GPOs) are the primary mechanism for centralizing management in a Windows environment. By utilizing GPOs at the OU level, we ensure that every machine or user placed within that organizational unit automatically receives the exact security settings, software, and resources they need, without any manual configuration on the endpoint itself.
