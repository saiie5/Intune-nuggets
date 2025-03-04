# Co-Management Setup with Microsoft Intune and SCCM

## Introduction to Co-Management
Co-management is a feature that allows organizations to manage Windows 10 devices using both Microsoft Intune and System Center Configuration Manager (SCCM) simultaneously. It provides a bridge between traditional on-premises device management (via SCCM) and modern cloud-based management (via Intune).

**Key Benefits of Co-Management:**
1. **Phased Transition:** Organizations can gradually move workloads from SCCM to Intune.
2. **Hybrid Management:** Devices are managed by both SCCM and Intune, allowing a mix of on-premise and cloud-based policies.
3. **Improved Compliance:** Leverages the compliance capabilities of Intune to enforce conditional access policies.
4. **Enhanced Security:** Ensure devices are compliant before granting access to corporate resources.

---

## Prerequisites for Enabling Co-Management
To set up co-management, the following prerequisites must be met:

1. **SCCM Version:** 1710 or higher.
2. **Windows 10 Version:** 1709 or later.
3. **Hybrid Azure AD Joined Devices:** Devices must be joined to both on-premises Active Directory and Azure Active Directory (Azure AD).
4. **Permissions:** Administrative access to both SCCM and Intune.
5. **Azure AD Connect:** To sync on-premises Active Directory with Azure AD.
6. **Auto-Enrollment Enabled:** This allows SCCM-managed devices to automatically enroll in Intune.

---

## Co-Management Setup Process

### 1. Enabling Co-Management in SCCM

1. Open the **SCCM Console** and navigate to:
   - **Administration** > **Cloud Services** > **Co-Management**.
2. Click on **Enable Co-Management**.
3. Sign in using **Global Administrator** credentials for your Intune tenant.
4. Select whether to apply co-management to:
   - **All Devices** or
   - **A Pilot Collection** (recommended for phased rollouts).
5. Configure **Auto-Enrollment**:
   - This allows SCCM devices to automatically enroll into Intune.
6. Review and complete the setup wizard.

### 2. Device Enrollment to Intune

After enabling co-management, devices need to enroll in Intune. Enrollment can be done through:

1. **Manual Enrollment:** Users navigate to *Settings > Access Work or School* and input their credentials.
2. **Group Policy:** Configure auto-enrollment using Active Directory Group Policies.
3. **SCCM Console:** Auto-enroll devices directly from SCCM.

---

## Co-Management Workloads

Workloads determine whether SCCM or Intune manages a specific policy or task.

### Available Workloads:

1. **Compliance Policies**: Manage device compliance rules.
2. **Resource Access Policies**: VPN, Wi-Fi, email profiles.
3. **Device Configuration**: Manage settings like BitLocker, Windows Defender.
4. **Office Click-to-Run**: Manage deployment of Office 365 apps.
5. **Windows Updates**: Control software updates and patches.
6. **Endpoint Protection**: Manage antivirus and firewall settings.

### Configuring Workloads:

1. In **SCCM Console**, go to **Co-Management Properties**.
2. For each workload, select one of the following options:
   - **SCCM**: Managed entirely by SCCM.
   - **Pilot Intune**: Manage only devices in a pilot collection through Intune.
   - **Intune**: Fully managed by Intune.

### Best Practice:
- Start by moving **Compliance Policies** to Intune first for better device compliance reporting.
- Gradually transition other workloads in phases.

---

## Monitoring Co-Management

### Logs for Troubleshooting:

1. **PolicyAgent.log** - Monitors policy application.
2. **CoManagementHandler.log** - Tracks workload transitions.
3. **CCMSetup.log** - Logs SCCM client installations.
4. **PolicyPv.log** - Tracks configuration policy deployments.

### Verifying Enrollment:

1. In **Intune Admin Center**, verify device status under **Devices**.
2. Check SCCM Console for **Device Collections**.
3. Confirm Co-Management status via Control Panel (CCM Applet).

---

## Migration Scenarios

### 1. From SCCM to Intune Standalone

- Gradually move workloads to Intune while maintaining SCCM for legacy tasks.
- Decommission SCCM once all devices and workloads are fully managed by Intune.

### 2. Hybrid to Co-Management

- Transition from a hybrid SCCM-Intune environment to co-management.
- Ensure Azure AD Hybrid Join is enabled for device synchronization.

### 3. Mixed Authority

- Some devices are managed by SCCM, others by Intune.
- Utilize **Importer Tool** for a gradual migration.

---

## Use Cases for Co-Management

1. **Modern Device Management**: Enable modern management capabilities without giving up on-premise controls.
2. **Remote Work Enablement**: Manage devices securely over the internet using Intune.
3. **Conditional Access**: Ensure devices comply with organizational policies before granting access to resources.
4. **Phased Transition**: Gradually move devices and policies to the cloud.

---

## Conclusion

Co-management allows organizations to blend traditional SCCM-based device management with modern cloud-based Microsoft Intune. By enabling co-management, businesses can transition at their own pace, enforce better compliance, and provide a more seamless experience across their device fleets.

