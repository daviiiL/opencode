# OpenCode Configuration Repository

Personal configuration for OpenCode — an AI coding agent — covering shell strategy, custom skills, MCP toolchains (PDF reading, codebase RAG), Plannotator integration, and LLM provider setup.

## Setup

### oh-my-opencode-slim

```bash
bunx oh-my-opencode-slim@latest install
```

### plannotator

To install Plannotator, run:
```bash
curl -fsSL https://plannotator.ai/install.sh | bash
```

### coderag-mcp (for the pdf reader mcp)

```bash 
npm install -g @sylphx/coderag-mcp @sylphx/pdf-reader-mcp
```
