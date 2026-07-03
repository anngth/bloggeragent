# Blogger Agent

A minimal multi-agent technical blog writer built with [Google ADK](https://google.github.io/adk-docs/) and Gemini.

Given a topic, the agent plans an outline, writes a full Markdown article, and suggests alternate titles plus tweet-length hooks.

## How it works

1. **BlogPlanner** — creates a Markdown outline (title, intro, 4–6 sections, conclusion)
2. **OutlineValidationChecker** — validates the outline; retries up to 3 times if needed
3. **BlogWriter** — writes the full article from the outline
4. **BlogPostValidationChecker** — validates the post; retries up to 3 times if needed
5. **Blogger** (root agent) — orchestrates planning and writing, then returns titles and hooks

## Prerequisites

- Python 3.10+
- A [Gemini API key](https://aistudio.google.com/apikey)

## Setup

```bash
cd bloggeragent
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
```

Edit `.env` and set `GOOGLE_API_KEY`. Optionally change `MODEL` (default: `gemini-flash-latest`).

Do not commit `.env` to version control — only `.env.example` is tracked.

## Run

### One-shot (CLI)

```bash
adk run . "Write a blog post about Python async/await"
```

### Interactive chat (CLI)

```bash
adk run .
```

Enter a topic when prompted. Type `exit` or `quit` to leave.

### Web UI

```bash
adk web . --port 8000
```

Open http://127.0.0.1:8000, select the **Blogger** agent, and enter a topic.

Sessions are stored locally in `.adk/session.db`.

## Output

You get a Markdown article aimed at software engineers, with:

- Title, intro, and structured sections (H2/H3)
- Code snippets where helpful
- Conclusion
- 3 alternate titles and 2 tweet-length hooks

A full run typically takes 1–2 minutes because the agent calls the LLM multiple times (plan, validate, write).

## Troubleshooting

| Issue                    | Fix                                       |
| ------------------------ | ----------------------------------------- |
| Invalid `GOOGLE_API_KEY` | Check the key in `.env`                   |
| `adk: command not found` | Activate the venv or use `.venv/bin/adk`  |
| Slow run / timeout       | Expected — multiple LLM steps per request |
| `EXPERIMENTAL` warnings  | Safe to ignore; these come from ADK       |
