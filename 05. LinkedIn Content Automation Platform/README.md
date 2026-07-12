# 📱 LinkedIn Content Automation Platform (with HITL) <a href="./LinkedIn_Content_Automation_Platform.json" download><img src="https://img.shields.io/badge/⬇_Download_JSON-2ea44f?style=flat-square" alt="Download JSON"></a>

![n8n](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google_Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![OpenRouter](https://img.shields.io/badge/OpenRouter-000000?style=for-the-badge&logo=openai&logoColor=white)
![Together AI](https://img.shields.io/badge/Together_AI_(Flux)-0F6FFF?style=for-the-badge&logo=ai&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram_Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
![LinkedIn](https://img.shields.io/badge/LinkedIn_API-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)

> A fully automated, schedule-driven LinkedIn content engine featuring a dual Human-in-the-Loop (HITL) approval system via Telegram. It autonomously generates role-specific posts and contextual AI images, publishing them only after explicit human consent.

<div align="center">
  <img src="../images/linkedin-automation.png" alt="LinkedIn Automation Architecture" width="100%">
  <br>
  <i>n8n Architecture: Dual HITL loops, AI Text/Image generation, and State Management</i>
</div>

---

## 🚨 The Problem
Maintaining a consistent personal brand on LinkedIn requires daily ideation, copywriting, and graphic design. However, fully delegating this to AI is risky—LLMs can hallucinate or generate off-brand content. A system was needed that handles 99% of the heavy lifting while reserving the final 1% (publishing authority) for the user.

## 💡 The Solution (System Architecture)
I designed a sophisticated **Human-in-the-Loop (HITL)** n8n workflow. It runs on a schedule, fetches topics from a database, uses AI to create content and images, and routes them to a Telegram bot for approval before posting.

### 🔄 End-to-End Workflow Execution
1. **Scheduled Trigger & State Check:** 
   - A `Schedule Trigger` initiates the workflow daily. 
   - It fetches the next unposted topic from a **Google Sheet** and passes it to the AI.
2. **Phase 1: Copywriting & Text Approval Loop:** 
   - An **AI Agent (via OpenRouter)** generates a LinkedIn post tailored to a specific user persona provided in the system prompt.
   - A **Telegram Node** sends the generated draft to the user with "Approve" or "Reject" inline buttons.
   - *The Loop:* If rejected, the workflow routes back to the AI Agent to regenerate the post. This loop continues until the user approves the text.
3. **Phase 2: Visual Generation & Image Approval Loop:** 
   - Once the text is approved, a second AI Agent analyzes the post and writes a highly detailed prompt for image generation.
   - The prompt is sent to **Together AI (using the FLUX model)** to render a relatable, high-quality image.
   - The image is sent to the user via Telegram for a second round of approval.
   - *The Loop:* If rejected, it loops back, generates a new image prompt, and renders a new image until approved.
4. **Publishing & Database Update:** 
   - Upon final approval, the text and image are combined and published directly to the user's feed using the **LinkedIn API**.
   - A success notification is sent to Telegram, and the Google Sheet row is updated to mark the topic as "Done" so the schedule picks a new topic the next day.

---

## ⚙️ Key Technical Implementations

| Technology / Logic | Purpose in System |
| :--- | :--- |
| **Human-in-the-Loop (Wait Nodes)** | Utilized n8n's "Wait for Webhook" functionality via Telegram inline keyboards to pause workflow execution until human intervention occurs. |
| **Dual Loop Logic** | Implemented nested looping structures to handle independent re-generation of text and images without restarting the entire workflow. |
| **Model Chaining** | Used OpenRouter LLMs for reasoning/copywriting, and Together.ai (Flux) for high-fidelity text-to-image generation. |
| **State Management** | Used Google Sheets as a lightweight CMS to manage content pipelines and track published topics. |

---

## 🛠️ How to Import and Use This Workflow

1. Download the `LinkedIn_Content_Automation_Platform.json` file from this repository folder.
2. Open your n8n instance -> **Workflows** -> **Add Workflow** -> **Import from File**.
3. **Prerequisites:** 
   - You need a **Telegram Bot Token** (via BotFather).
   - API keys for **OpenRouter** and **Together.ai**.
   - Connected credentials for **Google Sheets** and **LinkedIn**.
