Here is a detailed briefing document reviewing the main themes and most important ideas or facts from the provided excerpts on Windows Autopilot:

# Briefing Document: Intune Enrollment Types

**Source:** Excerpts from "#IntuneNugget 5- Types of Enrollment with Intune |Level 100" (Transcript) and "Types of Enrollment with Intune session notes"

**Overall Objective:** To provide a "Level 100" introduction to the various methods for enrolling different operating systems into Microsoft Intune. This is a foundational overview, with more in-depth details available in subsequent sessions.

**Core Concept:** Intune allows for centralized management of a wide range of devices. Understanding the different enrollment methods is crucial for administrators to effectively onboard and manage these devices based on ownership (personal vs. corporate), scale (individual vs. bulk), and existing infrastructure.

## Key Themes and Enrollment Types by Operating System:

### 1. iOS/macOS Device Enrollment:

* **APNs (Apple Push Notification Service)**: A fundamental and mandatory intermediary for any MDM service (including Intune) to communicate with iOS/Mac devices. Requires an Apple ID to set up and renew (recommend using a business Apple ID).

#### 1.1. Manual Enrollment (BYOD - Bring Your Own Device)

* **Use Case**: Primarily for Bring Your Own Device (BYOD) or personal devices.
* **Method**: User installs the "Company Portal" application from the App Store, opens it, logs in with their work credentials, and follows on-screen instructions to allow device management permissions. The device will then register with Intune and apply necessary policies.
* **Prerequisites**:
    * Intune License assigned to the user (direct or part of an EMS E3/E5 suite).
    * APNs Certificate set up in Intune.

#### 1.2. Apple Device Enrollment Program (DEP) - Corporate-Owned Devices

* **Use Case**: Bulk enrollment of corporate-owned iOS and macOS devices.
* **Method**: Leverages Apple's DEP program. Requires configuration within the Apple Business Manager (ABM) portal. The device automatically enrolls into Intune upon activation.
* **Prerequisites**:
    * Apple Business Manager account.
    * DEP token uploaded from ABM to Intune (**Devices > iOS/iPadOS enrollment > Enrollment program tokens**).
    * Intune License.
    * APNs Certificate.
    * Serial numbers of the devices to be managed (listed in ABM).
    * Creation of an Enrollment Profile in Intune to define settings like user affinity, supervision, and the "out-of-the-box" experience.
    * Supervision mode enabled for corporate control.

#### 1.3. Apple Configurator Enrollment

* **Use Case**: Another possible method, though "not very widely used." Primarily for smaller-scale corporate deployments or when DEP is not feasible.
* **Method**: Requires a Mac device with "Apple Configurator" software installed. An enrollment profile is created in Intune, and then the profile is applied to the iOS device via a physical USB connection. The device restarts and is enrolled into Intune.
* **Prerequisites**:
    * A Mac device with Apple Configurator installed.
    * Physical access to the iOS device via a USB cable.
    * Intune service and license.
    * APNs service.

### 2. Windows Device Enrollment:

#### 2.1. Manual Enrollment ("Enroll only in device management")

* **Use Case**: Manual enrollment for individual devices (domain-joined or workgroup).
* **Method**: User goes to **Start > Settings > Access work or school**, and clicks on the "enroll only in device management" hyperlink, then enters work credentials and signs in.
* **Prerequisites**:
    * Windows 10 1607 or later.
    * Intune License assigned to the user (direct or part of an EMS E3/E5 suite).
    * **Note**: This method does not perform Azure AD registration.

#### 2.2. Auto Enrollment

* **Use Case**: Automatic enrollment triggered by Azure AD registration.
* **Method**: User goes to **Start > Settings > Access work or school** and clicks "Connect", entering their username and password. This performs Azure AD registration. The device then gets Azure AD registered first, and if the MDM scope is configured, it is **automatically enrolled in Intune**.
* **Prerequisites**:
    * Windows 10 1607 or later.
    * Intune License.
    * MDM scope must be set for the user performing the Azure AD registration in Azure AD (**Mobility (MDM & MAM) > Microsoft Intune**).

#### 2.3. Group Policy Enrollment (Hybrid Azure AD Join)

* **Use Case**: Enrolling machines joined to an on-premises domain.
* **Method**: Using Group Policy Objects (GPO) to trigger enrollment.
    * Create a GPO for auto-enrollment: Enable "**Enable automatic MDM enrollment using default Azure AD credentials**" under **Computer Configuration > Administrative Templates > Windows Components > MDM**.
    * Link the GPO to an Organizational Unit (OU) with target devices.
    * Devices will enroll into Intune on their next sign-in.
* **Prerequisites**:
    * Windows 10 1709 or later.
    * Device must be joined to on-prem Active Directory.
    * Device must be in a **Hybrid Azure AD Join state** (joined to both the on-prem domain and Azure AD, typically configured using Azure AD Connect).
    * Intune License.
    * MDM user scope must be set in Azure AD.

#### 2.4. Windows Autopilot (Corporate-Owned)

* **Use Case**: Bulk deployment and enrollment of corporate-owned Windows devices, automating Azure AD Join and Intune enrollment during the Out-of-Box Experience (OOBE).
* **Method**: The device, upon first boot (or factory reset), automatically fetches its Autopilot profile and proceeds with pre-configured setup.
* **Prerequisites**:
    * Windows 10 1703 or later.
    * **Azure AD Premium license** (part of EMS suite) is required because the device gets Azure AD joined. An Intune direct license is not sufficient.
    * Hardware hash (like a serial number) of the devices needs to be uploaded to Intune (**Devices > Windows > Windows Enrollment > Devices**, then **Import**).
    * MDM scope for the user must be set.
    * An Autopilot profile must be created to define the OOBE and user experience.
    * Can be configured for **Hybrid Azure AD Join Autopilot** (joins on-prem domain and Azure AD) or **Normal Autopilot** (Azure AD join only). Hybrid Azure AD Join Autopilot requires additional setup like an ODJ connector and a domain join profile.
    * **Self-deploying Autopilot** requires a physical machine with a TPM 2.0 chip.
    * Device needs to be system reset or fresh out of the box.

#### 2.5. Co-management with SCCM

* **Use Case**: Enrolling devices currently managed by System Center Configuration Manager (SCCM) into Intune, bringing them into a co-managed state (dual management by SCCM and Intune).
* **Method**: Leverages SCCM to initiate the enrollment process. SCCM is configured to enable co-management and select workloads to move to Intune.
* **Prerequisites**:
    * Windows 10 1709 or later.
    * SCCM version 1710 or later.
    * Device must be joined to the on-premises domain (initially managed by SCCM).
    * Device must be in a **Hybrid Azure AD Join state** (configured using Azure AD Connect).
    * Intune License (direct or EMS suite).
    * MDM scope must be set for user-based enrollment.
    * Optionally, a collection of devices in SCCM if co-management is not enabled for the entire tenant.
    * No conflicting Group Policies or SCCM policies should be in place.

### 3. Android Device Management (Note: This section focuses on management approaches rather than distinct enrollment methods in the same way as iOS/Windows, but covers two broad categories):

* **Android version 4.0 or later** (for any Android management).
* **Intune License** assigned.

#### 3.1. Android Device Admin (Legacy)

* **Use Case**: Manages the "entire device." "Not a very good approach" for BYOD scenarios.
* **Status**: A "legacy method" that Google is expected to "decommission very soon."

#### 3.2. Android Enterprise (Recommended and Future Forward)

* **Use Case**: Recommended approach, especially for BYOD due to containerization.
* **Method**: Implements "containerization of data," creating a separate corporate container that Intune manages and is encrypted. The rest of the device is the personal container. Enrollment is done via the Company Portal app.
* **Prerequisites**: Android version 5.1 or later (specifically mentioned for BYOD).
* **Flavors for Company-Owned Devices**:
    * **Device Owner (Fully Managed)**: For company-owned devices where the corporate container is the only container. Enrollment is often done by scanning a QR code or using NFC/zero-touch enrollment.
    * **COSU (Company Owned Single Use)**: Kiosk-like devices running with only one application. Enrollment is done by scanning a QR code. Requires whitelisting of the single application.
    * **COBO (Company Owned Business Only)**: Fully managed devices running with multiple business applications. Enrollment is also done using a QR code. Uses the "Microsoft Intune app" (recently released) instead of the Company Portal.
* **General Prerequisites for Android Enterprise**: Device restriction profiles need to be set to guide users towards the appropriate enrollment method (e.g., ensuring BYOD users enroll as Android Enterprise BYOD). For COSU/COBO, enrollment is often done via QR code scanning instead of username/password.

## Common Prerequisites Across Operating Systems:

* **Intune License**: A fundamental requirement for a user or device to be managed by Intune. This can be a direct Intune license or part of a larger suite like EMS E3 or E5.
* **APNs (Apple Push Notification Service)**: Mandatory intermediary for any MDM service to communicate with Apple devices (iOS and and Mac). Requires an Apple ID for setup.
* **MDM Scope**: For Windows auto-enrollment and Group Policy enrollment, the MDM scope needs to be correctly set for the user in Azure AD to trigger automatic enrollment into Intune after Azure AD registration.
* **Hybrid Azure AD Join**: A requirement for Windows Group Policy enrollment and Hybrid Azure AD Join Autopilot/Co-management scenarios. This involves devices being joined to both an on-premises Active Directory domain and Azure AD, often configured using Azure AD Connect.

## Important Considerations:

* This nugget provides a "Level 100" overview; detailed setup procedures are covered in subsequent sessions.
* The choice of enrollment method often depends on device ownership (personal vs. corporate), the need for bulk deployment, and existing infrastructure (e.g., on-premises domain and SCCM).
* Android Device Admin is a legacy method and Android Enterprise is the recommended path forward.
* Apple DEP and Windows Autopilot are specifically designed for corporate-owned bulk deployments.

This briefing provides a foundational understanding of the various enrollment options available in Intune. Further detail and configuration steps for each method are available in more advanced resources.
