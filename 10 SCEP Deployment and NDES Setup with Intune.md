Hello everyone,

Welcome to today's session, where we will discuss **SCEP Deployment and NDES Setup with Intune**. This is a Level 200 session focused on configuring **Simple Certificate Enrollment Protocol (SCEP)** and **Network Device Enrollment Service (NDES)** for certificate deployment in Intune.

---

## **Agenda**
1. **Introduction to Certificates & Why They Are Needed**
2. **What is SCEP & Why Use It?**
3. **SCEP vs. PKCS – Key Differences**
4. **Integrating SCEP with Intune**
5. **Step-by-Step NDES Setup & Configuration**
6. **End-to-End SCEP Deployment Flow**
7. **Troubleshooting SCEP & NDES Issues**
8. **Best Practices & Live Demo**

---

## **1. Introduction to Certificates & Why They Are Needed**
Certificates play a critical role in **authentication and security** by enabling devices to securely connect to **Wi-Fi, VPNs, emails, and other enterprise services** without requiring passwords.

A certificate-based authentication system helps:
- **Eliminate the need for passwords**.
- **Provide seamless access to resources**.
- **Enhance security through encryption and trusted identities**.

---

## **2. What is SCEP & Why Use It?**
### **What is SCEP?**
- **Simple Certificate Enrollment Protocol (SCEP)** is a protocol used to distribute **digital certificates** securely to devices.
- Originally developed by **Cisco**, it works using **HTTP-based request/response mechanisms**.
- SCEP is used to **automate certificate deployment** to **managed devices** via Intune.

### **Why Use SCEP?**
- **More Secure**: Unlike PKCS, the private key **never leaves the device**.
- **Scalability**: Can issue **unique certificates** per device.
- **Supports User-less Devices**: Unlike PKCS, SCEP can issue certificates to devices **without users**.
- **Ideal for Wi-Fi, VPN, and Email authentication**.

---

## **3. SCEP vs. PKCS – Key Differences**
| Feature | SCEP | PKCS |
|---------|------|------|
| Private Key Stored on Device? | ✅ Yes | ❌ No |
| Requires NDES & IIS? | ✅ Yes | ❌ No |
| Uses Azure App Proxy? | ✅ Yes | ❌ No |
| Supports User-less Devices? | ✅ Yes | ❌ No |
| Used for Wi-Fi/VPN Authentication? | ✅ Yes | ✅ Yes |

---

## **4. Integrating SCEP with Intune**
### **SCEP Deployment Flow**
1. **User enrolls device in Intune**.
2. **Intune pushes SCEP configuration profile to the device**.
3. **Device contacts NDES via Azure App Proxy**.
4. **NDES requests a certificate from the CA (Certificate Authority)**.
5. **Certificate is issued and sent to the device**.
6. **Device uses the certificate for authentication** (Wi-Fi, VPN, Email, etc.).

### **Prerequisites for SCEP Deployment**
- **Windows Server 2016+ with Active Directory Certificate Services (ADCS)**.
- **NDES Role Installed on a Separate Server** (not on the CA).
- **Azure AD Premium Subscription**.
- **Azure App Proxy configured for external access**.
- **Devices enrolled in Intune**.

---

## **5. Step-by-Step NDES Setup & Configuration**
### **Step 1: Install NDES on a Windows Server**
1. Open **Server Manager** and select **Add Roles and Features**.
2. Install **NDES (Network Device Enrollment Service)**.
3. Configure NDES to communicate with the **on-prem CA**.

### **Step 2: Configure IIS for NDES**
1. Open **IIS Manager** on the NDES server.
2. Configure **HTTPS bindings** and install an SSL certificate.
3. Enable **ASP.NET & IIS 6 Management Compatibility** features.

### **Step 3: Install & Configure Intune Certificate Connector**
1. **Download** the connector from the Intune portal.
2. **Install** it on the NDES server.
3. **Bind a certificate** during installation.
4. **Ensure the service account has permissions** on the CA.

### **Step 4: Configure Azure AD App Proxy for NDES**
1. **Install the Azure App Proxy Connector** on the NDES server.
2. **Publish NDES as an Enterprise App in Azure**.
3. **Create an external URL mapped to the internal NDES URL**.

---

## **6. End-to-End SCEP Deployment Flow**
1. **Device enrolls in Intune** and receives the SCEP profile.
2. **Device queries NDES** via the external URL (Azure App Proxy).
3. **NDES forwards the request** to the CA.
4. **CA issues the certificate** to NDES.
5. **NDES sends the certificate back** to Intune.
6. **Intune pushes the certificate** to the device.

---

## **7. Troubleshooting SCEP & NDES Issues**
| Issue | Possible Cause | Solution |
|--------|--------------|---------|
| Device does not receive certificate | NDES misconfiguration | Check IIS and NDES logs |
| Certificate request fails | Incorrect CA permissions | Ensure NDES has CA enroll permissions |
| Authentication issues | Incorrect subject name format | Ensure correct UPN/CN settings in SCEP profile |
| Azure App Proxy fails | Firewall blocking traffic | Ensure correct outbound firewall rules |

### **Checking Logs for Debugging**
- **NDES Logs**: `C:\ProgramData\Microsoft\Intune PKI`.
- **CA Logs**: Check **Issued Certificates** in CA MMC.
- **Event Viewer**: `Applications and Services Logs > Microsoft > Windows > Certificate Services`.
- **IIS Logs**: Located at `C:\inetpub\logs`.

---

## **8. Best Practices & Live Demo**
### **Live Demo Overview**
- **Step-by-step setup of NDES and SCEP in Intune**.
- **Deploying a SCEP profile to Windows, iOS, and Android devices**.
- **Verifying successful certificate issuance**.
- **Checking logs and troubleshooting failed deployments**.

### **Best Practices**
✔️ **Deploy in a test environment before production**.
✔️ **Ensure NDES and CA servers are correctly configured**.
✔️ **Use Azure App Proxy for external access**.
✔️ **Monitor logs regularly for errors**.
✔️ **Review certificate expiration policies**.

---

## **Conclusion**
SCEP deployment via Intune provides a **scalable and secure** method to distribute certificates for **Wi-Fi, VPN, and authentication**. With the proper **NDES setup, Azure App Proxy integration, and troubleshooting practices**, organizations can ensure a **smooth and secure certificate deployment process**.

Let me know if you need a deep dive into any section!

