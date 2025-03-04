**Notes on Setting Up Co-Management in Intune and SCCM**

### Introduction to Co-Management
- Co-Management allows management of Windows devices via both Microsoft Endpoint Configuration Manager (SCCM) and Microsoft Intune.
- It enables organizations to transition workloads from SCCM to Intune gradually.

### Prerequisites for Co-Management
1. **SCCM Configuration Manager Server:** Ensure SCCM is running (version 1902 or later is recommended).
2. **Hybrid Azure AD Joined Devices:** Devices must be joined to on-premises AD and Azure AD.
3. **Azure AD Connect:** Ensure the Azure AD Connect tool is configured to sync with the on-premises domain.

### Demonstration Environment
- Two devices used:
  - "Co-Managed 2"
  - "Co-Management Demo"
- Devices are Windows 10 and hybrid Azure AD joined but not yet Intune-enrolled.

### Initial Setup Steps
1. **SCCM Client Installation:**
   - Verify that devices are reachable via ping.
   - Install the SCCM client from the SCCM console.
   - Monitor installation progress in `CCMSetup.log`.

2. **Verifying Client Installation:**
   - Confirm the `CCM` folder is created on the device (C:\Windows\CCM).
   - Check the SCCM console for client installation status.

3. **Registry Checks:**
   - Validate registry keys under `HKEY_LOCAL_MACHINE\Software\Microsoft\CCM`.

### Enabling Co-Management in SCCM
1. Navigate to **Administration > Cloud Services > Co-Management**.
2. Configure Co-Management:
   - Sign in with Global Admin credentials.
   - Choose **Pilot** or **All** for device enrollment.
   - Select workloads to transition (e.g., Compliance Policies, Device Configuration).

3. **Pilot Collection Configuration:**
   - Create a pilot collection in SCCM.
   - Add devices to the pilot collection.
   - Devices in the pilot collection will automatically attempt Intune enrollment.

### Automatic Enrollment Process
1. SCCM pushes a policy to devices with:
   - Workload information
   - Enrollment directive for Intune

2. Enrollment is triggered when a user logs in with a valid EMS (Enterprise Mobility + Security) license.

3. **Enrollment Monitoring:**
   - Track progress in the `CoManagementHandler.log`.
   - Successful enrollment updates registry keys under `HKEY_LOCAL_MACHINE\Software\Microsoft\Enrollments`.

### Types of Enrollment
1. **User-Driven Enrollment:**
   - Triggered when a user with an EMS license logs in.
   - Device is associated with the logged-in user.

2. **Userless Enrollment:**
   - Occurs if no valid EMS license is detected.
   - Device is enrolled without associating with any user.

### Key Log Files
1. `CCMSetup.log` – SCCM client installation status.
2. `CoManagementHandler.log` – Co-management and enrollment activities.
3. Event Viewer – Additional system-level errors or warnings.

### Task Scheduler Role
- SCCM triggers enrollment via scheduled tasks:
  - Task path: `Microsoft > Windows > EnterpriseMgmt`
  - Runs `DeviceEnroller.exe` for MDM enrollment.

### Behavior Based on User License
1. **EMS License Assigned:**
   - Immediate enrollment.
   - Device is associated with the user.
2. **No EMS License:**
   - Delayed enrollment.
   - Device remains userless.

### Verification in Intune
1. Confirm device presence under **Devices > All Devices** in Intune.
2. Validate co-management status in the SCCM applet.

### New Features in SCCM 1906
- Ability to assign different pilot collections for each workload.
- Granular control over workload migration.

### Recommendations
- Enable co-management even if full Intune migration is not planned.
- Offload **Compliance Policies** to Intune to leverage Conditional Access.

### Troubleshooting Tips
1. **Check Logs:** Review SCCM and Intune logs for errors.
2. **Validate Connectivity:** Ensure devices can communicate with SCCM and Intune endpoints.
3. **Confirm Licensing:** Ensure users have the correct EMS license for successful enrollment.

### Conclusion
- Co-management is a powerful feature to manage devices across SCCM and Intune.
- Gradual workload transition allows flexible management.
- Proper setup and monitoring ensure a smooth deployment process.

