

# Threat Actor "Baseball Card" Generator

**Complexity Level:** Low (Beginner)
**Reasoning:** This is a text synthesis and summarization use case using structured, known cyber threat intelligence sources. Since no real-time threat detection is required, it’s ideal for beginner GenAI teams — especially in a hackathon setting. With predefined CTI text files stored in S3, Bedrock Agents can simulate the full flow without relying on external scraping or APIs.

---

### **Core Concept:**

A user enters the name of a well-known APT or threat group (e.g., “APT28” or “Lazarus Group”). The system automatically retrieves reference documents from a curated S3 bucket and uses a Bedrock Agent to generate a compact, one-page **“Baseball Card” summary** with key information: origin, tools, targets, tactics, aliases, and more.

---

### **Primary Objective:**

To help cybersecurity analysts, students, and researchers **quickly understand adversary profiles** — without wading through long MITRE pages or dozens of vendor blogs. The “Baseball Card” serves as a ready-reference snapshot.

---

### **Problem Statement:**

Threat intelligence is scattered across multiple sources:

* MITRE ATT\&CK
* Vendor reports (CrowdStrike, Mandiant, FireEye)
* Government bulletins
  This makes it time-consuming to research a specific APT group. There's a need for **automated summarization** to improve speed and accessibility.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                      | Role                                                                                               |
| ------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Information Retrieval Agent** | Looks up the appropriate CTI documents in S3 based on the group name (e.g., `APT29.md`)            |
| **Data Synthesis Agent**        | Extracts key fields (aliases, targets, tools, techniques) from the document using LLM prompting    |
| **Card Generator Agent**        | Formats the extracted JSON data into a single-page HTML or Markdown "card" for display or download |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose |
| -------------- | ------- |
| **Amazon S3**  |         |

* `S3-CTI-KnowledgeBase/`: Contains `.md` or `.txt` files for top 20–50 APT groups (e.g., `APT28.md`)
* `S3-BaseballCardRequests/`: Stores incoming requests (group name, timestamp)
* `S3-BaseballCardResults/`: Saves generated `card_{group}.html` or `.json` |
  \| **AWS Lambda**         |
* `CardTriggerLambda`: Triggers Bedrock when a new request is submitted
* `RetrievalLambda`: Finds and fetches matching CTI document
* `CardSaverLambda`: Saves the generated HTML card to results bucket |
  \| **AWS Bedrock Agent (Claude 3 / GPT-4o)** |
* Extracts relevant fields from CTI text
* Formats output based on a structured JSON schema
* Renders a final, markdown-based card content (rendered to HTML)

---

### **Workflow from the User’s Perspective**

1. **Step 1:**
   A user types: `APT28` into the “Threat Baseball Card” tool interface.

2. **Step 2:**
   The system logs the request in `S3-BaseballCardRequests/APT28_{timestamp}.json`

3. **Step 3:**

   * `CardTriggerLambda` is invoked.
   * It queries `S3-CTI-KnowledgeBase/APT28.md`
   * The document is passed to Bedrock’s **Data Synthesis Agent**

4. **Step 4:**
   The LLM extracts and formats the result into a structured card:

   ```json
   {
     "name": "APT28",
     "aliases": ["Fancy Bear", "Sofacy"],
     "origin": "Russia (Suspected)",
     "targets": "Governments, Militaries, NATO Defense Contractors",
     "tools": ["X-Agent", "Zebrocy", "Sednit"],
     "ttp": ["Spearphishing", "Credential Dumping", "Exploiting public-facing apps"],
     "source": "MITRE ATT&CK (https://attack.mitre.org/groups/G0007/)"
   }
   ```

5. **Step 5:**
   **Card Generator Agent** formats it into a final `.html` or `.md` file like:

   ```
   NAME: APT28 (aka Fancy Bear, Sofacy)  
   ORIGIN: Russia (Suspected)  
   COMMON TARGETS: Governments, Militaries, NATO Defense Contractors  
   KEY MALWARE: X-Agent, Sednit, Zebrocy  
   COMMON TTPs: Spearphishing, Exploiting public-facing applications, Credential Dumping  
   SOURCE: MITRE ATT&CK | https://attack.mitre.org/groups/G0007/
   ```

6. **Step 6:**
   Final file is saved to `S3-BaseballCardResults/APT28_card.html` and displayed in the frontend.

---

### **Example Bedrock Prompt for Data Synthesis**

```txt
You are a cyber threat intelligence summarizer. Read the following text and extract the following fields:

1. Actor Name  
2. Known Aliases  
3. Suspected Country of Origin  
4. Common Target Sectors  
5. Tools/Malware Used  
6. Common TTPs (Tactics, Techniques, Procedures)  
7. Primary Reference Source URL

Return a JSON object with these fields. Limit each to one short sentence or comma-separated list.
```

---

### **Hackathon Viability:** ✅ Very High

* 📘 Simple input → high-impact, visually pleasing output
* 🧠 Excellent demonstration of LLM synthesis capability
* 🧱 Entire pipeline is **S3 + Lambda + Bedrock** — no external services required
* 🎓 Great for students, educators, and CTI analysts
* 🛠️ Can be expanded into a full database or wiki post-hackathon

---

### **Data Sources for Prototyping and Testing**

Preload `S3-CTI-KnowledgeBase/` with markdown-formatted threat actor reports:

* **MITRE ATT\&CK Group Pages**: Use groups like APT28, APT29, Lazarus Group
* **Vendor Reports**: CrowdStrike, Mandiant threat blogs (summarized into `.md` format)
* Sample files:

  * `APT28.md`
  * `APT29.md`
  * `Lazarus.md`

---

