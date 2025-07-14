

# CTI Feed De-duplication and Prioritization Service

**Complexity Level:** Medium-High
**Reasoning:** This use case introduces structured data ingestion, format normalization, cross-feed correlation, and enrichment—all under an agentic paradigm. It mimics the workload of a Cyber Threat Intelligence (CTI) analyst, and uses AWS Lambda for modular data handling, with Bedrock Agents providing reasoning, prioritization, and synthesis. The use of only three core AWS services ensures it remains lightweight, cost-effective, and ideal for hackathons.

---

### **Core Concept:**

The system ingests threat feeds from multiple vendors, detects duplicate indicators of compromise (IOCs), enriches each unique IOC with external context, and generates a **prioritized master feed** that eliminates redundancy and boosts intelligence quality.

---

### **Primary Objective:**

To help CTI teams automatically reduce noise, avoid redundancy in threat feeds, and produce a **clean, actionable, and prioritized threat intelligence dataset**.

---

### **Problem Statement:**

CTI analysts and security platforms often receive **overlapping indicators** from multiple intelligence sources. These feeds:

* May be in different formats (STIX, JSON, XML)
* Contain duplicate or near-duplicate IOCs
* Lack immediate context (like severity, confidence, or attack family)

Without de-duplication and enrichment, analysts waste time on redundant data, miss patterns, or misallocate response resources.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                   | Role                                                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Feed Parser Agent**        | Uses a Lambda tool to convert STIX, JSON, or XML feeds into a standardized JSON IOC format.             |
| **Duplicate Detector Agent** | Checks if each IOC already exists (by hash, IP, domain, etc.) in prior processed files.                 |
| **Threat Enrichment Agent**  | Enriches each unique IOC with metadata from external sources (e.g., severity, malware family, country). |
| **Feed Synthesizer Agent**   | Prioritizes and assembles all clean, enriched indicators into a consolidated “master feed.”             |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose                                                                                                     |
| -------------- | ----------------------------------------------------------------------------------------------------------- |
| **Amazon S3**  | Ingestion bucket for raw feeds (`S3-CTI-Raw`) and output bucket for processed results (`S3-CTI-Processed`). |
| **AWS Lambda** |                                                                                                             |

* `ParserLambda`: Converts files into normalized IOC JSON.
* `DedupeLambda`: Checks for existing IOCs (e.g., using S3 key lookup or embedded logic).
* `EnrichmentLambda`: Queries enrichment sources (mocked or offline context for hackathon use). |
  \| **AWS Bedrock Agent** |
  Central orchestrator for:
* Calling each tool Lambda
* Merging data
* Generating final prioritized feed
* Returning structured JSON for storage |

---

### **Workflow from the User's Perspective**

1. **Step 1:** Several threat vendors upload feeds (e.g., `stix_feed.json`, `xml_feed.xml`) to `s3://cti-feeds-raw/`

2. **Step 2:** S3 triggers a `FeedProcessorLambda`, which:

   * Sends the raw file to a **Bedrock Agent**
   * The agent:

     1. Invokes `ParserLambda` to standardize IOC structure
     2. Checks each IOC for duplicates via `DedupeLambda`
     3. For new indicators, calls `EnrichmentLambda` to fetch context

3. **Step 3:** The Bedrock Agent compiles the results into a **single enriched, de-duped, prioritized IOC list**.

4. **Step 4:** Lambda writes the final output to `s3://cti-feeds-processed/{timestamp}_master_iocs.json`.

---

### **Example Agent Prompt (in Lambda):**

```txt
You are a CTI assistant agent. You receive a parsed JSON list of indicators from multiple sources.
- Use Tool 1 (ParserLambda) to normalize input format.
- For each indicator, use Tool 2 (DedupeLambda) to check if it has already been processed.
- For unique indicators, call Tool 3 (EnrichmentLambda) to get more context.
- Sort indicators by severity and confidence score.
- Return a cleaned, de-duplicated, enriched list of IOCs in the format: 
  { "hash": "", "type": "", "source": "", "confidence": 85, "threat": "RedLine Stealer", "geo": "RU", "first_seen": "2023-06-22" }
```

---

### **Hackathon Viability:**

✅ **High**

* 🔁 Real-world CTI problem: Everyone deals with duplicate IOCs
* 🧱 Clean microservice architecture using S3 + Lambda + Bedrock
* 🧠 Ideal for agent reasoning (e.g., should two similar hashes be merged?)
* 🔎 Extendable: Can add scoring functions or downstream integrations
* ⚙️ Mock enrichment makes it viable even without third-party APIs

---

### **Data Sources for Prototyping and Testing:**

* Sample STIX feeds (e.g., from [OASIS Open](https://oasis-open.github.io/cti-documentation/stix/intro))
* Open-source CTI feeds: AlienVault OTX, AbuseIPDB, Anomali demo feeds
* Custom mock threat feeds for creating duplication and enrichment test cases

---

