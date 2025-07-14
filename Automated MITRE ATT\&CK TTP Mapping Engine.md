

# Automated MITRE ATT\&CK TTP Mapping Engine

### 🔹 Complexity Level: Medium

### 🔹 Reasoning:

This use case blends **semantic understanding** with **structured threat intelligence**. It leverages LLMs, embeddings, and vector databases to automate the mapping of free-text threat reports to ATT\&CK TTPs — a high-value, real-world SOC application.

---

## 🎯 Core Concept

The system ingests **unstructured cybersecurity narratives** (e.g., CTI reports, malware analyses, SIEM alerts), interprets adversary behaviors, and **automatically maps them to relevant MITRE ATT\&CK techniques (TTPs)** using LLMs and vector similarity search.

---

## 🎯 Primary Objective

To **accelerate threat understanding and profiling** by reducing the manual effort required to align observed behavior with the ATT\&CK framework — enabling **faster, standardized, and more actionable intelligence** in incident response.

---

## ⚠️ Problem Statement

Mapping observed cyber activity (e.g., “PowerShell used to download a payload”) to ATT\&CK techniques (e.g., `T1059.001`) is **time-consuming, inconsistent**, and relies on deep human expertise. Automating this process:

* Saves time during threat triage.
* Enables consistent documentation and reporting.
* Improves red/blue team communication.

---

## 🧠 Multi-Agent Team: Roles and Responsibilities

| Agent                                | Responsibility                                                                                                       |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| 🧾 **Report Parser Agent**           | Performs sentence segmentation and extracts descriptions of attacker behavior from CTI text.                         |
| 🧠 **ATT\&CK Knowledge Agent**       | Hosts the full MITRE ATT\&CK knowledge base as embeddings, allows fast semantic lookup of relevant TTPs.             |
| 🧩 **Mapping Agent**                 | Uses embeddings + LLM reasoning to match behavior text with the best-fitting ATT\&CK techniques.                     |
| ✅ **Verification & Reporting Agent** | Assigns confidence scores, justifies mappings, highlights source text, and formats the output (e.g., STIX2 or JSON). |

---

## 🛠 Agent Toolkit: Tools and Technologies

| Agent                          | Tools Used                                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------------------------- |
| Report Parser Agent            | `spaCy`, `NLTK`, regex for behavior extraction                                                    |
| ATT\&CK Knowledge Agent        | Vector DB (e.g., `ChromaDB`, `Pinecone`), MITRE STIX data (JSON or via API)                       |
| Mapping Agent                  | `sentence-transformers` (e.g., `all-mpnet-base-v2`), `GPT-4` or `Claude Opus` for final reasoning |
| Verification & Reporting Agent | LLM APIs + Output Formatter (STIX2 lib, JSON creator)                                             |

---

## 🧭 Workflow from the User's Perspective

1. The user pastes an excerpt from a CTI report:

   > “The threat actor used encoded PowerShell commands to download a second-stage payload from a remote server.”

2. The system responds:

   ```
   MAPPED TECHNIQUE: T1059.001 - PowerShell
   CONFIDENCE: 0.93
   TEXT TRIGGER: "used encoded PowerShell commands to download a second-stage payload"
   ADDITIONAL CANDIDATES: T1105 (Ingress Tool Transfer), T1027 (Obfuscated Files or Information)
   ```

3. The user can export the result as a STIX2 object or directly into a SIEM/TIP platform.

---

## ⚙️ Technical Deep Dive: Developer's View

### Step-by-Step Execution:

1. **Knowledge Base Prep (Offline Step)**:

   * Use `sentence-transformers` to embed descriptions of all MITRE ATT\&CK techniques (from JSON or API).
   * Store them in a vector DB (`Chroma`, `Weaviate`, or `Pinecone`).

2. **Live Mapping Pipeline**:

   ```mermaid
   graph TD
   A[Input CTI Text] --> B[Report Parser Agent]
   B --> C[Behavior Segments]
   C --> D[Mapping Agent]
   D --> E[ATT&CK Knowledge Agent]
   E --> F[Top-k Semantically Similar TTPs]
   F --> G[LLM Reasoning for Best Match]
   G --> H[Verification & Reporting Agent]
   H --> I[Final Mapped TTPs with Justification]
   ```

3. **Example Prompt for Mapping Agent**:

   ```txt
   Given the following attacker behavior:
   "used encoded PowerShell commands to download a second-stage payload"

   And the following ATT&CK techniques:
   - T1059.001: PowerShell
   - T1105: Ingress Tool Transfer
   - T1027: Obfuscated Files or Information

   Identify the most appropriate technique(s) and explain your reasoning.
   ```

---

## 🧪 Data Sources for Prototyping and Testing

| Source                 | Use                                                                                        |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| **MITRE ATT\&CK STIX** | Official technique descriptions, tactics mapping. ([GitHub](https://github.com/mitre/cti)) |
| **CTI Reports**        | Use vendor reports (e.g., CrowdStrike, FireEye) as real-world input data.                  |
| **Atomic Red Team**    | Test descriptions map directly to known TTPs — great for validation.                       |
| **APTNotes**           | Open-source collection of threat actor reports for parsing exercises.                      |

---

## 🧠 Sample Output (JSON Format)

```json
{
  "input": "Encoded PowerShell was used to download the next-stage payload",
  "technique_id": "T1059.001",
  "technique_name": "PowerShell",
  "confidence_score": 0.92,
  "source_snippet": "Encoded PowerShell was used...",
  "alternative_matches": [
    { "id": "T1027", "score": 0.79 },
    { "id": "T1105", "score": 0.77 }
  ]
}
```

---

## 🚀 Hackathon Viability

| Factor                          | Score      |
| ------------------------------- | ---------- |
| 🔧 Technical Feasibility (MVP)  | 🔥🔥🔥🔥   |
| 🧠 Innovation Level             | 🔥🔥🔥🔥🔥 |
| 👥 Team Collaboration Potential | 🔥🔥🔥🔥   |
| 🖥 Demo Visual Impact           | 🔥🔥🔥🔥🔥 |
| 📈 Practical Value              | 🔥🔥🔥🔥🔥 |

---

## ➕ Bonus Enhancements (Optional)

* ✅ Export results as **STIX2** JSON objects.
* 📍 Show ATT\&CK **Tactic-TTP map** visually.
* 💡 Link mapped TTPs to **defensive controls** (e.g., MITRE D3FEND).
* 🔗 Add bi-directional search (e.g., “What TTPs does APT29 typically use?”).

---

