# MCP (Model Context Protocol) Endpoint

This directory contains a machine-readable index of the Stacks SecOps BOK documentation, designed to be consumed by AI coding assistants and MCP-compatible tools.

## Overview

Hugo generates a structured JSON file at `/mcp/index.json` that provides:

- Complete documentation structure with sections and pages
- Full-text searchable content
- Metadata including word counts, reading times, and last modified dates
- MCP-compatible tool definitions for querying the documentation

## Accessing the MCP Endpoint

### Production URL

```text
https://ensono.github.io/stacks-secops-bok/mcp/index.json
```

### Local Development

When running `hugo serve`, access at:

```text
http://localhost:1313/mcp/index.json
```

## MCP Server Configuration

To use this documentation as an MCP resource in VS Code, create or update `.vscode/mcp.json`:

```json
{
  "servers": {
    "stacks-secops-bok": {
      "type": "http",
      "url": "https://ensono.github.io/stacks-secops-bok/mcp/",
      "description": "Ensono Stacks SecOps Body of Knowledge"
    }
  }
}
```

For local development:

```json
{
  "servers": {
    "stacks-secops-bok-local": {
      "type": "http",
      "url": "http://localhost:1313/mcp/",
      "description": "Ensono Stacks SecOps BOK (Local)"
    }
  }
}
```

## JSON Structure

The generated `index.json` includes:

### Top-Level Fields

- `version`: MCP schema version
- `name`: Documentation site title
- `description`: Brief description for MCP tools
- `baseURL`: Site base URL
- `generatedAt`: ISO 8601 timestamp of generation
- `capabilities`: What operations are supported (search, browse, context)
- `metadata`: Author, language, topics, source repository

### Sections Array

Each section includes:

- `id`: Section identifier (e.g., "basics", "ecosystems")
- `title`: Human-readable section name
- `path`: Relative path to section
- `weight`: Ordering weight
- `description`: Section summary
- `pages`: Array of page objects

### Page Objects

Each page includes:

- `id`: MD5 hash of permalink (unique identifier)
- `title`: Page title
- `path`: Relative path
- `url`: Full URL
- `weight`: Ordering weight within section
- `summary`: Truncated summary (200 chars)
- `keywords`: Array of relevant keywords
- `content`: Object with:
  - `wordCount`: Total words
  - `readingTime`: Estimated minutes
  - `plainText`: Full page content (markdown stripped)
- `metadata`: Last modified, publish date, section name

### Search Index

Optimized for full-text search:

- `entries`: Array of all pages with:
  - `id`: Page identifier
  - `title`: Page title
  - `section`: Section name
  - `url`: Full URL
  - `content`: Full plaintext content
  - `keywords`: Space-separated keyword string

### Tools Definitions

MCP tool interfaces for AI agents:

- `query`: Search documentation by keyword/phrase
- `getPage`: Retrieve specific page by ID or path
- `browse`: List pages in a section

## Use Cases

### AI Coding Assistants

GitHub Copilot and other AI assistants can:

1. **Search for specific topics**: "How do I update NodeJS dependencies?"
2. **Retrieve procedures**: "What's the Java update runbook?"
3. **Lookup references**: "Show me Dependabot configuration guidance"
4. **Context-aware suggestions**: Provide relevant security ops guidance based on current work

### Example Queries

```text
User: "How do I handle CVE disclosures for Python projects?"

AI: [Queries MCP endpoint]
    → Searches for "CVE Python" in content
    → Returns content/ecosystems/python.md
    → Provides relevant update procedures
```

```text
User: "What's our branch naming convention for security fixes?"

AI: [Queries MCP endpoint]
    → Searches for "branch security hotfix" in content
    → Returns content/basics/git.md
    → Cites specific guidance on hotfix/* prefix
```

### Custom MCP Clients

You can build custom tools that consume this endpoint:

```python
import requests

# Fetch MCP index
response = requests.get("https://ensono.github.io/stacks-secops-bok/mcp/index.json")
bok_index = response.json()

# Search for content
def search_bok(query):
    results = []
    for entry in bok_index["searchIndex"]["entries"]:
        if query.lower() in entry["content"].lower():
            results.append({
                "title": entry["title"],
                "section": entry["section"],
                "url": entry["url"]
            })
    return results

# Example: Find all references to Dependabot
results = search_bok("dependabot")
for result in results:
    print(f"{result['title']} ({result['section']}): {result['url']}")
```

## Updating the MCP Endpoint

The MCP endpoint is automatically regenerated on every Hugo build:

1. **GitHub Actions**: Automatically updates on push to `main` branch
2. **Local Development**: Regenerates on `hugo` or `hugo serve` commands
3. **Manual Build**: Run `hugo --minify` to generate optimized output

## Security Considerations

### Content Access

The MCP endpoint exposes all published documentation content:

- ✅ **Public information**: Procedures, guidelines, best practices
- ✅ **Repository references**: Public GitHub repository names
- ❌ **No secrets**: Never include credentials, tokens, or sensitive data
- ❌ **No internal systems**: Avoid documenting internal-only infrastructure

### Rate Limiting

When deployed to GitHub Pages:

- Served as static files (no backend rate limiting)
- Subject to GitHub Pages bandwidth limits
- Consider adding Cloudflare or similar CDN for high-traffic scenarios

### Caching

The JSON file includes a `generatedAt` timestamp:

- MCP clients should cache responses
- Check `Last-Modified` header to avoid unnecessary downloads
- Recommended cache TTL: 1 hour for production, 5 minutes for development

## Troubleshooting

### MCP endpoint not generated

Check Hugo configuration in `hugo.toml`:

```toml
[outputs]
  home = ["HTML", "RSS", "MCP"]

[outputFormats.MCP]
  name = "MCP"
  mediaType = "application/json"
  baseName = "index"
  isPlainText = true
  path = "mcp"
```

### JSON validation errors

Use a JSON validator:

```bash
curl https://ensono.github.io/stacks-secops-bok/mcp/index.json | jq .
```

### Empty content fields

Ensure pages have content (not just front matter):

- Pages must contain markdown content
- Hugo's `.Plain` variable extracts text
- Empty pages will have empty `content.plainText`

### Missing sections

Check that sections have `_index.md` files:

```text
content/
  basics/
    _index.md  ← Required for section to appear
    git.md
  ecosystems/
    _index.md  ← Required
    nodejs.md
```

## Further Resources

- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [Hugo Output Formats Documentation](https://gohugo.io/templates/output-formats/)
- [MCP Servers Documentation](https://ensono.github.io/stacks-secops-bok/agentic-security/mcp-servers/)
- [GitHub Copilot Configuration](https://ensono.github.io/stacks-secops-bok/agentic-security/copilot-instructions/)
