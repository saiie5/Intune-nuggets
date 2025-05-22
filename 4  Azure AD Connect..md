# Briefing Document: Azure AD Connect Review

**Source**: Excerpts from - "Setting up Azure AD Connect |Level 100" (Video Transcript)

**Date**: 22/5/2025

**Author**: Sai Mavuduru

**Subject**: Review of Azure AD Connect concepts, purpose, functionalities, and key considerations from an Intune management perspective.

## Key Themes

- **Hybrid Identity**: The integration of on-premises identity infrastructure (Active Directory) with cloud-based identity infrastructure (Azure Active Directory). This allows users to have a single identity for both on-premises and cloud resources.
- **Synchronization**: The process of replicating user and device information from on-premises Active Directory to Azure Active Directory using Azure AD Connect.
- **Authentication**: How users are verified when accessing cloud resources, with options for relying on either Azure AD or the on-premises Active Directory.
- **Intune Integration**: The necessity of syncing on-premises users to Azure AD for device enrollment and authentication with Intune.
- **Configuration Choices**: The various settings and determinations that must be made during Azure AD Connect installation, impacting functionality such as authentication method, device syncing, and password management.

## Most Important Ideas/Facts

### The Core Problem Azure AD Connect Solves (from an Intune Perspective)
- When an organization uses Intune for Mobile Device Management (MDM) alongside an on-premises Active Directory environment, a fundamental question arises: how do on-premises users access cloud services like Intune using their existing on-premises credentials?
- Conventionally, without synchronization, a user would need a separate identity (username and password) created specifically within the cloud service (Intune/Azure AD) to enroll devices or access cloud resources. This leads to maintaining multiple identities for the same user, creating confusion and increasing IT Help Desk costs.
- Azure AD Connect eliminates this need by syncing on-premises user accounts to Azure AD.
- **Quote**: "the most basic question that comes into our mind is how do we sync users from our on-prem to the cloud and the second question that comes into our mind is do we have to maintain two separate identity for the same user"
- **Quote**: "for the same user we are maintaining two different identities we are maintaining two usernames and two passwords which are mutually exclusive"
- **Quote**: "How about we sync all the users from our domain controller in our intranet to the internet that is to the azure"

### Azure AD Connect Facilitates Hybrid Identity
- Azure AD Connect is a lightweight tool from Microsoft designed to integrate on-premises directories with Azure AD.
- Its primary goal is to achieve a "hybrid identity goal," where the same user has the same username and password for both on-premises and cloud resources (e.g., Office 365, Azure, other SaaS applications integrated with Azure AD).
- This unification improves user experience and reduces the need for users to remember multiple credentials.
- **Quote**: "Azure ad Connect actually helps us integrate our on-premise directories with the azure ad so over the same identity has been singing with the ajar therefore the same identity can be used to access office 365 as your and other SAS applications integrated with D as your ad"
- **Quote**: "Azure ad Connect is nothing but a tool which helps us does this thing a sink of users from of an on-prem to the azure so that we so that for the same user we don't have to manage and maintain two different identities"
- **Quote**: "the same user can have the same username and password while accessing on Prem as well as the cloud resource and well the final aim or the final goal is that the user should have a vector XPi and they should have one and only one password to remember"

### Authentication Methods (PHS vs. PTA)
- During Azure AD Connect installation, a critical decision is made regarding how users will be authenticated when accessing cloud resources:
  - **Password Hash Synchronization (PHS)**:
    - The user's username and a hash of the hash of their on-premises password are synced to Azure AD.
    - Authentication for cloud services is performed by Azure AD.
    - This is a widely used method where Azure AD itself handles the authentication process in the cloud.
    - The password is not stored in plaintext in Azure AD; it is highly encrypted.
    - **Quote**: "in case of PHS it is the azure ad in this case it is the ad that is doing the authentication"
    - **Quote**: "The hash of the key is to the RSA key undergoes a hash process again and then that key is stored in the a show and as as we discussed in this case the azure ad authenticates the user in the cloud"
  - **Pass-through Authentication (PTA)**:
    - Only the user's username is synced to Azure AD; the password is not synced or stored in the cloud in any form.
    - When a user attempts to log into a cloud resource, Azure AD passes the authentication request back to the on-premises Active Directory domain controller for verification.
    - This method allows organizations to enforce their on-premises Active Directory security and password policies for cloud authentication.
    - Requires the on-premises domain controller to be available for authentication.
    - **Quote**: "What happens and pass through authentication is that in the azure ad connect when we are going to install the azure ad connect we are only going to we are only going to synchronize the username but we are not going to synchronize the password"
    - **Quote**: "Azure actually transfers this request to the on-prem domain controller and the on-prem domain controller is now going to confirm whether the password that the user has entered is correct or not"
    - **Quote**: "in case of PTA it is the on-prem ad that is doing the authentication"

### Synchronization of Devices (Hybrid Azure AD Join)
- Beyond users, Azure AD Connect can also sync device information from on-premises Active Directory to Azure AD.
- This is known as Hybrid Azure AD Join. Devices are joined to both the on-premises domain and Azure AD.
- Enabling this feature during Azure AD Connect setup is crucial for managing domain-joined devices with Intune.
- Azure AD Connect facilitates this by creating a Service Connection Point (SCP) in the on-premises Active Directory, which helps devices discover the Azure AD tenant information.
- **Quote**: "we also have an option of syncing the devices from over on Prem to our azure ad as well so that is also something that we can do syncing of devices using Azure ad connect"
- **Quote**: "if an environment has an unpromising ad footprint and we want the benefits of as your ad as well we can implement hybrid as your ad joined devices These devices are joined to adjoined both to our on-prem adiy and to our as your ideas well"
- **Quote**: "it actually installs something called SCP in the active in the ad on-prem SC is nothing but a service connection point and it is used by our devices to discover the azure ad talent information"

### Device Writeback
- This is the reverse process of syncing devices.
- Device writeback allows information about devices that are Azure AD joined (e.g., a Windows 10 device joined directly to Azure AD, not the on-prem domain) or Azure AD registered (e.g., personally owned devices registered for BYOD) to be written back to the on-premises Active Directory.
- This feature requires an Azure AD Premium license.
- It synchronizes device records (computer objects) from Azure AD back into a specific container within the on-premises Active Directory.
- **Quote**: "if I want that after this devices entry is created in the azure if I want that this device should actually get synced to our on Prem as well then that is something which is referred to as device a right back"
- **Quote**: "device right back as a name is suggesting is the functionality where in the azure ad is able to write back the information of a device to the on-prem meaning that if a device is joined to the azure the devices information is written back to the on-prem domain controller"

### Password Writeback
- By default, if using PHS, password changes made on-premises are synced to Azure AD, but password changes made in Azure AD (e.g., via self-service password reset or admin reset in the portal) are not written back to on-premises AD.
- Password writeback is a premium feature (requires Azure AD Premium license) that allows password changes made in Azure AD to be synchronized back to the user's account in the on-premises Active Directory in real-time.
- This enables users to reset their password in the cloud and have that change applied to their on-premises account, maintaining a single password.
- The new password set in the cloud is checked against the on-premises Active Directory password policies before being committed.
- **Quote**: "password right back gives us the ability to change the password on the azure ad Connect and once we do that the azure ad Connect goes back to the on Prem ad and writes the new password over there"
- **Quote**: "it is a feature that allows password changes in the cloud to be written back to an existing on-premise directory in real time"
- **Quote**: "the new password that I'm going to set for this user is subjected to the same policy as we would have been subjected to if we were resetting the password from here"

### Installation and Configuration
- Azure AD Connect must be installed on a server operating system (2008 and above) that is joined to the same on-premises domain as the users and devices to be synced.
- The server needs access to both the on-premises domain controller and the internet (to connect to Azure AD).
- The installation process requires global administrator credentials for Azure AD and enterprise administrator credentials for the on-premises Active Directory.
- The installer offers "Express settings" for a quicker setup or "Customize" for more granular control over features like PHS/PTA, Hybrid Azure AD Join, Device Writeback, and Password Writeback.
- During setup, specific Organizational Units (OUs) from the on-premises AD can be selected for synchronization, allowing organizations to choose which users and devices are synced to the cloud.
- After the initial setup, the Azure AD Connect console (launched via the shortcut) can be used to configure additional options or modify existing configurations.
- A PowerShell command `Start-ADSyncSyncCycle` can be used to trigger an immediate synchronization cycle, which is useful for testing.

## Summary
Azure AD Connect is an essential tool for organizations with a hybrid IT environment leveraging both on-premises Active Directory and cloud services like Intune. It enables hybrid identity by synchronizing user and device information to Azure AD, allowing users to use a single set of credentials. The tool offers various configuration options, including critical choices for authentication (PHS or PTA), device synchronization (Hybrid Azure AD Join and Device Writeback), and password management (Password Writeback), each impacting how identities and devices are managed and authenticated across the hybrid environment. Understanding these options is vital for a successful Intune implementation in a hybrid setup.
