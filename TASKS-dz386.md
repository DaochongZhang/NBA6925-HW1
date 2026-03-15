# TASKS.md

Build the "Smart Treasury Copilot" MVP as defined in `SPEC.md`. Follow these steps sequentially:

## 1. Project Scaffolding & Strict Constraints
- Initialize a Vite + React + TypeScript frontend.
- Set up a FastAPI backend strictly in `backend/main.py`.
- Apply professional CSS (corporate slate/navy/white theme).
- Ensure `OPENAI_API_KEY` is loaded via `.env` and NEVER exposed to the frontend.

## 2. Backend Implementation (FastAPI)
- Create a `POST /api/evaluate` endpoint.
- Implement the OpenAI API call using the exact system prompt from `SPEC.md`.
- Force the LLM to return the strict JSON schema: `{ status, liquidity_yield_analysis, compliance_note }`.
- Implement backend error handling for missing API keys or OpenAI timeouts.

## 3. Frontend UI Construction (React)
- Build a two-column layout (Input left, Results right; stack vertically on mobile).
- **Input Area:** Add textarea and a "Submit Proposal" button. Implement the exact `Evaluating risk and liquidity...` loading state. Disable submit if input is empty.
- **Results Dashboard:** Build the exact `Awaiting proposal data...` empty state.
- Map the backend JSON to UI blocks. Enforce exact badge rendering: `[🟢 APPROVED]` (green) and `[🔴 FLAGGED]` (red).

## 4. Error Handling & Wiring
- Connect the frontend form submission to the backend API.
- Implement the exact frontend error states defined in `SPEC.md` (System Error / Copilot Parse Error) to prevent UI crashes on bad API responses.

## 5. Validation & Testing (Manual Execution Required)
Before considering the project complete, the agent must ensure the following tests pass:
- **Test 1 (Approved):** Input a short-term deposit proposal. Verify `[🟢 APPROVED]` appears.
- **Test 2 (Flagged):** Input a long-term trust product for short-term idle cash. Verify `[🔴 FLAGGED]` appears.
- **Test 3 (Empty State & Input):** Verify the empty state text is exactly `Awaiting proposal data...` and the submit button cannot be clicked when empty.
- **Test 4 (Error State):** Temporarily break the `.env` API key. Verify the UI catches the error and displays the graceful error message instead of a blank white screen.