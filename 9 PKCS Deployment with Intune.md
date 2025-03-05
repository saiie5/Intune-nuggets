Hello everyone,

Welcome to today's session, where we will discuss **PKCS Deployment with Intune**. This is a Level 200 session focused on deploying **PKCS certificates** via **Microsoft Intune**, covering its benefits, configuration, troubleshooting, and best practices.

---

## **Agenda**
1. **Introduction to PKCS and Its Use Cases**
2. **PKCS vs. SCEP – Key Differences**
3. **End-to-End PKCS Deployment Flow**
4. **Configuring PKCS Deployment in Intune**
5. **On-Premises Requirements for PKCS Deployment**
6. **Troubleshooting PKCS Deployment Issues**
7. **Live Demo & Best Practices**

---

## **1. Introduction to PKCS and Its Use Cases**
### **What is PKCS?**
- PKCS (**Public Key Cryptography Standards**) is a certificate format that contains both **public and private keys**.
- Often referred to as **PKCS#12** or **PFX**, these certificates are essential for **secure authentication, encryption, and signing**.
- Used in **Wi-Fi authentication, VPN authentication, S/MIME email encryption, and client authentication**.

### **PKCS vs. .CER Certificate Format**
| Feature | PKCS (.PFX) | .CER |
|---------|------------|------|
| Contains Private Key? | ✅ Yes | ❌ No |
| Used for Authentication? | ✅ Yes | ❌ No |
| Used for Encryption? | ✅ Yes | ❌ No |

---

## **2. PKCS vs. SCEP – Key Differences**
### **PKCS Deployment Characteristics:**
- Requires **Intune Certificate Connector** on-prem.
- **More secure**, as certificates are issued directly to Intune.
- Does **not require a public-facing NDES server**.
- Suitable for organizations that do **not expose internal infrastructure to the internet**.

### **SCEP Deployment Characteristics:**
- Uses **NDES (Network Device Enrollment Service)**.
- **Private key remains on the device**.
- Requires **public-facing endpoints**.
- More **complex setup** than PKCS.

---

## **3. End-to-End PKCS Deployment Flow**
1. **Intune Admin creates a PKCS profile** in the **Intune portal**.
2. The profile is **pushed to enrolled devices**.
3. Devices request a certificate from **Intune Service**.
4. **Intune Service contacts the On-Prem Certificate Connector**.
5. **The connector requests a certificate from the CA (Certificate Authority)**.
6. The CA **issues a PKCS certificate**.
7. The **certificate is pushed to the device** via Intune.
8. The device uses the certificate for **Wi-Fi, VPN, or authentication**.

---

## **4. Configuring PKCS Deployment in Intune**
### **Step 1: Install Intune Certificate Connector**
1. **Download the Intune Certificate Connector** from Intune Portal:
   - Go to **Devices > Windows > Configuration Profiles > Certificate Authority**.
   - Click **Add** and download the connector.
2. Install the connector on a Windows Server (2016+ recommended).
3. **Sign in with a Global Admin account**.
4. **Ensure the server has access to the CA**.

### **Step 2: Configure the PKCS Profile in Intune**
1. Navigate to **Devices > Configuration Profiles > Create Profile**.
2. Select **PKCS certificate profile**.
3. Configure the following:
   - **Certificate Authority FQDN** (on-prem CA).
   - **Template Name** (matching the CA template name).
   - **Subject Name Format** (UPN or CN).
   - **Renewal & Expiry Settings**.
4. Assign the profile to **targeted device groups**.

### **Step 3: Configure On-Prem CA for PKCS Certificates**
1. Open **Certification Authority (CA) MMC**.
2. **Duplicate an existing certificate template**:
   - Go to **Manage Templates > Duplicate Template**.
   - Select **Computer or User Template**.
   - Enable **Allow private key to be exported**.
   - Add **"Client Authentication"** in Application Policies.
3. Assign the **correct permissions** to the **Intune Connector service account**.
4. Publish the certificate template in **CA MMC > Certificate Templates**.

---

## **5. On-Premises Requirements for PKCS Deployment**
- **Windows Server 2016+ with Intune Certificate Connector installed**.
- **Active Directory Certificate Services (ADCS) running on-prem**.
- **Domain-joined or Hybrid Azure AD Joined devices**.
- **Correct firewall and proxy configurations** to allow communication.

---

## **6. Troubleshooting PKCS Deployment Issues**
| Issue | Possible Cause | Solution |
|--------|--------------|---------|
| Certificate not issued | CA template misconfiguration | Verify template settings in CA MMC |
| Device does not receive cert | Intune Connector offline | Ensure connector is running and active in Intune portal |
| Authentication failure | Incorrect subject name format | Ensure correct UPN/CN settings in the profile |
| PKCS profile deployment fails | CA permissions issue | Grant correct permissions to Intune Connector service account |

### **Checking Logs for Debugging**
- **CA Logs**: Check **Issued Certificates** in CA MMC.
- **Intune Connector Logs**: Located at `C:\Program Files\Microsoft Intune\Logs`.
- **Event Viewer**: `Applications and Services Logs > Microsoft > Windows > Certificate Services`.
- **Device Logs**: Company Portal logs for mobile devices, Event Viewer for Windows.

---

## **7. Live Demo & Best Practices**
### **Live Demo Overview**
- Step-by-step creation of a PKCS certificate profile in **Intune**.
- Deploying the profile to **Windows, iOS, and Android devices**.
- **Verifying successful certificate issuance**.
- **Checking logs and troubleshooting failed deployments**.

### **Best Practices**
✔️ Always **test** PKCS deployment in a **pilot environment**.
✔️ Ensure **Intune Connector service is online** before deployment.
✔️ Use **Group Policy or Conditional Access** for security enforcement.
✔️ Periodically **review and update certificate templates**.
✔️ Check **Intune portal logs** to monitor deployment status.

---

## **Conclusion**
PKCS deployment via Intune provides a **secure and efficient** way to manage certificates for **Wi-Fi, VPN, and authentication**. By following the **correct setup steps**, ensuring **proper on-prem configurations**, and leveraging **Intune logs for troubleshooting**, organizations can **seamlessly deploy PKCS certificates** to their managed devices.

Let me know if you need any further clarifications!

