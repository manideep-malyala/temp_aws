

# SOC ChatOps Security Assistant

**Complexity Level:** Low
**Reasoning:** This use case leverages a conversational interface — a natural strength of LLMs — while keeping backend operations simple. Using Bedrock Agents for routing, reasoning, and action coordination makes it highly modular and manageable. Relying on AWS-native services ensures security and simplifies integration.

---

### **Core Concept**

A conversational assistant that lives within a web-based SOC dashboard or internal tool (e.g., built on a React/Streamlit UI). SOC analysts can type natural language questions or commands like "Check IP 123.45.67.89" or "Summarize Alert A123" into the tool. Bedrock Agents orchestrated via Lambda handle intent recognition, data access, and task execution — reducing context-switching and accelerating security operations.

---

### **Primary Objective**

To empower SOC analysts with a unified natural language interface to their internal datasets (e.g., alerts, IP reputation logs, and SOP documents), increasing speed and efficiency without switching between multiple consoles or tools.

---

### **Problem Statement**

Analysts investigating alerts must juggle logs, reputation data, SOPs, and case history — often across separate platforms. This leads to:

* Slower MTTR (Mean Time to Respond)
* Fragmented workflows
* Analyst fatigue

An in-context chat assistant reduces friction and centralizes tasks in one place.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name               | Role                                                                                                            |
| ------------------------ | --------------------------------------------------------------------------------------------------------------- |
| **Query Router Agent**   | Understands user input and routes it to the right agent. Classifies intent: `Retrieve`, `Action`, or `Explain`. |
| **Data Retrieval Agent** | Retrieves internal threat data or log excerpts from S3.                                                         |
| **Action Agent**         | Simulates security actions (e.g., blocking IP) and logs them securely.                                          |
| **Knowledge Agent**      | Performs Retrieval-Augmented Generation (RAG) over internal SOPs stored in S3.                                  |

---

### **Agent Toolkit: Associated AWS Services**

| AWS Service   | Role |
| ------------- | ---- |
| **Amazon S3** |      |

* `S3-SOC-Logs/`: Structured JSON files containing alerts, reputation logs, and simulated threat data
* `S3-SOC-KnowledgeBase/`: Markdown/text documents representing SOPs and IR playbooks
* `S3-SOC-Conversations/`: Stores chat history and action audit logs |
  \| **AWS Lambda**       |
* `ChatRouterLambda`: Triggers the correct Bedrock Agent based on user message intent
* `LogRetrievalLambda`: Retrieves IP or alert data from `S3-SOC-Logs/`
* `ActionLoggerLambda`: Logs requested actions to `S3-SOC-Conversations/`
* `RAGRetrieverLambda`: Pulls contextually relevant documents from the knowledge base |
  \| **AWS Bedrock Agents** |
* Use Claude 3 or GPT-4o to handle:

  * Intent detection
  * Structured reasoning
  * Query composition
  * Multi-document summarization |

---

### **Workflow from the User’s Perspective**

1. **User Input:**
   Analyst types into the SOC UI:
   `@SecBot what is the reputation of IP 123.45.67.89?`

2. **Routing:**
   `ChatRouterLambda` passes the query to Bedrock's **Query Router Agent**, which classifies:

   ```json
   {
     "intent": "retrieve",
     "entity_type": "ip_address",
     "entity_value": "123.45.67.89"
   }
   ```

3. **Retrieval:**
   The **Data Retrieval Agent** (via `LogRetrievalLambda`) scans the `S3-SOC-Logs/ip_reputation.json` file and finds:

   ```json
   {
     "ip": "123.45.67.89",
     "score": "High Risk",
     "flags": ["Botnet", "Known Malware Host"],
     "sources": 14
   }
   ```

4. **Bedrock Response:**
   Bedrock formats the response:

   > **🚩 High Risk IP Detected**
   > IP `123.45.67.89` is flagged by 14 sources for malware and botnet activity.

   > Would you like to initiate a simulated block of this IP and log the action? (Yes/No)

5. **User Confirms:**
   Analyst types: `Yes`

6. **Action Logging:**
   `ActionLoggerLambda` logs the block request to `S3-SOC-Conversations/IR-2201-action.json`:

   ```json
   {
     "action": "block_ip",
     "ip": "123.45.67.89",
     "requested_by": "analystA",
     "timestamp": "2025-07-14T15:15:00Z",
     "status": "simulated"
   }
   ```

7. **Confirmation:**
   Bedrock replies:

   > ✅ Action Logged: Simulated block of IP `123.45.67.89` recorded. Incident ID: `IR-2201`.

---

### **Sample Input: General Knowledge Request**

> `@SecBot what are the containment steps for ransomware attacks?`

The **Knowledge Agent** performs RAG over documents in `S3-SOC-KnowledgeBase/` and replies:

> 📘 **Ransomware Containment Steps** (from NIST-Playbook.md):
>
> 1. Isolate affected endpoints
> 2. Disable network sharing
> 3. Capture memory dumps for forensic analysis
> 4. Notify incident response team

---

### **Hackathon Viability: ✅ Very High**

* 🗣️ Natural language interface is engaging and demo-friendly
* 🧱 Clean architecture: S3 → Lambda → Bedrock → UI
* 🔄 Modular: Start with basic retrieval, then add actions and SOP summarization
* 🔐 No real security tools required — all logs and actions simulated in S3

---

### **Data Sources for Prototyping and Testing**

* **Logs & IP Reputations:** Create mock files like:

  * `S3-SOC-Logs/ip_reputation.json`
  * `S3-SOC-Logs/alerts.json`
* **SOPs & Playbooks:**

  * Upload content from NIST SP 800-61 or internal SOPs as `.md` or `.txt` to `S3-SOC-KnowledgeBase/`
* **Simulated Conversations:**

  * Store all agent outputs in `S3-SOC-Conversations/` for audit and demo purposes

---
