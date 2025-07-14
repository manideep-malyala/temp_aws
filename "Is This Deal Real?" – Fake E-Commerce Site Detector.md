
# "Is This Deal Real?" – Fake E-Commerce Site Detector

**Complexity Level:** Medium
**Reasoning:** This use case blends structured data analysis (domain metadata) with visual analysis (UI screenshot). It simulates the real-world work of cyber fraud analysts, using an agentic approach to evaluate site legitimacy from both **technical registration data** and **UI signals**. While the original concept requires WHOIS lookups and browser automation, in a serverless, AWS-native hackathon setup, we simulate or mock those with Lambda inputs and multimodal GenAI reasoning via Bedrock.

---

### **Core Concept:**

A user submits the **URL of an e-commerce website** they’re unsure about. The system:

* Retrieves domain registration metadata
* Captures or accepts a screenshot of the site
* Uses Bedrock Agents to evaluate domain age, visual layout, and language
* Returns a verdict: **Safe**, **Suspicious**, or **High Risk of Scam**

---

### **Primary Objective:**

To protect consumers from fraudulent online stores, especially during high-risk periods (e.g., holidays, flash sales), by automating scam detection based on domain credibility and visual design.

---

### **Problem Statement:**

Scammers launch thousands of fake websites offering unbelievable discounts to lure users into entering credit card details. These stores:

* Are registered very recently (red flag)
* Use generic templates, stock photos, vague contact info
* Vanish in days after scamming victims

Human users struggle to assess credibility by looks alone. An automated assistant can bridge that gap.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                    | Role                                                                                                                                        |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Domain Intelligence Agent** | Analyzes WHOIS-like metadata to assess domain age and trustworthiness.                                                                      |
| **Visual Heuristics Agent**   | Uses multimodal analysis on a website screenshot to spot scam patterns (e.g., generic design, lack of real branding, aggressive discounts). |
| **Verdict Synthesizer Agent** | Combines both metadata and visual findings to provide a human-readable scam likelihood verdict.                                             |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose |
| -------------- | ------- |
| **Amazon S3**  |         |

* Input Bucket: User uploads screenshot and optional `domain_info.json` into `S3-EcomScan-Inbox/`.
* Output Bucket: Results saved to `S3-EcomScan-Results/`. |
  \| **AWS Lambda** |
* `ScanTriggerLambda`: Triggered on new file uploads; coordinates input preprocessing.
* `DomainAnalysisLambda`: (Optional) Simulates WHOIS lookup for domain metadata.
* `ScreenshotValidationLambda`: Validates screenshot format and metadata. |
  \| **AWS Bedrock Agent (Claude 3 / GPT-4o)** |
* Multimodal model that:

  * Analyzes screenshots for scam indicators
  * Interprets domain age or registration data
  * Synthesizes final assessment and renders a user-friendly verdict |

---

### **Workflow from the User’s Perspective**

1. **Step 1:** A user sees a deal on `SuperShoeDeals.store` offering 80% off Air Jordans.

2. **Step 2:**

   * They paste the site URL and/or upload a screenshot to the tool.
   * Optional: A script or client-side Lambda fetches basic domain metadata (simulating WHOIS).
   * Screenshot and metadata are uploaded to: `s3://S3-EcomScan-Inbox/`

3. **Step 3:**

   * Upload triggers `ScanTriggerLambda`, which invokes a **Bedrock Agent** with:

     * Screenshot
     * Domain age (e.g., 5 days)
     * Brand name, UI features, prices, contact info extracted by OCR and vision

4. **Step 4:**

   * The Bedrock Agent performs:

     * Scam pattern detection from image: fake logos, templated layouts, vague CTAs
     * Reasoning based on metadata: recent registration, no business registry
     * Confidence scoring

5. **Step 5:**

   * Output like the following is generated and saved to `s3://S3-EcomScan-Results/{site_id}.json`

---

### **Example Output**

```json
{
  "verdict": "HIGH RISK OF SCAM SITE",
  "confidence_score": 92,
  "signals": {
    "domain_age_days": 5,
    "visual_flags": [
      "No contact address",
      "Suspicious 80% discount",
      "Stock images used for all products",
      "No privacy policy or terms"
    ]
  },
  "recommendation": "Avoid making purchases from this site. This appears to be a fraudulent store set up to steal personal and financial information."
}
```

---

### **Example Agent Prompt**

```txt
You are an e-commerce fraud detection assistant. 
A user uploaded a screenshot of a site and domain metadata.

Step 1: Analyze the image and extract features such as product layout, pricing style, branding, and policy presence.
Step 2: Evaluate domain metadata (e.g., age, registrar country, registrant name).
Step 3: Determine if this resembles a known scam pattern (e.g., recent domain + suspicious UI).
Step 4: Output a JSON object with:
{
  "verdict": "LOW/MEDIUM/HIGH RISK",
  "confidence_score": 0–100,
  "reasons": ["..."],
  "recommendation": "..."
}
```

---

### **Hackathon Viability:**

✅ **Very High**

* 💳 Real-world relevance: Online fraud is common and damaging
* 🧠 Clear application of LLM-based reasoning + image analysis
* 🛠️ Easy to simulate WHOIS and screenshot input without complex backend
* 🌐 Builds awareness: Great for consumer protection and cybersecurity demos
* ✅ Uses only S3 + Lambda + Bedrock = simple and cost-efficient

---

### **Data Sources for Prototyping and Testing**

* Scam lists:

  * `https://www.scamwatcher.com/`
  * Reddit's `r/scams`
  * Public phishing/fake store databases
* Legitimate test cases:

  * Amazon, Nike.com, BestBuy.com screenshots
* Screenshot creation:

  * Use browser screenshot tools or client-side Lambda scripts
* Domain simulation:

  * Use mock JSON inputs like: `{ "domain": "supershoedeals.store", "registered_days_ago": 5 }`

---
