Here is a detailed briefing document reviewing the main themes and most important ideas or facts from the provided excerpts on Windows Autopilot:

# Briefing: Windows Autopilot Deployment via Intune

**Source:** Excerpts from "#IntuneNugget 6- Autopilot deployment via Intune |Level 200" (Transcript) and "Windows Autopilot Deployment via Intune session notes"
**Date:** May 23, 2025
**Subject:** Review of Windows Autopilot functionality, benefits, prerequisites, flow, troubleshooting, new features, and configuration via Microsoft Intune.

## Key Takeaways:

* **Zero-Touch Deployment**: The core concept of Windows Autopilot is to provide a "zero-touch" experience for both IT administrators and end-users during the device provisioning process. This eliminates the need for traditional imaging methods.
* **Simplified Device Setup**: Autopilot streamlines the out-of-the-box experience (OOBE) for new devices, allowing users to configure a business-ready device with minimal interaction simply by powering it on and signing in with their organizational credentials.
* **Integration with Azure AD and MDM**: Autopilot relies heavily on integration with Azure Active Directory (Azure AD) for device registration and mobile device management (MDM) solutions (specifically Microsoft Intune in this source) for applying policies, configurations, and applications.
* **Multiple Deployment Scenarios**: Autopilot supports deploying fresh devices shipped directly from the vendor to the user and also resetting existing devices to a business-ready state.
* **Customization of OOBE**: While some initial setup steps (language, region, keyboard) are mandatory for user-driven profiles, Autopilot allows customization of aspects like the End-User License Agreement (EULA), privacy settings, and the sign-in experience with company branding.
* **Enrollment Status Page (ESP)**: A significant feature (available on Windows 10 1803+) that provides visual feedback to the user during the provisioning process and can block device usage until required applications, policies, and certificates are installed, ensuring the device is "business ready."
* **Hybrid Azure AD Join Support**: Autopilot can facilitate joining a device to both on-premises Active Directory and Azure AD, enabling management via tools like SCCM/Intune co-management. This requires an on-premises connector.
* **New Features Enhance Functionality**: Recent updates introduce features like self-deploying mode (for shared/kiosk devices, requiring TPM 2.0), pre-assigning a user to a device for pre-populated login, and device naming schema for consistent device naming.
* **Troubleshooting Tools**: Key tools for diagnosing Autopilot deployment issues include checking the registry (specifically `HKLM\SOFTWARE\Microsoft\Provisioning\Diagnostic\Autopilot`) and event logs (`Application and Services Logs > Microsoft > Windows > Provisioning-Diagnostic-Provider > Autopilot`).
* **Configuration Portals**: Autopilot profiles can be configured through Microsoft Intune, Microsoft Store for Business, and the O365 admin portal. The source primarily focuses on Intune.
* **Prerequisites**: Key requirements include a genuine version of Windows 10 1703 or later, Internet access, an Azure AD P1 license for the user, enabled auto-enrollment in Azure AD, allowing users to join devices to Azure AD, registered devices in the organization's tenant (via hardware hash/serial number import), configured company branding, and an MDM solution like Intune.

## Main Themes and Important Ideas/Facts:

### 1. The "Why" and "What" of Windows Autopilot:

* **Problem Solved**: Traditional device deployment often involves IT intervention, imaging, and time-consuming manual configurations. Autopilot aims to eliminate this.
* **Definition**: Windows Autopilot is a collection of technologies used to set up and pre-configure new devices, preparing them for productive use.
* **Core Benefit**: "making the end-users experience zero touch and saving the cost and the time behind building an image."
* **What Autopilot is Not**: It's explicitly stated that "auto pilot is not equal to the OSD via sec mr m dt." Autopilot doesn't deploy images; it configures a device based on its existing Windows installation. It **does not** deploy an OS—devices must already have Windows 10/11 installed.

### 2. Autopilot Prerequisites:

1.  **Operating System**: "The device can run with any version of Windows 10 17:03 or later." This is a critical minimum OS requirement.
2.  **Licensing**: "the user needs to have a dp1 license just like what user would normally have if he is trying to make use if he is trying to leverage conditional access." An Azure AD Premium P1 license is necessary. EMS E3 or A5 licenses are also mentioned.
3.  **Azure AD Configuration**: "we will have to enable the auto enrolment feature in the azure ad blade." This is essential for the device to enroll in Intune after Azure AD registration. Users must be allowed to enroll devices.
4.  **Device Registration**: Devices must be registered with the organization's tenant. This is done by importing hardware IDs (serial number and hardware hash), typically provided by the hardware vendor or extracted manually using PowerShell (`Install-Script -Name Get-WindowsAutopilotInfo`, then `Get-WindowsAutopilotInfo -OutputFile C:\Temp\Autopilot.csv`).
5.  **Company Branding**: Configuring company branding in Azure AD "is the way we are going to actually see and find out during the out-of-the-box experience whether it's just going through a normal out-of-the-box experience or whether it's going through and customized out-of-the-box experience which is provision to the device via autopilot."
6.  **MDM**: An MDM solution like Intune is required to deliver policies and applications.
7.  **Network Access**: Devices need internet access to connect to cloud services during setup. Specific URLs need to be accessible, similar to those required for Intune enrollment.

### 3. Windows Autopilot Deployment Process (The Flow):

* **Simplified Supply Chain**: "the hardware vendor is I mean going to deploy the device directly to the purse to the home address of the end-user." This bypasses IT handling of devices.
* **Device Registration**: The hardware vendor or IT admin uploads a CSV file containing device IDs (serial number, hardware hash) to the Azure portal. Azure then knows "which devices are associated with your tenant."
* **End-User Experience**:
    1.  User **unboxes** the device and powers it on.
    2.  Select language, region, and keyboard (mandatory for user-driven).
    3.  The device **connects to the Internet** and pulls the Autopilot profile.
    4.  The customized company branding page appears, confirming the Autopilot profile is applied.
    5.  User enters organizational credentials (username may be pre-populated if assigned).
    6.  Device performs Azure AD registration and enrollment into Intune.
    7.  Policies, configurations, and required applications are provisioned.
    8.  (Optionally) The Enrollment Status Page (ESP) is displayed, potentially blocking user access until provisioning is complete.
    9.  Device reaches a "business ready" state.

### 4. Autopilot Profile Types:

* **User Driven Mode**: The traditional mode where the user interacts with the OOBE, enters credentials, and the device is set up for that specific user. Supports Azure AD Join and Hybrid Azure AD Join.
* **Self-Deploying Mode (Preview)**: "taken zero touch experience to a new level... without even the user having to enter his username or password." This mode is for shared devices or kiosks where the device authenticates with Azure AD using a TPM 2.0 chip. It automatically joins Azure AD and enrolls in Intune, applying policies and apps without user login. Requires a physical machine with a TPM 2.0 chip.

### 5. Key Features Discussed:

* **Enrollment Status Page (ESP)**: (Windows 10 1803+) Provides a visual progress indicator during device setup. "What it does is when the device is going through the out-of-the-box experience... we can block the device in a state of limbo." It ensures the device has necessary configurations and apps before the user reaches the desktop.
* **Vendor Automatic Registration**: Hardware vendors can automatically register devices to a customer's tenant via the portal.
* **Zero Touch Deployment ID (ZTDID)**: Autopilot devices are tagged with this attribute, which can be used to create dynamic groups in Azure AD for easier targeting of Autopilot profiles.
* **Deleting Autopilot Devices**: The ability to delete Autopilot devices directly from Intune is a newer feature.
* **Windows Autopilot Reset (Preview)**: Allows resetting an existing Autopilot device remotely from the portal, reapplying the Autopilot profile and bringing it back to a business-ready state. Equivalent to re-imaging but without deploying an image.
* **Pre-assigning Autopilot to a User**: Allows assigning a specific user to an Autopilot device in the portal before deployment. When the user powers on the device, their username will be pre-populated in the OOBE login screen.
* **Device Naming Schema**: (Windows 10 1809+) Enables defining a consistent naming convention for Autopilot devices upon enrollment using templates (e.g., `MYLAB-Serial%`).
* **Hybrid Azure AD Join**: Allows Autopilot to automatically join devices to an on-premises Active Directory domain as well as Azure AD. This requires installing an on-premises connector. "the device can be shipped to any person all over the world and all that needs to be done is that the device needs and access to the Internet and once the device goes to the out-of-the-box experience the device can get joined to the local ad."

### 6. Configuration via Intune:

The primary focus of the demo is configuring Autopilot using the Microsoft Intune portal (accessed via `portal.azure.com`). The steps involve:

1.  **Collecting Device Information**: Obtain and upload the hardware hash from the device (or import from OEM).
2.  **Creating an Autopilot Profile**: Go to **Devices > Windows > Windows Enrollment > Deployment Profiles**. Click **Create Profile** and select **Windows PC**. Configure the profile:
    * **Deployment Mode**: User-driven or Self-deploying.
    * **Join Type**: Azure AD Join or Hybrid Azure AD Join.
    * **OOBE Customization**: Set company branding and privacy settings.
3.  **Assigning the Profile to Devices**: Assign the deployment profile to a **device group** (dynamic or static based on ZTDID). Ensure devices are added to this group.
4.  (Optional) Configuring the Enrollment Status Page profile and assigning it to the group.
5.  (Optional) Pre-assigning a user to a specific device.
6.  Assigning required applications to the group containing the Autopilot devices.

### 7. Troubleshooting Common Issues:

* **No OOBE Screen**: Could be due to a bad OS image, unexpected XML, or a partially completed OOBE session. Suggested fix: Restart from scratch.
* **"Not connected to a network" error**: Often related to proxy servers requiring user authentication before network access is established during OOBE.
* **Error 801C0003**: Indicates issues with the user's ability to join the device to Azure AD (permissions, hitting device limit).
* **Error 80180018**: Indicates problems with the user's ability to enroll the device in MDM (auto-enrollment not enabled or targeted correctly, missing Azure AD Premium/EMS license, hitting device enrollment limit).
* **Application Deployment Fails** (during ESP): Causes the device to remain in the limbo state on the ESP. Reasons can include application incompatibility, insufficient timeout for installation, or missing licenses for the application (e.g., Office).

### 8. Validation and Logs:

* **Registry**: Check `HKLM\SOFTWARE\Microsoft\Provisioning\Diagnostic\Autopilot` for configuration details applied by the Autopilot profile.
* **Event Viewer**: Look at `Application and Services Logs > Microsoft > Windows > Provisioning-Diagnostic-Provider > Autopilot` for event logs related to the Autopilot process.
* **Intune Portal**: Verify device enrollment status, assigned profile, and application installation status.

### 9. Comparison with Traditional Imaging:

* Autopilot completely bypasses the need for building and deploying golden images.
* Policies, applications, and updates are delivered from the cloud after the device is enrolled, rather than being pre-packaged in an image.

### 10. Kiosk Deployment Example:

* While self-deploying mode is ideal for kiosks, a similar outcome can be achieved with a user-driven profile combined with a Device Configuration Profile targeting a Kiosk browser application.
* This setup requires the initial user (who enrolled the device) to log in, create a local admin account for the kiosk user (which the kiosk profile might automatically create but not grant admin rights), log off, and then the kiosk user can log in without a password, launching the single specified application in a locked-down state.

## Overall Significance:

Windows Autopilot is presented as a transformative technology for modern device deployment, moving away from labor-intensive imaging processes towards a simplified, cloud-managed, and largely zero-touch experience. The continuous addition of new features, including hybrid join support and self-deploying mode, expands its applicability across various organizational needs. The reliance on Azure AD and Intune underscores the shift towards cloud-based device management.
