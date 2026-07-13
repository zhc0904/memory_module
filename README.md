# Finance Advisor Agent — Memory Module

A small [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/) project built with Claude Code, demonstrating the difference between an agent with **no memory** and one with **memory via `SQLiteSession`**.

## What's in the notebook

`finance_advisor_no_memory.ipynb` contains a single "Finance Advisor" agent (tracks spending by category and gives budget advice) run through the same two test queries twice:

1. **No-memory section** — each `Runner.run` call is independent. The agent has no idea what was said in the first query when it answers the second, so its second answer is generic.
2. **Task 3 — with memory** — the exact same agent/model, but a `SQLiteSession` is attached to `Runner.run`. The session automatically stores and replays conversation history, so the agent recalls the first message's expense breakdown ($120 groceries / $40 Uber / $60 restaurants) when answering the budget question.
3. **Side-by-side comparison** — a final cell prints both versions' answers to the same second query back to back, so the memory effect is easy to see directly.

Test queries used in both sections:
- `"I spent $120 on groceries, $40 on Uber, and $60 on restaurants this week."`
- `"My weekly budget is $250. Based on everything I told you so far, where can I cut spending next week?"`

The agent talks to `openai/gpt-5-mini` through an **OpenRouter**-compatible `AsyncOpenAI` client (the project's API key is an OpenRouter key, not an `api.openai.com` key), with tracing disabled and `max_tokens` capped to fit the account's credit limit.

## Other files

- `prompt_0_venv.md`, `prompt_1_basic_agent.md`, `prompt_2_add_memory.md` — the Claude Code prompts used to build this project step by step (environment setup → no-memory agent → memory section).
- `practice_opportunity_solution.md` — a follow-up practice prompt (not yet built) for a "Wellness Coach" agent that uses `SQLiteSession` to avoid repeating habit suggestions across sessions.
- `skills-lock.json` — Claude Code skills lockfile.

## Setup

```bash
python -m venv venv
venv\Scripts\activate          # Windows
pip install openai-agents python-dotenv ipykernel
python -m ipykernel install --user --name=venv --display-name="Python (venv)"
```

Create a `.env` file in the project root (not committed — see `.gitignore`):

```
OPENAI_API_KEY=your-openrouter-or-openai-key-here
```

Then open `finance_advisor_no_memory.ipynb` in VS Code/Jupyter, select the `venv` kernel, and run all cells top to bottom.
