# Intune Notes: Co-Management Setup and Enrollment

## Introduction to Co-Management
* Co-management allows devices to be managed by both SCCM (System Center Configuration Manager) and Microsoft Intune simultaneously.
* Provides a phased approach to modern management while retaining existing SCCM functionality.
* Supports a bridge from on-premise management to cloud management.

## Core Concept
Co-management signifies the coexistence of both Intune and SCCM managing the same Windows 10 device at the same time. It acts as a bridge to incrementally move devices from an existing SCCM infrastructure towards cloud-based management with Intune, providing a phased approach towards Enterprise Mobility Management (EMM). This allows organizations to leverage SCCM functionalities not yet in Intune (e.g., OSD deployment) while utilizing Intune's cloud-based capabilities (e.g., conditional access, real-time actions). The ability to manage the same device by both platforms is controlled by "workloads," which determine which platform is responsible for specific policies or settings.

## Key Concepts
* **SCCM (Config Manager)**: Traditional on-premises device management solution.
* **Microsoft Intune**: Cloud-based management solution focused on modern device management.
* **Hybrid Setup**: Uses Intune as a delivery mechanism while SCCM manages content and administration.
* **Mixed Authority**: Transition phase where some devices are managed by SCCM and others by Intune.

## What Co-management IS:

* **Simultaneous Management**: Co-management means both Intune and SCCM manage the same Windows 10 device concurrently.
* **Bridging Old and New**: It serves as a bridge to incrementally transition devices from an existing SCCM infrastructure to cloud-based management with Intune.
* **Phased Approach to EMM**: Co-management enables a phased approach to Enterprise Mobility Management (EMM), facilitating gradual migration of workloads.
* **Leveraging Strengths**: It allows the use of SCCM functionalities not yet available in Intune (e.g., OSD deployment) while simultaneously leveraging Intune's cloud-based capabilities (e.g., Conditional Access, real-time actions).
* **Workload Management**: The management of a single device by both platforms is governed by "workloads," which dictate whether SCCM or Intune is responsible for administering specific policies or settings.

## What Co-management is NOT (and related scenarios):

The source clearly distinguishes Co-management from previous scenarios involving both SCCM and Intune:

* **Hybrid Setup**: This older, deprecated model used Intune primarily as a delivery mechanism for policies and apps to mobile devices, with administration and content remaining on-premises with SCCM. The MDM authority was SCCM, and the goal was a "single pane of glass" for managing both on-premises and mobile devices.
    * Example Quote: "Conventionally in the intranet we would be having servers and we would be having machines... however if we wanted to manage a mobile device over the Internet then using a CCM conventionally it was not possible... in this case came into existence a solution called hybrid..."
    * Example Quote: "...hybrid actually used in tune as a delivery channel to devices but SE Siam's on Prem infra was used to administer the content."
    * Example Quote: "...the MDM authority was a CCM in this case and this solution is going to be deprecated very soon."
* **Mixed Authority**: This was a transitional phase between the Hybrid setup and Intune standalone. In a mixed environment, some users were managed by SCCM (via Hybrid) and some by Intune standalone, allowing a phased migration of mobile devices from SCCM/Hybrid to Intune. The overall MDM authority for the tenant was still SCCM.
    * Example Quote: "Between hybrid and in tune stand-alone came a transitional phase referred to as mixed... some users in our environment were managed to buy in tune and some users in our environment were managed by a CCM."
    * Example Quote: "...the transitional phase where in some part where in the hundred users were reporting to in tune and the other 900 users in his environment were reporting to a CCM was referred to as the mixed authority state."
    * Example Quote: "...in both the cases the MDM authority was SCCM and that is not a CO management scenario."
* **Intune Standalone**: This is when Intune is used exclusively to manage devices, without any involvement from SCCM in the management process.
* **Coexistence (with Third-Party MDM)**: This scenario involves a Windows 10 device managed by SCCM and a third-party MDM (not Intune). In this case, SCCM enters a read-only mode for certain workloads (like resource access policies, applications, endpoint protection) to prevent conflicts, relying on the third-party MDM for policy setting. There is no synchronization between SCCM and the third-party MDM.
    * Example Quote: "In case of coexistence what happens is let's just say that there is a device... which is being managed by a SCCM and the end user decides to enroll it to a third-party MDM not to in tune."
    * Example Quote: "...in this case the third-party MDM is going to set the policies... and config manager is only going to extract the relevant reports."

## Key Facts and Ideas about Co-management:

* **Windows 10 Only**: Co-management is currently applicable only to Windows 10 devices (laptops or desktops) that can have the SCCM client installed. It is not for Android, iOS, Mac, Windows Mobile, or Windows Servers.
    * Example Quote: "Co management is only applicable for Windows 10 devices not for Android iOS Mac devices or for Windows mobile phones only for Windows 10 devices..."
* **MDM Authority is Intune**: In a Co-management scenario, the MDM authority for the tenant is always Microsoft Intune, regardless of which platform is managing a specific workload.
    * Example Quote: "the authority for the tenant is in tune standalone in this case... the MDM authority please keep in mind guys is always going to be Microsoft Intune in case of co-management..."
* **No Policy Sync between SCCM and Intune**: Unlike the Hybrid model, in Co-management, policies do not synchronize between the SCCM server and the Intune service. The device determines which platform to get policies from based on the workload settings.
    * Example Quote: "in this case no sync happens the policy is reached to the device and the SCCM client in that device helps the device understand whether it needs to go to the SCCM console or whether it needs to go to the in tune console to fetch the policies..."
* **Workloads Determine Management Source**: Workloads are fundamental to Co-management. They dictate whether SCCM or Intune is responsible for managing specific device configurations or policies (e.g., compliance policies, resource access policies, Windows Update policies, endpoint protection, client apps, Office Click-to-Run apps).
    * Example Quote: "we have something called workload and that workload is used to determine which policy will be administered by s ECM and which policy will be administered by in tune."
* **Default Workload is SCCM**: By default, when Co-management is enabled, all workloads are initially set to SCCM. This means existing SCCM policies continue to apply unless explicitly changed.
    * Example Quote: "By default when we enable co-management all the workload are set at cumin ads are set at a CCM only which means that even if your map if you even if you are any bling co management all the devices by default will be managed by a CCM unless and until we wish to make any changes."
* **"Just Four Clicks" to Enable**: Enabling Co-management in the SCCM console is presented as a very simple process, highlighting its ease of initial setup.
    * Example Quote: "turning on co-management and adding the power of the Microsoft cloud to your config manager deployment is so simple... it literally is just four clicks" - Brad Anderson (Microsoft MVP)
* **No Extra Cost for Enabling**: There is no additional cost simply for enabling Co-management in your SCCM environment. Costs are associated with the Intune licenses required for the devices.
    * Example Quote: "The financial impact is was none cost is nothing in money didn't cost us anything" - Brad Anderson
    * Example Quote: "there is no extra cost involved in enabling co-management..."
* **Immediate Benefits from Enabling**: Even if no workloads are shifted to Intune, enabling Co-management provides immediate benefits, primarily access to Conditional Access.
    * Example Quote: "even if you're not ready to migrate your workload... you simply get new functionality delivered from the Microsoft cloud immediately" - Brad Anderson

## Prerequisites for Co-Management
1.  Config Manager version 1710 or higher.
2.  Windows 10 devices (version 1709 or higher).
3.  Devices must be Hybrid Azure AD joined.
4.  Active SCCM infrastructure.
5.  Auto-enrollment in Intune enabled.

## Architecture Overview
* Windows 10 devices can communicate with both SCCM and Intune.
* SCCM manages devices via on-premises infrastructure.
* Intune manages devices via cloud services.
* SCCM and Intune do not synchronize policies; workloads are directed based on administrator settings.

## Device Management Workloads
* Workloads define which service manages specific device policies.
* Three primary workloads:
    1.  Compliance Policies
    2.  Resource Access Policies
    3.  Client Applications
* By default, all workloads are managed by SCCM until changed.

## Enrollment Methods for Co-Management
1.  **Manual Enrollment**:
    * Users go to "Access Work or School" in Windows settings and enroll manually.
2.  **Group Policy Enrollment**:
    * Configure group policy for auto-enrollment via Active Directory.
3.  **SCCM Console Enrollment**:
    * Enable auto-enrollment in Intune from the SCCM console.

## Enabling Co-Management in SCCM
1.  Navigate to SCCM Console → Administration → Cloud Services → Co-Management.
2.  Sign in with Global Administrator credentials.
3.  Choose to enable co-management for all devices or a pilot collection.
4.  Ensure the SCCM client is installed on devices.
5.  Verify Hybrid Azure AD Join status.

## Policy Flow in Co-Management
1.  SCCM discovers the device.
2.  Pushes the SCCM client to the device.
3.  Device is enrolled to Intune through policy.
4.  SCCM and Intune assign policies based on workload settings.
5.  Device receives policies from the designated authority (SCCM or Intune).

## Co-Management Capabilities
* Compliance policies and conditional access can be managed from Intune.
* Application deployments can be managed via SCCM.
* SCCM remains the primary management tool until workloads are shifted to Intune.

## Monitoring and Troubleshooting
* Logs to monitor:
    * `CCMSetup.log` (Client installation)
    * `PolicyPV.log` (Policy deployment)
    * `CoManagementHandler.log` (Workload transition)
* Use the SCCM console to verify device status and workload assignments.

## Scenarios for Co-Management
1.  **SCCM Managed Devices to Co-Managed**:
    * Default scenario where devices are already under SCCM management and are enrolled in Intune.
2.  **Intune Managed Devices to Co-Managed**:
    * Less common scenario where Intune-managed devices receive the SCCM client.

## Advantages of Co-Management
* Leverage existing SCCM infrastructure while adopting cloud capabilities.
* Phased migration to modern management.
* Conditional access and compliance management through Intune.
* Flexibility in managing workloads.

## Limitations and Considerations
* Co-management is only supported for Windows 10 devices.
* Requires Hybrid Azure AD Join.
* SCCM and Intune do not synchronize policies.
* Workload conflicts must be managed carefully to avoid policy duplication.

## Steps to Transition Workloads
1.  Identify the workloads to transition (e.g., compliance policies).
2.  Set the workload owner (SCCM or Intune) in the SCCM console.
3.  Verify device enrollment in both SCCM and Intune.
4.  Monitor workload delivery via logs.

## Future of Co-Management
* Transitioning from SCCM to Intune standalone is encouraged as Microsoft phases out hybrid solutions.
* Co-management serves as an interim solution for organizations adopting cloud-based management.

## Summary
* Co-management is a powerful approach to modernize device management without abandoning legacy SCCM investments.
* It allows for a gradual transition to cloud-based management while maintaining on-prem capabilities.
* Understanding workloads, prerequisites, and policy flows is essential for successful implementation.

## Key Logs for Troubleshooting
* `CCMSetup.log`: Verifies SCCM client installation.
* `PolicyPV.log`: Tracks policy delivery and status.
* `CoManagementHandler.log`: Monitors workload management and transitions.

By following these guidelines and understanding the co-management architecture, administrators can effectively manage devices across both SCCM and Microsoft Intune platforms.
