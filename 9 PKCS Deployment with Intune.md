Here is a detailed briefing document based on the provided sources, outlining the main themes, most important ideas, and key facts about PKCS certificate deployment with Intune.

# Briefing Document: PKCS Deployment with Intune

**Source:** Excerpts from "#IntuneNugget 9- PKCS deployment with Intune |Level 200" (Transcript) and "PKCS Deployment with Intune | Level 200 session notes"
**Subject:** In-depth review of deploying PKCS certificates using Microsoft Intune, including comparison with SCEP, deployment flow, configuration requirements, troubleshooting logs, and practical tips.

## Key Themes:

* **PKCS vs. SCEP**: A significant portion of the source is dedicated to comparing PKCS (PKCS12/PFX) and SCEP (Simple Certificate Enrollment Protocol) deployments, highlighting the infrastructure requirements and operational differences between the two methods for delivering certificates to devices via Intune.
* **PKCS Deployment Process**: The source details the end-to-end flow of a PKCS certificate deployment, from the Intune admin creating a profile to the certificate being installed on the end user's device.
* **Configuration Requirements**: Specific configurations needed in both the Intune portal and on-premises Certificate Authority (CA) infrastructure are outlined.
* **Troubleshooting and Verification**: The source provides methods for tracking a successful PKCS deployment by examining logs and checking the certificate status on the CA and the end device.
* **Practical Tips**: Several important tips and potential pitfalls are shared to ensure successful PKCS deployments.

## Most Important Ideas/Facts:

### 1. Introduction to PKCS and Its Use Cases

* **What is PKCS?**
    * PKCS (Public Key Cryptography Standards) is a certificate format that contains both **public and private keys**.
    * Often referred to as **PKCS#12** or **PFX**, these certificates are essential for **secure authentication, encryption, and signing.** PFX is the current term and an improved, open standard version of the earlier PKCS12.
    * A PKCS/PFX certificate is a "bag of properties" that always contains both the public and private key for the associated certificate.
    * Example Quote: "a P FX format includes both the public and the private key for the Associated certificate and obviously if it's including the private key then it goes without saying that it's very very important and it should not be shared outside our organization"
* **Used for various purposes including:**
    * Mail Encryption (S/MIME)
    * Connecting to Wi-Fi and VPN seamlessly.
    * User Authentication.
    * Client Authentication.
    * Server Authentication.

* **PKCS vs. .CER Certificate Format:**
    | Feature               | PKCS (.PFX) | .CER    |
    | :-------------------- | :---------- | :------ |
    | Contains Private Key? | ✅ Yes      | ❌ No   |
    | Used for Authentication? | ✅ Yes      | ❌ No   |
    | Used for Encryption?  | ✅ Yes      | ❌ No   |

### 2. PKCS vs. SCEP – Key Differences

| Feature                 | PKCS Deployment Characteristics                                                                                                        | SCEP Deployment Characteristics                                                                                                         |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------- |
| **Infrastructure** | Requires **Intune Certificate Connector** on-prem. Does **not require a public-facing NDES server** (no IIS role, public endpoint, or Azure AD App Proxy). Considered "lightweight."                                                                                                                                                                                                      | Uses **NDES (Network Device Enrollment Service)**. Requires an additional server with the NDES role, which needs to be internet-facing. Often requires an Azure AD Application Proxy for publishing the NDES server. More **complex setup**. |
| Example Quote           | "in case of pkcs the device does not talk to the any of the service the device will not talk to the azure a proxy or the device will not talk to the end s of the device will not talk to the CA the device is only going to talk to the Indian service and in tune service is going to talk is going to work as a mediator" | "in case of skep you need to have a server which have which is present in your intranet but has connection to the internet as well that is public facing and that is the point of contact which talks between your CA and your internet" |
| **Private Key Handling** | **Crucial Distinction**: The private key travels with the certificate during deployment. In PKCS, the MDM server (Intune) generates the private key and the certificate on behalf of the mobile device and then deploys it.                                                                                   | The private key is generated on the device and never leaves it, making it more secure.                                                                                                                                  |
| Example Quote           | "in case of pkcs both the private as well as the public key travels to dessert"                                                        | "in case of skip the private key never leaves the device therefore it's more secure however in case of pkcs both the private as well as the public key travels to dessert" and "in case of skip the mobile device generates the private and the public key" |
| **Certificate Uniqueness** | The **same certificate** (containing the same public and private key) can be distributed to multiple devices for the same user (based on UPN).                                                                                                                                                     | A **unique key and certificate** are generated for every device, even if the same user enrolls multiple devices. This allows for revoking a certificate from just one specific device if needed.                                |
| Example Quote           | "in case of pkcs the same certificate can be distributed to multiple devices of the same user"                                         |                                                                                                                                         |

### 3. End-to-End PKCS Deployment Flow

1.  **Intune Admin creates a PKCS profile** in the Intune portal.
2.  The profile is **pushed to enrolled devices** (deployed to a user or group).
3.  The mobile device receives the profile but doesn't know how to process it directly. It **contacts the Intune service** for help.
4.  The Intune service acts as a middleman. It retrieves the PKCS profile and **contacts the on-premises server where the Intune connector is installed**. Intune knows the connector's location from its database entry created during installation.
5.  The **Intune connector receives the request** from the Intune service. This request includes information configured in the profile, such as the CA name (FQDN and friendly name), template name, subject name format, validity, etc.
6.  The Intune connector uses the configured account (system account or a specified service account) to **connect to the CA**. It requests a certificate based on the provided template name and user information. The CA must be able to find a template with the exact name provided.
7.  The CA, if successful, **issues a certificate** based on the template and user information. This certificate (containing public and private keys) is passed back to the Intune connector.
8.  The **Intune connector uploads the certificate to the Intune service.**
9.  The **Intune service delivers the certificate to the device.**
10. The device installs the certificate and reports the installation status back to the Intune portal. The device then uses the certificate for Wi-Fi, VPN, or authentication.

### 4. Prerequisites for PKCS Deployment

* Windows Server 2016+ (recommended for Intune Certificate Connector).
* Active Directory Certificate Services (ADCS) running on-premises (your on-premises CA).
* Domain-joined or Hybrid Azure AD Joined devices.
* Correct firewall and proxy configurations to allow communication.
* Intune Certificate Connector on-prem.

### 5. Configuring PKCS Deployment in Intune

#### Step 1: Install Intune Certificate Connector

* Download the Intune Certificate Connector from Intune Portal: Go to **Devices > Windows > Configuration Profiles > Certificate Authority**. Click **Add** and download the connector.
* Install the connector on a Windows Server (2016+ recommended).
* Sign in with a Global Admin account.
* Ensure the server has access to the CA.
* Select the "PFX distribution" option during installation.
* Configure the account (system or service account) the connector will use to request certificates from the CA. This account needs specific permissions on the CA.
* **Crucial:** Reboot the server after installing the Intune connector to ensure proper functionality and prevent excessive certificate issuance.

#### Step 2: Configure the PKCS Profile in Intune

* Navigate to **Devices > Configuration Profiles > Create Profile**.
* Select **PKCS certificate profile**.
* **Configure the following:**
    * Profile Name and Description.
    * Assignment to a user/group.
    * Renewal Threshold (e.g., 20%).
    * Certificate Validity Period (should match or be less than the CA template validity).
    * **Certification Authority FQDN**: The fully qualified domain name of the server hosting the CA role.
    * **Certification Authority Name**: The exact friendly name of the CA.
    * **Certificate Template Name**: The exact name of the certificate template configured on the CA.
    * **Subject Name Format** (e.g., Common Name, UPN).
    * **Subject Alternative Name (SAN)** (e.g., UPN).
    * **Key Storage Provider (KSP)** (for Windows, where the private key should be stored, e.g., TPM).
    * **Extended Key Usage** (e.g., Client Authentication, Secure Email).
    * **Root Certificate (for Android ONLY)**: PKCS profiles for Android devices must be linked to a Trusted Certificate profile containing the CA root certificate. This is for chain building on Android devices.

### 6. On-Premises CA Configuration for PKCS Certificates

* Open **Certification Authority (CA) MMC**.
* **Certificate Template Creation**:
    * Duplicate an existing template (suggested from "Computer" or "User"). Go to **Manage Templates > Duplicate Template**.
    * **Configure template properties:**
        * Give it a meaningful name (Template Name and Display Name).
        * Set Validity Period.
        * **Mandatory**: On the "Request Handling" tab, select "**Allow private key to be exported**".
        * Ensure the "Extensions" tab includes the required Application Policies (e.g., Client Authentication).
        * For iOS compatibility, **uncheck "Signature is proof of origin"** on the "Key Usage" tab if duplicating from a User template (or if that option is present).
    * **Permissions**: The account configured in the Intune connector for requesting certificates must have "**Read**" and "**Enroll**" (or "Autoenroll") permissions on the security tab of the certificate template. If duplicating from a User template, this account may also need "Request Certificates" permission on the CA's friendly name properties.
* **Publish the Template**: The newly created template must be published on the CA so it can issue certificates (**CA MMC > Certificate Templates**).
* **Export Root Certificate (for Android)**: Export the CA's root certificate as a .CER file to create a Trusted Certificate profile in Intune.

### 7. Troubleshooting PKCS Deployment Issues

#### Checking Logs for Debugging

* **CA Logs**: Check **Issued Certificates** in CA MMC. Examine the certificate properties (thumbprint, subject name, SAN, template) to confirm it matches expectations.
    * Example Quote: "whenever things are not following up wherever things are not moving ahead smoothly we will be knowing that okay the C hell it ca has not issued it therefore there is no point checking the company portal logs right" - Emphasizes the importance of following the flow for effective troubleshooting.
* **Intune Connector Logs (on the connector server)**:
    * Intune Connector Service Logs (SVC logs): Located at `C:\Program Files\Microsoft Intune\NDES Svc\logs\`. Look for entries indicating "generating San list", "San value is this", and "Issued pfx certificate for device Id this successfully". These show the connector receiving the request and processing it.
    * PFX Logs: Located at `C:\Program Files\Microsoft Intune\NDES Svc\PfxRequest\`. Check the Succeeded folder (after processing is complete) for XML files. These files contain the thumbprint of the issued certificate and the device ID it was intended for.
* **Event Viewer**: `Applications and Services Logs > Microsoft > Windows > Certificate Services`.
* **Device Logs/Store**:
    * **Company Portal Logs (Android/iOS)**: Shows the device receiving the profile, requesting the certificate, and the installation status. Look for entries like "received a certificate" and "state has changed from cert install requested to cert access granted". Verify the thumbprint matches.
    * **Windows Event Viewer**: Navigate to `Event Viewer -> Applications and Services Logs -> Microsoft -> Windows -> CertificateServicesClient-Lifecycle-User -> Operational`. Look for an event indicating a new certificate installation. The details will include the thumbprint.
    * **Device Certificate Store**:
        * Windows: Open `certmgr.msc` for the current user and check the "Personal" store. Verify the certificate is present and check its properties (issuer, subject, thumbprint).
        * iOS: Go to **Settings -> General -> Device Management -> Management Profile -> More Details**. Check the certificates listed.
        * Android: Requires a third-party app (like "My Certificate") to view user certificates in the store.
* **Intune Portal (Device Status/User Status)**: Finally, check the deployment status of the PKCS profile in Intune. It should show as "Succeeded" once the device reports back.

#### Common Issues and Solutions:

| Issue                      | Possible Cause                  | Solution                                                              |
| :------------------------- | :------------------------------ | :-------------------------------------------------------------------- |
| Certificate not issued     | CA template misconfiguration    | Verify template settings in CA MMC                                    |
| Device does not receive cert | Intune Connector offline        | Ensure connector is running and active in Intune portal             |
| Authentication failure     | Incorrect subject name format   | Ensure correct UPN/CN settings in the profile                       |
| PKCS profile deployment fails | CA permissions issue            | Grant correct permissions to Intune Connector service account         |

### 8. Best Practices

* ✔️ Always **test** PKCS deployment in a **pilot environment**.
* ✔️ Ensure **Intune Connector service is online** before deployment.
* ✔️ Use **Group Policy or Conditional Access** for security enforcement.
* ✔️ Periodically **review and update certificate templates**.
* ✔️ Check **Intune portal logs** to monitor deployment status.
* The Intune connector's sign-in account (used to connect to Azure) must have an EMS license and ideally only one role assigned (e.g., Intune Administrator) to avoid potential issues.
* Ensure all names (CA FQDN, CA friendly name, template name) are exact and case-sensitive when configuring the Intune profile and CA.
* For iOS compatibility, ensure the "Signature is proof of origin" option is unchecked in the template if duplicating from a User template.
* The certificate template must have the necessary key usages (e.g., Client Authentication) for the intended purpose.
* Having multiple Intune connectors for PKCS deployments can cause confusion for the Intune service; ideally, use only one connector for PKCS.
* **Crucial Tip**: Reboot the server after installing the Intune connector to avoid excessive and unsuccessful certificate issuance attempts by the CA.

### 9. Conclusion

PKCS certificate deployment with Intune offers a secure and efficient way to manage certificates for Wi-Fi, VPN, and authentication. It provides a lightweight alternative to SCEP by eliminating the need for an internet-facing NDES server and its associated components. The process involves configuring a profile in Intune, installing and configuring the Intune Certificate Connector on-premises, and setting up a certificate template on the CA with specific permissions and properties (including allowing private key export). While simpler in infrastructure, PKCS deployments involve the private key traveling over the wire and the same certificate potentially being used on multiple devices for a single user. Successful deployment can be meticulously tracked by examining logs on the CA, the Intune connector server, and the end device, culminating in the certificate being visible in the device's certificate store and the deployment status showing "Succeeded" in the Intune portal. Proper configuration of the CA template, connector account permissions, and crucially, rebooting the connector server after installation, are key to avoiding common pitfalls. By following the correct setup steps, ensuring proper on-prem configurations, and leveraging Intune logs for troubleshooting, organizations can seamlessly deploy PKCS certificates to their managed devices.
