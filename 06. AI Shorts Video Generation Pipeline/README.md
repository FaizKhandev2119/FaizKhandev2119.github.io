# 🎬 AI Shorts Video Generation Pipeline (Zero-Cost) <a href="./AI_Shorts_Video_Generation_Pipeline.json" download><img src="https://img.shields.io/badge/⬇_Download_JSON-2ea44f?style=flat-square" alt="Download JSON"></a>

![n8n](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Together AI](https://img.shields.io/badge/Together_AI-0F6FFF?style=for-the-badge&logo=ai&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Gemini_TTS-8E75B2?style=for-the-badge&logo=google&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase_Storage-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![Shotstack](https://img.shields.io/badge/Shotstack-000000?style=for-the-badge&logo=codeigniter&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram_Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)

> A 100% automated, zero-cost video generation pipeline that turns a simple text prompt into a fully edited, 10-scene short video with AI-generated voiceovers, dynamic image scaling (zoom effects), and seamless transitions.

<div align="center">
  <!-- PLACEHOLDER FOR WORKFLOW SCREENSHOT -->
  <img src="../images/video-generation.png" alt="Video Generation Architecture" width="100%">
  <br>
  <i>n8n Architecture: Complex Looping, Base64 Audio Manipulation, and Programmatic Video Editing</i>
</div>

---

## 🚨 The Problem
Creating faceless short videos (for YouTube Shorts, Reels, or TikTok) requires scripting, generating images, recording voiceovers, and manually editing everything together. Existing AI video generators (like InVideo or Pictory) are heavily monetized and expensive. A completely free, API-driven solution was needed.

## 💡 The Solution (System Architecture)
I engineered an end-to-end programmatic video creation engine in **n8n**. It orchestrates multiple free-tier AI models and APIs to write the script, generate assets, calculate audio durations, and render the final video without any human intervention.

### 🔄 End-to-End Workflow Execution
1. **User Intake (Trigger):** 
   - An n8n Form collects the core requirements: `Story Title`, `Language`, and `Animation Style`.
2. **AI Storyboarding (Script to Scenes):** 
   - The input is passed to an AI Story Writer Agent. 
   - The AI generates an engaging short story and perfectly splices it into exactly **10 distinct scenes**.
   - For each scene, it outputs a specific `Image Prompt` and the localized `TTS Text` (in the user's chosen language).
3. **Data Parsing & Looping Engine:** 
   - A **Code Node** splits the JSON array of scenes into distinct items.
   - The workflow enters a powerful `Loop Node` to process each of the 10 scenes individually.
4. **Inside the Loop (Asset Generation):**
   - **Visuals:** The Image Prompt is sent to **Together AI** to generate the scene's artwork. The raw image is uploaded to a **Supabase Storage Bucket**, returning a public download URL.
   - **Audio:** The TTS Text is sent to **Google Gemini** for voice synthesis. 
   - **Data Manipulation:** The returning audio data is converted to Base64 to programmatically calculate the exact audio duration/length. It is then uploaded to a Supabase Bucket for its public URL.
   - **Scene Rendering:** The Image URL, Audio URL, and Audio Length are sent to the **Shotstack API** to merge them into a single video clip, injecting a programmatic "Zoom-In" effect for dynamism.
5. **Final Assembly & Delivery:** 
   - Once the loop completes all 10 clips, they are stitched together with "Fade-Out" transitions to create the master video file.
   - The final render URL is autonomously sent to the user via **Telegram**.

---

## ⚙️ Key Technical Implementations

| Technology / Logic | Purpose in System |
| :--- | :--- |
| **Array Manipulation** | Leveraging n8n Code nodes (JavaScript) to destructure massive AI payloads into iterable objects. |
| **Base64 Processing** | Calculating precise audio durations on-the-fly by parsing raw Base64 data before file conversion. |
| **Cloud Storage Architecture** | Utilizing Supabase Buckets as a temporary CDN to feed raw assets into the Shotstack rendering engine. |
| **Programmatic Video Editing** | Using Shotstack JSON payloads to dictate timelines, zoom effects, and transitions without a GUI. |

---

## 🛠️ How to Import and Use This Workflow

1. Download the `AI_Shorts_Video_Generation_Pipeline.json` file from this repository folder.
2. Open your n8n instance -> **Workflows** -> **Add Workflow** -> **Import from File**.
3. **Prerequisites:** 
   - Connected credentials for **Together AI**, **Google Gemini**, **Shotstack**, and a **Telegram Bot Token**.
   - A properly configured **Supabase Project** with public storage buckets enabled.
