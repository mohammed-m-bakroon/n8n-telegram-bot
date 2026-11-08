# CLAUDE.md

## Project

One n8n workflow (`workflow.json`) that runs a Telegram bot. No app code, build or tests. See `README.md` and `docs/architecture.png`.

- Text -> AI Agent (Groq `llama-3.3-70b-versatile`, 10-message memory) with tools: Tavily search, Pollinations image generation, SerpAPI image search.
- Photo (with or without caption) -> Anthropic Messages API -> text reply.
- Agent reply containing `http` -> sent as a Telegram photo.

## Rules

- Keep node names stable; connections refer to them by name.
- `Anthropic Chat Model` is disabled and unused. The agent runs on Groq.
- Never commit real API keys. Placeholders: `YOUR_ANTHROPIC_API_KEY`, `YOUR_TAVILY_API_KEY`, `YOUR_SERPAPI_API_KEY`.
- After edits, update `docs/architecture.png` and the README if routing or tools changed.

## Known issues

- URL check is `contains "http"`, so a text reply with a link is sent as a photo.
- `Send a text message1` reads `content[1].text`; may be undefined if the response has one block.
- API keys are inline in nodes, not n8n credentials.
- Memory is lost on n8n restart; the photo path has no memory.

## Checks

```bash
python3 -c "import json; json.load(open('workflow.json'))"
grep -nE "sk-ant-|tvly-" workflow.json   # expect no output
```