# MiniBank MCP Agent
## Local OpenAI Responses API + MCP Server

---

## Quick start

### 1. Install

```bash
pip install -r requirements.txt
```

### 2. Start the MCP server

- By default, business rules are enforced at the tool implementation level

```bash
python mcp_server.py
# SSE endpoint → http://localhost:8888/sse
```

- If unrestricted (no business rules enforced) server needed, pass the `--unsafe` flag

```bash
python mcp_server.py --unsafe
# SSE endpoint → http://localhost:8888/sse
```

### 3. Get an OpenAI Responses API server with OGX

This uses Ollama as the model provider but you can use any major remote model provider with [OGX](https://ogx-ai.github.io/)

```bash
uv init ogx-starter-server && cd ogx-starter-server
uv add 'ogx[starter]' openai botocore
export OLLAMA_URL=http://localhost:11434/v1
uv run ogx stack run starter
```

With this, you should have the OGX server running at `http://localhost:8321/v1`.

### 4. Interact with your agent 🚀

```bash
curl http://localhost:8321/v1/responses \
  -H "Content-Type: application/json" \
  -d '{
    "model": "ollama/qwen3.5:2b",
    "input": "What is the balance on ACC001?",
    "tools": [{
      "type": "mcp",
      "server_label": "minibank",
      "server_url": "http://localhost:8888/sse",
      "require_approval": "never"
    }]
  }'
```
