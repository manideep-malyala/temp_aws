

# Security Hoax and FUD Fact-Checker

**Complexity Level:** Low (Beginner)
**Reasoning:** This use case focuses on Retrieval-Augmented Generation (RAG) using a static, trusted knowledge base — making it ideal for LLM reasoning. It avoids complex orchestration, and all components can run within AWS native services, making it perfect for a GenAI-powered beginner-friendly hackathon project.

---

### **Core Concept:**

A user copies and pastes a viral message or “security warning” they received via WhatsApp, Instagram, or email. The system analyzes the claim, checks it against a curated library of known hoaxes, myths, and real news, and tells the user whether the threat is **REAL**, **FAKE**, or **UNCONFIRMED**.

---

### **Primary Objective:**

To reduce the spread of **cybersecurity misinformation and hoaxes** by giving users a simple, AI-driven tool that fact-checks scary messages and alerts — empowering them with real knowledge and reducing unnecessary fear or harmful reactions.

---

### **Problem Statement:**

Fear-based hoaxes (e.g., "Watch out for the ‘Dance of the Pope’ video!") travel faster than facts. These messages:

* Cause unnecessary panic
* Waste time and attention of individuals and IT teams
* Sometimes lead to unsafe actions (e.g., deleting files, forwarding false alarms)
  We need an **accessible, automated tool** that provides **instant clarity** on these messages.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                 | Role                                                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Claim Extraction Agent** | Extracts the core claim from the pasted text and converts it into a short query for search (e.g., `"Dance of the Pope" virus hoax`). |
| **Fact-Checking Agent**    | Performs a simulated semantic search (RAG-style) over a knowledge base stored in S3.                                                 |
| **Verdict Agent**          | Synthesizes the result and delivers a final answer with confidence level, status ("FAKE", "REAL", "UNCONFIRMED"), and citation.      |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose |
| -------------- | ------- |
| **Amazon S3**  |         |

* `S3-HoaxKnowledgeBase/`: Stores a small set of markdown or `.txt` files sourced from trusted websites (Snopes, Hoax-Slayer, etc.)
* `S3-FUDInbox/`: Receives uploaded user messages (JSON/text)
* `S3-FUDResults/`: Stores results from the verdict agent |
  \| **AWS Lambda**         |
* `FUDTriggerLambda`: Triggered on new paste uploads
* It invokes Bedrock with document references and user input |
  \| **AWS Bedrock Agent (Claude 3 / GPT-4o)** |
* Extracts claims
* Matches them semantically to knowledge base text
* Generates a clean, readable verdict and explanation

---

### **Workflow from the User’s Perspective**

1. **Step 1:**
   User receives a viral message:
   *"‼️ URGENT WARNING! Hackers are sending a file called 'Dance of the Pope'. If you open it, your phone will be wiped. Pass this on!"*

2. **Step 2:**
   User copies and pastes the message into the fact-checking tool frontend. The input is saved to `s3://S3-FUDInbox/{timestamp}_hoax.json`.

3. **Step 3:**

   * `FUDTriggerLambda` is invoked.
   * It calls the **Claim Extraction Agent** → Extracts: `"Dance of the Pope" virus hoax`.

4. **Step 4:**

   * Lambda provides the extracted claim and reference to S3-hosted `.md` articles from `S3-HoaxKnowledgeBase/`.
   * **Fact-Checking Agent** performs an in-memory simulated RAG search and returns the best match.

5. **Step 5:**

   * **Verdict Agent** generates the result:

     ```json
     {
       "verdict": "FAKE",
       "confidence_score": 98,
       "explanation": "This message is a hoax that has circulated since 2015. No such virus exists.",
       "source": "snopes.com/dance-of-the-pope-virus"
     }
     ```
   * Final output is saved to `S3-FUDResults/{user_id}_verdict.json`.

---

### **Example Bedrock Agent Prompt**

```txt
You are a cybersecurity hoax detection assistant.

Input:
- User Message: "‼️ URGENT WARNING! Hackers are sending a file called 'Dance of the Pope'..."
- Knowledge base (5 markdown articles on hoaxes)
Task:
1. Extract the core claim being made
2. Search the knowledge base for related hoaxes or true alerts
3. Return a structured verdict:
  - FAKE / REAL / UNCONFIRMED
  - Confidence score
  - Short explanation
  - Source link (if available)
```

---

### **Example Output (Frontend View)**

```
✅ Verdict: FAKE  
🧠 Confidence: 98%  
📘 Explanation: This is a well-documented hoax. No such virus exists.  
🔗 Source: https://www.snopes.com/fact-check/dance-of-the-pope-virus  
```

---

### **Hackathon Viability:** ✅ Very High

* 🧠 Uses simple RAG-style knowledge search
* 💡 Works with small preloaded dataset (no complex DB setup needed)
* 🧱 Entirely implementable with S3, Lambda, and Bedrock
* 📱 Easy to build mobile-friendly frontend or chatbot wrapper
* 🔍 Solves a *real*, relatable problem — helps users and IT teams alike

---

### **Data Sources for Prototyping and Testing**

* **Snopes.com**, **Hoax-Slayer**, **Norton blog**, **Kaspersky blog** – copy 10–15 articles on famous cybersecurity hoaxes into S3 as markdown or `.txt` files
* Collect real viral messages from WhatsApp forwards, Facebook groups, Reddit, or X (formerly Twitter)

---
