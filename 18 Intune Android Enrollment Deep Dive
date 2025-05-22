Android Enrollment with Intune Notes
1. Introduction to Android Enrollment in Intune
1.1 Purpose of Android Enrollment
Android enrollment in Microsoft Intune enables organizations to manage Android devices, ensuring security, compliance, and efficient application deployment. Key objectives include:

Secure Management: Apply policies to protect corporate data.
Data Separation: Isolate work and personal data, especially for Bring Your Own Device (BYOD) scenarios.
Application Deployment: Distribute approved apps via the Managed Google Play Store.
Compliance Enforcement: Ensure devices meet organizational standards.
Troubleshooting and Monitoring: Address device and policy issues effectively.

1.2 Evolution of Android Management
Android device management via Mobile Device Management (MDM) has evolved significantly:

Early Support: MDM supported since Android 2.2 (Froyo, 2010).
Intune Support: Compatible with Android 4.0 (Ice Cream Sandwich) and above.
Legacy Method (Device Admin):
Managed the entire device, including personal data.
Supported actions like full device wipes.
Drawbacks:
No separation of work and personal data, unsuitable for BYOD.
Inconsistent API implementation by OEMs, leading to management challenges.


Status: Deprecated starting with Android 10 (API 29). Devices on Android 10+ must use Android Enterprise.


Modern Method (Android Enterprise):
Introduced with Android 5.0 (Lollipop).
Provides standardized management via mandatory APIs.
Key Benefits:
Containerization of work data in a secure, encrypted work profile.
No admin visibility into personal data (e.g., location, calls, photos).
Consistent management across devices.
Streamlined app distribution via Managed Google Play Store.
Support for Factory Reset Protection and zero-touch enrollment.





2. Android Enterprise Enrollment Variants
2.1 Work Profile (BYOD)

Use Case: Bring Your Own Device (BYOD) scenarios.
Description: Creates an encrypted work profile on the user’s personal device, managed by Intune.
Key Features:
Policies apply only to the work profile.
Personal data remains untouched; removing enrollment deletes only the work profile.
Supports two instances of apps (work and personal).
Data Loss Prevention (DLP) policies (MAM) prevent data leakage from work to personal apps.


Example: A user installs Teams in the work profile for corporate communication, while a personal Teams instance remains separate.

2.2 Device Owner (Corporate-Owned)

Use Case: Corporate-Owned Devices (COD).
Description: The organization fully manages the device.
Subcategories:
COBO (Corporate Owned, Business Only) / Fully Managed User Devices:
Entire device is managed for corporate use.
User-associated; supports compliance and MAM policies.
Appears as "Android fully managed" in Intune.
Example: A corporate-issued device for an employee, locked to business apps.


COSU (Corporate Owned, Single Use) / Dedicated Devices:
Designed for single-purpose use (e.g., kiosks, delivery scanners).
No user association; no compliance or MAM policies.
Configured to run only whitelisted apps.
Appears as "Android dedicated" in Intune.
Example: A retail kiosk running a single inventory app.


COPE (Corporate Owned, Personal Enabled):
Allows personal use on corporate-owned devices.
Manages both work and personal spaces.
Status: Not currently supported by Intune.





3. Enrollment Flow
3.1 Work Profile Enrollment (BYOD)

Process:
User installs the Company Portal app from the Google Play Store.
User signs in with corporate credentials (requires Intune license).
Company Portal performs package integrity checks.
Device registers with Google’s push notification service (GCM/FCM).
Company Portal connects to Microsoft Graph for branding.
Authentication:
Username triggers Home Realm Discovery.
Password authenticates via Azure AD or ADFS, issuing tokens.


Workplace Join: Device generates a certificate, registers with Azure AD, and receives an Azure AD device ID.
MDM Enrollment:
Another certificate is generated; its public key becomes the Intune device ID.
Enrollment restrictions and Intune license are verified.


Compliance is evaluated, and configuration profiles/policies are delivered.
Device categories can be selected for tagging in Intune.


Example:
A user downloads the Company Portal, signs in with their corporate email, and enrolls their personal Android phone. A work profile is created, and Intune pushes corporate apps (e.g., Outlook) and compliance policies.



3.2 COBO/COSU Enrollment (QR Code)

Prerequisites: Brand new or factory-reset device.
Process:
At the device’s welcome/language screen, tap the screen 4-5 times.
Connect to Wi-Fi.
Device downloads a QR code reader.
Scan the QR code generated in the Intune portal.
Device downloads the Microsoft Intune app (not Company Portal).
For COBO, user signs in with corporate credentials; for COSU, no user sign-in during setup.
Workplace Join and MDM enrollment occur as in the Work Profile flow.
Device registers with Intune and receives policies/apps.
For COSU, the Managed Home Screen app is deployed to provide a kiosk experience.


Example:
A factory-reset tablet for a retail kiosk scans a QR code, enrolls as a COSU device, and is locked to a single inventory app via the Managed Home Screen.



4. Policy Distribution

Model: Push-based using Google’s GCM/FCM service.
Process:
Intune sends a push notification via GCM/FCM using the device’s registration ID.
The MDM broker (Company Portal or Microsoft Intune app) receives the notification.
The broker initiates a direct Device Management (DM) session with Intune to fetch policies.


Communication: Typically over port 5228.
Example: Intune pushes a policy to restrict camera use in the work profile. The Company Portal receives the notification and applies the policy during the next check-in.

5. Intune Portal Configuration for Enrollment
5.1 Enrollment Restrictions

Purpose: Control allowed enrollment types (Device Admin, Android Enterprise) and OS versions.
Configuration:
In the Microsoft Endpoint Manager admin center, go to Devices > Enroll devices > Enrollment restrictions.
Create or edit a restriction policy.
Specify allowed platforms (e.g., Android Enterprise only) and minimum/maximum OS versions.
Assign the policy to user groups.
Note: If both Device Admin and Android Enterprise are allowed, Android Enterprise takes precedence.


Example: Restrict enrollment to Android Enterprise Work Profile for devices running Android 10 or later.

5.2 COBO/COSU Enrollment Tokens

Process:
Go to Devices > Android > Android enrollment > Corporate-owned, fully managed or dedicated devices.
Create an enrollment profile and generate a QR code or token.
Assign the token to a dynamic Azure AD group for policy targeting.


Example: Create a token named “Kiosk_Devices” for COSU enrollment, assign it to a dynamic group, and target a policy to whitelist a single app.

5.3 Device Configuration Profiles

Purpose: Configure device settings and, for COSU, whitelist apps.
Configuration:
Go to Devices > Configuration profiles > Create profile.
Select Android Enterprise and the appropriate mode (Work Profile, Fully Managed, Dedicated).
Configure settings (e.g., Wi-Fi, app restrictions).
For COSU, specify whitelisted apps in kiosk mode.
Assign the profile to a device or user group.


Example: Create a COSU profile to lock a device to the “InventoryApp” and disable all other apps.

6. Troubleshooting and Challenges
6.1 OEM Customizations

Issue: Some OEMs (e.g., Xiaomi, Oppo, Vivo, Poco) customize Android, potentially breaking management APIs.
Solution: Test devices with the Test DPC app (available on Google Play) to verify work container creation.
Example: If a Xiaomi device fails to enroll, install Test DPC. If it fails to create a work container, the issue is likely OEM-related.

6.2 Battery Optimization

Issue: Android’s battery optimization may put apps (e.g., Outlook) into a low-power state, affecting MAM policies or causing data resets.
Solution: Disable battery optimization for company apps.
Configuration:
On the device, go to Settings > Apps > Select the app (e.g., Company Portal).
Select Battery > Battery optimization > Set to Don’t optimize.


Example: Disable battery optimization for Outlook to ensure MAM policies apply consistently.

6.3 Allowing Unknown Sources

Issue: Required for Line-of-Business (LOB) app deployment, triggering a security prompt.
Solution: Guide users to enable “Install from unknown sources” in the Company Portal settings.
Example: A user receives a prompt to allow unknown sources when installing a custom LOB app via Company Portal.

6.4 Log Analysis

Types of Logs (from Company Portal):
Company Portal Log: General enrollment troubleshooting (scrubbed logs).
OMA DM Log: Details post-enrollment events (policy/app deployment).
Device Log: Device-level information.
Broker Auth Log: Authentication details.


Process:
In the Company Portal app, go to Settings > Send diagnostic data.
Save logs to the SD card or email them.


Example: Analyze OMA DM logs to identify why a policy failed to apply (e.g., timestamped error for a specific configuration).

6.5 Decoding QR Codes

Process: Use an online QR code decoder to view the token and signature in COBO/COSU QR codes.
Unsupported Customization: Edit decoded text (e.g., add app bundle IDs) and generate a new QR code (not supported by Microsoft).
Example: Decode a QR code to verify the enrollment profile name matches the intended COSU token.

7. Key Concepts and Best Practices

Android Enterprise Requirement: Mandatory for Android 10+ due to Device Admin deprecation.
Data Containerization: Critical for BYOD, ensuring work and personal data separation.
Enrollment Type Selection:
Use Work Profile for BYOD.
Use COBO for user-associated corporate devices.
Use COSU for single-purpose devices.


Dynamic Groups: Leverage Azure AD dynamic groups for targeting policies based on enrollment tokens.
Testing: Use Test DPC to diagnose enrollment issues before deploying.
Battery Optimization: Always check for company apps to ensure policy reliability.
Logs: Regularly collect and analyze logs for troubleshooting.

8. Example Scenarios
Scenario 1: BYOD Work Profile Enrollment

Objective: Enroll a user’s personal Android device with a work profile.
Steps:
User installs Company Portal from Google Play.
User signs in with corporate credentials.
Device enrolls, creating a work profile.
Intune pushes MAM policies and apps (e.g., Outlook, Teams).


Configuration:
Create an enrollment restriction allowing Android Enterprise Work Profile.
Assign a MAM policy to prevent data sharing between work and personal apps.


Outcome: User accesses corporate apps in the work profile, with personal data untouched.

Scenario 2: COSU Kiosk Setup

Objective: Configure a tablet as a retail kiosk.
Steps:
Factory reset the tablet.
Tap the welcome screen 5 times, connect to Wi-Fi, and scan a QR code from Intune.
Device enrolls as COSU and installs the Microsoft Intune app.
Intune deploys the Managed Home Screen and a single inventory app.


Configuration:
Create a COSU enrollment token in Intune.
Assign a configuration profile to whitelist the inventory app and enable kiosk mode.
Use a dynamic Azure AD group to target the profile.


Outcome: Tablet runs only the inventory app in kiosk mode.

9. Conclusion
Android enrollment with Intune has transitioned from the limited Device Admin model to the robust Android Enterprise framework, offering standardized management and data containerization. Work Profile supports BYOD, while COBO and COSU cater to corporate-owned devices with varying use cases. Proper configuration of enrollment restrictions, tokens, and policies, combined with troubleshooting tools like Test DPC and logs, ensures effective device management. Despite challenges from OEM customizations, Android Enterprise provides a consistent and secure approach to managing Android devices in Intune.
