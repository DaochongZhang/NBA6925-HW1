# SPEC.md

## Product Summary
Build a single-page web app titled **Smart Treasury Copilot**.

This is an MVP for a corporate treasury decision-support tool. It allows treasury professionals to input hypothetical idle cash allocation proposals. An AI agent acts as a "Risk & Liquidity Copilot," evaluating the proposal against strict corporate finance rules. 

## Scope
Implement the following:
- A Vite + React + TypeScript frontend.
- Plain CSS stylesheets for a corporate "slate/navy/white" aesthetic.
- A single-page interface with a prominent input area and a results panel.
- Server-side evaluation using a real LLM API.
- A FastAPI backend.

## Engineering Constraints
- **Security:** The `OPENAI_API_KEY` must ONLY be used in the backend. It must never be exposed to or bundled with the frontend.
- **Backend Path:** The backend must be strictly located in `backend/main.py`.
- **Environment:** Use a `.env` file for secrets.
- **State Strictness:** The AI and the backend must only ever process and return two statuses: `APPROVED` or `FLAGGED`. No third state is allowed.

## Page Structure & UI Components
Create a single page with this layout:
- Header displaying the title: `Smart Treasury Copilot`.
- Desktop: Two-column layout (Input on the left, Evaluation Results on the right).
- Mobile: The columns must stack vertically (Input on top, Results below).

### Left Column: Proposal Input
- Section title: `Idle Cash Proposal`.
- A textarea for entering the scenario with placeholder: `e.g., 200M available for 7 days. Propose rolling over in overnight deposits...`
- A `Submit Proposal` button.
- **Loading State:** Upon click, disable the button and change its text to `Evaluating risk and liquidity...`.

### Right Column: Evaluation Dashboard
- Section title: `Copilot Assessment`.
- **Empty State:** Before submission, show muted text exactly as: `Awaiting proposal data...`
- **Result State:** After submission, render three blocks:
  1. **Status Badge:** Must exact-match `[🟢 APPROVED]` (green styling) or `[🔴 FLAGGED]` (red styling).
  2. **Liquidity & Yield:** A short explanatory paragraph.
  3. **Compliance Note:** A short risk paragraph.

## Error Handling & Edge Cases
- **Empty Input:** If the textarea is empty, disable the `Submit Proposal` button.
- **API Key Missing/Backend Error:** If the backend fails to initialize the OpenAI client or throws a 500 error, display in the Right Column: `System Error: API Key missing or backend unavailable.`
- **Malformed JSON/Timeout:** If the AI returns invalid JSON or times out, display in the Right Column: `Error: Copilot failed to parse the risk parameters. Please try again.`

## System Prompt Template
Use this exact prompt template in the backend. Insert the user's strategy where indicated.

```text
You are an expert Corporate Treasury and Risk Management AI Copilot.
Evaluate the following idle cash allocation proposal based on these strict internal rules:
1. Funds idle for LESS than 30 days must prioritize extreme liquidity (e.g., demand deposits, 7-day notice deposits). Trust products or long-term bonds are strictly forbidden and must be FLAGGED.
2. Unsecured loans to non-whitelisted third parties must always be FLAGGED.
3. If the proposal meets liquidity needs and appears safe, mark it as APPROVED and comment on its yield efficiency.

Proposal to evaluate:
{proposal}

Respond STRICTLY in this JSON format:
{
  "status": "APPROVED" | "FLAGGED",
  "liquidity_yield_analysis": "1-2 sentences analyzing the financial sense.",
  "compliance_note": "1-2 sentences on risk and internal control."
}