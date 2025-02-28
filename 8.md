Hello everyone,

Welcome to today's session, where we will discuss **Microsoft Graph API + PowerShell Integration with Intune**. This is a Level 200 session focusing on automating Intune tasks using **Microsoft Graph API** and **PowerShell**.

## **Agenda**

1. Understanding **APIs & Microsoft Graph Explorer**
2. Making API Calls with **Graph Explorer**
3. Understanding **REST API Calls & Querying Data**
4. Automating Intune Tasks with **PowerShell & Graph API**
5. Live Demo & Use Cases
6. Useful Links & Documentation

---

## **Understanding APIs & Microsoft Graph Explorer**

### **What is an API?**

- API (**Application Programming Interface**) acts as a **bridge** between different applications.
- It allows us to **fetch, modify, and manage** data programmatically.
- Example: If you need to access Intune data without using the UI, an API call can retrieve the information directly.

### **What is Microsoft Graph?**

- **Microsoft Graph** is the API gateway to interact with Microsoft 365 services, including **Intune, Azure AD, Teams, Exchange, and SharePoint**.
- It is a RESTful API that allows **data retrieval and management**.
- Graph API supports **GET, POST, PATCH, and DELETE** operations.

### **Microsoft Graph Explorer**

- A web-based tool to **test and execute API calls**.
- URL: [https://developer.microsoft.com/en-us/graph/graph-explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)
- Allows you to authenticate, fetch, and modify Microsoft 365 data.

### **Making API Calls with Graph Explorer**

1. **Go to Graph Explorer** and sign in.
2. **Modify API Permissions**:
   - Click **Modify Permissions**.
   - Grant permissions for **Intune device management**.
3. **Run Queries**:
   - Fetch user details: `https://graph.microsoft.com/v1.0/me`
   - List all devices: `https://graph.microsoft.com/v1.0/deviceManagement/managedDevices`

---

## **Understanding REST API Calls & Querying Data**

- **REST API (Representational State Transfer)** allows interaction with web services.
- Graph API calls return data in **JSON format**.

### **Example API Call for Fetching Intune Devices**

```http
GET https://graph.microsoft.com/v1.0/deviceManagement/managedDevices
```

- **GET**: Retrieves information.
- **POST**: Creates new objects.
- **PATCH**: Updates existing data.
- **DELETE**: Removes objects.

#### **Using Query Parameters in API Calls**

Example: Filter devices based on OS version:

```http
GET https://graph.microsoft.com/v1.0/deviceManagement/managedDevices?$filter=operatingSystem eq 'Windows'
```

---

## **Automating Intune Tasks with PowerShell & Graph API**

### **Step 1: Install PowerShell Modules for Microsoft Graph**

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
Import-Module Microsoft.Graph
```

### **Step 2: Authenticate to Microsoft Graph**

```powershell
Connect-MgGraph -Scopes "DeviceManagementConfiguration.ReadWrite.All"
```

- Logs in to Microsoft Graph with **Intune management permissions**.

### **Step 3: Fetch Intune Device Information**

```powershell
Get-MgDeviceManagementManagedDevice | Select-Object DeviceName, OperatingSystem, ComplianceState
```

### **Step 4: Create an Intune Policy via PowerShell**

```powershell
$policy = @{
    "displayName" = "Windows 10 Compliance Policy";
    "description" = "Policy for Windows 10 devices";
    "@odata.type" = "#microsoft.graph.deviceCompliancePolicy";
}

New-MgDeviceManagementDeviceCompliancePolicy -BodyParameter $policy
```

### **Step 5: Delete a Device from Intune**

```powershell
Remove-MgDeviceManagementManagedDevice -ManagedDeviceId "<Device_ID>"
```

---

## **Live Demo & Use Cases**

1. **Fetching Intune Device Data with PowerShell**
2. **Creating & Updating Device Configuration Policies**
3. **Automating Bulk Device Management Tasks**
4. **Using PowerShell Scripts for Automation**

---

## **Summary**

Microsoft Graph API and PowerShell enable automation for **Intune device management**. By leveraging API calls, we can efficiently manage **devices, users, and policies** without relying on the UI.

Let me know if you need a deep dive into any section!

