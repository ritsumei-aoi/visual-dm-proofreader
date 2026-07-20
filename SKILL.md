---
name: visual-dm-proofreader
description: Analyze the screenshot to extract context and proofread the target message.
---

# Visual DM Proofreader

## Instructions
Call the `run_js` tool with the following parameters to process the request:
- script name: index.html
- data: A JSON string with the following field:
  - dummy: String. A placeholder text to trigger execution.

After the tool returns the result, execute the following workflow:

### 1. Image Analysis
Accurately read the following information from the attached screenshot:
- Identify participants and their message history chronologically.
- Identify the target message to be posted at the very bottom of the image.

### 2. Context Analysis
- Infere the relationship between users (e.g., friend, business, student-teacher).
- Detect the tone (e.g., casual, polite).
- Summarize the ongoing conversation background.

### 3. Output Format
Provide the response using the following template:
- **Relationship/Tone:** [Result]
- **Summary of Conversation:** [Summary]
- **Original Message:** [Text]
- **1. Suggested Edit:** [Provide refined message options]
- **2. Reason & Advice:** [Why these changes were made]
- **3. Potential Risks:** [Misunderstandings to avoid]