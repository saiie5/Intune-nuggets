
# 📘 Windows Automatic Enrollment in Microsoft Intune

This guide explains how to configure **automatic MDM enrollment** in Microsoft Intune for Windows devices.

## 🔧 Step 1: Configure MDM User Scope

1. Go to [Microsoft Endpoint Manager Admin Center](https://endpoint.microsoft.com)
2. Navigate to: **Devices** > **Enroll devices** > **Windows Enrollment**
3. Under **General**, select **Automatic Enrollment**
4. On the MDM and MAM scope configuration page, configure as shown below:



- Select **Some** for MDM user scope
- Choose the desired Azure AD group (e.g., `MDM test group 1`)
- Click **Select** > **Save**
- You will see a confirmation: _"Successfully updated Microsoft Intune"_
![MDM Scope Settings](https://raw.githubusercontent.com/saiie5/Intune-nuggets/main/1..png)


## ✅ Step 2: Verify Licensing Requirements

Ensure users are assigned Intune licenses (e.g., EMS E5).

## 🖥️ Step 3: Join Windows 10 Device to Azure AD

Follow these steps on a Windows 10 machine:
1. Go to **Settings** > **Accounts** > **Access work or school**
2. Click **Connect** > **Join this device to Azure Active Directory**
3. Enter the credentials for a licensed user in the MDM group
4. Complete MFA (if required)
5. Follow on-screen prompts to finish device registration

## 🔄 Step 4: Validate Device Enrollment

1. Sign in with the user credentials
2. Configure Windows Hello (PIN, etc.)
3. Go to **Settings** > **Accounts** > **Access work or school**
4. Click on the account > **Info**
5. Check **Sync status** and **Management** info

## 🎉 Final Validation

- Go back to Microsoft Endpoint Manager > **Devices** > **All Devices**
- Confirm device shows as **Managed by Intune**
- Check primary user, compliance, and OS details

---

✅ You have now successfully set up automatic enrollment for Windows in Intune!
