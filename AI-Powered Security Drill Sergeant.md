

# **AI-Powered Security Drill Sergeant**

**Complexity Level:** Medium
**Reasoning:** This use case introduces **human-in-the-loop interactivity** within a cybersecurity training context. It integrates **multimodal reasoning (image + coordinates)** and **click-based validation**, which makes it more interactive and engaging than passive training. The use of AWS Bedrock for contextual feedback and reasoning, combined with Lambda for orchestration and S3 for scenario storage, ensures a simple, serverless, and scalable implementation.

---

### **Core Concept:**

Instead of just showing static security awareness tips, this tool **actively quizzes users**. It presents screenshots of phishing emails, fake login pages, or scam SMS messages, and prompts the user to **click on parts of the image** where they see a red flag. The AI then provides **real-time feedback**.

---

### **Primary Objective:**

To improve phishing awareness and scam detection skills in the general public or corporate employees through an **interactive, gamified AI-powered training experience**.

---

### **Problem Statement:**

Most security training tools are **passive, boring, and easy to ignore**. Users are shown sample attacks or best practices, but they don’t actually engage in identifying phishing red flags in a practical, scenario-based way.
This creates a **skills gap**, where users understand theory but fail to apply it under pressure.

---

### **The Multi-Agent Team: Roles and Responsibilities**

| Agent Name                   | Role                                                                                           |
| ---------------------------- | ---------------------------------------------------------------------------------------------- |
| **Scenario Fetcher Agent**   | Randomly selects a phishing scenario image and its associated red flag metadata from S3.       |
| **Click Validator Agent**    | Compares user click coordinates with the known correct regions (bounding boxes) for red flags. |
| **Feedback Generator Agent** | Uses reasoning to explain why the clicked region is right or wrong, and provides coaching.     |

---

### **Agent Toolkit: Associated Tools and Technologies**

| Tool or Technology                        | Description                                                                                                                                   |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon S3**                             | Stores phishing training data (images + red flag metadata in JSON). Example file: `phishing_scenario_12.png` and `phishing_scenario_12.json`. |
| **AWS Lambda**                            | Handles training session orchestration, receives click coordinates, invokes Bedrock Agents, and routes response back to the web app.          |
| **AWS Bedrock Agent (Claude 3 / GPT-4o)** | Analyzes user input, performs coordinate-based validation, and generates tailored, conversational feedback.                                   |

---

### **Workflow from the User's Perspective**

1. **Step 1:** A user opens the training tool web app and clicks “Start Scenario.”

2. **Step 2:** Behind the scenes:

   * The web app calls an API endpoint (via API Gateway).
   * Lambda is triggered and selects a **random phishing image + answer key JSON** from S3.
   * The scenario is presented to the user: “Click on any part of this message that looks suspicious.”

3. **Step 3:**

   * The user clicks on the image (e.g., clicks on a spoofed sender email).
   * The front end sends the click coordinates to the backend via another API call.

4. **Step 4:**

   * Lambda receives the user click and invokes a **Bedrock Agent** with:

     * User’s click (x, y)
     * Red flag regions from metadata
   * The agent responds with:

     * `"Correct!"` or `"Incorrect!"`
     * A detailed explanation: e.g., “You correctly clicked on the email address. The domain is misspelled as `micr0soft.com`.”
   * Lambda returns the feedback to the UI in real time.

---

### **Example Agent Prompt (in Lambda):**

```txt
You are a security training assistant. A user clicked on coordinates (455, 188) in an image. 
The known red flag regions are:
1. (420–500, 160–200): The spoofed email address
2. (300–370, 270–310): The suspicious link preview

Determine if the user's click was within one of these bounding boxes. 
Then provide an educational explanation of the red flag they clicked or missed.
Return a JSON with: { "correct": true/false, "reason": "...", "tip": "..." }
```

---

### **Hackathon Viability:**

✅ **Very High**

* 🎮 Interactive and fun: Combines learning with gaming
* 🧠 Multimodal: Uses both images and reasoning from GenAI
* ☁️ Serverless: Fully powered by S3, Lambda, and Bedrock Agents
* 🧑‍🏫 Educational impact: Perfect for corporate demos or public training
* 📦 Expandable: Add more scenarios, different difficulty levels, or adaptive feedback with ease

---

### **Data Sources for Prototyping and Testing:**

* OpenPhish and PhishTank: Public phishing email repositories
* Reddit & YouTube: Scam examples and UI screenshots
* Create synthetic phishing emails using templates + GenAI image generation tools (optional)
* Manually label bounding boxes in images with JSON for red flag regions

---

