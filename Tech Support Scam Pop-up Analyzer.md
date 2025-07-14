

**Tech Support Scam Pop-up Analyzer**

**Complexity Level:** Medium
**Reasoning:** This use case introduces the image modality using a screenshot, enabling multimodal GenAI analysis. The image must be analyzed for embedded text, fake branding (e.g., Microsoft, Apple), and visual scam patterns. While OCR and visual analysis are straightforward with modern multimodal LLMs (e.g., Claude 3, GPT-4o), orchestrating it through AWS Bedrock Agents with Lambda and S3 offers both scalability and hackathon-friendly architecture.

---

### **Core Concept:**

A user receives a scary, browser-locking pop-up on their computer that claims:
“**Your system is infected! Call Microsoft immediately at 1-800-123-4567!**”
They take a screenshot and upload it to a secure tool that analyzes the image and delivers a clear verdict: scam or safe.

---

### **Primary Objective:**

To protect users—especially non-technical or elderly individuals—from falling victim to pop-up-based tech support scams by providing a simple and immediate AI-powered image analysis.

---

### **Problem Statement:**

Tech support scams are a prevalent form of cyber fraud. Pop-ups often impersonate Microsoft or Apple support, display fake antivirus warnings, and use fear tactics to manipulate victims into calling a fraudulent helpline.
Victims are tricked into:

* Paying money for fake support
* Downloading malware
* Sharing personal/financial data

Since these pop-ups lock the browser or simulate real alerts, they are difficult for users to judge or dismiss without help.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                        | Role                                                                                                           |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Image Text Extractor Agent**    | Uses OCR capabilities to extract all visible text (e.g., phone numbers, warning messages) from the screenshot. |
| **Visual Pattern Detector Agent** | Identifies fake UI elements such as antivirus mimic screens, red alert banners, or impersonated logos.         |
| **Scam Verifier Agent**           | Cross-references extracted elements with known scam templates, numbers, and phrasing patterns.                 |
| **Final Verdict Agent**           | Synthesizes all analysis and produces a user-friendly risk summary and recommendation.                         |

---

### **Agent Toolkit: Associated Tools and Technologies**

⚙️ Only the following AWS services are used to maintain simplicity and hackathon readiness:

| Tool or Technology                        | Description                                                                                                                                    |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon S3**                             | File storage. Screenshot is uploaded to an S3 “inbox” bucket. Result JSON is written to an “outbox” bucket.                                    |
| **AWS Lambda**                            | Event-driven logic that connects S3 and Bedrock. Converts image data, invokes the Bedrock agent, and handles output formatting.                |
| **AWS Bedrock Agent (Claude 3 / GPT-4o)** | Multimodal reasoning: OCR, visual analysis, scam classification, and final report generation—all handled by a single or chained Bedrock Agent. |

---

### **Workflow from the User's Perspective:**

1. **Step 1:** A user encounters a pop-up saying:
   “Your system has been infected. Call Microsoft at 1-800-999-0000 now to unlock your device.”

2. **Step 2:** The user takes a screenshot and uploads it to the online tool.

3. **Step 3:**

   * The image is sent to an **S3 bucket**
   * A **Lambda function** triggers and invokes a **Bedrock Agent**
   * The agent:

     * Extracts the phone number and warning text
     * Recognizes fake antivirus elements (e.g., Windows Defender UI mimic)
     * Matches the phone number to scam patterns
     * Generates a response

4. **Step 4:** The user receives a response (via the web or downloadable result):

   ```
   ⚠️ SCAM DETECTED!
   - Extracted Text: "Your PC is infected! Call Microsoft at 1-800-999-0000"
   - Detected fake antivirus layout and cloned Microsoft logo.
   - This matches known support scam tactics.
   ✅ Advice: Do NOT call the number. Close your browser using Task Manager (Ctrl+Shift+Esc).
   ```

---

### **Hackathon Viability:**

**Very High** ✅

* ⚡ Multimodal reasoning use case (image + language)
* 🧠 Easy to demo: Upload → Analyze → Verdict
* 🧰 Uses only **3 AWS services (S3, Lambda, Bedrock)**
* 👩‍🔬 Valuable in real-world public protection scenarios
* 🧩 Can be extended with very little effort (e.g., browser plugin, dashboard)

---

### **Data Sources for Prototyping and Testing:**

* YouTube videos showcasing scam pop-ups (pause and screenshot)
* Reddit threads with scam screenshots
* Create synthetic scam images using fake alert generators or prompt-based tools (e.g., DALL·E or Leonardo.ai)
* Use real system alerts for negative (safe) test cases

---

