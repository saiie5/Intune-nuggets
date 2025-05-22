# Briefing Document: Win32 App Deployment via Intune - The Real Game Changer

**Source**: Excerpts from "#IntuneNugget 22 - Win32 app deployment via Intune- The Real GameChanger!"

**Date**: 22/5/2025

**Author**: Saikumar Mavuduru

**Subject**: Review of Win32 App Deployment capabilities in Microsoft Intune

## Executive Summary

This briefing document summarizes key concepts and functionalities related to Win32 application deployment using Microsoft Intune, highlighted as a "real game changer." The feature addresses previous limitations in Intune's application deployment capabilities, enabling the deployment of .exe and packaged applications, advanced detection logic, and dependency management. The process involves packaging applications using the Intune Win32 App Packaging Tool into a .intunewin format, configuring deployment parameters in the Intune portal, and leveraging the Intune Management Extension (IME) on target devices for installation and monitoring. The source emphasizes the importance of silent installation support for applications, correct detection logic, and understanding the deployment process flow.

## Key Themes and Important Ideas

### Addressing Previous Intune Limitations
- Prior to the introduction of Win32 app deployment (around 2018), Intune had limitations in deploying .exe files and complex packaged applications.
- While .msi files could be deployed as Line-of-Business (LOB) applications, .exe and packaged applications were not natively supported.
- This was a "road blocker" or "blocker" for organizations moving towards a co-managed scenario with SCCM and Intune, as many LOB applications are in .exe format.
- Win32 app deployment filled this gap, enabling the deployment of .exe, packages, and multiple .msi files via Intune.
- This also allows for deploying specific drivers, hardware patches (KBs), and scripts, which were not natively supported through Windows Update rings in Intune.

### Introduction of Key Concepts
- **Win32 App**: Refers to applications (typically .exe or packaged) that can be deployed via Intune using this specific method.
- **Detection Logic**: A critical feature that allows administrators to define criteria for Intune to determine if an application is already installed on a device. This prevents unnecessary re-installations.
  - Detection can be based on file existence, folder existence, registry keys, or custom PowerShell scripts.
  - The logic is evaluated before downloading and installing the app and again after the installation to confirm success.
  - Incorrect detection logic can lead to installation failures or apps being incorrectly reported as installed when they are not.
  - **Quote**: "what detection logic brings into the table is – first we detect whether a specific application is present in the device or not if after i mean by using any specific kind of detection logic... only if that detection is not met meaning I have set a detection for chrome only if chrome is not detected is when chrome gets downloaded into the device Chrome gets installed in the device and once chrome is installed the same detection logic is evaluated again to see whether after the app deployment and whether after the app installation whether now the detection logic detects the pre..."
- **Dependencies**: The ability to define that one application must be installed before another.
  - If an application (App A) is dependent on another (App B), Intune will automatically install App B first if it is not detected on the device, even if App B was not directly assigned to the user or device.
  - **Quote**: "with win32 app approach this is a major add-on and now we have the ability of setting up dependencies what that means is I can set up the deployment of the win32 application in a way that I can say that you know what app a is dependent on app b what that means is even if we have only deployed app a even if we have not even targeted and deployed the app b but when we are deploying app a we have made this dependency of app a on app b then automatically app b will be installed on the device first once app b is installed and detected only then app a installation will start on the device"

### Prerequisites for Win32 App Deployment
- The target device must be Azure AD Joined or Hybrid Azure AD Joined and enrolled in Intune.
- Devices that are only Azure AD Registered and enrolled, or only enrolled in Intune (Enroll only in device management), cannot receive Win32 app deployments.
- The operating system version should be Windows 10 1607 or later.
- The application file/package must be less than 8 GB in size.

### Packaging Applications using Intune Win32 App Packaging Tool
- A free utility available on GitHub.
- Converts standard installation files (.exe, .msi, etc., and potentially KBs or other formats) into a .intunewin file.
- The tool compresses the source folder and encrypts the content using SHA-256 hash.
- The resulting .intunewin file contains the compressed setup files and a detection.xml file within a metadata subfolder. The detection.xml contains information needed to decrypt the package and other configurations.
- **Quote**: "by using the intune vin app utility we can convert an msi not just an msi we can convert other formats also to a dot intune win file we convert that and then we encrypt that file also so the file that is deployed via intune to the device to the ime is dot intune win file and that file is encrypted using a sha-256 algorithm"
- It is crucial to place only the necessary installation files in the source folder to avoid excessively large .intunewin files. Avoid using root directories like C:\ as the source.

### Deployment Process and Configuration in Intune
- Applications are deployed through the Client Apps section in the Intune portal.
- The .intunewin file is uploaded.
- Key configurations include:
  - **Install Command**: The command-line arguments needed to install the application silently. This is a critical step and requires testing the command manually on a device first.
  - **Uninstall Command**: The command-line arguments for uninstalling the application.
  - **Device Restart Behavior**: Configures if a device restart is needed after installation (e.g., No specific action, App install may force a device restart, Intune will force a device restart).
  - **Return Codes**: Defines how different exit codes from the installer are interpreted (e.g., Success, Failure, Hard Reboot, Soft Reboot).
  - **Requirements**: Defines criteria that a device must meet before the application is downloaded and installed (e.g., Minimum OS version, Architecture, Disk Space, RAM, Logical Processors, CPU speed). Additional requirement rules can be added based on file/folder existence, registry keys, or scripts.
  - **Detection Rules**: Configures how Intune detects if the application is installed (as discussed above).
  - **Install Context**: Determines whether the application is installed under the System context (for all users) or the User context (for the currently logged-in user). Win32 app installers run with administrator privileges by default, even in user context if the user has admin rights.

### The Intune Management Extension (IME)
- The "remote hand" on the Windows 10 device responsible for handling Win32 app deployments and PowerShell scripts.
- IME gets installed automatically when the first Win32 app or PowerShell script is assigned to a device.
- IME installation is initiated by the creation of a registry key (HKLM\SOFTWARE\Microsoft\EnterpriseDesktopAppManagement) which contains a download URL for the IME MSI.
- The Background Intelligent Transfer Service (BITS) downloads the IME MSI from a Content Delivery Network (CDN) location based on the URL in the registry.
- Once installed, IME manages the download, decryption, staging, installation, and monitoring of Win32 apps.
- IME writes detailed logs (located at C:\ProgramData\Microsoft\IntuneManagementExtension\Logs) that are crucial for troubleshooting.
- **Quote**: "ime is our remote hand right so everything that ime is doing is locked in a log file called into the management extension log"

### Win32 App Deployment Process Flow (Under the Hood)
- **Policy Retrieval**: IME periodically checks for policies from Intune, including app deployments.
- **Dependency Check**: If dependencies are configured, IME checks for the presence of dependent applications using their detection logic. If not detected, the dependent apps are processed first.
- **Detection Logic Evaluation (Pre-Install)**: IME evaluates the detection rules for the target application. If the application is detected as already installed, the process stops.
- **Requirement Rules Evaluation**: If the application is not detected, IME checks if the device meets the configured requirement rules. If not, the process stops.
- **Download and Staging**: If requirements are met, IME downloads the encrypted .intunewin package from the CDN to an incoming folder and then moves it to a staging folder.
- **Decryption**: IME decrypts the package using information from the detection.xml file.
- **Installation**: IME runs the configured install command with the appropriate context (System or User).
- **Monitoring and Exit Code Check**: IME monitors the installation process and checks the exit code against the defined return codes to determine success or failure.
- **Device Restart Behavior**: If configured, IME handles device restarts after installation.
- **Detection Logic Evaluation (Post-Install)**: IME evaluates the detection rules again to confirm that the application is now successfully installed.
- **Status Reporting**: IME reports the app installation status back to the Intune service.
- **Toast Notifications**: IME displays toast notifications to the end user about the download and installation status.
- **Quote**: "ime is checking if the discovered apps is already installed in the device as per the detection logic and if it is if it is uh already installed well and good let's assume it's not installed in that case uh detection logic fails as in the detection logic is not met meaning the application is not installed therefore the next process will come wherein it will check the applicability and the requirement rules that has been said by the admin"

### Troubleshooting and Common Errors
- Logs from the Intune Management Extension are crucial for troubleshooting.
- Common issues include:
  - Application installer not supporting silent installation. This is a fundamental requirement.
  - Incorrect installation or uninstallation commands.
  - Incorrect or flawed detection logic.
  - Dependency issues (e.g., dependent app fails to install).
  - Requirement rules not being met on the target device.
  - Network connectivity issues preventing download.
  - Antivirus software interfering with the installation.
  - Issues with unzipping the downloaded package (rare).
- The source emphasizes the importance of testing the silent installation command and verifying the detection logic manually on a test device before deploying via Intune.
- Intune provides the ability to collect IME logs remotely from the portal for a specific device when a Win32 app deployment fails.

### Advanced Use Cases
- **Deploying Specific KBs (Patches)**: Win32 app deployment allows for deploying individual .msu files by packaging them and defining appropriate installation and detection methods (often using PowerShell scripts to check for the KB's presence). This provides more granular control over patching than Windows Update rings.
- **Using PowerShell for Detection Logic**: Provides flexibility in defining complex detection criteria beyond simple file, folder, or registry checks. PowerShell scripts can query system information or other custom conditions.
- **Recovering .intunewin files**: A third-party tool/script (referenced from Oliver Kieselbach) allows administrators to decode an encrypted .intunewin file that has been deployed to a device, recovering the original installer file. This is useful if the original package is lost.

## Important Facts/Data Points
- Win32 app deployment capabilities were introduced around 2018.
- The maximum .intunewin package size is 8 GB.
- Supported OS version: Windows 10 1607 or later.
- Intune Management Extension logs location: C:\ProgramData\Microsoft\IntuneManagementExtension\Logs.
- IME download URL is stored in the registry at HKLM\SOFTWARE\Microsoft\EnterpriseDesktopAppManagement\MSI.
- IME downloads the MSI using the BITS service from a CDN endpoint (e.g., https://fef.msuc01.manage.microsoft.com/).
- Exit code 0 is typically considered a successful installation. Other return codes can be configured for different outcomes (e.g., 1707, 1717 for success, 1774, etc.).

## Conclusion
Win32 app deployment is presented as a powerful and essential feature in Intune that significantly expands its application management capabilities. By enabling the deployment of various application formats, incorporating advanced detection and dependency logic, and providing detailed logging through the Intune Management Extension, it allows administrators to manage a wider range of applications more effectively and with greater control, bridging gaps that existediuos versions of Intune's application deployment features. Understanding the underlying process and the importance of proper packaging, silent installation, and detection logic is crucial for successful Win32 app deployments.
