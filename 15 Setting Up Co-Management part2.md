# Intune Notes: Co-Management Setup and Enrollment

## Core Concept
Co-Management is a state where a single Windows 10 device is managed simultaneously by both SCCM and Microsoft Intune. This allows organizations to leverage the strengths of both management solutions during a transition to cloud-based management or for ongoing dual management.

## Key Themes and Most Important Ideas

### 1. Prerequisites for Co-Management Setup

The demo highlights the essential prerequisites before configuring Co-Management:

* A working SCCM server (version 1902 is used in the demo).
* Windows 10 devices joined to the on-premises domain.
* Azure AD Connect tool configured, resulting in devices being in a hybrid Azure AD joined state.

The demonstration starts with two devices already meeting these prerequisites, but not yet enrolled in Intune.

### 2. Initial SCCM Client Installation

The first practical step in the Co-Management setup is ensuring the SCCM client is installed and functional on the target devices. This is demonstrated by manually initiating the client installation from the SCCM console.

**Key Fact:** The SCCM client installation process can be tracked on the client device by monitoring the `CCMSetup.log` file. A successful installation is indicated by an exit code of `0`.

After successful installation, the device status in the SCCM console will change to indicate that the client is installed (a green tick is expected, though it might initially show a question mark).

### 3. Enabling Co-Management in the SCCM Console

Enabling Co-Management in the SCCM console is a one-time configuration step.

#### Configuration Steps:

1.  Navigate to **Cloud Services > Co-management** in the SCCM console.
2.  Click "Configure co-management".
3.  Provide global credentials to sign in and link the SCCM environment to Intune.
4.  Configure **Workloads**: Select which management authority (SCCM or Intune) controls different aspects like Compliance Policies, Device Configuration, Client Apps, etc.
5.  Configure **Scope**: Choose whether Co-Management applies to "All" devices or a "Pilot" collection.

**Key Fact:** Selecting "Pilot" allows for a staged deployment of Co-Management, only impacting devices within the specified pilot collection.

**Important Idea:** Enabling Co-Management, especially when targeting a pilot collection, triggers an attempt for automatic enrollment of the devices in that collection into Intune.

### 4. Device Enrollment to Intune (Triggered by SCCM Policy)

Once a device with the SCCM client is added to the configured pilot collection, a configuration policy is sent from SCCM to the device. This policy contains information about the workloads and, critically, instructs the device to enroll in Intune.

**Key Fact:** The SCCM configuration policy ID can be viewed in the SCCM console by accessing the XML definition of the Co-management settings. This policy is visible in the client's `CoManagementHandler.log`.

The device will then attempt to enroll in Intune based on this policy.

### 5. Understanding the Intune Enrollment Process and User Affinity

The demo explores different scenarios for how the Intune enrollment is triggered and the impact on user affinity (whether the device is primarily associated with a specific user).

**Scenario 1: Cloud User with EMS License:**
If the user logged into the device is an Azure AD user (either cloud-created or hybrid-synced) and has an EMS (Enterprise Mobility + Security) license assigned, the enrollment happens **instantly** and seamlessly. The device is then **attributed to the user** in the Intune portal. The `CoManagementHandler.log` will show the enrollment attempt using the user's Azure AD credentials and a successful MDM enrollment. The registry (`HKLM\SOFTWARE\Microsoft\Enrollments`) will show the enrollment was triggered by "Config Manager" and the UPN will likely be the logged-in user's UPN.

**Scenario 2: Cloud User without EMS License:**
If the user logged into the device is an Azure AD user but **lacks an EMS license**, the enrollment also happens **instantly** in the demo's observation. However, the device is **not attributed to any user** in the Intune portal; it's enrolled without user affinity. The `CoManagementHandler.log` might still show the enrollment attempt using the user's credentials, but the successful enrollment seems to occur using Azure AD device credentials. The registry might show "foo user" as the UPN.

**Scenario 3: Local Admin/Local User:**
If the user logged into the device is a **local administrator or a local user** (not synced to Azure AD and thus no EMS license), the enrollment **does not happen instantly**. The device will attempt to enroll, requiring an Azure AD user to log in. After multiple failed attempts and potentially a significant delay (hours to a day or more in the demo), the device will eventually get enrolled as a **user-less device** using Azure AD device credentials. The `CoManagementHandler.log` will show repeated attempts, messages indicating the need for an AAD user login, and finally a successful enrollment using device credentials. The registry will show "foo user" as the UPN.

**Key Quote:** "the enrollment will happen when a user logs into the device who has a EMS license assigned to it... there is one more way of getting the device enrolled and for that I have another machine ready we will take a look over that later on which talks about user less enrollment that is for if I mean if I don't log into the device with a user with which he who has EMS license what happens in that case" (This sets up the exploration of different enrollment scenarios).

**Key Quote:** "However what we saw was in the first case the user who had the image license was logged into the device the device got enrolled seamlessly and successfully... in the second case the user who was logged into the device did not have an EMS license but was a ad user in that case also the device got seamlessly enrolled however the device was not attributed to a user... however in both the cases the enrollment happens instantly as we have seen in both the cases however if the user who is logged in is a local admin or a local user... the device will eventually get enrolled after several of attempts... however the device will not be attributed to a user" (This summarizes the findings on enrollment behavior based on user type and license).

**Key Fact:** Devices enrolled via SCCM Co-Management are typically categorized as "Corporate" owned in Intune.

### 6. Mechanism of Automatic Enrollment (Scheduled Task)

The automatic enrollment triggered by SCCM is accomplished through a **scheduled task** created on the client device. This scheduled task is located under `Task Scheduler Library > Microsoft > Windows > EnterpriseMgmt`. The task executes `DeviceEnroller.exe`, which is responsible for the MDM enrollment process using an API.

**Key Quote:** "as we spoke during our previous nugget it is the schedule task that enrolls the device to in tune so at this point of time... it is the scheduler task that enrolls the device to in tune... this scheduled task is actually how the enrollment is triggered and the scheduler task is actually created either via the group policy or in this case why is ECM once the device is added to the SCCM collection" (Explains the role of the scheduled task).

### 7. Monitoring Co-Management Status and Workloads

The Co-Management status and current workload assignments can be checked on the client device through the **Configuration Manager applet** in the Control Panel. It will show "Co-management is enabled/disabled" and "Co-management capabilities" with a numerical value reflecting the managed workloads (e.g., 17 if Windows Update is managed by Intune, 1 if all workloads are with SCCM).

* The `CoManagementHandler.log` provides detailed logs of the Co-Management process, including policy reception, enrollment attempts, and workload changes.
* The registry key `HKLM\SOFTWARE\Microsoft\Enrollments` contains information about the Intune enrollment, including the enrollment type (Config Manager) and the UPN used (potentially "foo user" for SCCM-triggered enrollment).
* The `dsregcmd /status` command line utility can provide information about Azure AD join status and MDM URLs, confirming Intune enrollment.

### 8. Workload Management and Granular Control (SCCM 1906 Enhancement)

The demo briefly touches upon workload management and a new feature introduced in SCCM version 1906. In SCCM 1906, the Co-Management configuration interface allows assigning **different pilot collections to individual workloads**. This provides more granular control over which devices are transitioned to Intune management for specific areas (like Compliance Policies, Client Apps, etc.).

**Key Quote:** "this was actually done because of the feedback that Microsoft received so now with the version of 1906 and SCCM co-management gives the end users the ability of linking different pilot group to each workload thereby I mean giving more granular approach into how to manage the devices" (Highlights the improvement in workload assignment in 1906).

### 9. Recommendations and Benefits of Co-Management

The presenter strongly recommends setting up Co-Management even if full Intune adoption is not an immediate goal. A key recommendation is to offload the **Compliance Policies workload to Intune**.

**Benefit:** Managing compliance in Intune allows leveraging **Conditional Access**, enabling policies that grant or restrict access to cloud resources based on device compliance status.

**Key Quote:** "at this point of time the recommendation is that go to a CCM even though you are not of even if you don't want to make use of cumin I mean in tune still get your device's co-managed and then go to the a CCM console and offload the workload of compliance to in tune and once the compliance is offloaded to in tune... we can actually leverage something called conditional access... so just four clicks away guys just get this set up and make use of this wonderful feature" (Emphasizes the ease of setup and the benefits of using Intune compliance with Conditional Access).

## In Summary:

The demonstration provides a practical walkthrough of setting up Co-Management, detailing the necessary prerequisites and configuration steps in SCCM. It highlights the critical role of the pilot collection in triggering automatic Intune enrollment. The most significant finding relates to the nuances of the automatic enrollment process based on the logged-in user's type and EMS license status, demonstrating that enrollment can be instant and user-attributed for licensed cloud users, instant but user-less for unlicensed cloud users, and delayed and user-less for local users/admins. The briefing concludes with a strong recommendation to implement Co-Management, specifically offloading compliance to Intune, to leverage the power of cloud-based management and Conditional Access. The enhancement in SCCM 1906 for granular workload assignment to different pilot collections is also noted as an improvement for staged deployments.
