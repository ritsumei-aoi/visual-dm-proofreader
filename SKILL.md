---
name: contextual-dm-assistant
description: Analyzes chat screenshots to infer relationship and context, then suggests or proofreads optimal messages.
---

# Contextual DM Assistant Skill

## Tools
- run_js:
    description: Execute the internal script to initialize the environment.
    parameters:
      type: object
      properties:
        script_name:
          type: string
          description: The script file to run. Must be "index.html".
        data:
          type: string
          description: A serialized JSON string payload.

## Instructions
Before processing the request, you must call the `run_js` tool with the following arguments:
- script_name: "index.html"
- data: '{"action": "init"}'

Once the tool execution is completed, analyze the input (image and optional user text) and follow the workflow below.

---

### Phase 1: Input Analysis & Mode Branching

1. **Check Text Input:**
   - **Mode A (Image + Text Input):** The user provided additional text along with the screenshot.
     - *Case A-1 (Draft Continuation):* If the text is a draft message, treat it as the text to be proofread. Refine it based on the image's context and any specific user instructions (e.g., "make it more polite").
     - *Case A-2 (Reply Generation):* If the text is a request/question (e.g., "How should I reply?"), generate new reply options tailored to the situation in the screenshot.
   - **Mode B (Image Only Input):** The user provided only a screenshot.
     - Locate the message at the very bottom (input field or latest unsent/sent bubble) and treat it as the **Target Draft Message** to be proofread.

2. **Language Rule:**
   - Detect the primary language used in the chat screenshot.
   - **Suggested Messages / Drafts:** Output using the language found in the screenshot.
   - **Explanations, Reasons, & Risks:** Output in the user's primary instruction language (default: Japanese) to ensure clear understanding.

---

### Phase 2: Screenshot Context Extraction

- **Context Identification:** Analyze chronological message history, relationship between users (e.g., business, friends, student-teacher), and the current tone (casual, formal, polite).
- **Separation Protocol (Mode B):** Clearly distinguish between "Past Chat History" and the "Target Draft Message".

---

### Phase 3: Output Format

Generate the response following this structure:

#### ■ 文脈・前提の認識 (Context Recognition)
- **関係性・トーン (Relationship/Tone):** [e.g., ビジネス（丁寧）, 友人（カジュアル）]
- **これまでの経緯 (Summary):** [Brief summary of the ongoing conversation in Japanese]
- **認識した対象文面 (Target Text):** 「[Draft text extracted from image or user input]」

#### ■ 1. メッセージ案・修正案 (Suggested / Refined Messages)
[Provide optimal message options in the language of the chat. Offer 2-3 variations if applicable, e.g., standard, polite, concise]

#### ■ 2. 修正・提案の理由 (Reason & Advice)
[Explain why these adjustments fit the context and tone in Japanese]

#### ■ 3. 懸念点・リスクの指摘 (Potential Risks & Nuances)
[Highlight any potential misunderstandings, tone mismatches, or cultural/situational nuances to be careful of in Japanese]