

# Automated Threat Modeling Assistant

**Complexity Level:** High
**Reasoning:** This use case combines multimodal understanding (architecture diagrams + textual app descriptions), threat classification via STRIDE, and security recommendations. It automates what is usually a deeply manual, expert-driven process and introduces intelligent, context-aware suggestions via agentic workflows. While the underlying logic is complex, the modular use of AWS Bedrock Agents, Lambda, and S3 makes it highly feasible for hackathon execution.

---

### **Core Concept:**

A developer uploads an **application architecture diagram** (PNG/JPG) and a short **textual description** of the system. An intelligent agent automatically analyzes both to:

* Identify components and data flows
* Apply threat modeling frameworks like **STRIDE**
* Generate threat scenarios and mitigation strategies

---

### **Primary Objective:**

To empower **DevOps, DevSecOps, and AppSec teams** with a **fully automated threat modeling assistant**, accelerating secure design practices and enabling early-stage security reviews during development ("shift-left" security).

---

### **Problem Statement:**

Threat modeling is a **critical but time-consuming and manual** process requiring security expertise. Most teams skip it or perform shallow reviews, leaving systems exposed to:

* Tampering, spoofing, information disclosure, etc.
* Lack of compliance with secure architecture best practices

Automation is needed to make threat modeling **scalable, consistent, and accessible**, especially in fast-paced DevOps pipelines.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                    | Role                                                                                                                                                      |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Architecture Parser Agent** | Analyzes the uploaded architecture image and textual description to identify components, trust boundaries, and data flows.                                |
| **Threat Classifier Agent**   | Applies the STRIDE model (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) to surface threat vectors. |
| **Security Controls Agent**   | Suggests mitigations and security best practices tailored to the detected risks (e.g., encryption, authentication, monitoring).                           |
| **Threat Model Report Agent** | Generates a readable markdown file detailing the threats, risks, and recommendations.                                                                     |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose                                                                                                                           |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon S3**  | Input files (image + description) uploaded to `S3-ThreatModel-Inbox/`; Final markdown report stored in `S3-ThreatModel-Results/`. |
| **AWS Lambda** |                                                                                                                                   |

* `AnalysisTriggerLambda`: Invoked on new uploads; coordinates the threat modeling flow.
* `ThreatAnalysisLambda`: Calls the Bedrock agent with STRIDE prompts and component data.
* `ControlsLambda`: Queries the Bedrock agent again with identified threats to suggest mitigations. |
  \| **AWS Bedrock Agent (Claude 3 / GPT-4o)** |
  Multimodal agent used for:
* Image + text understanding
* Structured threat modeling
* STRIDE analysis
* Security recommendations
* Report generation in Markdown |

---

### **Workflow from the User's Perspective**

1. **Step 1:** Developer uploads two files to the tool:

   * `diagram.png`: A system architecture image
   * `description.txt`: A brief description (e.g., "This app has a public API gateway, two web servers, and an RDS backend.")

2. **Step 2:**

   * The upload to `s3://S3-ThreatModel-Inbox/` triggers `AnalysisTriggerLambda`
   * The Lambda:

     * Sends both file paths to the **Bedrock Agent**
     * The agent:

       * Parses components from image and text (e.g., public LB, app server, database)
       * Applies **STRIDE** to each element
       * Identifies threats (e.g., “Database is at risk of Information Disclosure if unencrypted”)
       * Calls `ThreatAnalysisLambda` and `ControlsLambda` as needed

3. **Step 3:**

   * The agent generates a detailed **threat modeling report in Markdown**
   * Lambda saves the result to: `s3://S3-ThreatModel-Results/threat_model_{timestamp}.md`

---

### **Example Agent Prompt (used in Lambda):**

```txt
You are a cybersecurity assistant. A developer has uploaded a system diagram and description of their app.
Analyze the image and text to extract the following:
1. List of components (e.g., web server, DB)
2. Data flows between components
3. Trust boundaries (if evident)
4. Apply STRIDE to each component and data flow.
5. For each threat, suggest a relevant mitigation (e.g., TLS, WAF, IAM).

Format the final response as a Markdown threat modeling report with:
- Components and architecture summary
- Threats organized by STRIDE category
- Security recommendations
```

---

### **Hackathon Viability:**

✅ **High**

* 💡 Cutting-edge automation of a traditionally manual process
* 🧠 Leverages multimodal reasoning (image + text)
* 💼 Aligns with real-world DevSecOps practices
* 🧱 Uses only S3, Lambda, and Bedrock Agents — no external tooling required
* 🛠️ Valuable for enterprise, compliance, and training use cases
* 📄 Markdown output is easy to integrate into CI/CD documentation pipelines

---

### **Data Sources for Prototyping and Testing:**

* Sample app architecture diagrams from:

  * AWS Well-Architected Framework
  * Public GitHub DevOps projects
  * Microsoft Threat Modeling Tool examples
* STRIDE framework documentation for threat guidance
* Manually written `description.txt` files for varied app scenarios (e.g., monolith, microservices, serverless)

---
