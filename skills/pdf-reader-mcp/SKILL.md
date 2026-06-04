---
name: pdf-reader-mcp
description: Extract text, metadata, page counts, and embedded images from local or remote PDF files via the @sylphx/pdf-reader-mcp MCP server. Use when the user asks to read, parse, summarize, or pull data from a PDF (path or URL), including batch reads and specific page ranges.
license: MIT
compatibility: opencode
metadata:
  package: "@sylphx/pdf-reader-mcp"
  upstream: "https://github.com/SylphxAI/pdf-reader-mcp"
  transport: stdio
---

## What this skill does

Wraps the `@sylphx/pdf-reader-mcp` server, which exposes a single MCP tool: **`read_pdf`**. It can:

- Extract full text from PDFs (local path or HTTPS URL)
- Pull metadata (title, author, creation date, etc.)
- Report page counts
- Decode embedded images (base64, with format auto-detected)
- Process multiple PDFs concurrently in one call
- Restrict to specific page ranges (e.g. `"1-5,10-15"`)

## When to use this skill

Invoke when the user asks to:

- **Read / parse / extract** content from a PDF they reference by path or URL
- **Summarize** or **answer questions about** a PDF
- **Get metadata** (author, page count, creation date) from a PDF
- **Pull embedded images** out of a PDF
- Process **multiple PDFs at once**

Do NOT use for:

- Editing or writing PDFs (this server is read-only)
- OCR on scanned/image-only PDFs (it extracts the embedded text layer, not image text)
- DOCX, EPUB, or other non-PDF documents

## Server setup

If the MCP server is not yet registered, add it (Claude Code form):

```bash
claude mcp add pdf-reader -- npx @sylphx/pdf-reader-mcp
```

For OpenCode's `opencode.jsonc`:

```jsonc
{
  "mcp": {
    "pdf-reader": {
      "type": "local",
      "command": ["npx", "@sylphx/pdf-reader-mcp"],
      "enabled": true
    }
  }
}
```

Requires **Node.js >= 22.13.0**.

### Hardening (recommended for untrusted inputs)

- Restrict filesystem access: pass `--allow-dir <path>` flags or set `MCP_PDF_ALLOWED_DIRS` (comma-separated)
- Disable network fetches: pass `--no-http` if you don't need URL sources

## The `read_pdf` tool

### Parameters

| Field | Type | Default | Notes |
|---|---|---|---|
| `sources` | `Array<Source>` | **required** | One entry per PDF |
| `include_full_text` | `boolean` | `false` | Set `true` to actually get text out — **commonly forgotten** |
| `include_metadata` | `boolean` | `true` | Title, author, dates, etc. |
| `include_page_count` | `boolean` | `true` | Total pages |
| `include_images` | `boolean` | `false` | Returns base64 images; can be large |

Each `Source` has:
- `path` — local file (absolute or relative to the server's working directory), OR
- `url` — HTTPS URL to fetch, OR
- `pages` — optional page-range string (e.g. `"1-3,7,10-12"`) applied to that source

### Usage patterns

**Get text from one local PDF:**
```json
{
  "sources": [{ "path": "documents/report.pdf" }],
  "include_full_text": true
}
```

**Fetch a remote PDF and extract specific pages:**
```json
{
  "sources": [{ "url": "https://example.com/spec.pdf", "pages": "1-5" }],
  "include_full_text": true
}
```

**Batch — multiple files in one call (processed concurrently):**
```json
{
  "sources": [
    { "path": "a.pdf" },
    { "path": "b.pdf", "pages": "1-2" },
    { "url": "https://example.com/c.pdf" }
  ],
  "include_full_text": true,
  "include_metadata": true
}
```

**Just metadata / page count (cheap, no text):**
```json
{ "sources": [{ "path": "report.pdf" }] }
```

## Tips & gotchas

- **`include_full_text` defaults to `false`** — if the user asks "what does this PDF say?", you must set it to `true`. Easy to miss.
- Text ordering uses Y-coordinate layout preservation, so multi-column PDFs may interleave in unexpected ways. If output looks jumbled, mention this to the user rather than trusting it blindly.
- Image extraction can produce very large base64 payloads — only enable `include_images` when the user explicitly wants images, and consider extracting one PDF at a time.
- For scanned PDFs with no text layer, `include_full_text: true` will return empty/whitespace strings. Suggest an OCR tool instead in that case.
- Relative `path` values resolve against the server's CWD, not the user's. Prefer absolute paths when in doubt.
- Errors per source are isolated — one bad PDF in a batch does not fail the others. Check each result.
