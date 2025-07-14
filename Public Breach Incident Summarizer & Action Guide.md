# Public Breach Incident Summarizer & Action Guide

---

## 🎯 Target Audience

* General public
* Non-technical users
* Digital safety educators
* Journalists and cyber-awareness teams

---

## 💡 Core Concept

When a **major data breach** occurs, users are often flooded with confusing headlines and contradictory advice. This tool provides a **concise, accurate summary** of what happened and a **personalized action checklist** for users to protect themselves.

---

## 🛡️ Primary Objective

To eliminate confusion, panic, and inaction following a breach by giving users:

* A clear, trustworthy summary of the incident
* A personalized, easy-to-follow **response checklist**

---

## ⚠️ Problem Statement

News reports about breaches are often:

* Contradictory
* Lacking in actionable steps
* Full of technical jargon

As a result, affected users don’t know:

* If they are impacted
* What kind of data was stolen
* What steps they should take *right now*

---

## 🧠 The Multi-Agent Team: Roles and Responsibilities

### 📰 1. **News Gathering Agent**

* **Role**: Pulls top articles related to the breach.
* **Task**: Takes the breached entity’s name (e.g., "ShopMart") and queries a news API or search API.
* **Goal**: Get the top 5–10 most relevant and recent sources.

---

### 🧾 2. **Fact Synthesizer Agent**

* **Role**: Extracts consistent facts from multiple articles.
* **Task**:

  * Use an LLM (e.g., Bedrock GPT-4) to:

    * Discard rumors
    * Detect what data was stolen (emails, credit cards, passwords, etc.)
    * Identify the timeline and estimated impact
* **Output**: A **factual, de-duplicated, plain-language breach summary**

---

### ✅ 3. **Action Plan Generator Agent**

* **Role**: Creates a tailored checklist.
* **Task**:

  * Input: Type(s) of data stolen
  * Output: Security best practices via an LLM prompt (e.g., GPT-3.5 or Claude)
  * Example Rules:

    * If **passwords stolen** → Reset password, enable 2FA
    * If **credit card info stolen** → Monitor transactions, consider credit freeze
    * If **email stolen** → Watch for phishing attempts

---

### 📄 4. **Report Assembler Agent**

* **Role**: Final user-facing formatter
* **Task**: Combine:

  * Fact Synthesizer’s summary
  * Action Plan checklist
  * Format as a **clean, readable report** (Markdown/HTML/JSON)

---

## 🧰 Agent Toolkit: Associated Tools and Technologies

| Agent                  | Tool or API                           |
| ---------------------- | ------------------------------------- |
| News Gathering Agent   | NewsAPI, Serper, or GNews API         |
| Fact Synthesizer Agent | Amazon Bedrock (Claude/GPT-4)         |
| Action Plan Generator  | Amazon Bedrock (LLM, few-shot prompt) |
| Report Assembler Agent | Markdown/HTML formatter (or JSON API) |

---

## 👤 Workflow from the User's Perspective

### 🎯 Scenario:

* A user hears that **ShopMart** was hacked and searches “ShopMart” in the tool.

### 🧠 System Response:

```markdown
# 🛡️ The ShopMart Data Breach – What You Need to Know

---

### 🔍 What Happened:
Hackers breached ShopMart's systems between June 18 and July 2, 2025. Reports confirm that attackers gained access to the company's user database.

### 📦 What Was Stolen:
- Names
- Email addresses
- Encrypted passwords

### 🧠 Estimated Impact:
~5.2 million customer accounts affected globally.

---

# ✅ YOUR PERSONAL ACTION PLAN

1. **Change your ShopMart password immediately.**
2. **Change your password on any other site where you used the same one.**
3. **Enable Two-Factor Authentication (2FA) on your ShopMart account.**
4. **Watch out for phishing emails that mention ShopMart.**
5. **Use a password manager to ensure strong, unique passwords.**
```

---

## 🔧 Technical Deep Dive: Developer’s Perspective

### ⚙️ Architecture Flow

```mermaid
flowchart LR
    A[User Enters "ShopMart"] --> B[News Gathering Agent]
    B --> C[Article Corpus]
    C --> D[Fact Synthesizer Agent]
    D --> E[Summary + Stolen Data]
    E --> F[Action Plan Generator Agent]
    F --> G[Checklist]
    D --> H
    F --> H
    H[Report Assembler Agent] --> I[Final Output to User]
```

---

### 💡 Prompt Logic (Example):

**Fact Synthesizer Prompt**:

> “Summarize the key confirmed facts from these articles about the ShopMart breach. Focus only on verifiable details: breach date, stolen data, affected users.”

**Action Plan Prompt**:

> “Generate a 5-step checklist for a user whose encrypted password and email address were stolen in a breach.”

---

## 🧪 Data Sources for Prototyping and Testing

* **NewsAPI / GNews** → real news sources
* Example test breaches:

  * **Equifax (2017)**
  * **LinkedIn (2021)**
  * **Facebook (2021 leak)**
  * **T-Mobile (2023)**

---

## 🚀 Hackathon Viability

| Criteria        | Evaluation                                              |
| --------------- | ------------------------------------------------------- |
| **Relevance**   | ⭐⭐⭐⭐⭐ – Timely & extremely helpful                      |
| **Complexity**  | ⭐⭐ – Beginner-friendly multi-agent flow                 |
| **Use of LLMs** | ⭐⭐⭐⭐⭐ – Ideal for summarization + generation            |
| **Impact**      | ⭐⭐⭐⭐⭐ – Addresses public confusion during crises        |
| **Demo Power**  | ⭐⭐⭐⭐ – Instantly generates clear reports from real news |

---
