# Intune Ibiza Console Walkthrough and Briefing

**Source**: Excerpts from "#IntuneNugget 3- A Walk through the Ibiza Console |Level 100"

**Date**: January 23, 2019

**Author**: Not Specified

## Session Notes: Intune Ibiza Console Walkthrough (Level 100)

Hello everyone,

Welcome to today's session, where we will discuss Intune. This is a Level 100 video, providing a walkthrough of the Intune Ibiza console. We will cover policy creation, configuration, compliance, protection policies, MAM and MDM configuration, and device enrollment setup.

### Accessing the Intune Portal

There are three ways to access the Intune portal:
1. **Portal.azure.com** – Direct login via the Azure portal.
2. **Portal.office.com** – Login via Office portal and navigate to the Admin Center.
3. **DeviceManagement.microsoft.com** – Direct access to the Intune device management console.

#### Differences Between Portals
- **DeviceManagement.microsoft.com**: Used mainly by Intune Admins.
- **Portal.azure.com**: Used by Global Admins managing both Intune and Azure.

The Ibiza console is the modern Intune console, replacing the older Silverlight console.

### Understanding Azure Active Directory & User Management
- Users and groups in Azure Active Directory (AAD) function like an on-premise domain controller.
- Users can be created manually in Azure AD or synced from an on-prem AD using Azure AD Connect.
- Password resets for cloud users are possible, but not for on-prem synced users unless password write-back is enabled.

### Microsoft Intune Console Overview
The console contains various sections:
- **Dashboard**: Displays device enrollment, compliance, configuration, and app deployment status.
- **Devices Section**: Shows all devices managed by Intune.
- **Users Section**: Lists users assigned to the tenant.
- **Groups Section**: Contains security and distribution groups.
- **Conditional Access**: Used for access control policies.

### Understanding Device Enrollment
#### Prerequisites for Device Enrollment
1. **Assign an Intune License**: The user must have an EMS E3/E5 license.
2. **Set MDM Scope**:
   - Navigate to **Azure AD > Mobility (MDM & MAM) > Microsoft Intune**.
   - Set MDM scope to "All".

#### Enrollment Methods
- **Windows Devices**: Users enroll through Settings > Accounts > Access work or school.
- **iOS Devices**: Requires an Apple Push Notification (APN) certificate setup.
- **Android Devices**:
  - **Android Legacy Enrollment**: Standard enrollment using Company Portal.
  - **Android Enterprise**: Provides additional restrictions and management options.

#### Enrollment Restrictions
Admins can restrict enrollment by:
- **Device Type Restrictions**: Limit device types (e.g., block iOS devices for certain users).
- **Device Limit Restrictions**: Restrict the number of devices a user can enroll.

### Device Management
#### Difference Between All Devices and Azure AD Devices
- **All Devices (Intune)**: Devices actively managed by Intune.
- **Azure AD Devices**: All devices registered or joined to Azure AD, including those not managed by Intune.

#### Device Compliance and Cleanup Rules
- **Device Cleanup Rules**: Automatically removes inactive devices after a set period.
- **Device Categories**: Helps classify devices for better management.

### Apple APN Certificate Setup
1. Download the CSR file from the Intune portal.
2. Upload it to Apple's Push Certificate Portal.
3. Download the APN certificate and upload it back to Intune.

This setup is required for Intune to communicate with Apple devices.

### Android for Work Setup
- Default Android enrollment is **Legacy**.
- To enable **Android Enterprise**, configure device type restrictions to allow Android for Work.

### Windows Autopilot & Bulk Enrollment
- **Windows Autopilot**: Simplifies bulk enrollment of corporate-owned Windows devices.
- **Device Enrollment Manager (DEM)**: Allows a single account to enroll multiple shared devices.

### Enrollment Status Page
- Displays installation progress of policies and applications.
- Admins can configure it to block access until all required apps are installed.

### Audit Logs & Device Actions
- **Audit Logs**: Tracks administrative actions in Intune.
- **Device Actions**: Logs admin-initiated actions like wipes and resets.

---

## Briefing Document: Navigating the Intune Ibiza Console

**Summary**:

This document provides a level 100 walkthrough of the Microsoft Intune Ibiza console, focusing on demonstrating how to perform common administrative tasks rather than explaining underlying concepts like MDM or MAM. The source highlights the different ways to access the console, key sections like Devices, Device Enrollment, Device Compliance, Device Configuration, Client Apps, Users, Groups, Roles, and the Troubleshoot blade. A significant portion is dedicated to explaining the distinction between Azure AD devices and Intune-managed devices and the differences between device compliance and device configuration policies.

### Key Themes and Ideas

#### Accessing the Intune Ibiza Console
There are primarily three ways to access the Intune portal (Ibiza console):
- Directly via [portal.azure.com](https://portal.azure.com): This opens the Azure portal, from which you can navigate to "Microsoft Intune". This is typically used by Global Admins managing both Intune and Azure components.
- Directly via [devicemanagement.microsoft.com](https://devicemanagement.microsoft.com): This opens the Intune portal directly and is often the preferred method for Intune Admins.
- Via the Office portal ([portal.office.com](https://portal.office.com)): From the admin center within the Office portal, you can navigate to Intune.
- It is crucial to use the correct, current URLs ([portal.azure.com](https://portal.azure.com) or [devicemanagement.microsoft.com](https://devicemanagement.microsoft.com)) and avoid the old Silverlight portal.

#### Prerequisites for Accessing and Using Intune
- The user accessing the console must have either an Intune Admin or Global Admin role assigned.
- Users who will be enrolling devices require an EMS E3 or E5 license or an Intune license.

#### Core Navigation and Blades
The console is organized into various "blades" or sections. Important blades for an Intune Admin include:
- **Users**: View and manage users, including assigning licenses. This blade is synced from Azure Active Directory (Azure AD).
- **Groups**: View and manage groups (security groups) which are used for assigning policies and applications. This blade is also synced from Azure AD.
- **Devices**: Manage and view enrolled devices.
- **Device Enrollment**: Configure settings to allow users to enroll devices.
- **Device Compliance**: Create and manage policies that define the security and health requirements for devices to be considered "compliant".
- **Device Configuration**: Create and manage policies that enforce specific settings and restrictions on devices.
- **Client Apps**: Manage and deploy applications to users and devices.
- **Roles**: Manage role-based access control (RBAC) to delegate administrative permissions.
- **Troubleshoot**: A centralized area to view user and device information for troubleshooting.
- Many blades (Users, Groups, Conditional Access) are pulled from or are closely linked to Azure AD.

#### Users and Groups
- Users and groups can be created directly in Azure AD (cloud-only) or synced from an on-premises Active Directory using Azure AD Connect.
- Intune Admins need to be able to assign licenses (like EMS) to users via the Users blade.

#### Device Enrollment
Several prerequisites must be met by the admin to enable device enrollment for users:
- Assigning a suitable license (EMS E3/E5 or Intune) to the end-user.
- Setting the MDM scope to "All" in the Azure AD blade under Mobility (MDM and MAM). This action requires a Global Admin role.
- Specific enrollment configurations are needed for different platforms:
  - **iOS**: Requires setting up an APNs certificate by downloading a CSR from Intune, uploading it to the Apple portal, and then uploading the resulting .pem file back to Intune. This is a one-time setup.
  - **Android**: Supports two enrollment types: Normal Android and Android Enterprise (Android for Work). Android Enterprise provides more granular control. Managed Google Play needs to be configured for Android Enterprise, where the admin approves apps for the Play Store.
  - **Windows**: Requires no out-of-the-box configuration for basic enrollment, but requires licensing and MDM scope to be set.
- Advanced enrollment methods exist for bulk scenarios:
  - **Apple DEP (Device Enrollment Program)**: For bulk enrollment of company-owned iOS devices.
  - **Windows Autopilot**: For customizing the out-of-the-box experience and provisioning Windows devices in bulk.
- The Company Portal app is essential for the end-user enrollment experience. Users download it and sign in with their licensed credentials.
- **Device Limit Restrictions**: Admins can limit the number of devices a single user can enroll (default is 15).
- **Device Type Restrictions**: Admins can restrict which platforms users can enroll (e.g., block iOS enrollment for a specific group).
- **Device Categories**: Admins can create categories that users must select during enrollment to tag devices for organizational purposes (e.g., department). This is a mandatory step if configured.
- **Corporate Device Identifiers**: Admins can pre-declare devices as "corporate" by uploading their IMEI or serial numbers, ensuring they are tagged as corporate upon enrollment.
- **Device Enrollment Managers (DEM)**: A DEM account can enroll a larger number of devices (around 100+) using a single license, useful for shared devices or kiosks. However, DEM-enrolled devices have limited end-user management options.
- **Company Portal Abandonment**: The console provides reporting on enrollment failures and at which stage the abandonment occurred, aiding in troubleshooting.

#### Devices Blade
- **All Devices**: Shows devices managed by Intune (enrolled devices). You can view details, add/remove columns (like IMEI, jailbroken status), and perform remote actions (sync, reset password, remote lock, wipe, retire, delete).
- **Azure AD Devices**: Shows all devices registered or joined to Azure AD. This is a superset of Intune-managed devices. A device can be Azure AD registered/joined without being enrolled in Intune.
- Deleting a device from "All Devices" removes its Intune management and data (if configured) instantly. Deleting from "Azure AD Devices" can take longer (around 30 days for cleanup).

#### Device Compliance vs. Device Configuration
This is a critical distinction.
- **Device Compliance Policies**: Define the required security and health state for devices (e.g., minimum OS version, password complexity, BitLocker enabled, not jailbroken). Devices are evaluated against these policies after enrollment. If a device doesn't meet the criteria, it is marked as non-compliant. Compliance policies are not compulsory in terms of blocking enrollment or access on their own.
- **Device Configuration Policies (Profiles)**: Enforce specific settings and restrictions on devices (e.g., block screen capture, require minimum password length, configure Wi-Fi profiles, deploy certificates, configure Kiosk mode). If a device does not adhere to a configuration policy, Intune will keep prompting the user to take the necessary action until the policy is met. Configuration policies are generally compulsory for the device to function as intended by the policy.
- Compliance policies are processed before configuration policies.
- The compliant/non-compliant status from compliance policies can be used with Conditional Access policies (configured in Azure AD, but accessible from the Intune portal) to grant or deny access to resources based on device compliance state.

#### Client Apps
Intune allows deployment of various application types:
- **LOB (Line-of-Business) Apps**: Custom applications developed internally (e.g., .msi files for Windows, .apk for Android, .ipa for iOS). .exe files are not supported directly as LOB apps (Win32 apps are an alternative for Windows).
- **Store Apps**: Applications from public app stores (Google Play, Apple App Store, Microsoft Store for Business).
- **Managed Google Play**: For Android Enterprise, apps must be approved in Managed Google Play before they can be deployed.
- **Microsoft Store for Business**: For Windows, similar to VPP for iOS.
- **Volume Purchase Program (VPP)**: For bulk purchasing and deploying iOS apps. Requires setting up a VPP token.
- Applications can be assigned as:
  - **Available**: Users can see the app in the Company Portal and choose to install it.
  - **Required**: The app will be automatically installed on the device (may require user interaction to approve on iOS, automatic download/install on Android).
- **App Protection Policies (MAM - Mobile Application Management)**: Apply policies to supported applications (MAM-aware apps) to protect corporate data within the app, even if the device is not enrolled in Intune. Examples include blocking copy/paste between managed and unmanaged apps.
- **App Configuration Policies**: Configure the behavior of specific applications.
- **App Selective Wipe**: Initiate a wipe of corporate data from a specific application.

#### Software Updates
- Intune can manage software updates, primarily for Windows 10, by creating Update Rings.
- Update Rings control when updates are installed (e.g., defer periods, scheduled times, update channels) and the user experience during updates.
- Intune does not allow selecting which specific patches are installed. If granular patch selection is required, traditional tools like SCCM or WSUS are needed.

#### Audit Logs
- Intune logs administrative actions performed within the console.
- Audit logs are available at a high level and within specific blades, providing details about who performed an action, what was modified, and when. This helps track administrative changes.

#### Roles (RBAC)
- Allows administrators to delegate specific permissions to users or groups.
- Built-in roles (like Intune Administrator, Help Desk Operator) and custom roles can be created to limit access to different areas or actions within the console.

#### Troubleshoot Blade
- A valuable tool for troubleshooting issues for individual users.
- Entering a user's UPN provides a consolidated view of their enrolled devices, group memberships, assigned licenses, targeted apps (required and available), compliance policies, configuration policies, and enrollment restrictions. This helps quickly identify potential causes of user-specific issues.

### Important Facts and Quotes
- "so guys this is going to be a level hundred kind of a video where and I'm going to browse through all the various sections of Intune visa console and I'm going to show you guys how to do it" - States the video's purpose.
- "so guys one of the prerequisites before watching this video is you should know what you are supposed to do and in this video I'm going to show you how exactly to do it" - Emphasizes the focus on "how" over "what".
- "this user should not be a normal user a normal user will not be able to access any of the blades so that user needs either to be and the global admin or an intern admin" - Defines the required administrative roles.
- "the point that I'm trying to make guys is if you go to portal dot ICO calm you have two choices... and if you look closely this is actually same as the previous the other tab which we had opened so that is device management or Microsoft comm and they both look the same" - Explains the two main portal access points and their similarity.
- "by default it syncs only users but you can sync devices as well so it will sync all the users and devices to DES to your tenant so it will sync everything from your on-prem to the cloud" - Describes Azure AD Connect functionality.
- "just for easing applicant just for our convenience and just to make sure that we don't have to switch between two portals we have these two in fact these three conditional access those groups these three blades are actually present under good the sections are the Microsoft Intune as well as under NGO ad" - Highlights the integration of Azure AD blades (Users, Groups, Conditional Access) within Intune.
- "the devices which I am able to see under Microsoft Intune oil devices are the devices which are managed wire in tune these are the devices which are enrolled to in tune" - Defines "All Devices".
- "ejoyed II devices the scan this blade is like kind of a superset and all devices is like a subset of a surety devices" - Explains the relationship between Azure AD Devices and All Devices.
- "a device cannot be enrolled to in tune the doubt being as your ad registered or as your ad joint" - Clarifies that Azure AD registration/join is a prerequisite for Intune enrollment.
- "if you delete this device you will no longer be able to view and manage the device from YouTube portal the device will no longer be allowed to access your company's corporate resource company data may be wiped from the device of the device tries to check in after your destiny" - Describes the impact of deleting a device from Intune.
- "if bi unenroll a device it's entry from all devices will get pretty much removed from there instantly but its entry does not get automatically removed from here that is from our area devices because the clean up the disk cleanup is something which is taken care by the azure ad team" - Explains the difference in cleanup behavior after unenrollment.
- "the first prerequisite is that the admin has to assign a EMS three or five licensed to be end-user... and the second prerequisite is that the admin has to set the MDM scope has all in the azure ad blade" - Outlines key admin prerequisites for user enrollment.
- "this company portal app is like a broker so whatever policies we are pushing from the Indian portal from the intern service it goes to the device via the company portal application" - Describes the role of the Company Portal app.
- "the end user has to download the company portal act and then login into that company portal app using is the username and password that username and password needs to have the EMS license assigned to it" - Describes the end-user enrollment process.
- "what this means is any user will not be able to enroll more than fifteen devices using the same account" - Explains the default device limit restriction.
- "unless and until he selects one of these and does are continue his device will not get enrolled and his device will not show up to the admin this is a compulsory step and this is something which the user cannot bypass" - Describes the mandatory nature of device categories if configured.
- "a device compliance or compliance policy is processed first the configuration policy is processed afterwards" - States the processing order of policies.
- "device compliance is something which is not compulsory however device configuration is something which is compulsory" - Highlights the fundamental difference in enforcement.
- "if the device is not meeting the criteria of password that has been specified under the device compliance policy then my device is going to be marked as non-compliant" - Explains the outcome of non-compliance for compliance policies.
- "if there was no compliance and only configuration policy then when the device is enrolled... it is going to keep prompting the user for I mean it will the prompt will keep on happening until and unless the device the user actually goes to the settings of the device and changes the password to it" - Explains the enforcement behavior of configuration policies.
- "this compliance state of a device can be used in conjunction with something called conditional access" - Explains how compliance integrates with Conditional Access.
- "this is exactly how device compliance is going to work so I hope that this helps us understand what exactly device compliance is whatever setting we set up over here helps helps the in tune Hotel determine the compliance attribute of a device" - Summarizes the purpose of device compliance policies.
- "device configuration is something which is a mandate and which the device has to comply with so if it's password or OS or anything like that the device is going to keep on prompting unless and until the user takes the necessary action" - Reiterates the mandatory nature of configuration policies.
- "if I were pushing a device restriction policy and I want that the end user should not be able to install any application... all I'll have to do is I will just have to block the app store and that's just it" - Provides a practical example of using device restriction policies.
- "just keep in mind guys that Windows 10 update ring cannot help us decide which update should happen and which update should not happen" - A crucial limitation of Intune software updates.
- "there are only two things that we need to keep in mind while pushing an application so just keep in mind this is the section from where and you will be pushing an application while pushing an application just need to keep two things in mind first is the app type... second is the assignment type" - Highlights key considerations when deploying apps.
- "if I as an admin want that the end-users should have Outlook and should have Excel on their devices it should be a mandate... then I would be pushing pushing Excel as a required app however if there are some applications I want to push as available then the end-users will have an option of either installing it or not installing it" - Explains the difference between Required and Available app assignments.
- "what I have protection policy does is let's just say that I have an Outlook application and I want that the users should not be able to copy data from Outlook to any other application... in that case I would be making use of something called a protection policy" - Defines the purpose of App Protection Policies (MAM).
- "these are the applications which understand the a protection policy of into so these are the application shall they're out of the box if you let's just say that we have a Adobe application onto which we want to apply a ma'am policy then there are two ways of doing that we can either get our hands on that application and then apple app it using something called the app trapper tool... or we can develop that application using the Intune sdk" - Explains how applications become MAM-aware.
- "if we want to delegate the control in that case we want we can make use of this role" - Defines the purpose of RBAC roles.
- "this pain helps would help us troubleshoot I mean if there is just one user who's impacted then instead of going into all the tabs... we can probably trouble start the troubleshooting from here as well" - Explains the utility of the Troubleshoot blade for user-specific issues.

### Overall Assessment
This source provides a valuable, albeit slightly outdated (due to UI changes and new features since 2019), walkthrough of the Intune Ibiza console. It effectively demonstrates the basic navigation and the purpose of key administrative sections. The explanation of the distinction between Azure AD devices and Intune-managed devices, and the difference between compliance and configuration policies, is particularly helpful for understanding core Intune concepts at a foundational level. While the "Level 100" designation is appropriate as it focuses on demonstrating "how to click" rather than in-depth conceptual understanding, it successfully covers a wide range of essential Intune administration tasks and their location within the console. The inclusion of practical examples for policy creation and app deployment enhances the value. The source clearly emphasizes the prerequisites for both administrators and end-users.

### Areas for Further Exploration (based on the source's limitations)
- Deeper dives into specific configuration policy types (e.g., Wi-Fi, VPN, certificates).
- Detailed explanations of enrollment methods like DEP and Autopilot.
- More in-depth coverage of App Protection Policies and their configurations.
- Updates on newer features and UI changes in the Intune console since 2019.
- Practical demonstrations of Conditional Access policies and their interaction with Intune compliance.

