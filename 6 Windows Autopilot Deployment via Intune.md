Hello everyone,

Welcome to today's session, where we will discuss **Windows Autopilot Deployment via Intune**. This is a Level 200 session focusing on the setup, configuration, troubleshooting, and best practices for deploying Autopilot in your organization.

## **Why Use Windows Autopilot?**
Windows Autopilot simplifies the process of setting up new Windows devices. Instead of manually imaging and configuring each device, Autopilot allows IT to pre-configure settings, policies, and applications before the user even receives the device.

### **Key Benefits:**
- **Zero-Touch Deployment**: IT doesn’t need to manually set up devices.
- **User-Driven Setup**: Users can configure their device with minimal IT support.
- **Seamless Integration**: Works with **Azure AD, Intune, and Microsoft 365**.
- **Security & Compliance**: Ensures all devices meet corporate security policies.

## **What Windows Autopilot Is & What It’s Not**
### **Autopilot Capabilities:**
- Automates device provisioning and policy application.
- Supports **Azure AD Join** and **Hybrid Azure AD Join**.
- Configures apps, policies, and settings out of the box.
- Provides **self-deployment mode** for kiosk/shared devices.

### **What Autopilot Doesn't Do:**
- It **does not** replace traditional imaging tools like SCCM/MDT.
- It **does not** deploy an OS—devices must already have Windows 10/11 installed.

## **Prerequisites for Windows Autopilot**
1. **Devices must have Windows 10 version 1703 or later** (or Windows 11).
2. **Intune license assigned to users**.
3. **Azure AD automatic enrollment enabled**.
4. **Device must be connected to the Internet during setup**.
5. **Windows Autopilot profile created and assigned**.
6. **OEM or IT must provide device hardware hashes for enrollment**.

## **Windows Autopilot Deployment Process**
### **Step 1: Collecting Device Information**
1. Obtain the **hardware hash** from the device:
   - Run the PowerShell script:
     ```powershell
     Install-Script -Name Get-WindowsAutopilotInfo
     Get-WindowsAutopilotInfo -OutputFile C:\Temp\Autopilot.csv
     ```
2. Upload the CSV file to Intune:
   - Go to **Devices > Windows > Windows Enrollment > Devices**.
   - Click **Import** and upload the CSV.

### **Step 2: Creating an Autopilot Profile**
1. Go to **Devices > Windows > Windows Enrollment > Deployment Profiles**.
2. Click **Create Profile** and select **Windows PC**.
3. Configure the profile:
   - **Deployment Mode**: User-driven or Self-deploying.
   - **Join Type**: Azure AD Join or Hybrid Azure AD Join.
   - **OOBE Customization**: Set company branding and privacy settings.
4. Assign the profile to a **device group**.

### **Step 3: Assigning the Profile to Devices**
1. Ensure devices are **added to a Dynamic Device Group** based on hardware hash.
2. Assign the deployment profile to this group.
3. When the user powers on the device, it will automatically fetch the profile.

### **Step 4: User Experience**
1. User **unboxes** the device and powers it on.
2. The device **connects to the Internet** and pulls the Autopilot profile.
3. User **logs in** using corporate credentials.
4. Apps and policies deploy automatically.
5. Device becomes **business-ready** with no IT intervention.

## **Troubleshooting Autopilot Issues**
### **Common Issues & Fixes:**
- **Device does not receive Autopilot profile** → Ensure the device is correctly imported into Intune.
- **User cannot sign in during OOBE** → Verify Azure AD and licensing requirements.
- **Autopilot Enrollment Fails (Error 80180018)** → Check if the user is allowed to enroll devices.
- **Application Deployment Fails** → Increase the app installation timeout in Intune.

## **Advanced Features in Windows Autopilot**
1. **Enrollment Status Page (ESP)**: Prevents users from accessing the desktop until all policies/apps are installed.
2. **Hybrid Azure AD Join**: Allows on-prem domain-joined devices to be enrolled in Intune.
3. **Autopilot Reset**: Resets a device while retaining its Azure AD join and Intune enrollment.
4. **Self-Deploying Mode**: Deploys devices without user credentials (ideal for kiosks/shared devices).
5. **Device Naming Templates**: Automatically names devices using a custom format.

## **Summary**
Windows Autopilot revolutionizes device deployment by providing a zero-touch, cloud-driven setup. With **Intune integration**, it ensures all devices are secure, compliant, and ready for business use without IT intervention.

Let me know if you need further details or a step-by-step walkthrough on any section!

