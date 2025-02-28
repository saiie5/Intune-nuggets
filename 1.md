# Introduction to Microsoft Intune (Level 100)

## 1. What is Microsoft Intune?
- Microsoft Intune is a cloud-based device and application management solution provided by Microsoft.
- It is part of the Microsoft Endpoint Manager suite and allows management without an on-premises infrastructure.
- It enables organizations to secure and manage devices (Windows, macOS, iOS, Android) and applications.

## 2. Why Do We Need Intune?
- **Modern Device Usage**: Users access corporate data from personal devices outside the corporate network.
- **Cloud-Based Management**: Traditional on-premises management tools (e.g., Group Policy, SCCM) are limited to domain-joined devices.
- **Secure Access**: Ensure only authorized users and devices access corporate data while protecting sensitive information.
- **Zero-Touch Deployment**: Automate device setup and configuration remotely.

## 3. Device Management Overview
- **Traditional Device Management**:
  - Relies on on-prem infrastructure (e.g., Active Directory, SCCM).
  - Requires domain-joined devices and VPN for remote access.
- **Modern Device Management (MDM)**:
  - Manages devices over the cloud.
  - Supports personal devices (BYOD) and corporate-owned devices.
  - No need for on-prem infrastructure or VPN access.

## 4. Key Components of Microsoft Intune
- **MDM (Mobile Device Management)**:
  - Manages entire devices and enforces security policies.
  - Used for corporate-owned devices.
  - Capabilities: enforce encryption, block features (e.g., cameras), push OS updates, and factory reset devices.
- **MAM (Mobile Application Management)**:
  - Manages specific applications without controlling the entire device.
  - Ideal for BYOD scenarios.
  - Capabilities: control copy-paste, enforce encryption within apps, and wipe corporate data from applications.

## 5. How Intune Works
1. **Admin Portal**: IT admins configure policies via the Intune web-based console.
2. **Device Enrollment**: Users enroll devices by installing the Company Portal app.
3. **Policy Deployment**: Policies and configurations are pushed to devices.
4. **Monitoring & Reporting**: Devices report back to Intune, allowing monitoring and compliance checks.

## 6. Use Cases of Microsoft Intune
- **Secure Email Access**: Control access to on-premises and cloud-based (Office 365) email.
- **Application Management**: Deploy and control corporate applications.
- **Compliance Policies**: Ensure devices meet security standards (e.g., password complexity, encryption).
- **Remote Management**: Support remote workforce without requiring VPN.

## 7. Intune Architecture
- **Cloud-Based Management**: Intune operates as a SaaS (Software as a Service) on Azure.
- **Graph API**: Intune uses Graph API for communication and policy enforcement.
- **Azure AD Integration**: Authentication is managed via Azure Active Directory.
- **Device Communication**: Devices communicate with Intune over the internet for policy updates and reporting.

## 8. Enrollment Process
1. Admin configures Intune policies.
2. Users install the Company Portal app.
3. Devices register with Intune.
4. Policies are applied, and devices sync periodically (6-8 hours).

## 9. Choosing Between MDM and MAM
| **Feature**                | **MDM**                          | **MAM**                        |
|----------------------------|-----------------------------------|---------------------------------|
| **Scope**                  | Entire Device                    | Specific Applications          |
| **Best for**               | Corporate-owned devices           | BYOD (Bring Your Own Device)   |
| **Capabilities**           | Device settings, OS updates       | Data protection within apps    |
| **Device Enrollment**      | Required                          | Not required                  |
| **Data Removal**           | Full device wipe                 | Selective app data wipe        |

## 10. Design Considerations
- **Identity Management**: Choose between Azure AD authentication or on-prem AD with ADFS.
- **Device Type**: Identify whether to manage corporate devices (MDM) or personal devices (MAM).
- **Security Policies**: Implement multi-factor authentication (MFA) and compliance checks.
- **Integration**: Integrate with other Microsoft services (Azure Information Protection, Windows Defender).

## 11. Benefits of Microsoft Intune
- **Scalability**: Manage thousands of devices remotely.
- **Security**: Enforce security standards and prevent data leakage.
- **Flexibility**: Supports multiple device platforms and hybrid environments.
- **User Experience**: Seamless access to corporate resources with minimal user intervention.

