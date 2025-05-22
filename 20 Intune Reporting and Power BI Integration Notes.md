Intune Reporting and Power BI Integration Notes
1. Introduction to Intune Reporting
1.1 Purpose of Reporting in Intune
Reporting in Microsoft Intune is critical for managing and optimizing endpoint environments. It serves several key purposes:

Monitoring Health and Activity: Tracks endpoint behavior and check-in frequency with Intune.
Informing Management and Decision-Making: Provides actionable insights for management to identify trends and improve infrastructure.
Troubleshooting Issues: Identifies patterns of failure, non-compliant devices, and configuration issues.
Improving Employee Experience: Enables proactive resolution of issues impacting users.
Identifying Application Usage: Tracks application performance and usage patterns.

The core logic of reporting is consistent across IT management platforms (e.g., SCCM, Group Policy), providing a centralized database for actionable insights.
1.2 Evolution of Intune Reporting
Intune reporting has evolved significantly:

Legacy Method (Silverlight Portal, 2014-2016):
Used the Silverlight console, primarily for Windows 7 PC management.
Required an agent on endpoints for communication.
Limited reporting capabilities focused on patch management and inventory.
Challenges:
Limited device details (e.g., certificates).
Difficult to export reports; limited export formats.
No integration with external tools.
No online report sharing.


Status: Decommissioned and out of support.


Hybrid Deployment (with SCCM):
Integrated Intune with SCCM, leveraging SQL reporting services for more granular data.
Disadvantage: Required setting MDM authority to SCCM, not Intune.
Status: Decommissioned; existing tenants migrated to Intune standalone.


Modern Method (Ibiza Console/Endpoint Manager):
Utilizes the modern Endpoint Manager portal.
Offers a redesigned reporting framework with enhanced capabilities.
Key Improvements:
Export functionality for most reports.
Fast report generation, even for large tenants.
Data paging, search, and sort capabilities.
Pictorial representations (graphs, donut charts, pie charts).
Dedicated report sections for compliance, configuration, etc.
Access to detailed data points (e.g., certificates, encryption readiness).





2. Modern Intune Reporting Framework
2.1 Types of Reports
The modern Intune reporting framework (Ibiza console) categorizes reports into four types:

Operational Reports:
Provide timely, targeted data for troubleshooting.
Example: Non-compliant devices, devices without compliance policies.


Organizational Reports:
Offer high-level summaries for management.
Example: Device management state, assignment status.


Historical Reports:
Show trends over time (up to ~60 days).
Example: Enrollment trends, compliance trends.


Specialist Reports:
Allow customization, often integrated with tools like Log Analytics.
Example: Custom queries for specific data points.



2.2 Report Features in the Ibiza Console

Export: Most reports can be exported with customizable columns (e.g., IMEI, device name).
Performance: Quick generation, even for large datasets.
Data Paging: Navigate through data pages or jump to specific pages.
Search and Sort: Filter and sort data directly in the UI.
Visualization: Includes graphs, donut charts, and pie charts.
Report Examples:
Managed Devices List: Exportable with details like IMEI, serial number.
Compliance Policy Status: Shows compliant/non-compliant devices, devices without policies.
Device Configuration Profile Status: Tracks success, errors, or conflicts.
Certificate Status: Includes serial number, issuer, expiry date.
Device Actions Logs: Tracks actions like retire, wipe, restart, sync.
App Protection Status: Monitors MAM policy deployment and check-in status.
App Installation Status: Tracks app installation success/failure.
Discovered Apps: Lists applications detected on devices.



2.3 Access Requirements

Access typically requires Global Admin, Intune Admin, or roles with read permissions.
Reports are accessible via the Endpoint Manager portal.

3. Advanced Reporting Methods
3.1 Graph API / PowerShell

Overview: Provides access to Intune’s backend data in real-time via REST API calls.
Tools: Graph Explorer, PowerShell.
Capabilities:
Highly customized and automated reporting.
Real-time data retrieval.
Supports filtering similar to PowerShell’s Select-Object.


Requirements:
Appropriate permissions (e.g., Intune Admin).
Microsoft Intune license.


Example:To retrieve a list of managed devices using PowerShell:Connect-MgGraph -Scopes "DeviceManagementManagedDevices.Read.All"
$devices = Get-MgDeviceManagementManagedDevice
$devices | Select-Object deviceName, complianceState, lastSyncDateTime | Export-Csv -Path "Devices.csv" -NoTypeInformation

This script connects to Microsoft Graph, retrieves managed devices, and exports selected properties to a CSV file.

3.2 Intune Data Warehouse

Overview: A repository holding nightly snapshots of Intune data, retained for ~60 days.
Access: Via OData protocol using tools like Power BI.
Data Snapshot: Taken at midnight UTC.
Use Case: Historical data analysis (up to 60 days).

4. Power BI Integration with Intune Data Warehouse
4.1 Overview

Purpose: Enables advanced, customized, and visual reporting using Power BI.
Benefits:
Access to historical data (~60 days).
Customizable reports with specific columns and relationships.
Advanced visualizations (graphs, tables, pie charts, donut charts, bar charts).
Drill-down functionality for granular data exploration (e.g., from OS version to device name).
Online report publishing and automatic refresh capabilities.


Data Insights:
License usage.
App installation attempts/failures.
Device enrollment summaries.
Device inventory details.



4.2 Configuration Setup for Power BI Integration
To integrate Power BI with the Intune Data Warehouse, follow these steps:
Step 1: Obtain Intune Data Warehouse URL

Log in to the Microsoft Endpoint Manager admin center (https://endpoint.microsoft.com).
Navigate to Reports > Data Warehouse.
Copy the Data Warehouse URL provided in the portal.
Example URL: https://reports.office.com/data/odata/IntuneDataWarehouse.



Step 2: Install Power BI Desktop

Download and install Power BI Desktop (free) from https://powerbi.microsoft.com/desktop/.
Ensure you have a Power BI license (free tier is sufficient) and an account with Intune Admin or Global Admin permissions.

Step 3: Connect Power BI to Intune Data Warehouse

Open Power BI Desktop.
Click Get Data > OData Feed.
Paste the Intune Data Warehouse URL copied from the Intune portal.
Sign in with your Microsoft 365 credentials (must have Intune read permissions).
Select the desired tables (e.g., devices, deviceCompliancePolicies, appInstallations) from the Intune database.
Click Load to import the data into Power BI.

Step 4: Build and Customize Reports

Use Power BI’s visualization tools to create reports:
Select fields to display (e.g., device name, compliance state, OS version).
Create visualizations (e.g., pie charts for compliance status, bar charts for app installations).
Use relationships to link tables (e.g., devices to users).
Enable drill-down for detailed analysis (e.g., click on an OS version to see device details).


Example Report: Device Compliance Overview
Fields: Device Name, Compliance State, Last Check-in Time.
Visualization: Pie chart showing compliant vs. non-compliant devices.
Drill-Down: Click a segment to view device details (e.g., serial number, user).


Save the report as a .pbix file.

Step 5: Publish and Share Reports

Click Publish in Power BI Desktop to upload the report to the Power BI service (https://app.powerbi.com).
Configure automatic refresh (if supported) to update the report with new data snapshots.
Share the report with stakeholders via the Power BI service (requires appropriate permissions).

4.3 Example Power BI Report
Report Title: Device Compliance and Enrollment Trends

Data Tables:
devices: Device name, OS version, compliance state, last check-in.
deviceCompliancePolicies: Policy name, compliance status.
enrollmentEvents: Enrollment date, device type.


Visualizations:
Pie Chart: Proportion of compliant vs. non-compliant devices.
Line Chart: Enrollment trends over 60 days.
Table: Detailed device list with sortable columns (device name, OS, compliance state).


Drill-Down: From compliance state to individual device details (e.g., serial number, user).
Filters: Filter by OS (e.g., Windows, iOS), device type, or compliance status.

5. Key Concepts and Best Practices

Permissions: Ensure users have Global Admin, Intune Admin, or read permissions for report access.
Data Retention: Intune Data Warehouse retains data for ~60 days; plan reports accordingly.
Real-Time vs. Historical:
Use Graph API for real-time data.
Use Power BI/Data Warehouse for historical trends.


Report Customization: Leverage Power BI’s flexibility to tailor reports to specific needs (e.g., focus on non-compliant devices).
Performance: The Ibiza console and Power BI handle large datasets efficiently, but optimize queries for complex reports.
Security: Ensure only authorized users access sensitive data (e.g., device or user details).

6. Example Scenarios
Scenario 1: Troubleshooting Non-Compliant Devices

Objective: Identify devices failing compliance checks.
Method: Use an Operational report in the Ibiza console or Power BI.
Steps:
In the Intune portal, go to Reports > Device Compliance.
Filter for Non-Compliant devices.
Export the report or use Power BI to visualize non-compliant devices by policy.
Drill down to identify specific issues (e.g., missing encryption).


Power BI Example:
Table: deviceCompliancePolicies.
Visualization: Bar chart showing non-compliant devices by policy.
Filter: Non-compliant status.
Output: List of devices with details (e.g., device name, user, policy failure reason).



Scenario 2: Monitoring App Installation Success

Objective: Track app deployment success/failure rates.
Method: Use Power BI with the Intune Data Warehouse.
Steps:
Connect to the Data Warehouse and select the appInstallations table.
Create a report with fields: App Name, Installation Status, Device Name.
Visualize using a donut chart for success vs. failure rates.
Publish the report for management review.


Power BI Example:
Table: appInstallations.
Visualization: Donut chart showing success (70%), failure (20%), pending (10%).
Drill-Down: Click on failures to view affected devices and error codes.



7. Conclusion
Intune’s modern reporting framework, combined with Power BI integration, provides a robust solution for monitoring and managing endpoint environments. The Ibiza console offers built-in reports with export and filtering capabilities, while Power BI enables advanced, visual, and customizable reporting using historical data from the Intune Data Warehouse. By leveraging these tools, administrators can gain deep insights into device compliance, app performance, and enrollment trends, ultimately improving IT operations and employee experience.
