Windows Enrollment with Intune Notes
1. Introduction to Windows Enrollment in Intune
1.1 Purpose of Windows Enrollment
Windows enrollment in Microsoft Intune enables organizations to manage Windows 10 devices, ensuring security, compliance, and efficient policy and application deployment. Key objectives include:

Secure Management: Apply policies to protect corporate data and ensure compliance.
Application Deployment: Distribute apps to devices, including available apps via the Company Portal.
Device Management: Enable centralized management for both corporate-owned and BYOD devices.
Troubleshooting and Monitoring: Provide tools to verify enrollment and resolve issues.
Integration with Azure AD: Leverage Azure AD for identity and access management, depending on the enrollment method.

1.2 Key Concepts

Azure AD Registration (Workplace Join): Establishes trust between a device and Azure AD, primarily for BYOD scenarios, allowing access to cloud-protected resources.
Azure AD Join: Joins a company-owned device to Azure AD, replacing on-premises domain join in cloud-native environments.
Hybrid Azure AD Join: Combines on-premises Active Directory (AD) domain join with Azure AD integration for hybrid environments.
MDM Scope: Determines which users’ devices are automatically enrolled in Intune upon Azure AD Registration or Join.
Enrollment Goal: Regardless of the method, the end state is a device managed by Intune with consistent policy application.

2. Windows Enrollment Methods
2.1 Enroll Only in Device Management

Description: Direct enrollment into Intune without Azure AD registration or join.
Use Case: Organizations with only an Intune license, no Azure AD Premium or EMS suite.
Process:
User navigates to Start > Settings > Accounts > Access work or school > Enroll only in device management.
User enters their corporate email address, triggering MDM endpoint discovery.
User enters their password, and enrollment completes.


Key Features:
Device appears only in All devices in the Intune portal, not in Azure AD devices.
No Azure AD integration.


Event Log: Successful enrollment generates Event ID 72 in DeviceManagement-Enterprise-Diagnostic-Provider > Admin.
Example: A contractor’s personal laptop enrolls in Intune to receive security policies without Azure AD integration.

2.2 Auto-Enrollment via Azure AD Registration (Workplace Join)

Description: User performs Azure AD Registration, and if included in the MDM scope, the device is automatically enrolled in Intune.
Use Case: BYOD scenarios where users access cloud resources.
Process:
User navigates to Start > Settings > Accounts > Access work or school > Connect.
User enters corporate credentials.
Device registers with Azure AD (Workplace Join).
If the user is in the MDM scope, Intune enrollment is triggered automatically.


Key Features:
Device appears in both Azure AD devices (as registered) and All devices in Intune.
No device-level changes; a certificate is issued by MS-Organization-Access.


Verification:
Run dsregcmd /status: Shows WorkplaceJoined: YES.
Check Azure AD devices for device entry with user and registration details.


Event Log: Successful enrollment generates Event ID 72.
Example: A salesperson registers their personal laptop with Azure AD to access corporate apps, triggering automatic Intune enrollment.

2.3 Auto-Enrollment via Azure AD Join

Description: Device is joined to Azure AD, and if the user is in the MDM scope, it is automatically enrolled in Intune.
Use Case: Company-owned devices in cloud-native environments.
Process:
User navigates to Start > Settings > Accounts > Access work or school > Join this device to Azure Active Directory.
User enters corporate credentials, and the device restarts.
If the user is in the MDM scope, Intune enrollment occurs automatically.


Key Features:
Device joins Azure AD domain; user logs in with work/school account.
Benefits include SSO, Windows Hello for Business, and device-based Conditional Access.
Device appears in Azure AD devices (as Azure AD Joined) and All devices in Intune.


Verification:
Run dsregcmd /status: Shows AzureAdJoined: YES, DomainJoined: NO.
Check certificate issued by MS-Organization-Access.


Event Log: Successful enrollment generates Event ID 72.
Example: A corporate laptop is Azure AD joined during setup, automatically enrolling in Intune for policy management.

2.4 Enrollment via Group Policy (Hybrid Azure AD Join)

Description: Group Policy triggers enrollment for Hybrid Azure AD Joined devices.
Use Case: Organizations with on-premises AD and Azure AD integration.
Prerequisites:
Device must be Hybrid Azure AD Joined (using Azure AD Connect).
User must have an EMS or Intune license.
Group Policy configured with Windows 10 Administrative Templates.


Process:
Configure a Group Policy Object (GPO) to enable automatic MDM enrollment.
GPO creates scheduled tasks running deviceenroller.exe.
Device performs Azure AD Registration (if not already done) and enrolls in Intune if in the MDM scope.


Key Features:
Device appears in Azure AD devices (as Hybrid Azure AD Joined) and All devices in Intune.
Requires UPN suffix alignment if on-premises domain differs from Azure AD.


Verification:
Run dsregcmd /status: Shows AzureAdJoined: YES, DomainJoined: YES.
Check Event ID 75 in DeviceManagement-Enterprise-Diagnostic-Provider > Admin.


Example: A domain-joined desktop in a hybrid environment enrolls in Intune via GPO, receiving security policies.

2.5 Enrollment via SCCM/Co-Management

Description: Leverages SCCM client to enroll Hybrid Azure AD Joined devices in Intune.
Use Case: Organizations transitioning from SCCM to Intune in a hybrid setup.
Process: SCCM triggers enrollment, similar to Group Policy, for devices in the MDM scope.
Key Features:
Device appears in both Azure AD devices and All devices in Intune.
Supports hybrid management with SCCM and Intune.


Event Log: Successful enrollment generates Event ID 75.
Example: A corporate PC managed by SCCM enrolls in Intune for additional cloud-based policies.

3. Azure AD Concepts and Their Relation to Intune
3.1 Azure AD Registration (Workplace Join)

Purpose: Establishes trust between a device and Azure AD for accessing cloud resources.
Use Case: BYOD devices accessing corporate apps.
Process: User initiates via Access work or school > Connect.
Device Changes: No system-level changes; adds a certificate to the user’s store.
Relation to Intune: Not a prerequisite but enables auto-enrollment if in MDM scope.
Verification:
dsregcmd /status: WorkplaceJoined: YES.
Certificate issued by MS-Organization-Access.
Device entry in Azure AD devices.



3.2 Azure AD Join

Purpose: Joins company-owned devices to Azure AD, replacing on-premises domain join.
Use Case: Cloud-native organizations or devices without on-premises AD.
Process: User initiates via Access work or school > Join this device to Azure AD.
Device Changes:
Device joins Azure AD domain.
Work/school account added to local administrators.


Benefits:
Seamless SSO with Primary Refresh Token (PRT).
Support for Windows Hello, Conditional Access, and Enterprise State Roaming.


Relation to Intune: Enables auto-enrollment if in MDM scope.
Verification:
dsregcmd /status: AzureAdJoined: YES, DomainJoined: NO.
Certificate issued by MS-Organization-Access.



3.3 Hybrid Azure AD Join

Purpose: Combines on-premises AD domain join with Azure AD integration.
Use Case: Hybrid environments with existing AD infrastructure.
Process: Configured via Azure AD Connect.
Relation to Intune: Required for Group Policy or SCCM enrollment.
Verification:
dsregcmd /status: AzureAdJoined: YES, DomainJoined: YES.
Device entry in Azure AD devices as Hybrid Azure AD Joined.



4. Intune Portal Configuration for Enrollment
4.1 MDM Scope for Auto-Enrollment

Location: Azure portal > Azure Active Directory > Mobility (MDM and MAM).
Function: Specifies users whose devices auto-enroll in Intune upon Azure AD Registration or Join.
Settings: None, All, or Some (specific group).
Scope: Applies only to Windows devices.
Configuration:
In the Azure portal, navigate to Azure Active Directory > Mobility (MDM and MAM) > Microsoft Intune.
Set MDM user scope to All or Some (select a group, e.g., “Intune_Users”).
Save the configuration.


Example: Set MDM scope to a group containing all employees to auto-enroll their Azure AD-joined devices in Intune.

4.2 Group Policy for Hybrid Azure AD Join Enrollment

Prerequisites:
Devices must be Hybrid Azure AD Joined.
Users must have an EMS or Intune license.
Windows 10 Administrative Templates (.admx) installed on the domain controller.


Configuration:
Download Windows 10 Administrative Templates from Microsoft.
Copy policy definition files to Sysvol folder on the domain controller.
Open Group Policy Management and create a new GPO (e.g., “Intune_Enrollment”).
Navigate to Computer Configuration > Policies > Administrative Templates > Windows Components > MDM.
Enable Automatic MDM enrollment using default Azure AD credentials.
Apply the GPO to an Organizational Unit (OU) containing target devices.
If the on-premises domain differs from Azure AD, add a matching UPN suffix in Active Directory Domains and Trusts.


Example: Configure a GPO to enroll all domain-joined PCs in an OU into Intune automatically.

5. Company Portal Application

Purpose: Provides a user interface for accessing available apps, syncing devices, or unenrolling.
Necessity: Not required for enrollment; can be triggered via Settings or deep links.
Configuration:
In Intune, go to Apps > All apps > Add.
Select Store app > Windows > Microsoft Store for Business.
Search for Company Portal, add it, and assign it as Available to users.


Example: Deploy the Company Portal to allow users to install optional corporate apps.

6. Enrollment Verification and Troubleshooting
6.1 Verification Tools

dsregcmd /status:
Checks Azure AD join/registration status and PRT presence.
Example Output:+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+
AzureAdJoined : YES
DomainJoined : NO
WorkplaceJoined : NO




Event Viewer Logs:
DeviceManagement-Enterprise-Diagnostic-Provider > Admin:
Event ID 72: Enroll only or auto-enrollment via Azure AD Registration.
Event ID 75: Group Policy or SCCM enrollment.


User Device Registration > Admin: Logs for Azure AD Registration/Join.


Registry Keys:
HKLM\SOFTWARE\Microsoft\Enrollments: Contains GUID subkeys with enrollment details (e.g., UPN, Intune Device ID).


Certificates:
MS-Organization-Access: Indicates Azure AD Registration or Join.
Microsoft Intune MDM Device CA: Indicates successful Intune enrollment.


MDM Diagnostic Log Report:
Generated via Access work or school > Info > Export.
Located at C:\Users\Public\Documents\MDMDiagnostics.
Details management status and policies.



6.2 Portal Views

Azure AD Devices: Shows Azure AD Registered, Joined, or Hybrid Joined devices.
All Devices (Intune): Shows all Intune-enrolled devices, regardless of Azure AD status.
Note: These views are mutually exclusive; a device enrolled only in Intune won’t appear in Azure AD devices.

6.3 Troubleshooting Steps

Issue: Enrollment fails.
Check: Run dsregcmd /status to verify Azure AD status.
Check: Ensure user is in MDM scope (for auto-enrollment).
Check: Verify Event IDs (72 or 75) in Event Viewer.
Check: Confirm certificates in user/device store.


Issue: Policies not applying.
Check: Review MDM Diagnostic Log for policy application errors.
Check: Ensure device is enrolled in Intune (All devices).


Example: A device fails auto-enrollment. dsregcmd /status shows WorkplaceJoined: NO. Check MDM scope in Azure portal and add the user to the scope group.

7. Example Scenarios
Scenario 1: BYOD Auto-Enrollment via Azure AD Registration

Objective: Enroll a user’s personal laptop in Intune.
Steps:
User navigates to Settings > Access work or school > Connect.
User enters corporate credentials, triggering Azure AD Registration.
Device auto-enrolls in Intune (user is in MDM scope).
Intune pushes security policies and apps.


Configuration:
Set MDM scope to include the user’s group.
Deploy Company Portal as an available app.


Outcome: Laptop is managed by Intune, with corporate apps accessible via the work profile.

Scenario 2: Hybrid Azure AD Join via Group Policy

Objective: Enroll domain-joined PCs in Intune.
Steps:
Configure Azure AD Connect for Hybrid Azure AD Join.
Create a GPO to enable automatic MDM enrollment.
Apply GPO to the OU containing target PCs.
Verify enrollment in Intune (All devices).


Configuration:
Add UPN suffix in AD Domains and Trusts if needed.
Set MDM scope to include relevant users.


Outcome: PCs are managed by both on-premises AD and Intune.

8. Key Concepts and Best Practices

Choose Enrollment Method:
Enroll Only: For organizations without Azure AD Premium.
Auto-Enrollment: For BYOD or cloud-native corporate devices.
Group Policy/SCCM: For hybrid environments.


MDM Scope: Configure carefully to control auto-enrollment.
Licensing: Ensure users have EMS or Intune licenses for auto-enrollment.
Troubleshooting: Use dsregcmd, Event Viewer, and MDM Diagnostic Logs for quick diagnosis.
Certificates: Verify both Azure AD and Intune certificates for successful enrollment.
Portal Views: Understand the distinction between Azure AD devices and All devices.

9. Conclusion
Windows enrollment in Intune offers multiple methods tailored to organizational needs, from BYOD to hybrid environments. Understanding Azure AD concepts (Registration, Join, Hybrid Join) is crucial for leveraging auto-enrollment, while Group Policy and SCCM support hybrid scenarios. Robust verification and troubleshooting tools ensure successful enrollment and policy application, enabling consistent device management across diverse environments.
