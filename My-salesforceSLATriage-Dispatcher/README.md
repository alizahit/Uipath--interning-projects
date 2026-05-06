<div align="center">

![UiPath](https://img.shields.io/badge/UiPath-Dispatcher-FA4616?style=for-the-badge&logo=uipath&logoColor=white)
![Salesforce](https://img.shields.io/badge/Salesforce-Integration-00A1E0?style=for-the-badge&logo=salesforce&logoColor=white)

</div>

# 🚀 Salesforce SLA Triage - Dispatcher

This project is the **Dispatcher** component of the Salesforce SLA Triage automation, built using the UiPath Robotic Enterprise Framework (REFramework). 

## 📖 Project Overview

**Overall Project Purpose:**  
The **Salesforce SLA Triage** automation is a single, unified RPA solution split into two components: a Dispatcher and a Performer. The overarching goal of this automation is to systematically evaluate incoming Salesforce Cases against Service Level Agreement (SLA) business rules. It automatically identifies and escalates critical or high-value customer issues (based on revenue, priority, or keywords) to the appropriate teams, ensuring rapid response times and maintaining high customer satisfaction.

**Dispatcher Role:**  
This specific project acts as the **Dispatcher** component of the unified solution. Its primary responsibility is to extract new or updated Salesforce case data and populate a UiPath Orchestrator Queue. It acts as the data-gathering engine, ensuring that all relevant cases are queued up for the Performer bot to evaluate and process.

## 🛠️ Workflow Components

### 📥 `addQueueTemplate.xaml`
* Receives a data table containing Salesforce cases (`in_dt_veriler`).
* Iterates through each record.
* Uploads each case to the configured Orchestrator Queue using `Add Queue Item`.
* Uses the Salesforce `CaseID` as the unique Reference for the queue item.

### 🧹 `clearQueueTemplate.xaml`
* Utility workflow designed to manage and clear the Orchestrator queue when necessary.

## ⚙️ How It Works
1. Data is gathered into a DataTable format.
2. The `addQueueTemplate` workflow loops through all records.
3. Items are systematically added to the Orchestrator Queue, preparing them for the SLA evaluation logic handled by the Performer bot.
