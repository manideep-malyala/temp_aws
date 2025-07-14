

# AI-Powered Phishing Campaign Simulator for Security Training

### 🔹 Complexity Level: Medium

### 🔹 Reasoning:

Generating realistic phishing content is easy with LLMs. The challenge lies in personalization, image generation, email delivery, and tracking user interactions — making this a highly engaging and educational GenAI hackathon project.

---

## 🎯 Core Concept

Build a dynamic phishing simulation platform that uses generative AI to create **realistic, personalized phishing emails and visual lures**, distributes them to test users, and tracks interaction results to improve cybersecurity awareness.

---

## 🎯 Primary Objective

To **train employees to recognize modern phishing threats** by simulating attacks that are indistinguishable from real-world examples — improving vigilance through immersive, personalized exposure.

---

## ⚠️ Problem Statement

Security awareness programs often rely on outdated, generic templates. As a result:

* Employees can spot *training emails* but still fall for *real attacks*.
* There's limited data on actual user behavior under more realistic threat scenarios.
  This system solves that by **generating believable, adaptive phishing simulations** with real-time feedback and reporting.

---

## 🧠 Multi-Agent System: Roles and Responsibilities

| Agent                            | Role                                                                                                       |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Target Profiler Agent**        | Inputs or retrieves contextual information (department, job role) to personalize email content.            |
| **Email Crafting Agent**         | Uses an LLM (e.g., GPT-4) to generate convincing email subject lines, bodies, and sender identities.       |
| **Visual Lure Agent (Optional)** | Generates fake invoices, login pages, logos, or QR codes to attach to phishing emails.                     |
| **Campaign Reporter Agent**      | Tracks user actions (opens, clicks, reports), aggregates training metrics, and generates feedback reports. |

---

## 🛠 Agent Toolkit: Associated Tools & Technologies

| Agent                   | Tools                                                                                   |
| ----------------------- | --------------------------------------------------------------------------------------- |
| Target Profiler Agent   | CSV/JSON employee datasets, SQLite/Mock DB                                              |
| Email Crafting Agent    | OpenAI GPT-4 / Claude 3, prompt chaining                                                |
| Visual Lure Agent       | QR code libraries (qrcode), image generators (Stable Diffusion API, DALL·E)             |
| Campaign Reporter Agent | Flask + SQLite or Supabase + Mailgun/SendGrid + analytics logging (open, click, report) |

---

## 🧭 Workflow from the User's Perspective

1. A **Security Awareness Officer** starts a new campaign:

   * Target: e.g., "Sales Team – 30 Employees"
   * Topic: e.g., “Q2 Commission Bonus Alert”

2. The system:

   * Crafts **custom phishing email content** with targeted context
   * Optionally adds an **image/attachment** (e.g., fake invoice PDF or login page screenshot)

3. Emails are sent with **unique tracking links**.

4. The Reporter Agent:

   * Logs each **email open**, **link click**, or **report**.
   * Tracks timestamp, IP address, and user agent.

5. At the end of the campaign:

   * The manager sees a real-time dashboard with stats like:

     * 67% opened
     * 42% clicked
     * 21% reported correctly
   * A PDF report is generated for compliance or training feedback.

---

## ⚙️ Technical Deep Dive: Developer’s Architecture

### 🔁 Agent Flow:

```mermaid
graph TD
A[CSV: Target List] --> B[Target Profiler Agent]
B --> C[Email Crafting Agent]
C --> D[Visual Lure Agent]
C --> E[Send Email (unique tracking links)]
E --> F[Campaign Reporter Agent]
F --> G[Final Metrics Report]
```

### 🧪 Sample Prompt for Email Crafting Agent:

```txt
Write a phishing email targeted at a sales executive. Make it appear as an urgent commission adjustment notification from the HR department. Use persuasive language and include a link to "review the new payout report."
```

### 📦 Tracking Link Format:

* Email contains: `https://phishsim.company/report?id=E123`
* Flask backend maps `E123` to a user in the DB and logs the event

---

## 📊 Example Output

**Email Generated (Sales Team):**

> Subject: **URGENT: Q2 Commission Payout Adjustments**
>
> From: **[HR-Payroll@company-hrteam.com](mailto:HR-Payroll@company-hrteam.com)**
>
> Hi John,
> Please review the updated Q2 commission breakdown for your region. Your team has been impacted by new adjustments.
> 👉 \[Review Commission Report]
>
> This must be reviewed by **5 PM today**.

**Campaign Summary:**

```json
{
  "opened": 24,
  "clicked": 14,
  "reported": 6,
  "most_clicked_department": "Sales - West Region"
}
```

---

## 🚀 Hackathon Viability

| Criteria                      | Rating     |
| ----------------------------- | ---------- |
| Creativity                    | 🔥🔥🔥🔥🔥 |
| Educational Impact            | 🔥🔥🔥🔥🔥 |
| Technical Feasibility (MVP)   | 🔥🔥🔥🔥   |
| Scope Control & Extensibility | 🔥🔥🔥🔥🔥 |
| Demo Worthiness               | 🔥🔥🔥🔥🔥 |

---

## 🧪 Data Sources for Testing

| Type                  | Example                                                           |
| --------------------- | ----------------------------------------------------------------- |
| Phishing Templates    | [PhishTank](https://www.phishtank.com/), UCI SpamAssassin dataset |
| Employee Data         | Dummy CSV: `name, email, department`                              |
| Logo Assets           | Public logo APIs or dummy generators                              |
| QR Codes / Fake Sites | Generated locally with `qrcode` and Flask landing pages           |

---

## ➕ Bonus Extensions (Optional)

* **Gamified UI:** Show employees feedback like "You spotted a phishing email! 🎉"
* **Adaptive Learning:** Automatically suggest more training for those who clicked.
* **Risk Scoring Engine:** Assign a risk score per employee based on behavior.

---

