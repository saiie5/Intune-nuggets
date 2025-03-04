## **Notes on Intune Co-Management Setup**

### **Introduction to Co-Management**
- This is the second part of the Intune Co-Management nugget.
- The first part covered theoretical concepts about what co-management is and why it's necessary.
- This session involves a demonstration of the co-management process.

### **Prerequisites for Co-Management Setup**
1. **Configuration Manager Server**:
   - Running SCCM version 1902 (1906 is in preview during this demo).
2. **Devices**:
   - Two devices joined to the on-premises domain.
   - Devices are in a hybrid Azure AD joined state.
3. **Azure AD Connect**:
   - Set up and functioning, enabling synchronization between on-prem AD and Azure AD.

### **Device Status Overview**
- Two devices are available:
  - 'Co-managed-2'
  - 'Co-management-demo'
- Both devices are hybrid Azure AD joined but not yet enrolled in Intune.

### **Step 1: Installing the SCCM Client**
1. Ensure communication between the SCCM server and the devices.
2. Validate the client installation via:
   - SCCM setup logs (`C:\Windows\CCMSetup\Logs`)
   - Registry entries (e.g., `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\CCM`)
3. Successful installation confirmation:
   - Status changes to "Yes" under the SCCM console.
   - `CCMSetup.log` exits with return code 0.

### **Step 2: Adding Devices to the Pilot Collection**
1. Navigate to **Assets and Compliance** in the SCCM console.
2. Add devices to the designated "Pilot Device Collection."
3. Confirm device addition via collection refresh.

### **Step 3: Enabling Co-Management**
1. Access **Administration** > **Cloud Services** > **Co-Management**.
2. Configure co-management by signing in with Global Admin credentials.
3. Choose between **All Devices** or **Pilot**:
   - "Pilot" allows staged deployment.
4. Define workloads:
   - Workloads can be managed by either SCCM or Intune.
   - By default, workloads remain with SCCM.

### **Step 4: Monitoring Device Enrollment**
1. When a device joins the pilot collection:
   - A configuration policy is sent to the device.
2. Enrollment
