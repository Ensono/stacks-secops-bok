+++
title = "MCP Documentation Endpoint"
weight = 4.4
+++

## Overview

This documentation site generates a **Model Context Protocol (MCP)** compatible JSON endpoint, making the entire Body of Knowledge machine-readable for AI coding assistants and automated tools.

## Accessing the Endpoint

### Production URL

```text
https://ensono.github.io/stacks-secops-bok/mcp/index.json
```

### Local Development

When running `hugo serve`:

```text
http://localhost:1313/mcp/index.json
```

> [!TIP]
> The MCP endpoint automatically regenerates whenever Hugo builds the site, ensuring the AI-accessible content stays in sync with the human-readable documentation.

## Configuring VS Code

Add the endpoint to your `.vscode/mcp.json`:

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

After configuration, GitHub Copilot and other MCP-compatible AI assistants can:

- **Search documentation** for relevant security operations procedures
- **Retrieve specific pages** by topic or section
- **Provide context-aware suggestions** based on documented best practices
- **Answer questions** using authoritative content from the BOK

## Example AI Interactions

### Searching for Procedures

```text
User: "How do I update NodeJS dependencies in Ensono Stacks projects?"

AI: [Queries MCP endpoint]
    → Searches for "NodeJS dependencies update" in searchIndex
    → Returns content from ecosystems/nodejs.md
    → Provides specific npm audit fix procedures
    → Cites framework-specific guidance for NX and Next.js
```

### Retrieving Git Conventions

```text
User: "What's the branch naming convention for security hotfixes?"

AI: [Queries MCP endpoint]
    → Searches for "branch naming security hotfix" in content
    → Returns content from basics/git.md
    → Cites: "use hotfix/ prefix for security branches"
    → Example: hotfix/cve-2024-45590
```

### Context-Aware Code Suggestions

```text
User: [Working in a Python project]
      "I need to update packages for a security vulnerability"

AI: [Queries MCP endpoint]
    → Identifies Python ecosystem context
    → Retrieves ecosystems/python.md content
    → Suggests: "Use poetry update for poetry-based projects"
    → Provides complete runbook with venv setup
```

## JSON Structure

The MCP endpoint exports a comprehensive, structured representation of all documentation:

### Top-Level Structure

```json
{
  "version": "1.0.0",
  "name": "Ensono Stacks SecOps Body of Knowledge",
  "description": "MCP-compatible documentation index",
  "baseURL": "https://ensono.github.io/stacks-secops-bok",
  "generatedAt": "2025-11-04T13:00:00Z",
  "capabilities": {
    "search": true,
    "browse": true,
    "context": true
  },
  "sections": [...],
  "searchIndex": {...},
  "tools": {...}
}
```

### Sections Array

Each documentation section includes:

```json
{
  "id": "ecosystems",
  "title": "Ecosystems",
  "path": "/ecosystems/",
  "weight": 3,
  "description": "Technology-specific security operations guidance",
  "pages": [
    {
      "id": "a3f8e9b...",
      "title": "Java",
      "path": "/ecosystems/java/",
      "url": "https://ensono.github.io/stacks-secops-bok/ecosystems/java/",
      "weight": 3.3,
      "summary": "Java Stacks Projects are driven via Maven based workflows...",
      "keywords": ["ecosystems", "java", "maven"],
      "content": {
        "wordCount": 234,
        "readingTime": 2,
        "plainText": "Full page content without markdown formatting..."
      },
      "metadata": {
        "section": "Ecosystems",
        "lastModified": "2025-11-04T10:00:00Z",
        "publishDate": "2025-11-03T15:30:00Z"
      }
    }
  ]
}
```

### Search Index

Optimized for full-text search:

```json
{
  "searchIndex": {
    "description": "Searchable content from Stacks SecOps BOK",
    "entries": [
      {
        "id": "a3f8e9b...",
        "title": "Java",
        "section": "ecosystems",
        "url": "https://ensono.github.io/.../ecosystems/java/",
        "content": "Full plaintext content for searching...",
        "keywords": "ecosystems java maven pom.xml dependencies"
      }
    ]
  }
}
```

### Tool Definitions

MCP-compatible tool interfaces:

```json
{
  "tools": {
    "query": {
      "description": "Search the documentation content",
      "parameters": {
        "query": "string",
        "section": "string (optional)"
      }
    },
    "getPage": {
      "description": "Retrieve a specific page by ID or path",
      "parameters": {
        "id": "string (page ID)",
        "path": "string (page path)"
      }
    },
    "browse": {
      "description": "Browse documentation by section",
      "parameters": {
        "section": "string"
      }
    }
  }
}
```

## Custom Client Example

Build tools that consume the MCP endpoint:

```python
#!/usr/bin/env python3
"""Query Stacks SecOps BOK via MCP endpoint."""

import requests
import sys

BOK_MCP_URL = "https://ensono.github.io/stacks-secops-bok/mcp/index.json"

def search_bok(query):
    """Search the Body of Knowledge for query string."""
    response = requests.get(BOK_MCP_URL)
    bok_index = response.json()

    results = []
    for entry in bok_index["searchIndex"]["entries"]:
        if query.lower() in entry["content"].lower():
            results.append({
                "title": entry["title"],
                "section": entry["section"],
                "url": entry["url"],
                "excerpt": extract_excerpt(entry["content"], query)
            })

    return results

def extract_excerpt(content, query, context_chars=200):
    """Extract excerpt around query match."""
    lower_content = content.lower()
    lower_query = query.lower()

    pos = lower_content.find(lower_query)
    if pos == -1:
        return content[:context_chars] + "..."

    start = max(0, pos - context_chars // 2)
    end = min(len(content), pos + len(query) + context_chars // 2)

    excerpt = content[start:end]
    if start > 0:
        excerpt = "..." + excerpt
    if end < len(content):
        excerpt = excerpt + "..."

    return excerpt

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: search_bok.py <query>")
        sys.exit(1)

    query = " ".join(sys.argv[1:])
    results = search_bok(query)

    print(f"\nFound {len(results)} results for '{query}':\n")

    for i, result in enumerate(results, 1):
        print(f"{i}. {result['title']} ({result['section']})")
        print(f"   {result['url']}")
        print(f"   {result['excerpt']}\n")
```

**Usage:**

```bash
python search_bok.py "dependabot configuration"
# Found 2 results for 'dependabot configuration':
#
# 1. Dependabot (basics)
#    https://ensono.github.io/stacks-secops-bok/basics/dependabot/
#    ...Dependabot is in use across the majority of stacks repositories...
#
# 2. Dependency Management (agentic-security)
#    https://ensono.github.io/stacks-secops-bok/agentic-security/dependency-management/
#    ...Dependabot automation with AI assistance: version: 2 updates...
```

## Security Considerations

### Content Exposure

The MCP endpoint makes all published documentation publicly accessible in JSON format:

- ✅ **Appropriate content**: Security procedures, best practices, public repository references
- ❌ **Never include**: Credentials, API keys, internal system details, non-public infrastructure

> [!IMPORTANT]
> Review all documentation content to ensure nothing sensitive is inadvertently exposed through the MCP endpoint. The same content rules apply as for the human-readable documentation.

### Rate Limiting

Static file serving via GitHub Pages:

- No server-side rate limiting (static JSON file)
- Subject to GitHub Pages bandwidth limits (100GB/month soft limit)
- Consider implementing client-side caching with appropriate TTLs
- For high-traffic scenarios, add CDN layer (Cloudflare, Fastly)

### Caching Strategy

Recommended caching approach:

```javascript
// Browser/client caching
const CACHE_TTL_SECONDS = 3600; // 1 hour

async function fetchBOK() {
  const cached = localStorage.getItem("bok_mcp_cache");
  const cacheTime = localStorage.getItem("bok_mcp_cache_time");

  if (cached && cacheTime) {
    const age = Date.now() - parseInt(cacheTime);
    if (age < CACHE_TTL_SECONDS * 1000) {
      return JSON.parse(cached);
    }
  }

  const response = await fetch(BOK_MCP_URL);
  const data = await response.json();

  localStorage.setItem("bok_mcp_cache", JSON.stringify(data));
  localStorage.setItem("bok_mcp_cache_time", Date.now().toString());

  return data;
}
```

## Implementation Details

### Hugo Configuration

The MCP endpoint is configured in `hugo.toml`:

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

### Template Processing

The `layouts/index.mcp.json` template:

1. Iterates through all site sections
2. Extracts page metadata (title, weight, summary)
3. Strips markdown formatting using Hugo's `.Plain` variable
4. Calculates word counts and reading times
5. Generates unique page IDs using MD5 hashes
6. Builds searchable index with full content
7. Outputs valid JSON structure

### Build Process

MCP endpoint generation happens automatically:

1. **GitHub Actions**: Triggered on push to `main` branch
2. **Hugo Build**: Processes `index.mcp.json` template
3. **Output**: Generates `public/mcp/index.json`
4. **Deployment**: Published to GitHub Pages with rest of site

### Validation

Check generated JSON is valid:

```bash
# Using curl and jq
curl -s https://ensono.github.io/stacks-secops-bok/mcp/index.json | jq .

# Check specific fields
curl -s https://ensono.github.io/stacks-secops-bok/mcp/index.json | \
  jq '.sections[] | {title, pageCount: (.pages | length)}'
```

## Troubleshooting

### Empty or Missing Content

**Problem**: Page content appears empty in MCP output

**Solutions**:

- Ensure pages have markdown content (not just front matter)
- Check that Hugo's `.Plain` variable can extract text
- Verify shortcodes render properly before text extraction

### Invalid JSON

**Problem**: MCP endpoint returns 404 or invalid JSON

**Solutions**:

- Confirm `hugo.toml` has MCP output format configured
- Check `layouts/index.mcp.json` template syntax
- Run `hugo --minify` locally to test generation
- Review Hugo build logs for template errors

### Missing Sections

**Problem**: Some sections don't appear in MCP output

**Solutions**:

- Verify each section has `_index.md` file
- Check section `weight` is properly set in front matter
- Ensure section is not marked as `draft: true`
- Confirm section contains at least one published page

### Large JSON File Size

**Problem**: MCP JSON file becomes very large

**Solutions**:

- Enable Hugo's `--minify` flag (removes whitespace)
- Consider excluding verbose content from search index
- Implement pagination for large documentation sets
- Use CDN with compression enabled (gzip/brotli)

## Further Resources

- **Complete MCP Documentation**: See `/static/mcp/README.md` for detailed reference
- **MCP Server Configuration**: [MCP Servers](./mcp-servers.md) guide
- **AI Assistant Setup**: [Copilot Instructions](./copilot-instructions.md) documentation
- **Model Context Protocol Spec**: [modelcontextprotocol.io](https://modelcontextprotocol.io/)
- **Hugo Output Formats**: [gohugo.io/templates/output-formats](https://gohugo.io/templates/output-formats/)
