# Contract Analyser

A Flask web app that extracts structured data from PDF contracts using the Anthropic Claude API.

---

## What it does

A user uploads a PDF contract and the app sends the extracted text to Claude, which returns a validated JSON object containing parties, governing law, contract term, key dates, obligations, a plain-English summary, and risk clauses with severity ratings. The results are rendered as a two-panel UI: a structured summary card on the left and colour-coded risk cards (red/amber/green) on the right. Analyses can be exported as a formatted Word document.

## Architecture

pypdf extracts raw text from the uploaded PDF, which is passed in a single API call to Claude with a strict system prompt that enforces a defined JSON schema — no prose, no markdown, just structured output. Flask handles all routing, session management, rendering, and file generation via python-docx. Claude does one thing only: structured extraction. This demonstrates a clean pattern where Claude acts as an embedded extraction layer rather than a chat interface — deterministic input, validated output, no conversational state.

## Stack

- Python 3.9+
- Flask
- Anthropic Claude API (`claude-sonnet-4-20250514`)
- pypdf
- python-docx
- python-dotenv

## Setup

1. Clone the repository
2. Create and activate a virtual environment: `python3 -m venv venv && source venv/bin/activate`
3. Install dependencies: `pip install -r requirements.txt`
4. Add your API key to `.env`: `ANTHROPIC_API_KEY=your_key_here`
5. Run the app: `python3 app.py`

Visit `http://127.0.0.1:5000`.

## Running locally

```bash
git clone <repo-url> && cd contract-analyser   # Clone the repository and enter the directory
python3 -m venv venv                            # Create the virtual environment
source venv/bin/activate                        # Activate it — easy to forget; libraries won't be found without this step
pip install -r requirements.txt                 # Install all dependencies into the active environment
python3 app.py                                  # Start the Flask development server
```

Visit `http://127.0.0.1:5000`.

**Stopping the server:** Press `Ctrl+C` in the terminal to stop Flask.

## Key architectural decisions

- **Single API call rather than multi-turn:** Contract analysis is a stateless transformation — the full document is available upfront, so a single call with a well-constructed system prompt is more reliable and cheaper than an agentic loop.
- **Structured JSON output rather than free prose:** Enforcing a strict schema in the system prompt makes Claude's output directly machine-readable without post-processing heuristics. The app parses the response with `json.loads` and fails explicitly if the contract is malformed.
- **Flask rather than a heavier framework:** The app has four routes and no database. Flask keeps the stack minimal and the pipeline easy to follow — adding Django or FastAPI would introduce configuration overhead with no benefit at this scope.

## Cost

Each contract analysis costs approximately $0.01–$0.03 using Claude Sonnet, depending on document length.
