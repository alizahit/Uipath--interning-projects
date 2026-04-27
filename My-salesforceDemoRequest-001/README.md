<div align="center">

![UiPath](https://img.shields.io/badge/UiPath-REFramework-FA4616?style=for-the-badge&logo=uipath&logoColor=white)
![Salesforce](https://img.shields.io/badge/Salesforce-API_Integration-00A1E0?style=for-the-badge&logo=salesforce&logoColor=white)
![Data Management](https://img.shields.io/badge/Data_Management-Scoring-007396?style=for-the-badge)

</div>

# 🤖 Entegra Bilişim - Salesforce Lead Qualification and Assignment Automation

This project is an enterprise **Robotic Process Automation (RPA)** solution that automates the process of reviewing new potential customer (Lead) records reaching the Entegra Bilişim A.Ş. sales department, scoring them according to business rules, and assigning them to the appropriate sales representative without human intervention.

## 1. Project Overview
This automation process is designed to enable sales teams to reach potential customers faster, more systematically, and with the correct strategy. System interactions are carried out end-to-end via the **Salesforce API** to eliminate the slowness and fragility risks brought by user interface (UI) automation. It operates as a fast, reliable, and highly scalable background process (Background Process).

## 2. Technical Architecture
The process is structured over the industry-standard **UiPath REFramework (Robotic Enterprise Framework)** and the **Single Process Queue** (a combination of Dispatcher and Performer structures within the same flow) architecture.

1. **Init (Data Collection):** Executes a SOQL query directly across the Salesforce database to retrieve leads in the relevant status and uploads them to the Orchestrator Queue, preventing duplicate records (duplicate checks).
2. **Process (Processing and Assignment):** Retrieves queue items (Queue Items) in a transactional structure, scores them via a designated "Decision Tree", and updates the record backwards in Salesforce via API.
3. **End (Reporting):** Upon workflow completion, an analysis is performed regarding queue data in memory, reporting it by email.

## 3. Workflow Components

* **`Main.xaml`**: The main backbone of the REFramework. Manages the control of Initialization, Get Transaction Data, Process, and End Process states, as well as Exception Handling and Retry algorithms.
* **`clearQueue.xaml`**: Called before the process begins. Prepares a secure and clean environment (`Clean Slate`) by removing pending items in "New" status remaining from previous runs from the UiPath Orchestrator queue.
* **`serachSOQL.xaml`**: Executes the logical query `Status = 'New' OR Status = 'Open - Not Contacted'` to extract data meeting the specified criteria. Returns the results as a structured list object (`curated_soqlQuery_List`).
* **`addQueue.xaml`**: Loops through the SOQL results and loads them into the Orchestrator queue, making each Lead ID a unique reference (`Reference`) number. Catches possible Duplicate Reference exceptions and ignores them to ensure the process continues without breaking.
* **`LeadScoring.xaml`**: The rule engine where business rules are executed (Scoring). Makes mathematical decisions based on customer data:
    * **🔥 Hot Lead:** Annual Revenue > 5 Billion OR Number of Employees > 10,000 -> `Salesforce_OwnerID1`
    * **🌤️ Warm Lead:** Annual Revenue >= 1 Billion OR Lead Source contains "Show" -> `Salesforce_OwnerID2`
    * **🧊 Cold Lead:** All other cases -> `Salesforce_OwnerID3`
* **`Update.xaml`**: Uses the Rating and OwnerID values ​​coming from the Scoring step to update the record status on Salesforce to "Working - Contacted" and assigns the record to the respective representative.
* **`report.xaml`**: Activates at the "End Process" stage. Retrieves successfully and unsuccessfully processed queue items directly over the Orchestrator API and counts them effectively with `LINQ` queries. Processes the transaction result into a dynamic report using an HTML template and forwards it to process owners as an email.

## 4. Input/Output Specifications

* **Input:** SOQL Data extracted from Salesforce.
    * *Used Fields:* `ID`, `FirstName`, `LastName`, `AnnualRevenue`, `NumberOfEmployees`, `LeadSource`, `Status`, `Rating`
* **In-Process Carrier (Processing):** UiPath Orchestrator Queue Item (Specific contents are carried structurally similar to JSON within the Item Information section).
* **Output:** 
    1. Customer records assigned to the respective OwnerID and updated on the Salesforce side.
    2. An HTML-based email notification report containing operational success rates (`RPA-01 Lead Qualification Report - Process Completed`).

## 5. Configuration
All environment variables and rule limits are fed dynamically over `Data/Config.xlsx`. No codebase intervention is required in case of any changes:
* **Queue Configuration:** `OrchestratorQueueName` (Queue Name), `OrchestratorQueueFolder` (Queue Folder).
* **Salesforce Assignment Roles:**
    * `Salesforce_OwnerID1` (Hot Leads - Sales Director)
    * `Salesforce_OwnerID2` (Warm Leads - Senior Representative)
    * `Salesforce_OwnerID3` (Cold Leads - Standard Representative)
* **Reporting & SMTP Settings:** `server`, `port`, `fromMail`, `toMail` and password (`pswd`) details. The `.data/HtmlContent0.html` template is actively used for email body customizations.
