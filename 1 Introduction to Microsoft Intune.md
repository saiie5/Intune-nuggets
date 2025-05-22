# Briefing Document: Introduction to Microsoft Intune - Functionality, Use Cases, and Core Concepts

**Source**: Excerpts from "#IntuneNugget 1- Introduction to Intune |Level 100"

**Date**:22/5/2025

**Author**: Saikumar Mavuduru

**Subject**: Introduction to Microsoft Intune - Functionality, Use Cases, and Core Concepts

## I. Executive Summary

This briefing document summarizes the core concepts and functionality of Microsoft Intune, a cloud-based service designed for modern device and application management. It highlights the shift from traditional on-premises management methods (like Group Policy and SCCM) to cloud-based solutions driven by the increasing use of personal and mobile devices accessing corporate data over the internet. Intune offers two key pillars: Mobile Device Management (MDM) for managing entire devices (typically company-owned) and Mobile Application Management (MAM) for managing specific applications (often on personal/BYOD devices), allowing organizations to secure corporate data regardless of the device's ownership or location.

## II. Main Themes and Important Ideas

### The Need for Device Management
The source emphasizes the fundamental need for device management in any organization to protect and secure resources and data. With users accessing corporate data from various devices (both personal and corporate), administrators must ensure the security of both the device and the data.
- **Quote**: "a key task of any administrator is to protect and secure an organization's resource and data. This is actually device management."
- **Quote**: "device management enables organizations to protect and secure their resources and their they data."
- **Quote**: "The use of device management is making sure that sensitive work-related data is secured in personal devices as well as in corporate devices."

### Evolution from Traditional to Modern Management
The briefing highlights the transition from traditional, on-premises device management solutions like Group Policy and SCCM, which require devices to be joined to a domain, to modern cloud-based solutions like Intune. This shift is crucial for managing devices that are not domain-joined, such as mobile phones and personal laptops accessing resources over the internet.
- **Quote**: "Traditionally we have been doing this by using something called a group policy... That was 10 years back. Now comes the cloud environment wherein the users are using their personal devices which are no longer joined to the domain... Therefore we need a modern management to manage the devices and that is wherein in turn comes into the picture."
- **Quote**: "As we cannot join these phones or these mobile devices to the domain and these devices only have access to the Internet we need a solution Varon we should be able to protect the data and protect the devices which are present on the internet or over the Internet."

### Microsoft Intune as a Cloud-Based Solution
Intune is presented as Microsoft's cloud-based service for modern device and application management. It's a SaaS (Software as a Service) application residing on the Azure cloud, eliminating the need for on-premises infrastructure like servers and databases traditionally required for SCCM or Group Policy.
- **Quote**: "one of the ways of accessing cloud management of cloud device management that is device management over the cloud is by using Microsoft's product of managing devices over the cloud which is called in tune."
- **Quote**: "in tune is a cloud-based service it's a SAS application to be exact it is an application which is sitting on the Azure cloud."
- **Quote**: "the best thing about in tune is no on Prem infra is needed everything is there on Azure."

### Key Benefits of Cloud-Based Management (Intune)
- **Secure Access over the Internet**: Facilitates secure access to corporate resources from devices over the internet without needing VPNs.
  - **Quote**: "the main need of having a management product over the cloud is to facilitate access to corporate resources over the Internet and not just access over the Internet to facilitate secure access to inter secure access over the Internet."
  - **Quote**: "most importantly no longer VPN is needed."
- **Reduced On-Premises Dependency**: Eliminates the need for organizations to invest in and maintain extensive on-premises infrastructure.
  - **Quote**: "we want to manage the devices without any dependency on the on-prem infra."
  - **Quote**: "the organization no longer needs to invest on the on-prem infrastructure no longer needs to build servers no longer needs to install any heavy-duty MDM softwares."
- **Simplified Administration**: Centralized management through a web console, reducing the need for physical interaction with devices.
  - **Quote**: "the administration is centralized which means that the IT admin will no longer have to walk to that device in order to control that device."
  - **Quote**: "Everything is centrally managed."
- **Seamless End-User Experience**: Automation and zero-touch provisioning for deploying applications and configurations, minimizing user effort.
  - **Quote**: "making the users feel at ease acts feel at ease."
  - **Quote**: "minimize the end-users interaction or let's just say that to minimize the end-users efforts."
  - **Quote**: "making things zero-touch are automated."

### The Two Pillars of Intune: MDM and MAM
The source clearly defines and differentiates between Mobile Device Management (MDM) and Mobile Application Management (MAM), the two fundamental functionalities of Intune.
- **MDM (Mobile Device Management)**: Manages the entire device, providing control over device attributes and functionalities. Typically used for company-owned devices.
  - **Quote**: "M D M stands for mobile device management."
  - **Quote**: "in this case we are managing the device so the solution that we are going to use our leverage out of in tune is MDM or mobile device management."
  - **Quote**: "wherein we are managing the device v as an admin want to make sure that okay the end user should not be able to use the camera... or they should not be able to install a specific application in their device."
- **MAM (Mobile Application Management)**: Manages specific applications on a device, controlling how corporate data is used within those applications. Typically used for personal/BYOD devices where full device management is not desired or feasible.
  - **Quote**: "ma M stands for mobile application management."
  - **Quote**: "using mobiles application management the IT admin manages a application in the device and not the entire device on the whole."
  - **Quote**: "what this means is the IT admin can't govern supervise and put in polished policies which would affect how exactly the end users experience is going to be when he goes into a browser or when he opens up an Outlook and tries to access and email which is his work-related."

### Choosing Between MDM and MAM
The decision between MDM and MAM (or a combination) depends on the type of device being managed (company-owned vs. BYOD) and the desired level of control.
- **Quote**: "whenever we are of working with in tune it either comes into the first category that is MDM or it comes into the second category of M a.m."
- **Quote**: "What we need to keep in mind is what is the solution we are looking for if we want to manage the devices then goes without saying we have to go with the MDM approach however if we want to manage the applications only and not the device we have to go with the ma M approach."
- **Quote**: "company-owned devices versus BYOD so almost nine out of ten if not ten out of ten cases if the device is provided by the organization the device is going to be enrolled because obviously the company's owning that device... if the device is BYOD... if that is the scenario then we have to go ahead with the mam functionality."

### Intune Architecture and Integration
Intune operates as a web-based console within the Azure cloud and is tightly integrated with Azure Active Directory (Azure AD) for authentication and authorization. Policies are created in the console, translated into Graph API calls, and pushed to devices connected to the internet.
- **Quote**: "the management console as I said is a web console... All we need to do is open up our Internet Explorer or Google Chrome or any browser and then go to the Microsoft Intune website and from there we can manage all the devices."
- **Quote**: "Microsoft Intune is very closely interlinked and related with the Azure Active Directory."
- **Quote**: "the authentication per se would not be done by Microsoft Intune the authentication would always be done by the azure ad stack of it."

### Device and Application Lifecycle in Intune
The source outlines the typical lifecycle, including the admin creating policies in the web console, the end user enrolling their device using the Company Portal app (for MDM and MAM) or built-in OS functionality (for Windows 10 MDM), policies and applications being pushed to the device, the device reporting back to the Intune service, and the ability to retire or wipe corporate data from the device/application.
- **Quote**: "as an admin di T will make policies in the portal... the end-user enrolls their device to in tune... now that device can be managed via in tune."
- **Quote**: "the admin can push configuration policies to the device... the admin can also make a profile for an application he can push an application for the device."
- **Quote**: "the device report reports back to the Intune service and the in tunes inventory or the data warehouse house is updated."
- **Quote**: "if the user goes on and unenrolled the device... or if the IT admin from the portal unenrolled the device who initiates a vibe then all the data from the device is gone."

### Key Considerations for Intune Deployment
Before implementing Intune, organizations should consider various factors, including identity management (cloud vs. AD FS), MFA, email environment (Exchange Online vs. On-Prem), existing MDM solutions (like SCCM), device types and platforms, desired capabilities (MDM vs. MAM), enrollment types (manual vs. automatic), enrollment restrictions, device ownership, and certificate solutions.

### Intune Requirements/Prerequisites
Key requirements for using Intune include:
- **Licenses**: Users or devices require an Intune license or an EMS suite license.
- **Device Scope**: Configuring device scope in the Intune portal to define whether MDM, MAM, or both are used.
- **Office 365 Subscription (for App Protection Policies)**: Necessary for deploying Office applications and utilizing app protection policies.
- **APNs Certificate (for iOS/Mac Management)**: Required to enable management of Apple devices due to Apple's push notification service architecture.
- **Azure AD Connect (Optional)**: A tool to sync on-premises identities to Azure AD for cloud-based authentication.
- **DEP/Autopilot (for Bulk Enrollment)**: Programs for simplifying bulk enrollment of iOS and Windows devices, respectively.

## III. Important Facts and Quotes
- Intune is a cloud-based SaaS application from Microsoft for device management.
- Intune resides on the Azure cloud.
- Intune is closely integrated with Azure Active Directory for authentication.
- Intune offers two main pillars: MDM (Mobile Device Management) and MAM (Mobile Application Management).
- MDM is typically used for company-owned devices and manages the entire device.
- MAM is typically used for BYOD devices and manages specific applications.
- **Quote**: "M D M stands for mobile device management... ma M stands for mobile application management."
- Intune eliminates the need for on-premises MDM infrastructure.
- **Quote**: "the best thing about in tune is no on Prem infra is needed everything is there on Azure."
- Intune uses a web-based console for administration.
- Policies are translated into Graph API calls in the background.
- The Company Portal application is used for enrolling devices (except Windows 10 MDM).
- Windows 10 MDM enrollment is built into the operating system.
- APNs certificates are required for managing iOS and Mac devices.
- Azure AD Connect is optional for syncing on-premises identities.
- DEP and Autopilot are used for bulk enrollment of iOS and Windows devices, respectively.
- Licenses (Intune or EMS) are required for users/devices to be managed by Intune.

## IV. Use Cases Highlighted
- Protecting on-premises email and data accessed via mobile devices.
- Securing access to Office 365 from various devices.
- Managing corporate-provided devices (laptops, phones).
- Managing applications on personal/BYOD devices while respecting user privacy.
- Preventing data leakage from corporate applications.
- Configuring device settings and features (e.g., disabling camera, enforcing password policies).
- Deploying applications to devices.
- Setting up compliance policies for devices.
- Managing OS updates and patches.
- Integrating with security solutions like Windows Defender and Conditional Access.

## V. Unsupported Platforms (as of the source)
- Linux and UNIX devices.
- Windows Server operating systems.

**Note**: NotebookLM can be inaccurate; please double check its responses.
