Hello everyone,

Welcome to today's session, where we will discuss Intune. This is a Level 100 video, providing a walkthrough of the Intune Ibiza console. We will cover policy creation, configuration, compliance, protection policies, MAM and MDM configuration, and device enrollment setup.

## Accessing the Intune Portal

There are three ways to access the Intune portal:
1. **Portal.azure.com** – Direct login via the Azure portal.
2. **Portal.office.com** – Login via Office portal and navigate to the Admin Center.
3. **DeviceManagement.microsoft.com** – Direct access to the Intune device management console.

### Differences Between Portals
- **DeviceManagement.microsoft.com**: Used mainly by Intune Admins.
- **Portal.azure.com**: Used by Global Admins managing both Intune and Azure.

The Ibiza console is the modern Intune console, replacing the older Silverlight console.

## Understanding Azure Active Directory & User Management
- Users and groups in Azure Active Directory (AAD) function like an on-premise domain controller.
- Users can be created manually in Azure AD or synced from an on-prem AD using Azure AD Connect.
- Password resets for cloud users are possible, but not for on-prem synced users unless password write-back is enabled.

## Microsoft Intune Console Overview
The console contains various sections:
- **Dashboard**: Displays device enrollment, compliance, configuration, and app deployment status.
- **Devices Section**: Shows all devices managed by Intune.
- **Users Section**: Lists users assigned to the tenant.
- **Groups Section**: Contains security and distribution groups.
- **Conditional Access**: Used for access control policies.

## Understanding Device Enrollment
### Prerequisites for Device Enrollment
1. **Assign an Intune License**: The user must have an EMS E3/E5 license.
2. **Set MDM Scope**:
   - Navigate to **Azure AD > Mobility (MDM & MAM) > Microsoft Intune**.
   - Set MDM scope to "All".

### Enrollment Methods
- **Windows Devices**: Users enroll through Settings > Accounts > Access work or school.
- **iOS Devices**: Requires an Apple Push Notification (APN) certificate setup.
- **Android Devices**:
  - **Android Legacy Enrollment**: Standard enrollment using Company Portal.
  - **Android Enterprise**: Provides additional restrictions and management options.

### Enrollment Restrictions
Admins can restrict enrollment by:
- **Device Type Restrictions**: Limit device types (e.g., block iOS devices for certain users).
- **Device Limit Restrictions**: Restrict the number of devices a user can enroll.

## Device Management
### Difference Between All Devices and Azure AD Devices
- **All Devices (Intune)**: Devices actively managed by Intune.
- **Azure AD Devices**: All devices registered or joined to Azure AD, including those not managed by Intune.

### Device Compliance and Cleanup Rules
- **Device Cleanup Rules**: Automatically removes inactive devices after a set period.
- **Device Categories**: Helps classify devices for better management.

## Apple APN Certificate Setup
1. Download the CSR file from the Intune portal.
2. Upload it to Apple's Push Certificate Portal.
3. Download the APN certificate and upload it back to Intune.

This setup is required for Intune to communicate with Apple devices.

## Android for Work Setup
- Default Android enrollment is **Legacy**.
- To enable **Android Enterprise**, configure device type restrictions to allow Android for Work.

## Windows Autopilot & Bulk Enrollment
- **Windows Autopilot**: Simplifies bulk enrollment of corporate-owned Windows devices.
- **Device Enrollment Manager (DEM)**: Allows a single account to enroll multiple shared devices.

## Enrollment Status Page
- Displays installation progress of policies and applications.
- Admins can configure it to block access until all required apps are installed.

## Audit Logs & Device Actions
- **Audit Logs**: Tracks administrative actions in Intune.
- **Device Actions**: Logs admin-initiated actions like wipes and resets.

---

This document provides a detailed overview of Intune Ibiza console navigation and management. Let me know if you need further elaboration on any section!

