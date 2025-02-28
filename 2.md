## **Detailed Guide: Setting Up Microsoft Intune Portal (Level 100)**

### **1. Introduction to Microsoft Intune**
- Intune is a cloud-based service for **device and application management**.
- This guide focuses on the practical steps required to set up **Microsoft Intune** for administrators.

**Prerequisites:**
- Basic understanding of Intune and its purpose.
- Access to the **Azure Portal** and administrative privileges.

---

### **2. Setting Up Microsoft Intune**

#### **2.1 Creating an Intune Tenant**
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

---

#### **2.2 Accessing Intune Portal**

Two ways to access Intune:

1. **Azure Portal**: [portal.azure.com](https://portal.azure.com) → Microsoft Intune Blade.
2. **Direct URL**: [device.management.microsoft.com](https://device.management.microsoft.com).

**Customizing Portal Navigation:**
- Remove unnecessary services by unchecking starred items.
- Keep "Azure Active Directory" and "Microsoft Intune" as favorites for quick access.

---

### **3. Configuring MDM Authority**

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

> **Note:** Hybrid management will be phased out by Microsoft soon.

---

### **4. Creating Users and Assigning Licenses**

#### **4.1 Creating Users**

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

---

#### **4.2 Assigning Licenses**

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

> **Note:** Ensure each user has the correct license to enroll devices.

---

### **5. Customizing Company Portal Branding**

Enhance user experience during device enrollment with branded elements.

**Steps to Configure Company Branding:**
1. Go to **Azure Active Directory** → **Company Branding**.
2. Select **Configure** and upload:
   - Background Image (300 KB max).
   - Banner Logo (10 KB max).
   - Sign-in Text (e.g., Company Name).
3. Save the configuration.

You can also configure branding for multiple languages, allowing a more localized user experience during device enrollment.

---

### **6. Enabling Device Enrollment**

Device enrollment must be enabled for users to register their devices.

1. Go to **Device Enrollment**.
2. Enable enrollment for the required platforms:
   - **Windows**: Enabled by default.
   - **Android**: Enabled by default.
   - **iOS**: Requires **Apple Push Notification (APN)** setup.

#### **6.1 Setting Up APN for iOS Enrollment**

iOS devices require APN for communication between **Intune** and **Apple**.

**Steps to Set Up APN:**

1. Go to **Apple Enrollment** → **Apple MDM Push Certificate**.
2. Download the **CSR** file.
3. Visit [Apple Push Certificates Portal](https://identity.apple.com/pushcert).
4. Sign in with an **organization-specific** Apple ID (not personal).
5. Upload the CSR to generate a **.PEM** certificate.
6. Download the **.PEM** file.
7. Upload the **.PEM** certificate to Intune and complete the process.

> **Note:** APN certificate expires annually and must be renewed.

#### **6.2 Configuring MDM Scope**

1. Go to **Azure Active Directory** → **Mobility (MDM and MAM)**.
2. Select **Microsoft Intune**.
3. Set **MDM Scope** to **All** and **MAM Scope** to **None**.
4. Click **Save**.

---

### **7. Enrolling Devices**

#### **7.1 Windows Device Enrollment**

1. Go to **Settings** → **Accounts** → **Access Work or School**.
2. Click **Connect** and enter Intune credentials.
3. Follow the prompts to complete enrollment.

#### **7.2 Android Device Enrollment**

1. Download **Microsoft Intune Company Portal** from Google Play Store.
2. Sign in using user credentials.
3. Follow the setup process to register the device.

#### **7.3 iOS Device Enrollment**

1. Download **Intune Company Portal** from the App Store.
2. Sign in using user credentials.
3. Follow the steps to register the device.

> **Note:** Ensure APN setup is completed before enrolling iOS devices.

---

### **8. Managing Devices and Policies**

- Monitor enrolled devices and ensure compliance.
- Create security groups for policy assignment.
- Push configuration policies to manage device settings.

---

### **9. Summary**

This guide covers:

1. **Creating an Intune Tenant**.
2. **Setting MDM Authority**.
3. **Creating Users and Assigning Licenses**.
4. **Customizing Company Portal Branding**.
5. **Enabling Device Enrollment**.
6. **Enrolling Devices (Windows, Android, iOS)**.
7. **Managing Devices and Policies**.

With this setup, you can successfully manage devices across platforms using **Microsoft Intune**.

