Here is a detailed briefing document reviewing the main themes and most important ideas or facts from the provided sources about Microsoft Graph, Graph Explorer, and their integration with Intune and PowerShell.

# Briefing Document: Microsoft Graph, Graph Explorer, and Intune Integration

**Source:** Excerpts from "#IntuneNugget 8- Microsoft Graphs+Powershell Integration with Intune |Level 200" and "Microsoft Graph API + PowerShell Integration with Intune session notes"

**Overview:**
This document summarizes a tutorial focused on understanding and utilizing Microsoft Graph Explorer and integrating Microsoft Graph API calls with Windows PowerShell to manage Microsoft Intune. The tutorial aims to explain what APIs and Graph Explorer are, why they are useful, and how they can be leveraged, particularly for automation and troubleshooting when the traditional Azure portal (Ibiza console) is unavailable or displaying incorrect information.

## Key Themes:

* **Understanding APIs and Microsoft Graph**: The core concept of an API as an interface for data exchange and Microsoft Graph as a specific layer or "fence" for accessing data from various Microsoft cloud services.
* **Microsoft Graph Explorer**: A tool for testing and working with Microsoft Graph APIs, allowing users to visualize API calls and responses.
* **Integration with Intune**: How Microsoft Graph is the underlying technology powering the Azure portal's interaction with Intune data and how Graph Explorer can be used to directly interact with Intune data.
* **PowerShell Integration**: The ability to make Microsoft Graph API calls programmatically using Windows PowerShell for automation and scripting purposes.
* **Use Cases and Practical Applications**: Demonstrating real-world scenarios where Graph Explorer and PowerShell integration are valuable, such as troubleshooting portal display issues and automating administrative tasks.

## Most Important Ideas and Facts:

### 1. Understanding APIs & Microsoft Graph Explorer

* **What is an API?**
    * API (Application Programming Interface) acts as a "window" or "hole in the wall" or a "bridge" that allows for the exchange of data between different applications or services.
    * It allows us to fetch, modify, and manage data programmatically.
    * Example: If you need to access Intune data without using the UI, an API call can retrieve the information directly.
    * Example Quote: "so API stands for application programming interface so let me give you an analogy guys and probably that should help us understand what is an API... in this analogy the window in the wall is actually an API"

* **What is Microsoft Graph?**
    * Microsoft Graph is the API gateway to interact with Microsoft 365 services, including Intune, Azure AD, Teams, Exchange, and SharePoint.
    * It is a RESTful API that allows data retrieval and management.
    * Microsoft Graph acts as a layer or "fence" between applications (like Intune) and services, serving as the primary medium for making REST API calls to access and modify data across various Microsoft cloud services.
    * Graph API supports GET, POST, PATCH, and DELETE operations.
    * Example Quote: "so graph in technical terms is a layer which is sitting between the applications and the services"
    * Example Quote: "so graph or the fence is the medium via which we are making the API calls"

* **Microsoft Graph Explorer**
    * A web-based tool to test and execute API calls.
    * URL: `https://developer.microsoft.com/en-us/graph/graph-explorer` (also referred to as `ak.ms/ge`).
    * Allows you to authenticate, fetch, and modify Microsoft 365 data.
    * Example Quote: "graph Explorer in technical term is a tool which we can use to test and work with the api's"
    * Example Quote: "all we have to do is we have to open up any browser and over there we have to type the URL a K dot M s / g e"

### 2. Making API Calls with Graph Explorer

* **Authentication and Authorization are Required**: To access data via Microsoft Graph, authentication and authorization are necessary. This involves obtaining an access token (like a JSON Web Token - JWT).
    * Example Quote: "so when we go via that graph the first thing that we need to do is authentication and authorization"
    * Example Quote: "I just I just want you to have a roundabout view of this diagram when I'm talking about authenticate and authorize I'm actually going to show it in the demo that how exactly the authentication or authorization happens what token we actually get how that token actually looks and without that token how and why we are not able to access the data"
* **Steps to Make API Calls in Graph Explorer**:
    1.  Go to Graph Explorer and sign in.
    2.  Modify API Permissions: Click Modify Permissions. Grant permissions for Intune device management.
    3.  Run Queries:
        * Fetch user details: `https://graph.microsoft.com/v1.0/me`
        * List all devices: `https://graph.microsoft.com/v1.0/deviceManagement/managedDevices`

### 3. Understanding REST API Calls & Querying Data

* **REST API (Representational State Transfer)**: Allows interaction with web services.
* **Graph API calls return data in JSON format.** OData (Open Data Protocol) is also mentioned as a protocol used for querying data.
    * Example Quote: "whatever data that we are getting that data is in the JSON format"
    * Example Quote: "OData is actually a protocol it stands for open data protocol"
* **Graph API Call Syntax**: The basic syntax for a Graph API call is `https://graph.microsoft.com/{version}/{resource}/{id}/{property}?{query_parameters}`.
    * Common versions are `v1.0` (recommended, stable) and `beta` (preview, subject to change).
    * Example Quote: "HTTP colon slash slash graph dot Microsoft comm this would always remain the same and then depending upon the use case we'll have to put an aversion we'll have to put in a resource we'll have to put in an ID and then we'll have to put in a property and then we can query that property"
* **HTTP Verbs (Methods)**: Different actions are performed using different HTTP verbs:
    * **GET**: Retrieving data. Example: `GET https://graph.microsoft.com/v1.0/deviceManagement/managedDevices`
    * **POST**: Creating new data (e.g., a new policy).
    * **PATCH**: Modifying existing data (e.g., editing a policy).
    * **DELETE**: Removing objects.
    * Example Quote: "so these are just one of the kinds of commands that we can actually pass and then modify or play with the data... We refer to them as HTTP verbs"
* **Using Query Parameters in API Calls**: Parameters like `$select`, `$filter`, and `$orderBy` can be used in GET requests to control which properties are returned, filter results based on criteria, and sort the output.
    * Example: Filter devices based on OS version: `GET https://graph.microsoft.com/v1.0/deviceManagement/managedDevices?$filter=operatingSystem eq 'Windows'`
    * Example Quote: "we are formatting the output that we are going to get that is something which we do by using this query parameter"

### 4. Integration with Intune & Use Cases

* **Azure Portal (Ibiza Console) Uses Graph API Calls**: The tutorial emphasizes that the actions performed within the Azure portal (Ibiza console, `portal.azure.com`) for Intune management are, in the background, making Microsoft Graph API calls.
    * Demonstrations using the browser's Developer tab (F12) show that clicking on Intune blades triggers GET and POST Graph API calls.
    * Example Quote: "I also want you guys to know that when we are clicking on any blade in the portal it is in the background actually making a graph call it is in the background actually making a REST API call"
* **Graph Explorer for Troubleshooting Portal Issues**: Graph Explorer is a valuable tool when the Azure portal is experiencing issues (e.g., not responding, displaying incorrect data). By making direct Graph API calls, users can verify the data stored in the backend service.
    * Example Quote: "the use case is let's just say that for some reason the Ibiza console is not working or the Ibiza console is giving us a 4 0 4 error okay what do we do then... We can make a REST API call in the graph Explorer and then see what's the data it is returning"
* **Graph Explorer is for Ibiza, Not Silverlight**: Graph Explorer was designed to interact with the data exposed through the Azure portal (Ibiza) and cannot retrieve information for devices managed solely through the older Silverlight console.
    * Example Quote: "graph explorer is actually meant for the ibiza console and not for the silverlight console"

### 5. Automating Intune Tasks with PowerShell & Graph API

* **PowerShell Integration for Automation**: While Graph Explorer is for individual API calls, PowerShell, specifically using the `Invoke-RestMethod` cmdlet, can be used to automate Graph API calls. This is essential for performing bulk operations (e.g., creating hundreds of policies).
    * Example Quote: "so in graphics via graph explorer we is a medium as I said via that we are making rest API calls but over here we can make only one call at a time we cannot automate it... we will be seeing how exactly we can automate that as well using PowerShell"
    * Example Quote: "now we are going to do this part now I am going to do this integrating API with Windows PowerShell"

* **Step 1: Install PowerShell Modules for Microsoft Graph**
    * Install the `Microsoft.Graph` module (and `Microsoft.Graph.Intune` specifically for Intune-related commandlets) and the Azure AD module (`AzureAD`) to leverage specific Intune-related commandlets and handle authentication.
        ```powershell
        Install-Module Microsoft.Graph -Scope CurrentUser
        Import-Module Microsoft.Graph
        ```
    * Example Quote: "for managing in tune via PowerShell we need a Intune module"
    * Example Quote: "we will have to install the azure ad module as well"

* **Step 2: Authenticate to Microsoft Graph**
    * Generate an Auth Token for PowerShell API Calls: To make Graph API calls from PowerShell using `Invoke-RestMethod`, a valid authentication token is required. This token can be generated using scripts provided by Microsoft or other methods.
    * Connect to Microsoft Graph with Intune management permissions:
        ```powershell
        Connect-MgGraph -Scopes "DeviceManagementConfiguration.ReadWrite.All"
        ```
    * Example Quote: "to integrate my window I have to integrate my API with Windows PowerShell the first thing that I need is an ad the first thing that I need guys here is an ad token"
    * Example Quote: "this auth token is very important once we have this auth token after that using that auth token is we are going to do that authorization and authentication and only then we will be able to make use of the fence... and then do the rest API calls"

* **Step 3: Fetch Intune Device Information**
    * Using `Get-MgDeviceManagementManagedDevice`:
        ```powershell
        Get-MgDeviceManagementManagedDevice | Select-Object DeviceName, OperatingSystem, ComplianceState
        ```

* **Step 4: Create an Intune Policy via PowerShell**
    * Using `New-MgDeviceManagementDeviceCompliancePolicy`:
        ```powershell
        $policy = @{
            "displayName" = "Windows 10 Compliance Policy";
            "description" = "Policy for Windows 10 devices";
            "@odata.type" = "#microsoft.graph.deviceCompliancePolicy";
        }
        New-MgDeviceManagementDeviceCompliancePolicy -BodyParameter $policy
        ```

* **Step 5: Delete a Device from Intune**
    * Using `Remove-MgDeviceManagementManagedDevice`:
        ```powershell
        Remove-MgDeviceManagementManagedDevice -ManagedDeviceId "<Device_ID>"
        ```

* **Using Invoke-RestMethod in PowerShell**: The `Invoke-RestMethod` cmdlet is the core command for making REST API calls from PowerShell. It requires specifying the URI (the Graph endpoint), the HTTP method (e.g., GET, POST), and potentially a request body for methods like POST and PATCH. Authentication headers are also necessary, including the generated auth token.
    * Example Quote: "the command so now I'm going to do the actual thing... The command is invoke - rest method"

### 6. Live Demo & Use Cases

* Fetching Intune Device Data with PowerShell
* Creating & Updating Device Configuration Policies
* Automating Bulk Device Management Tasks
* Using PowerShell Scripts for Automation

### 7. Summary

The tutorial effectively demystifies the underlying technology powering Intune management in the Azure portal by explaining Microsoft Graph and its API calls. It introduces Graph Explorer as a direct way to interact with this data for troubleshooting and exploration. Crucially, it highlights the significant advantage of integrating Graph API calls with Windows PowerShell, enabling automation of Intune administrative tasks that would be time-consuming or impossible through the portal or Graph Explorer alone. The process involves understanding the API syntax, obtaining necessary authentication tokens, and utilizing the `Invoke-RestMethod` cmdlet in PowerShell. Microsoft Graph API and PowerShell enable automation for Intune device management. By leveraging API calls, we can efficiently manage devices, users, and policies without relying on the UI.
