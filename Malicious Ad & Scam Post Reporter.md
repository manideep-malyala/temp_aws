
# Malicious Ad & Scam Post Reporter

**Complexity Level:** Medium
**Reasoning:** While post analysis is relatively straightforward with LLMs, the key innovation is **agentic action** — simulating or preparing to report malicious content. Browser automation is outside the AWS-native scope, so we simulate this interaction within Lambda and Bedrock. The outcome is a semi-automated reporter that classifies and recommends an action — or logs it for future human or API-based execution.

---

### **Core Concept:**

A user submits the **URL of a suspicious social media post or advertisement**. A GenAI-based agent analyzes the content for fraud patterns and auto-generates a **simulated report submission** to a social media platform.

---

### **Primary Objective:**

To help the general public and fraud-fighting teams **identify and report scammy social media posts** more easily and effectively by automating risk classification and reporting actions — even if the actual “click-through” reporting is simulated for demo purposes.

---

### **Problem Statement:**

Manual scam reporting on social platforms is:

* Multi-step and inconsistent across platforms
* Discouraging to less technical users
* Too slow to stem the flood of fraudulent content

Automation is needed to **speed up detection and action** — even if only as a support tool or step toward full automation.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                   | Role                                                                                                                                                       |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Post Analyzer Agent**      | Analyzes post content (text, URL, images) for scam indicators using GenAI.                                                                                 |
| **Reporting Strategy Agent** | Determines the appropriate report category (e.g., Fraud, Misinformation, Malware Link).                                                                    |
| **Reporter Simulator Agent** | Logs or simulates a step-by-step “click-through” of the reporting flow for the specific platform. This can later be replaced with real browser automation. |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose |
| -------------- | ------- |
| **Amazon S3**  |         |

* Input: Scam post URL submitted via frontend and stored in `S3-PostReports-Inbox/`
* Output: JSON report and simulated action flow saved to `S3-PostReports-Results/` |
  \| **AWS Lambda** |
* `ReportTriggerLambda`: Invoked when a new post URL is uploaded
* `PostContentFetcherLambda`: (Optional) Simulates text extraction or fetch metadata if scraping is unavailable
* `StrategyLambda`: Invokes Bedrock agent for risk classification and strategy |
  \| **AWS Bedrock Agent (Claude 3 / GPT-4o)** |
* Used for:

  * Scam analysis from post text or preview
  * Reporting category selection
  * Simulated click-through report generation (in narrative or JSON steps) |

---

### **Workflow from the User’s Perspective**

1. **Step 1:** A user sees a suspicious Instagram ad for "SuperFreeiPhones.net" and copies the post link.

2. **Step 2:**

   * User submits the link through the app.
   * The URL and optional screenshot/description are uploaded to `s3://S3-PostReports-Inbox/`

3. **Step 3:**

   * `ReportTriggerLambda` is triggered.
   * Lambda invokes the **Post Analyzer Agent** to:

     * Evaluate risk (based on scam patterns, language, or malicious link)
   * The **Reporting Strategy Agent** determines:

     * Category: e.g., "Scam or Fraud"
   * The **Reporter Simulator Agent** outputs a detailed log:

     ```json
     {
       "action_sequence": [
         "Visit Post URL",
         "Click '...' menu",
         "Click 'Report'",
         "Select reason: Scam or Fraud",
         "Click Submit"
       ],
       "platform": "Instagram",
       "status": "Simulated report ready for human confirmation"
     }
     ```

4. **Step 4:**

   * Final report (verdict + simulation steps) is saved to `s3://S3-PostReports-Results/report_{timestamp}.json`
   * User sees:

     ```
     ✅ SCAM DETECTED!
     Platform: Instagram  
     Risk: High – Malicious ecommerce link, aggressive discount, zero reviews.  
     Suggested Report Category: Scam or Fraud  
     Simulated Action: Report flow created.  
     ```

---

### **Example Bedrock Prompt**

```txt
You are a scam post analysis and reporting assistant.

Input:
- A social media post link and/or description (e.g., "SuperFreeiPhones.net - 100% free phones, only pay shipping")
Task:
1. Analyze the content for scam/fraud/malware signals.
2. Recommend the appropriate report category.
3. Simulate a click-through reporting sequence for platforms like Instagram or Facebook.

Output:
{
  "verdict": "SCAM DETECTED",
  "report_category": "Scam or Fraud",
  "reasoning": [
    "Unrealistic offer: 'Free iPhones'",
    "Uses non-trusted domain",
    "Low credibility design and language"
  ],
  "simulated_steps": [
    "Click menu",
    "Click Report",
    "Select category: Scam",
    "Submit"
  ]
}
```

---

### **Hackathon Viability:**

✅ **High**

* ⚙️ Simulates both **analysis and action** – unique from pure detection tools
* 🧱 100% AWS-native: all agents operate within **S3, Lambda, Bedrock**
* 🧪 No need for real automation – simulate action flow while still building real value
* 🎯 Applicable across multiple platforms: Instagram, Facebook, Twitter/X
* 👏 Makes for an exciting demo with step-by-step scam mitigation

---

### **Data Sources for Prototyping and Testing**

* Use public Instagram/Facebook post links with known scam patterns (e.g., fake giveaways)
* Paste real ad text into test inputs (e.g., “You just won \$1,000, click this link!”)
* Create dummy scam ad examples using common language (e.g., “Pay only shipping for free MacBook”)

---
