# 🚗 AI Auto-Inspector & Quotation Bot

![n8n](https://img.shields.io/badge/Orchestrator-n8n-ff6e5c?style=flat&logo=n8n)
![LangChain](https://img.shields.io/badge/Agent-LangChain-blue?style=flat&logo=langchain)
![Mistral](https://img.shields.io/badge/Vision-Mistral_AI-yellow?style=flat)
![Gemini](https://img.shields.io/badge/LLM-Google_Gemini-4285F4?style=flat&logo=google)

> **An autonomous AI agent that turns car photos into professional repair quotes.** > *Perception • Reasoning • Action*
---
<img width="1152" height="577" alt="Screenshot 2025-12-16 073526" src="https://github.com/user-attachments/assets/29db98a1-aa28-46c7-949d-69e61fb4e4a5" />



---

## 📖 Overview

This project is a fully automated **AI Service Advisor**. Instead of waiting days for a human inspector, a user can simply send a photo of their damaged car to a Telegram bot.

The system uses **Computer Vision** to detect damage, **Agentic AI** to look up repair costs in a database, and **Automation Tools** to book appointments and generate formal PDF quotations instantly.

It is built on a **Microservices Architecture** using n8n, separating the "Brain" (The Agent) from the "Hands" (Document Generation & Emailing).

---

## ✨ Key Features

* **👀 AI Vision Analysis:** Instantly identifies car parts (e.g., Hood, Bumper) and damage types (e.g., Dent, Scratch, Hail) from raw photos.
* **🧠 Intelligent Conversation:** A LangChain agent negotiates with the user to get missing details (like Car Model or Country) and remembers context.
* **💰 Dynamic Pricing:** Cross-references damage data with a live **Google Sheets** price list (DRS) to calculate accurate repair estimates.
* **📅 Auto-Scheduling:** Checks real-time availability in **Google Calendar** and books repair slots.
* **📄 Instant Documentation:** Generates a branded **PDF Quotation** and sends it via **Email** (Gmail) and **Telegram**.

---

## ⚙️ System Architecture (In-Depth)

The system follows a robotic **Perception-Cognition-Action** loop.

### 1. Perception Layer (The "Eyes")
* **Input:** User uploads a photo to Telegram.
* **Process:** The workflow downloads the binary file and transcodes it to Base64.
* **Analysis:** **Mistral Vision AI** scans the image. It doesn't just "see" a car; it structures the data into JSON:
    ```json
    { "part": "windshield", "issue": "crack", "severity": "high" }
    ```
<img width="1495" height="684" alt="Screenshot 2025-12-15 233843" src="https://github.com/user-attachments/assets/6af8a458-1639-4f3f-8f0f-9fa638912d7f" />

    

### 2. Cognitive Layer (The "Brain")
* **Engine:** **LangChain Agent** (running on Google Gemini/OpenRouter).
* **Logic:** The agent uses a **ReAct Pattern** (Reason + Act).
    * *Observation:* "I see a broken windshield, but I don't know if this is a Sedan or SUV."
    * *Action:* Ask User: "Is your car a Sedan or an SUV?"
    * *Reasoning:* "Now that I have the car type, I will query the pricing database."
* **Memory:** Retains conversation history using the Telegram `chat_id` as a session key.

### 3. Execution Layer (The "Hands")
* **Microservices:** To ensure reliability, heavy tasks are split into separate workflows triggered via **Webhooks**.
    * **PDF Worker:** Takes the data $\to$ Fills HTML Template $\to$ Renders PDF $\to$ Uploads to Telegram.
    * **Email Worker:** Drafts a professional email using AI $\to$ Sends via Gmail API.

<img width="837" height="567" alt="Screenshot 2025-12-16 080118" src="https://github.com/user-attachments/assets/900fe283-9000-455b-a152-ed4290ee9b42" />
<img width="1347" height="577" alt="Screenshot 2025-12-15 233903" src="https://github.com/user-attachments/assets/18ccae40-6137-47da-aee7-38b45a1b90b3" />


---

## 🚀 How It Works (User Flow)

1.  **User:** Sends a photo of a dented car bumper to the Telegram Bot.
2.  **Bot:** "I see a dent on the front bumper. To give you an exact price, is this a Luxury car or Standard?"
3.  **User:** "It's a Luxury car."
4.  **Bot:** *Thinking... (Looks up 'Bumper Dent' + 'Luxury' in Google Sheets)*.
5.  **Bot:** "The estimated repair is **450 AED**. Would you like to book a repair slot?"
6.  **User:** "Yes, next Tuesday."
7.  **Bot:** *Thinking... (Checks Google Calendar, Books Slot)*.
8.  **Bot:** "Booked! I have sent a formal PDF quote to your email and here in the chat."

---

## 🛠️ Tech Stack

* **Workflow Automation:** [n8n](https://n8n.io/)
* **Computer Vision:** Mistral AI (Pixtral/Vision models)
* **Large Language Models:** Google Gemini Flash & OpenRouter
* **Database:** Google Sheets (Pricing/DRS Table)
* **Tools:**
    * Google Calendar (Booking)
    * Gmail (Communication)
    * HTML-to-PDF Service
    * Telegram API (Bot Interface)

---

## 📦 Setup & Installation

### Prerequisites
1.  An instance of **n8n** (Cloud or Self-Hosted).
2.  **API Keys** for: Telegram, Mistral, OpenRouter, Google Cloud (OAuth2 for Sheets/Gmail/Calendar).

### Import Workflows
This project consists of 3 workflow files:
1.  **Main_Agent.json**: The core logic and vision pipeline.
2.  **PDF_Generator.json**: The webhook-based PDF renderer.
3.  **Email_Dispatcher.json**: The email composition tool.

### Configuration
1.  **Google Sheet:** Create a sheet with columns: `part`, `damage_type`, `car_type`, `cost`, `currency`.
2.  **Environment Variables:**
    * Never hardcode API keys! Use n8n Credentials.
    * Update the `Webhook URLs` in the Main Agent to point to your specific n8n instance.

---

## 🔒 Security Note
* **API Keys:** Ensure all API keys are stored in n8n Credentials, not in the JSON nodes.
* **Authentication:** The current workflow includes a "Security Check" node that restricts usage to specific Telegram User IDs. Remove or update this node to allow public access.

---

## 🔮 Future Roadmap
* [ ] **Vector Database:** Replace Google Sheets with Pinecone for faster, semantic pricing lookups.
* [ ] **Voice Mode:** Allow users to send voice notes describing the accident.
* [ ] **Multi-Image Support:** Allow the agent to analyze multiple angles of the car at once.

---

### Author
**Talha Shaikh** *Building the future of automated service.*
