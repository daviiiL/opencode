---
name: querying-dart-mcp
description: Use when Dart best practices, idiomatic patterns, lint rules, analyzer guidance, package/pub conventions, Flutter recommendations, or Dart guidance questions arise from a user or any agent.
---

# Query Dart MCP for Best Practices

Use this skill whenever a question arises about Dart best practices, idiomatic code, lint rules, package patterns, pub best practices, Flutter recommendations, or any Dart-related guidance question from a user or any agent/subagent.

**Trigger:** Any question from user or agent that seeks "best practices", "idiomatic Dart", "lint/analyzer guidance", "package structure", "pub recommendations", or Flutter best practices.

**Mandatory action:** Before answering, query `dart-mcp-server` using the available Dart MCP tools/resources to get authoritative guidance. The `dart-mcp-server` provides real-time, version-specific, and context-aware best practice information that general knowledge cannot match.

If the `dart-mcp-server` or its tools are unavailable, explicitly state this limitation before providing any fallback answer based on general knowledge.

## Common Mistakes

Answering from general knowledge is insufficient because:

| Failure | Reason |
|---------|--------|
| Answering from general knowledge | Outdated, version-agnostic, or incorrect for specific Dart/Flutter versions |
| Skipping MCP query | Misses lint rule exceptions, package-specific guidance, and version nuances |
| Assuming standard pattern | May conflict with current analyzer recommendations or Flutter framework patterns |

## Common Triggers

| Trigger | Action |
|---------|--------|
| "What are Dart best practices for..." | Query dart-mcp-server before answering |
| "How should I handle X in Dart?" (idiomatic question) | Query dart-mcp-server before answering |
| "Is this lint warning meaningful?" | Query dart-mcp-server before answering |
| "What's the recommended pattern for Y?" | Query dart-mcp-server before answering |
| Flutter async/error/state management questions | Query dart-mcp-server before answering |
| Any question referencing "best practice", "idiomatic", or "lint" | Query dart-mcp-server before answering |

## Baseline Failure to Avoid

**Baseline failure:** A subagent answering "What are Dart best practices for async error handling?" from general knowledge instead of querying `dart-mcp-server`.

**Correct approach:**
1. Detect the best-practice question
2. Query `dart-mcp-server` for the authoritative answer before answering
3. Cite MCP sources or explicit limitations
4. Only fallback to general knowledge if MCP unavailable

## Keywords

Dart, dart, best practices, best-practice, idiomatic, lint, analyzer, package, pub, Flutter, dart-mcp-server, MCP
