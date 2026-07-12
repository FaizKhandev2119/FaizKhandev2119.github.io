# 🧪 Intelligent Manual Test Case Generator

![n8n](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Gemini_Vision-8E75B2?style=for-the-badge&logo=google&logoColor=white)
![Mistral OCR](https://img.shields.io/badge/Mistral_OCR-F55036?style=for-the-badge&logo=mistral&logoColor=white)
![Google Workspace](https://img.shields.io/badge/Google_Sheets_%26_Gmail-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)

> An intelligent n8n workflow that completely automates the creation of manual test cases by analyzing Jira tickets, screenshots, and PRDs using Vision AI and LLMs.

<div align="center">
  <!-- PLACEHOLDER FOR WORKFLOW SCREENSHOT -->
  <img src="../images/testcase-generator.png" alt="Workflow Architecture" width="90%">
  <br>
  <i>n8n Workflow Architecture</i>
</div>

---

## 🚨 The Problem
Writing comprehensive manual test cases is one of the most time-consuming aspects of the QA lifecycle. QA engineers spend hours reading Jira tickets, extracting requirements, and structuring positive, negative, and edge test cases manually into spreadsheets. This process is highly repetitive, prone to human error, and delays the actual testing phase.

## 💡 The AI Solution
I designed a completely automated **n8n pipeline** that acts as an AI QA Assistant. It takes multiple forms of inputs (Jira tickets, PRD documents, or UI screenshots) and transforms them into highly structured, ready-to-use test cases in minutes.

### ⚡ Key Features
- **Multimodal Input Processing:** Uses **Gemini Vision** to understand UI mockups/screenshots and **Mistral OCR** for text extraction from documents.
- **Jira Integration:** Automatically fetches requirements from Jira Epics and Stories via REST APIs.
- **Comprehensive Coverage:** Instructs the LLM to generate Positive, Negative, Functional, Edge, and Validation test cases.
- **Automated Delivery:** Automatically creates a structured Google Sheet (mapping Severity, Priority, Preconditions, Steps, Expected Results) and emails it to the stakeholders.

---

## ⚙️ n8n Nodes & Tech Stack Used

| Node / Technology | Purpose in Workflow |
| :--- | :--- |
| **Webhook / HTTP Request** | To trigger the workflow and fetch Jira ticket data via Jira REST API. |
| **Google Gemini (Vision)** | To analyze UI mockups and extract visual testing requirements. |
| **Mistral OCR** | To parse and extract text from attached PDF requirements or documents. |
| **Code Node (JavaScript)** | To parse the LLM's JSON output and structure the data into an array of items for iteration. |
| **Google Sheets Node** | To dynamically create a new spreadsheet and append the generated test cases row by row. |
| **Gmail Node** | To notify the QA team and stakeholders with the link to the generated Google Sheet. |

---

## 📈 Business Impact
- **Time Saved:** Reduced manual test design time by **75%**.
- **Efficiency:** Saved approximately **15+ hours per week** for the QA team, allowing them to focus on actual exploratory testing rather than documentation.

---

## 🛠️ How to Import and Use This Workflow

1. **Download the JSON:** Download the `workflow.json` file from this repository folder.
2. **Import into n8n:** 
   - Open your n8n instance.
   - Go to **Workflows** -> **Add Workflow**.
   - Click the `...` menu in the top right -> **Import from File**.
   - Select the downloaded `workflow.json` file.
3. **Configure Credentials:**
   - You will need to add your own credentials for **Google Workspace (Sheets/Gmail)**, **Gemini API**, and **Jira API** to run this successfully.
