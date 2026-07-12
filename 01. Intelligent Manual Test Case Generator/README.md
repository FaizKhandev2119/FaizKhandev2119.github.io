# 🧪 Intelligent Manual Test Case Generator

![n8n](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Gemini_Vision_Flash-8E75B2?style=for-the-badge&logo=google&logoColor=white)
![Mistral OCR](https://img.shields.io/badge/Mistral_OCR-F55036?style=for-the-badge&logo=mistral&logoColor=white)
![Jira](https://img.shields.io/badge/Jira_Software-0052CC?style=for-the-badge&logo=jira&logoColor=white)
![Google Workspace](https://img.shields.io/badge/Google_Sheets_%26_Drive-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)

> An advanced, multi-modal n8n workflow that acts as an AI QA Assistant. It dynamically processes User Inputs, Documents, UI Screenshots, and Jira tickets to auto-generate structured manual test cases and deliver them directly to the user.

<div align="center">
  <img src="../images/testcase-generator.png" alt="Workflow Architecture" width="100%">
  <br>
  <i>n8n Workflow Architecture: Complex branching and Multimodal AI processing</i>
</div>

---

## 🚨 The Problem
Writing manual test cases requires QA engineers to extract context from various scattered sources—reading Jira descriptions, analyzing UI mockups, or parsing PRDs. Consolidating this diverse data into structured test cases (Positive, Negative, Edge) is highly manual, repetitive, and time-consuming.

## 💡 The AI Solution (Workflow Architecture)
I designed a completely automated, end-to-end **n8n pipeline** that standardizes test case creation regardless of the input format. 

### 🔄 How It Works (Step-by-Step)
1. **The Trigger (User Intake):** The workflow begins with an **n8n Form Trigger** where the user inputs their Name, Email, and selects the input format (Text, Docs, Screenshot, or Jira URL).
2. **Dynamic Routing (Switch Node):** Based on the selected input, the workflow routes the data to specific processing branches:
   * **Text:** Directly passed to the AI.
   * **Docs (PDF/Word):** Sent to **Mistral AI** for OCR and text extraction.
   * **UI Screenshots:** Converted to Base64 and sent to **Gemini Vision (3.0 Flash)** with a specific "Tester Persona" prompt to extract every functional UI detail.
   * **Jira Integration:** Extracts the ticket description. *Crucially, if the ticket contains image attachments, it loops through each image, processes it with Gemini Vision, and merges the visual context with the text description.*
3. **The AI Agent (Brain of Junior QA):** All processed context converges into an AI Agent node. The Agent is instructed to generate comprehensive test cases covering Positive, Negative, Functional, and Edge scenarios.
4. **Structured Output & Delivery:** 
   * The AI formats the output into specific data points: `Test Case ID`, `Severity`, `Priority`, `Pre-condition`, `Steps`, and `Expected Result`.
   * The workflow dynamically creates a new **Google Spreadsheet**, populates it with these columns, and shares the file directly with the user via the email provided in the initial form.

---

## ⚙️ Key n8n Nodes & Logic Handled

| Node / Logic | Purpose in Workflow |
| :--- | :--- |
| **n8n Form Trigger** | To capture user details and dynamic input types. |
| **Switch & If Nodes** | To route workflows based on input type and check for Jira attachments. |
| **Loop Node** | To iterate through multiple images attached to a single Jira ticket. |
| **Advanced AI Agent** | To process massive context limits and structure the final QA output. |
| **Google Drive/Sheets Nodes** | To automate file creation, data insertion, and permission sharing. |

---

## 🛠️ How to Import and Use This Workflow

1. Download the `Intelligent Manual Test Case Generator.json` file from this repository folder.
2. Open your n8n instance -> **Workflows** -> **Add Workflow**.
3. Click the `...` menu in the top right -> **Import from File**.
4. Configure your credentials for **Google Workspace**, **Mistral AI**, **Gemini API**, and **Jira Software**.
