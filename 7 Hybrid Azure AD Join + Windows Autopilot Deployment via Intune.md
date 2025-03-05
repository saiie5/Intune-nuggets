Hello everyone,

Welcome to today's session, where we will discuss **Hybrid Azure AD Join + Windows Autopilot Deployment via Intune**. This is a Level 150 session that builds upon our previous discussions on **normal Autopilot deployment**.

## **What is Hybrid Azure AD Join Autopilot?**
Hybrid Azure AD Join Autopilot enables devices to join both **Azure AD** and **on-prem Active Directory**, combining the benefits of cloud and traditional domain-joined environments. This ensures seamless access to **on-prem resources** while enabling **modern management via Intune**.

### **Why Use Hybrid Azure AD Join Autopilot?**
- **Seamless User Experience**: Users receive a ready-to-use device with both **on-prem and cloud** connectivity.
- **Zero-Touch IT Deployment**: Reduces IT overhead and imaging requirements.
- **Security & Compliance**: Ensures all devices meet corporate security policies.
- **Consistent Identity Management**: Uses **Azure AD Connect** for seamless authentication.

---

## **Prerequisites for Hybrid Azure AD Join Autopilot**
1. **Windows 10 version 1809 or later** (or Windows 11).
2. **Azure AD P1 License** assigned to users.
3. **Azure AD Connect configured for Hybrid Join**.
4. **User permissions to join devices to Azure AD**.
5. **Device must have internet access and line-of-sight to the on-prem DC**.
6. **Intune Connector for Active Directory installed on a Windows Server 2016+**.
7. **Company branding enabled (Optional for user experience enhancement).**

---

## **Step-by-Step Hybrid Azure AD Join Autopilot Deployment**

### **Step 1: Configure Azure AD Connect for Hybrid Join**
1. Install **Azure AD Connect** on an **on-prem domain controller**.
2. Select **Hybrid Azure AD Join** and configure sync settings.
3. Ensure **device synchronization** is enabled for on-prem computers.

### **Step 2: Install the Intune Connector for Active Directory**
1. Download the **Intune Connector for Active Directory** from the Intune portal.
2. Install the connector on a **Windows Server 2016 or later**.
3. Sign in with a **Global Administrator account**.
4. Verify the connector is active in **Intune > Windows Enrollment > Intune Connector**.

### **Step 3: Extract and Upload Hardware Hash**
1. Run the following **PowerShell script** on the target device:
   ```powershell
   Install-Script -Name Get-WindowsAutopilotInfo
   Get-WindowsAutopilotInfo -OutputFile C:\Temp\Autopilot.csv
   ```
2. Upload the **CSV file** to Intune:
   - **Devices > Windows > Windows Enrollment > Devices**.
   - Click **Import** and upload the CSV.

### **Step 4: Create and Assign Autopilot Deployment Profile**
1. Go to **Devices > Windows > Windows Enrollment > Deployment Profiles**.
2. Click **Create Profile**, select **Hybrid Azure AD Join**.
3. Configure settings:
   - **Deployment Mode**: User-driven.
   - **Join Type**: Hybrid Azure AD Join.
   - **Customize OOBE experience** (privacy settings, company branding, etc.).
4. Assign the profile to a **dynamic device group**.

### **Step 5: Create and Assign Offline Domain Join (ODJ) Configuration Profile**
1. Go to **Devices > Configuration Profiles**.
2. Create a new profile:
   - **Platform**: Windows 10 and later.
   - **Profile Type**: **Domain Join**.
3. Configure settings:
   - **Computer Name Prefix** (e.g., CORP-XXX).
   - **Domain Name** (on-prem AD domain).
   - **Organizational Unit (OU)** where devices should be placed.
4. Assign the **ODJ profile** to the same **device group** as the Autopilot profile.

### **Step 6: Device Enrollment & Deployment Process**
1. User powers on the new device.
2. The device connects to **Intune** and downloads the **Autopilot profile**.
3. User signs in with **Azure AD credentials**.
4. **Device is Azure AD Joined and enrolled in Intune**.
5. **Intune pushes the Offline Domain Join (ODJ) blob** to the device.
6. Device **contacts the on-prem domain controller** and joins the **Active Directory**.
7. Device **reboots** and applies **Group Policies (GPOs) and Intune policies**.

---

## **Troubleshooting Hybrid Azure AD Join Autopilot Issues**
### **Common Issues & Fixes:**
- **Device does not receive Autopilot profile** → Ensure the device is correctly imported into Intune.
- **User cannot sign in during OOBE** → Verify Azure AD and licensing requirements.
- **Autopilot Enrollment Fails (Error 80180018)** → Check if the user is allowed to enroll devices.
- **ODJ Blob Not Applied** → Ensure Intune Connector is properly installed and configured.
- **Device does not Hybrid Join after reboot** → Confirm **Azure AD Connect sync** is functioning properly.

---

## **Summary**
Hybrid Azure AD Join Autopilot provides a seamless deployment experience by combining **cloud-based management (Intune)** with **on-prem Active Directory integration**. By leveraging **Autopilot, ODJ, and Azure AD Connect**, organizations can deploy secure, **business-ready devices** with minimal IT effort.

Let me know if you need a detailed walkthrough on any specific step!

