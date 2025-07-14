# Voice Phishing ("Vishing") Call Analyzer

**Complexity Level:** Medium
**Reasoning:** This use case introduces the **audio modality**, requiring speech-to-text transcription, text analysis, and voice characterization. While the full pipeline typically includes external tools, we simulate this pipeline using **S3 uploads, Lambda orchestration, and Bedrock multimodal agent reasoning**, making it a unique and feasible hackathon project that shows off agentic audio analysis capabilities.

---

### **Core Concept:**

A user uploads a voicemail or phone call recording suspected to be a scam. The system:

* Transcribes the voice to text
* Analyzes the transcript for scam tactics
* Assesses the audio signal quality and cadence
* Returns a verdict on whether the call is likely part of a **vishing (voice phishing)** scam.

---

### **Primary Objective:**

To **automatically detect fraudulent voice calls**, protecting vulnerable populations (especially elderly users) from high-pressure scam tactics that aim to trick them into giving away sensitive information or making fraudulent payments.

---

### **Problem Statement:**

Scammers increasingly use phone calls to impersonate legitimate organizations like tax authorities, banks, or tech support. These **vishing attacks** rely on:

* High-pressure language
* Requests for unusual payments (gift cards, crypto)
* Robotic or synthetic voices
  Manual detection is slow and error-prone, and victims often act before they verify. An **automated vishing analyzer** can significantly reduce risk.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                         | Role                                                                                                  |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Audio Transcriber Agent**        | Converts uploaded audio into text using simulated transcription (via Bedrock prompt or mocked logic). |
| **Text-Based Scam Analyzer Agent** | Detects scam keywords, urgency, payment requests, impersonation language from the transcript.         |
| **Voice Pattern Analyzer Agent**   | Analyzes cadence, tone, pauses, and artifacts in the audio file (mocked or approximated).             |
| **Final Verdict Agent**            | Synthesizes all signals into a simple, human-readable risk report.                                    |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool / Service | Purpose |
| -------------- | ------- |
| **Amazon S3**  |         |

* Input: User uploads `.mp3` or `.wav` files to `S3-Vishing-Inbox/`
* Output: Transcripts and reports stored in `S3-Vishing-Results/` |
  \| **AWS Lambda** |
* `TranscriptionLambda`: Simulates transcription using metadata or prompts (if no Whisper API).
* `VishingTriggerLambda`: Invoked on upload; coordinates agent analysis pipeline. |
  \| **AWS Bedrock Agent (Claude 3 / GPT-4o)** |
* Multimodal Bedrock Agent performs:

  * Speech-to-text (simulated via prompts)
  * Textual scam analysis
  * Tone and cadence analysis
  * Final risk verdict reasoning and markdown report generation |

---

### **Workflow from the User’s Perspective**

1. **Step 1:** User uploads a voicemail (e.g., `irs_arrest_voicemail.wav`) to `s3://S3-Vishing-Inbox/`.

2. **Step 2:**

   * S3 upload triggers `VishingTriggerLambda`
   * Lambda invokes the Bedrock Agent with:

     * Audio file reference
     * Optional metadata (e.g., file length, speaker characteristics if pre-extracted)
   * Agent performs:

     * **Step 1: Transcription** of audio to text
     * **Step 2: Textual scam detection** (threats, payment requests, impersonation)
     * **Step 3: Audio behavior analysis** (robotic tone, AI-synthesized speech clues)
     * **Step 4: Final verdict with confidence score**

3. **Step 3:**

   * Agent returns a structured report
   * Lambda writes report to: `s3://S3-Vishing-Results/{timestamp}_vishing_report.json`

---

### **Example Output**

```json
{
  "verdict": "SCAM DETECTED",
  "confidence_score": 93,
  "findings": {
    "transcript": "This is the IRS. A warrant has been issued for your arrest. Pay using Apple gift cards.",
    "scam_indicators": [
      "Threatening language: 'warrant', 'arrest'",
      "Unusual payment method: 'Apple gift cards'",
      "Authority impersonation: 'IRS'"
    ],
    "voice_characteristics": "Monotone delivery, synthetic cadence, no natural pauses — likely AI-generated"
  },
  "recommendation": "Block this number. Do not respond or call back. This is a known IRS impersonation tactic."
}
```

---

### **Example Bedrock Agent Prompt**

```txt
You are a voice phishing detection assistant.

Input:
- An audio file and its transcript (or simulated transcript content)
- Your goal is to determine:
  1. Does the message contain high-pressure tactics?
  2. Is the speaker impersonating an authority (IRS, police, bank)?
  3. Are there clues that suggest AI-generated voice (cadence, robotic tone)?
  4. Should the message be classified as a scam?

Output a JSON object with:
- "verdict": ("Safe", "Suspicious", "Scam Detected")
- "confidence_score": 0-100
- "reasons": [...list of detection points...]
- "recommendation": "...user advice..."
```

---

### **Hackathon Viability:**

✅ **High**

* 🎤 Working with audio makes the demo unique and engaging
* 🧠 Strong use of GenAI reasoning (language + tone + impersonation detection)
* 📁 No need for complex real-time streaming — offline upload works perfectly
* 🧱 Fully implementable with **S3, Lambda, and Bedrock**
* 📊 Results are easily visualized (e.g., in a dashboard)

---

### **Data Sources for Prototyping and Testing**

* YouTube: Real scam voicemail recordings
* Public scam awareness databases (e.g., FCC, IRS warning samples)
* Create your own: Record scam scripts read by humans or TTS tools
* Optional: Add multiple risk levels (e.g., mild warning for legitimate robocalls)

---

