<div align="center">

![UiPath](https://img.shields.io/badge/UiPath-REFramework-FA4616?style=for-the-badge&logo=uipath&logoColor=white)
![Salesforce](https://img.shields.io/badge/Salesforce-Integration-00A1E0?style=for-the-badge&logo=salesforce&logoColor=white)
![LinkedIn](https://img.shields.io/badge/LinkedIn-Data_Extraction-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)

</div>

# 🚀 LinkedIn to Salesforce Contact Sync

This project is a Robotic Process Automation (RPA) solution built on the **UiPath Robotic Enterprise Framework (REFramework)**. It automates the extraction of LinkedIn connections, cross-references them against existing Salesforce Contacts, and automatically adds any missing connections into Salesforce as new Contacts.

## 📖 Project Overview

The main objective of this automation is to seamlessly synchronize a user's professional LinkedIn network with their Salesforce CRM. By automatically identifying and adding new connections, the bot eliminates manual data entry, prevents duplicates, and ensures the CRM is always up-to-date with the latest networking data.

## 🛠️ Workflow Components

### 🔄 Initialization (`z_init`)
* **`clearQueue.xaml`**: Clears any residual items in the Orchestrator Queue to ensure a clean slate.
* **`GetAccountList.xaml`**: Connects to Salesforce to retrieve the current list of existing Contacts/Accounts to establish a baseline for comparison.
* **`ExtractLinkedIn.xaml`**: Automates the browser to extract connection data directly from LinkedIn.
* **`addQueue.xaml`**: Compares the extracted LinkedIn data against the existing Salesforce list and adds only the **new, unrecorded connections** to the Orchestrator Queue.

### ⚙️ Processing (`z_process`)
* **`addAccount.xaml`**: Consumes items from the Orchestrator Queue. For each new connection, it integrates with Salesforce to create a new Contact/Account record with the corresponding details.

### 🏁 Finalization (`z_end`)
* **`report.xaml`**: Generates a final execution report summarizing how many new contacts were successfully added and any exceptions encountered during the run.

## 🚀 How It Works
1. **Scraping:** The bot extracts the user's connection list from LinkedIn.
2. **Reconciliation:** It fetches existing Salesforce Contacts and cross-references the two lists.
3. **Queuing:** Missing connections are dispatched to the UiPath Orchestrator Queue as transaction items.
4. **Data Entry:** The bot processes the queue, automatically inserting each new contact into Salesforce.
5. **Reporting:** A final summary report is generated upon completion.

---
*Developed as part of RPA internship focused on scalable enterprise automation and CRM integration.*
