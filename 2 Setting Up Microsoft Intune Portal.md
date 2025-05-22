# Briefing Document: Intune Portal Setup - Level 100

**Source**: Excerpts from "#IntuneNugget 2- Setting up the Intune Portal |Level 100"

**Date**: 2023-10-27

**Author**: Not Specified

**Purpose**: This document summarizes the key steps and concepts required for an Intune administrator to initially set up the Microsoft Intune portal and facilitate device and application management. It builds upon the foundational understanding of what Intune is and why it's needed. Additionally, a detailed guide is provided to outline the practical steps for setting up the Intune portal.

## Main Themes

- **Intune Tenant Setup**: The fundamental first step, analogous to setting up a domain on-premises.
- **Accessing the Intune Portal**: Understanding the different URLs and interfaces available.
- **Setting MDM Authority**: A crucial prerequisite determining how devices are managed (Intune standalone, hybrid with SCCM, or co-management).
- **User Management and Licensing**: Creating users and assigning appropriate Intune or related licenses for enrollment and management.
- **Company Portal Branding**: Customizing the end-user experience during device enrollment.
- **Enabling Device Enrollment**: Configuring Intune to allow various device types (iOS, Android, Windows) to enroll.
- **Enrollment Demonstration**: Showing the practical steps and results of enrolling different device types.
- **Initial Policy Deployment**: A brief introduction to pushing a sample policy to enrolled devices.

## Most Important Ideas/Facts

- **Intune Tenant**: The core environment for Intune management, similar to an Azure domain. It's a one-time setup. 
  - **Quote**: "Tenant in a way is like our domain... an organization has to set up an in tun tenant as well again it's just a one-time thing and inside the tenant is wherein we create users groups and devices."
- **Free Trial**: A 30-day free trial is available to set up and explore an Intune tenant.
- **Accessing the Portal**:
  - The primary URL for the Intune portal is [device.management.microsoft.com](https://device.management.microsoft.com).
  - The Intune portal can also be accessed via the Azure portal ([portal.azure.com](https://portal.azure.com)) by navigating to the Microsoft Intune blade. Both methods lead to the same "Ibiza console."
- **MDM Authority**: This setting is a critical prerequisite that must be set before end-users can utilize Intune MDM. 
  - **Quote**: "we won't be able to the end users won't be able to make use of in tune MDM unless and until we set the MDM authority."
  - There are two primary MDM Authority options:
    - **Intune Standalone**: Cloud-only management using the Azure portal. This is the recommended and future-proof approach. 
      - **Quote**: "Indian standalone means that we have got mobile devices... which we are going to manage using our MDM which in this case is in tune."
    - **Config Manager (Hybrid)**: Integration with on-premises SCCM for Windows 10 devices, managed primarily from the SCCM console. This setup is "actually going away... probably by the end of 2019."
  - The MDM Authority can be changed from Config Manager to Intune Standalone, but devices must connect to the service afterward for settings to update.
- **User Creation and Sync**:
  - Users can be created directly in Azure AD within the tenant.
  - Users can be synced from an on-premises Active Directory using the Azure AD Connect tool. 
    - **Quote**: "use as your ad connect to integrate your as your ad with your Windows Server ad or any other directory from your network."
- **Licensing**: Assigning a license (like Intune Direct, EMS E3/E5, or relevant bundles) to a user is a mandatory prerequisite for that user to enroll their device and utilize Intune's features. 
  - **Quote**: "assigning licenses is is the main thing... an end user needs to have an license assigned to him and only then he can enroll his device and make use of MDM or ma M which in tune is providing."
  - A user's usage location must be specified in Azure AD before a license can be assigned to them. 
    - **Quote**: "If I assign a license to this user without setting a location... license cannot be assigned to a user without a user's location specified."
  - Licenses (other than the default Intune trial) can be acquired via the Azure portal (licenses blade) or the Office 365 admin center ([portal.office.com](https://portal.office.com) - billing -> purchase services).
- **Company Portal Branding**: Allows administrators to customize the end-user enrollment experience by adding organizational logos, text, and images to the Company Portal app. Configured in Azure AD -> Company Branding.
  - **Quote**: "the use of company portal branding is that lets you say end user is enrolling his device - in tune... if he over here if we upload the name of the organization or the logo... then the end users experience would be like if he is enrolling his device and if he sees this logo... then it would give the end user an idea that he is enrolling his device right."
- **Supported Devices**: Intune supports managing iOS, Android, and Windows devices (desktops/laptops). Windows Server OS cannot be managed by Intune as of the time of the source.
- **Device Enrollment Prerequisites**:
  - **Android and Windows**: Device enrollment is enabled by default. The MDM scope should be set (typically to "All" for MDM and "None" for MAM initially).
  - **iOS**: Requires setting up the Apple MDM Push Certificate (APNs).
    - iOS devices never directly communicate with the MDM service (Intune). Communication happens via the Apple Push Notification Service (APNs).
    - This requires establishing a trust between the Intune service and APNs using certificates. 
      - **Quote**: "the communication between and iOS device and in tune service does not happen directly it happens via something called an APN... What needs to be done is we need to establish this channel and only then the device gets only then we facilitate device enrollment."
    - The process involves downloading a CSR from Intune, uploading it to the Apple Push Certificates Portal (using a dedicated organizational Apple ID, not a personal one), downloading a .PEM certificate from Apple, and uploading it back to Intune. This trust "needs to be established by the Indian admin it's a one-time thing." The certificate is valid for 365 days and must be renewed.
- **Enrollment Process**:
  - **iOS**: Users download the Company Portal app and follow the enrollment steps.
  - **Windows**: Enrollment is done via Settings -> Accounts -> Access work or school -> Connect. Company Portal app is not required for Windows enrollment.
- **Post-Enrollment Actions**: Once devices are enrolled, administrators can view them in the Intune portal, see device information, run syncs, retire/wipe devices, and apply policies.
- **Policies**: Device compliance and configuration policies can be created and assigned to users or groups to manage device settings.

## Key Takeaway Quotes
- "this nugget is for you I'm going to show you guys everything that needs to be done by an admin on the Intune portal so as to facilitate the device and the application management."
- "assigning licenses is is the main thing... an end user needs to have an license assigned to him and only then he can enroll his device."
- "unles and until we set the MDM authority... this is one of the prerequisites so once the tenant is made the first thing that needs to be done is that the Indian admin needs to set the MDM authority."
- "this hybrid setup is actually going away... probably by the end of 2019 this hybrid setup would no longer be there and the management of mobile devices would take care from the Indian standalone portal."
- "in case of iOS devices the any MDM not just in tune cannot directly talk with the iOS devices to talk to it via the APN service."

## Detailed Guide: Setting Up Microsoft Intune Portal (Level 100)

### 1. Introduction to Microsoft Intune
- Intune is a cloud-based service for **device and application management**.
- This guide focuses on the practical steps required to set up **Microsoft Intune** for administrators.

**Prerequisites:**
- Basic understanding of Intune and its purpose.
- Access to the **Azure Portal** and administrative privileges.

### 2. Setting Up Microsoft Intune

#### 2.1 Creating an Intune Tenant
- **Tenant**: An Azure-based domain that represents your organization's identity.

**Steps to Create a Tenant:**
1. Go to [Microsoft Intune Trial Page](https://www.microsoft.com/en-us/microsoft-365/try).
2. Click on **Start Free Trial** (valid for 30 days).
3. Provide required information:
   - Business Name
   - Administrator Details (First Name, Last Name, Email)
   - Organization Size
4. Create the first **admin user** for the tenant.
5. Confirm and complete the setup process.

The **Intune Tenant** acts as your organization's cloud-based domain. After sign-up, access **portal.azure.com** or **device.management.microsoft.com** to manage Intune services.

#### 2.2 Accessing Intune Portal
Two ways to access Intune:
1. **Azure Portal**: [portal.azure.com](https://portal.azure.com) → Microsoft Intune Blade.
2. **Direct URL**: [device.management.microsoft.com](https://device.management.microsoft.com).

**Customizing Portal Navigation:**
- Remove unnecessary services by unchecking starred items.
- Keep "Azure Active Directory" and "Microsoft Intune" as favorites for quick access.

### 3. Configuring MDM Authority
**MDM Authority** determines how devices are managed. There are three options:
1. **Intune Standalone** (Recommended)
   - Devices are managed entirely via Intune.
2. **Hybrid (SCCM Integration)** (Phased out by Microsoft)
   - Integration with System Center Configuration Manager (SCCM).
3. **Co-Management**
   - Manage Windows devices via both **Intune** and **SCCM** simultaneously.

**Setting the MDM Authority:**
1. Navigate to **Device Enrollment** within Intune.
2. Select **MDM Authority**.
3. Choose **Intune MDM** for standalone management.
4. Confirm your selection.

> **Note**: Hybrid management will be phased out by Microsoft soon.

### 4. Creating Users and Assigning Licenses

#### 4.1 Creating Users
1. In the **Azure Portal**, go to **Azure Active Directory**.
2. Select **Users** → **New User**.
3. Provide the following:
   - Display Name
   - User Name (e.g., user@yourdomain.onmicrosoft.com)
4. Set password and directory roles if needed.
5. Click **Create**.

**Alternative Method:**
- Use **Azure AD Connect** to sync on-premises AD users to the cloud.
- Download **Azure AD Connect** from the **Azure Active Directory** portal and install it on a Windows Server.

#### 4.2 Assigning Licenses
1. Go to **Azure Active Directory** → **Users**.
2. Select a user.
3. Ensure **Usage Location** is set (e.g., United States, India).
4. Click **Licenses** → **Assign Licenses**.
5. Choose from available subscriptions:
   - **Intune** (device management).
   - **EMS Suite** (comprehensive management).
   - **Azure AD Premium** (for advanced Azure capabilities).

**Purchasing Additional Licenses:**
1. Go to **portal.office.com** → **Billing** → **Purchase Services**.
2. Choose and buy the required licenses (e.g., Office 365, Power BI).

> **Note**: Ensure each user has the correct license to enroll devices.

### 5. Customizing Company Portal Branding
Enhance user experience during device enrollment with branded elements.

**Steps to Configure Company Branding:**
1. Go to **Azure Active Directory** → **Company Branding**.
2. Select **Configure** and upload:
   - Background Image (300 KB max).
   - Banner Logo (10 KB max).
   - Sign-in Text (e.g., Company Name).
3. Save the configuration.

You can also configure branding for multiple languages, allowing a more localized user experience during device enrollment.

### 6. Enabling Device Enrollment
Device enrollment must be enabled for users to register their devices.

1. Go to **Device Enrollment**.
2. Enable enrollment for the required platforms:
   - **Windows**: Enabled by default.
   - **Android**: Enabled by default.
   - **iOS**: Requires **Apple Push Notification (APN)** setup.

#### 6.1 Setting Up APN for iOS Enrollment
iOS devices require APN for communication between **Intune** and **Apple**.

**Steps to Set Up APN:**
1. Go to **Apple Enrollment** → **Apple MDM Push Certificate**.
2. Download the **CSR** file.
3. Visit [Apple Push Certificates Portal](https://identity.apple.com/pushcert).
4. Sign in with an **organization-specific** Apple ID (not personal).
5. Upload the CSR to generate a **.PEM** certificate.
6. Download the **.PEM** file.
7. Upload the **.PEM** certificate to Intune and complete the process.

> **Note**: APN certificate expires annually and must be renewed.

#### 6.2 Configuring MDM Scope
1. Go to **Azure Active Directory** → **Mobility (MDM and MAM)**.
2. Select **Microsoft Intune**.
3. Set **MDM Scope** to **All** and **MAM Scope** to **None**.
4. Click **Save**.

### 7. Enrolling Devices

#### 7.1 Windows Device Enrollment
1. Go to **Settings** → **Accounts** → **Access Work or School**.
2. Click **Connect** and enter Intune credentials.
3. Follow the prompts to complete enrollment.

#### 7.2 Android Device Enrollment
1. Download **Microsoft Intune Company Portal** from Google Play Store.
2. Sign in using user credentials.
3. Follow the setup process to register the device.

#### 7.3 iOS Device Enrollment
1. Download **Intune Company Portal** from the App Store.
2. Sign in using user credentials.
3. Follow the steps to register the device.

> **Note**: Ensure APN setup is completed before enrolling iOS devices.

### 8. Managing Devices and Policies
- Monitor enrolled devices and ensure compliance.
- Create security groups for policy assignment.
- Push configuration policies to manage device settings.

### 9. Summary
This guide covers:
1. **Creating an Intune Tenant**.
2. **Setting MDM Authority**.
3. **Creating Users and Assigning Licenses**.
4. **Customizing Company Portal Branding**.
5. **Enabling Device Enrollment**.
6. **Enrolling Devices (Windows, Android, iOS)**.
7. **Managing Devices and Policies**.

With this setup, you can successfully manage devices across platforms using **Microsoft Intune**.


