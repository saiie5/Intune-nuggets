Hello everyone,

Welcome to today's session, where we will discuss **Types of Enrollment with Intune**. This is a Level 100 session focusing on different enrollment methods for various operating systems, including Windows, iOS, macOS, and Android.

## Overview of Enrollment Methods
Microsoft Intune supports multiple enrollment methods depending on device ownership (corporate or personal) and management requirements.

---

## **iOS/macOS Enrollment Methods**
### 1. Manual Enrollment (BYOD - Bring Your Own Device)
**Steps:**
1. Install the **Company Portal App** from the App Store.
2. Open the app and sign in using **work credentials**.
3. Follow on-screen instructions to allow **device management permissions**.
4. The device will register with Intune and apply necessary policies.

**Prerequisites:**
- **Intune License** assigned to the user.
- **APNs Certificate** set up in Intune to enable communication with Apple.

---

### 2. Apple Device Enrollment Program (DEP) - Corporate-Owned Devices
**Steps:**
1. Ensure the device is listed in **Apple Business Manager (ABM)**.
2. In Intune, navigate to **Devices > iOS/iPadOS enrollment > Enrollment program tokens**.
3. Upload the **DEP token** from ABM.
4. Create an **Enrollment Profile** to define settings like user affinity and supervision.
5. Assign the profile to DEP-managed devices.
6. The device automatically enrolls into Intune upon activation.

**Prerequisites:**
- **Apple Business Manager** account.
- **DEP token** imported into Intune.
- **Supervision mode enabled** for corporate control.

---

### 3. Apple Configurator Enrollment
**Steps:**
1. Install **Apple Configurator** on a **Mac device**.
2. Connect the iOS device to the Mac via **USB**.
3. Create an **Enrollment Profile** in Intune.
4. Apply the profile to the device via Apple Configurator.
5. The device restarts and is enrolled into Intune.

**Prerequisites:**
- A **Mac device** with **Apple Configurator** installed.
- **USB cable** for physical connection.
- **APNs Certificate** for device communication.

---

## **Windows Enrollment Methods**
### 1. Manual Enrollment
**Steps:**
1. Go to **Settings > Accounts > Access work or school**.
2. Select **Enroll only in device management**.
3. Enter **work credentials** and sign in.
4. Follow on-screen instructions to complete enrollment.

**Prerequisites:**
- **Windows 10 version 1607 or later**.
- **Intune License** assigned to the user.

---

### 2. Auto Enrollment
**Steps:**
1. In Azure AD, navigate to **Mobility (MDM & MAM) > Microsoft Intune**.
2. Configure **MDM user scope** (All or Selected Users).
3. On the Windows device, go to **Settings > Accounts > Access work or school**.
4. Click **Connect**, enter work credentials, and complete Azure AD Join.
5. The device is **automatically enrolled into Intune**.

**Prerequisites:**
- **Windows 10 version 1607 or later**.
- **Azure AD Join enabled**.
- **MDM user scope configured**.

---

### 3. Group Policy Enrollment (Hybrid Azure AD Join)
**Steps:**
1. Ensure the device is **joined to on-prem Active Directory**.
2. Install and configure **Azure AD Connect** for Hybrid Azure AD Join.
3. Create a **Group Policy Object (GPO)** for auto-enrollment:
   - Open **Group Policy Management**.
   - Navigate to **Computer Configuration > Administrative Templates > Windows Components > MDM**.
   - Enable **Enable automatic MDM enrollment using default Azure AD credentials**.
4. Link the GPO to an **Organizational Unit (OU)** with target devices.
5. Devices will enroll into Intune on their next sign-in.

**Prerequisites:**
- **Windows 10 version 1709 or later**.
- **Hybrid Azure AD Join configured**.
- **MDM user scope set** in Azure AD.

---

### 4. Windows Autopilot (Corporate-Owned)
**Steps:**
1. Obtain **hardware hash** from the device.
2. Navigate to **Devices > Windows > Windows Enrollment > Devices** in Intune.
3. Upload the **hardware hash**.
4. Create an **Autopilot Profile** to define user experience.
5. Assign the profile to the **Autopilot devices**.
6. The device will automatically join Azure AD and enroll into Intune upon first boot.

**Prerequisites:**
- **Windows 10 version 1703 or later**.
- **Azure AD Premium license**.
- **Hardware hash registered in Intune**.

---

### 5. Co-Management with SCCM
**Steps:**
1. Ensure **SCCM version 1710 or later** is installed.
2. Enable **Hybrid Azure AD Join**.
3. In **SCCM**, configure co-management settings and select workloads to move to Intune.
4. Devices managed by **SCCM** will register with Intune.

**Prerequisites:**
- **Windows 10 version 1709 or later**.
- **SCCM configured for co-management**.
- **Intune License** assigned.

---

## **Android Enrollment Methods**
### 1. Android Enterprise (Work Profile - BYOD)
**Steps:**
1. Install **Company Portal App** from Google Play Store.
2. Open the app and sign in using work credentials.
3. The device will register and apply policies in a **Work Profile** container.

**Prerequisites:**
- **Android version 5.1 or later**.
- **Intune License** assigned.

---

### 2. Android Enterprise (Fully Managed - Corporate-Owned)
**Steps:**
1. Factory reset the Android device.
2. On the setup screen, scan the **QR Code** generated in Intune.
3. The device will enroll as a fully managed device.

**Prerequisites:**
- **Android version 6.0 or later**.
- **Device Enrollment Restrictions configured** in Intune.

---

## **Summary**
Microsoft Intune offers multiple enrollment methods to accommodate **personal (BYOD)** and **corporate-owned** devices. Each enrollment type has different **prerequisites and configurations**, making it important to choose the appropriate method based on organizational needs.

Let me know if you need a detailed walkthrough on any specific enrollment method!

