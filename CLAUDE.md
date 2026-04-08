# Contract Analyser — Claude Working Rules

## Project Context

Flask web app that uses the Anthropic Claude API to extract structured JSON from PDF contracts and render the results as a two-panel risk report (summary card left, colour-coded risk clauses right). Analyses can be exported as Word documents via python-docx.

**Stack:** Python 3.9+, Flask, pypdf, python-docx, python-dotenv, Anthropic SDK.

**The single API call pattern is intentional.** Claude receives the full contract text and returns a JSON object in one shot. Do not refactor to multi-turn or agentic unless explicitly asked.

**The JSON schema is the contract between Claude and the rendering layer.** `results.html` references field names directly (`data.parties`, `data.risk_clauses`, etc.). Do not rename schema fields without updating every template reference in the same change.

---

## Workflow Orchestration

- **Plan before building** for any task touching more than one file or function. Write out what will change and why before writing any code.
- **Stop and re-plan if something goes wrong.** Do not push through errors by adding workarounds. Diagnose the root cause first.
- **Write a clear spec before making changes.** For UI changes: what does it look like. For logic changes: what goes in, what comes out, what fails explicitly.
- **Include verification steps in every plan**, not just build steps. A plan that ends at "write the code" is incomplete.

---

## Code Standards

- **Comments explain why, not what.** The code shows what. Comments are for intent, constraints, and non-obvious decisions.
- **Never mark a task complete without testing it in the browser.** Flask serving a 200 is not the same as the page rendering correctly.
- **Handle JSON parsing failures explicitly.** `json.loads` must be wrapped. A malformed Claude response should surface a clear error to the user, not a 500 or silent empty state.
- **Maintain the law firm aesthetic in all UI changes.** Black and white palette, Georgia serif for headings and body text, minimal layout, no decorative colour except the risk severity indicators (red/amber/green). When in doubt, remove rather than add.

---

## Verification Before Done

1. Check the terminal for errors or warnings after every change.
2. Confirm the upload page loads at `http://127.0.0.1:5000`.
3. Confirm a real PDF can be uploaded and analysed end to end.
4. Confirm the results page renders both panels correctly.
5. Confirm the Word export downloads and opens without errors.
6. Only mark complete once all of the above pass.

---

## Lessons

_This section is updated after any correction. Each entry records the pattern that caused the mistake and the fix applied._

<!-- entries added here as they occur -->
