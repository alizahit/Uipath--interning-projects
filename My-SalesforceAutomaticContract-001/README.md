<div align="center">

![UiPath](https://img.shields.io/badge/UiPath-REF-FA4616?style=for-the-badge&logo=uipath&logoColor=white)
![Salesforce](https://img.shields.io/badge/Salesforce-Integration-00A1E0?style=for-the-badge&logo=salesforce&logoColor=white)

</div>

# 🤖 Salesforce Automated Contract Generator & Dispatcher

This project is a robust Robotic Process Automation (RPA) solution developed using the **UiPath Robotic Enterprise (RE) Framework**. It automates the end-to-end process of generating client contracts based on **Salesforce** data, updating the CRM records, and distributing the finalized documents to relevant stakeholders via email.

## 📖 Project Overview

The automation streamlines the contract lifecycle, eliminating manual data entry and ensuring compliance. Key features include:
* **Automated Contract Generation:** Dynamically populates Word/PDF contract templates with Salesforce record data.
* **CRM Synchronization:** Automatically updates Salesforce records (e.g., status changes) and attaches the generated documents.
* **Stakeholder Communication:** Dispatches finalized contracts via automated email to clients and internal teams.
* **Standardized Error Handling:** Leverages the REFramework for resilient processing, logging, and exception management.

## 🚀 How It Works (Process Workflow)

1. **Initialization:** Loads configuration parameters from `Config.xlsx`, initializes Salesforce connections, and sets up the required environment.
2. **Data Retrieval:** Fetches transaction data (e.g., "Closed Won" Opportunities or new Account records) from an Orchestrator Queue or directly from Salesforce.
3. **Contract Processing:** Uses Microsoft Word/Document generation activities to populate contract templates with accurate data (Client Name, Pricing, Terms).
4. **Salesforce Update:** Updates the corresponding Salesforce record to reflect the contract generation and attaches the file to the record.
5. **Distribution:** Formats and sends an email to the client/stakeholders containing the contract as an attachment.

## 🛠️ Workflow Components

### 🏗️ Main.xaml
The core State Machine following REFramework best practices. It manages the high-level transitions between `Initialization`, `Get Transaction Data`, `Process Transaction`, and `End Process`.

### 📝 ContractEdit.xaml
The document processing engine.
* Reads the standardized contract template.
* Replaces placeholders with dynamic Salesforce data.
* Exports the final document for distribution.

### ☁️ SalesforceUpdate.xaml
Handles the communication with Salesforce.
* Locates the processed record.
* Uploads the generated contract document.
* Updates required fields and object statuses to maintain CRM hygiene.

### 📧 sendmail.xaml
The communication module.
* Validates the successful generation of the contract file.
* Uses Mail Activities to send emails with professional formatting and the contract attached.

## 📋 Input/Output Specifications

| Type | Name | Description |
| :--- | :--- | :--- |
| **Input** | `Config.xlsx` | Contains Orchestrator Queue Names, File Paths, and Email configuration. |
| **Input** | `Salesforce Record` | Entity data containing client details, opportunity products, and terms. |
| **Output** | `Contract_{ClientName}.pdf/docx` | The generated, finalized contract document. |
| **Output** | `Email` | Sent to stakeholders/clients with the document attached. |

## ⚙️ Configuration & Best Practices

* **Error Handling:** Wrapped in `Try-Catch` blocks within the REFramework to ensure that system exceptions trigger retries or graceful terminations without partial data commits.
* **Template Management:** Contract templates are externalized, allowing business users to update legal text without modifying the bot's core logic.
* **Scalability:** Designed to handle high volumes of contract requests continuously using Orchestrator Queues.

---
