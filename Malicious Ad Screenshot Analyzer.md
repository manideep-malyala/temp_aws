

# Malicious Ad Screenshot Analyzer

## 🎯 Target Audience

* **General Public**
* **Internet Users Concerned with Online Safety**
* **Parents, Seniors, and Non-Technical Individuals**
* **Digital Literacy Training Platforms**

---

## 💡 Core Concept

A user stumbles upon a **suspicious online advertisement**, often called a *malvertisement*—it could falsely claim virus infections, impersonate a brand, or use fake celebrity endorsements. Instead of ignoring it or falling victim, the user takes a **screenshot** and uploads it to an analysis tool.

This tool uses an **AI-powered multimodal agent** to:

1. Read and understand the text in the image
2. Analyze visual elements
3. Detect scam signals
4. Provide an instant, human-readable **risk report**

The tool empowers the public to fight online scams with a simple upload and educates users along the way.

---

## 🏗️ AWS Architecture & Workflow

### 📥 **Trigger (S3)**

* **Bucket**: `S3-Ad-Inbox`
* **User Action**: Uploads a screenshot of the suspicious ad (e.g., `screenshot_ad_001.png`)
* **Trigger**: Upload event initiates a Lambda function (`Ad-Analysis-Lambda`)

---

### ⚙️ **Orchestration (Lambda → Bedrock)**

* **Function**: `Ad-Analysis-Lambda`
* **Flow**:

  1. Triggered by new upload in `S3-Ad-Inbox`
  2. Reads the image file path
  3. Sends the image as input to a **Bedrock Agent**
  4. Receives the analysis result
  5. Stores or displays the result (return via frontend or command-line for MVP)

---

### 🧠 **Agentic Analysis (Bedrock Agent)**

* **Agent Name**: `MalvertisementInspectorAgent`
* **Model Type**: Multimodal (e.g., GPT-4o)
* **Capabilities**:

  * 🧾 **OCR**: Extract all visible text from the image
  * 🖼️ **Visual Analysis**:

    * Detects fake UI (e.g., fake browser alerts, antivirus warnings)
    * Identifies fake brand logos or unauthorized celebrity photos
  * 🔍 **Textual Scam Signal Detection**:

    * Flags phrases like:

      * *"Doctors hate this trick!"*
      * *"Limited-time offer!"*
      * *"Your device is infected!"*
    * Checks for urgency, manipulation, and deception
  * 📋 **Risk Assessment Output**:

    * Scam Likelihood: High / Medium / Low
    * Red Flags: Bullet list of key issues
    * Advice: What to do next (e.g., avoid clicking, report ad, etc.)

---

#### 🧠 Sample Prompt to Bedrock Agent

```
You are an AI assistant that reviews screenshots of suspicious online ads.

Given this uploaded image:
1. Perform OCR and extract all visible text.
2. Identify any scam-indicative language or manipulative messaging.
3. Analyze the visuals for elements like:
   - Fake browser alerts
   - Misused logos
   - Fake celebrity endorsements
4. Summarize the top red flags.
5. Assign a risk score: High / Medium / Low.
6. Give user-friendly feedback on what the user should do.

Respond in plain language for a general audience.
```

---

### 📤 **Response**

* For MVP/hackathon scope: Output returned to frontend or terminal.
* Optional (for full implementation): Results can be sent via:

  * Email (Amazon SES) – *Not used here per scope restriction*
  * Push notifications (Amazon SNS) – *Not used here*

---

## 🔁 Hackathon Viability

| Criteria           | Evaluation                                                     |
| ------------------ | -------------------------------------------------------------- |
| **Relevance**      | ⭐⭐⭐⭐⭐ – Malvertising is a growing threat                       |
| **Feasibility**    | ⭐⭐⭐⭐⭐ – Simple architecture using just S3, Lambda, and Bedrock |
| **Innovation**     | ⭐⭐⭐⭐ – Uses multimodal LLM capabilities creatively             |
| **User Impact**    | ⭐⭐⭐⭐ – Helps everyday people protect themselves                |
| **Demo Potential** | ⭐⭐⭐⭐⭐ – Visual, interactive, and easy to showcase              |

---
