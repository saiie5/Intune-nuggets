Here is a detailed briefing document reviewing the main themes and most important ideas or facts from the provided sources about Hybrid Azure AD Join Autopilot:

# Briefing Document: Hybrid Azure AD Join Autopilot

**Date:** May 23, 2025
**Source:** Excerpts from "#IntuneNugget 7- Hybrid Azure AD+ Autopilot |Level 150" and "Hybrid Azure AD Join + Windows Autopilot Deployment via Intune session notes"
**Subject:** Overview of Hybrid Azure AD Join Autopilot, its prerequisites, flow, and end-user experience.

## Summary:

This briefing document provides a detailed overview of Microsoft's Hybrid Azure AD Join Autopilot feature, a deployment method for Windows 10 devices that combines the benefits of traditional on-premises Active Directory domain joining with the modern management capabilities of Azure Active Directory and Microsoft Intune. Building upon the concept of "normal autopilot" (Azure AD Join only), Hybrid Azure AD Join Autopilot enables organizations to automatically provision new or existing devices, making them business-ready with minimal IT intervention while maintaining a connection to their on-premises domain. The document outlines the prerequisites, the step-by-step flow of the process, the administrative tasks involved, and the end-user experience.

## Key Themes and Ideas:

### 1. What is Autopilot and Why is it Needed?

* Autopilot is described as a "magical" process that transforms a brand new device into a "business ready state" with minimal user interaction.
    * Example Quote: "there is something just magical about us opening up a laptop getting a brand new device powering it on and the device automatically automagically knowing who we are and getting us into a business ready state that is what exactly the autopilot does"
* It aims to eliminate the traditional need for IT admins to manually image devices using golden images, ISOs, or tools like SCCM or MDT for operating system deployment.
* Autopilot is a "modern management collection of technologies" involving Intune, Azure, Microsoft 365, Windows, and cloud-based services.
* It extends to device resets for used devices, not just brand new ones.
* **Important Note**: Autopilot is a configuration and customization tool; it requires an operating system to be already present on the device.

### 2. What is Hybrid Azure AD Join Autopilot?

* This feature is an extension of normal autopilot, specifically designed for scenarios where devices need to be joined to both Azure Active Directory and an on-premises Active Directory domain.
    * Example Quote: "If a device is joined to the Azure ad and the device is joined to the on-prem ad then that scenario is referred to as the hybrid as your editor"
* The "hybrid" term signifies the combination of these two join states.
* Unlike normal autopilot (Azure AD Join only), Hybrid Azure AD Join adds the complexity and requirement of integrating with the on-premises infrastructure.
* **Why Use Hybrid Azure AD Join Autopilot?**
    * **Seamless User Experience**: Users receive a ready-to-use device with both on-prem and cloud connectivity.
    * **Zero-Touch IT Deployment**: Reduces IT overhead and imaging requirements.
    * **Security & Compliance**: Ensures all devices meet corporate security policies.
    * **Consistent Identity Management**: Uses Azure AD Connect for seamless authentication.

### 3. Prerequisites for Hybrid Azure AD Join Autopilot:

1.  **Windows Version**: Devices must be running Windows 10 version 1809 or later (or Windows 11). This is a key difference from normal autopilot.
2.  **Licensing**: User needs an Azure AD P1 license assigned to users.
3.  **Azure AD Configuration**: Auto-enrollment must be enabled, and users must be allowed to join devices to Azure AD.
4.  **On-Prem Connectivity**: The device needs to be in "direct line of sight with the domain controller". This is a critical prerequisite specific to hybrid join and means the device needs to be on the corporate network or able to communicate with the domain controller. This generally means it does not work over a standard VPN connection.
5.  **Intune/MDM**: Microsoft Intune (or another compatible MDM) must be configured and the device must be able to reach the Intune service (requiring internet access and appropriate firewall/proxy configuration).
6.  **Active Directory Connector**: A crucial component, the Intune Connector for Active Directory, must be installed on an on-premises server (Windows Server 2016 or later) that has access to both the internet (to communicate with Intune) and the intranet (to communicate with the domain controller).
7.  **Azure AD Connect**: The Azure AD Connect tool must be installed and configured on an on-premises domain controller to synchronize computer objects from the on-premises AD to Azure AD, enabling the "hybrid Azure AD join" feature within the tool. Ensure device synchronization is enabled for on-prem computers.
8.  **Organizational Unit (OU) and Permissions**: A dedicated OU must be created in the on-premises Active Directory, and the computer account of the server hosting the Intune Connector for Active Directory must have delegated permissions to create and manage computer objects within that OU.
9.  **Company branding enabled (Optional for user experience enhancement).**

### 4. Step-by-Step Hybrid Azure AD Join Autopilot Deployment Flow (Detailed):

The process involves multiple steps, requiring interaction between the IT admin, Intune, Autopilot service, Azure AD, the end-user device, the on-premises connector server, and the on-premises domain controller.

1.  **Admin Actions (Initial Configuration)**:
    * Install and configure Azure AD Connect on an on-prem domain controller for hybrid Azure AD join. Select Hybrid Azure AD Join and configure sync settings.
    * Create a dedicated OU in the on-premises AD for Autopilot devices.
    * Delegate permissions to the computer account of the server hosting the Intune connector to create/manage computer objects in the dedicated OU.
    * Download and install the Intune Connector for Active Directory on a Windows Server 2016 or later machine with network access to both the internet and intranet. Sign in with a Global Administrator account. Verify the connector is active in **Intune > Windows Enrollment > Intune Connector**.
    * Extract the hardware hash from devices using PowerShell commands (`Install-Script -Name Get-WindowsAutopilotInfo`, then `Get-WindowsAutopilotInfo -OutputFile C:\Temp\Autopilot.csv`).
    * Upload hardware hashes to the Intune portal (**Devices > Windows > Windows Enrollment > Devices**, then click **Import** and upload the CSV).
    * Create a Hybrid Azure AD Join Autopilot deployment profile in Intune (**Devices > Windows > Windows Enrollment > Deployment Profiles**, then **Create Profile**, select **Hybrid Azure AD Join**). Configure settings like **Deployment Mode: User-driven**, **Join Type: Hybrid Azure AD Join**, and customize OOBE experience (privacy settings, company branding, etc.).
    * Create an Offline Domain Join (ODJ) Device Configuration Profile in Intune (**Devices > Configuration Profiles**, then **Create new profile**, **Platform: Windows 10 and later**, **Profile Type: Domain Join**). Configure settings: **Computer Name Prefix** (e.g., CORP-XXX), **Domain Name** (on-prem AD domain), and the **Organizational Unit (OU)** where devices should be placed.
    * Assign both the Autopilot deployment profile and the ODJ device configuration profile to a security group (dynamic or static) containing the target devices (often using the ZTD ID for dynamic groups).

2.  **Service Interactions (Background)**:
    * Intune communicates with the Autopilot service's Device Discovery Service (DDS), creating a ZTD ID (Zero Touch Deployment ID) for the device.
    * DDS interacts with Azure AD's Device Registration Service (DRS) to create a pre-populated device object in Azure AD (applicable to both normal and hybrid join). This object can be added to a dynamic group for targeting.

3.  **End-User Powers On & Initial Setup**:
    * User powers on the new device and connects it to the corporate network (direct line of sight to the domain controller required).
    * They select regional/language settings (these screens are not customizable by Autopilot).
    * The device connects to Intune and downloads the Autopilot profile. The device's Autopilot DLL contacts the DDS, fetches the assigned Autopilot profile, and displays the customized Out-of-Box Experience (OOBE) with company branding.

4.  **User Authentication & Azure AD Join**:
    * The customized company branding appears on the login screen, confirming the Autopilot profile is applied.
    * User enters credentials (or just password if pre-assigned), authenticating with Azure AD.
    * Authentication is successful, and the device becomes Azure AD registered/joined and enrolled in Intune. (Up to this point, the flow is the same as normal autopilot).

5.  **Policy Delivery & ODJ Blob Request**:
    * Intune pushes assigned policies and profiles to the enrolled device, including the critical Offline Domain Join (ODJ) Device Configuration Profile.
    * The device receives the ODJ profile and realizes it needs to join the on-premises domain. It cannot directly communicate with the domain controller for the required "ODJ blob".

6.  **Communication via Connector**:
    * The device requests the ODJ blob from the Intune service.
    * Intune, using the registered on-premises connector, locates the server hosting the connector.
    * Intune forwards the ODJ request to the connector server.
    * The connector server forwards the request to the on-premises domain controller.

7.  **Domain Controller Actions**:
    * The domain controller creates a computer object for the device in the designated OU using the connector's delegated permissions. It then creates the ODJ blob.

8.  **Blob Delivery**:
    * The domain controller sends the ODJ blob back to the connector server.
    * The connector server uploads the blob to the Intune service.
    * Intune delivers the ODJ blob to the end device.

9.  **On-Prem Join & Reboot**:
    * The device receives the blob and performs a silent ping to the domain controller. If communication is successful ("direct line of sight"), the device performs the on-premises domain join and undergoes a reboot.

10. **Azure AD Connect Sync**:
    * The Azure AD Connect tool synchronizes the newly created on-premises computer object to Azure AD. This is what completes the hybrid Azure AD join state. This may result in two entries in Azure AD for the same device (one for the initial Azure AD registration via Autopilot, and one for the hybrid join via Azure AD Connect sync), though the hybrid entry is the relevant one.

11. **User Login & Policy Application**:
    * After the reboot, the login screen explicitly indicates the option to "Sign in to [On-Prem Domain Name]", confirming successful on-premises domain join.
    * The user logs in using their on-premises domain credentials.
    * The device receives Group Policies from the on-premises domain controller.
    * The device continues setting up applications and policies via the Enrollment Status Page (ESP).

### 5. Troubleshooting Hybrid Azure AD Join Autopilot Issues:

* Understanding the detailed flow is crucial for troubleshooting.
* **Common Issues & Fixes:**
    * **Device does not receive Autopilot profile** → Ensure the device is correctly imported into Intune.
    * **User cannot sign in during OOBE** → Verify Azure AD and licensing requirements.
    * **Autopilot Enrollment Fails (Error 80180018)** → Check if the user is allowed to enroll devices.
    * **ODJ Blob Not Applied** → Ensure Intune Connector is properly installed and configured.
    * **Device does not Hybrid Join after reboot** → Confirm Azure AD Connect sync is functioning properly.
* **Key areas to check** include network connectivity (internet for Intune/Autopilot, intranet for domain controller), firewall/proxy configurations, Azure AD Connect sync status, OU permissions, and the status of the Intune Connector for Active Directory.
* **Event Logs**: The logs on the server hosting the Intune Connector for Active Directory (specifically in the "ODJ Connector Service" under Application and Services Logs, looking for Event IDs 30120 and 30140) are essential for verifying the processing of ODJ requests and blob creation.
* **Network traces** (like Netmon or Fiddler) can help identify where the device is attempting to communicate and if any connections are being blocked.

### 6. Most Important Facts:

* Hybrid Azure AD Join Autopilot requires Windows 10 1809 or later (or Windows 11).
* It joins devices to both Azure AD and the on-premises AD.
* A key prerequisite is "direct line of sight" with the on-premises domain controller (generally meaning on the corporate network, not reliably over standard VPN).
* The Intune Connector for Active Directory is a mandatory component, installed on a Windows Server 2016+ on-premises server.
* An Offline Domain Join (ODJ) Device Configuration Profile is used via Intune to facilitate the on-premises domain join.
* Delegated permissions for the connector server's computer account in a specific on-premises OU are required for creating computer objects.
* The process involves a reboot to complete the on-premises domain join.
* Successful on-premises domain join is indicated by the ability to sign in with on-premises domain credentials and the domain name appearing on the login screen.
* Azure AD Connect sync is necessary to reflect the on-premises computer object in Azure AD as a "hybrid Azure AD joined" device.

This detailed overview provides a comprehensive understanding of the Hybrid Azure AD Join Autopilot feature, its operational flow, and the necessary steps for successful implementation and troubleshooting.
