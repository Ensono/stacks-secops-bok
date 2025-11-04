+++
archetype = "chapter"
title = "MCP Servers"
weight = 4.1
+++

Model Context Protocol (MCP) servers extend the capabilities of AI coding assistants by providing structured access to external data sources and tools. For security operations, MCP servers enable agents to interact with vulnerability databases, project management systems, and source control platforms while maintaining security boundaries.

## Recommended MCP Servers

The following MCP servers are recommended for security operations work within Ensono Stacks projects:

### GitHub MCP Server

The GitHub MCP server provides comprehensive access to GitHub repositories, issues, pull requests, and releases. This is essential for security operations as it enables AI agents to:

- Search for vulnerabilities across repositories
- Create and update security-related issues
- Review pull requests for dependency updates
- Check release notes for security patches

**Configuration**:

The GitHub MCP server is typically built into GitHub Copilot and requires no additional configuration beyond GitHub authentication.

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

**Common Use Cases**:

- Searching for outdated dependencies across multiple repositories
- Automating security advisory responses
- Coordinating vulnerability fixes across related projects
- Generating security impact assessments from commit history

### Azure DevOps MCP Server

For organizations using Azure DevOps, this MCP server provides access to work items, pipelines, and repositories. It's particularly valuable when security operations are managed through Azure DevOps.

**Configuration**:

```json
{
  "inputs": [
    {
      "id": "azureDevOpsPat",
      "type": "promptString",
      "description": "Azure DevOps Personal Access Token",
      "password": true
    },
    {
      "id": "ado_org",
      "type": "promptString",
      "description": "Azure DevOps Organization",
      "default": "ensonodigitaluk"
    }
  ],
  "servers": {
    "azure-devops": {
      "command": "npx",
      "args": ["-y", "@azure-devops/mcp", "${input:ado_org}"],
      "env": {
        "AZURE_DEVOPS_PAT": "${input:azureDevOpsPat}"
      }
    }
  }
}
```

**Personal Access Token Setup**:

1. Navigate to Azure DevOps User Settings → Personal Access Tokens
2. Create a new token with appropriate scopes:
   - **Work Items**: Read & Write
   - **Code**: Read (for repository access)
   - **Build**: Read (for pipeline information)
3. Store the token securely using your organization's secrets management approach
4. Configure the token to expire and rotate regularly

> [!IMPORTANT]
> Personal Access Tokens should be treated as credentials. Never commit them to source control, store them in plain text, or share them via insecure channels.

**Common Use Cases**:

- Linking security vulnerabilities to work items
- Tracking security patch deployment through pipelines
- Coordinating security updates across Azure DevOps projects
- Generating security reports from work item queries

### NVD (National Vulnerability Database) MCP Server

The NVD MCP server provides direct access to the NIST National Vulnerability Database, enabling AI agents to query vulnerability information, CVE details, and security advisories.

**Configuration**:

```json
{
  "inputs": [
    {
      "id": "nvdApiKey",
      "type": "promptString",
      "description": "Enter your NVD API Key",
      "password": true
    }
  ],
  "servers": {
    "mcp-nvd": {
      "command": "uvx",
      "args": ["mcp-nvd"],
      "env": {
        "NVD_API_KEY": "${input:nvdApiKey}"
      }
    }
  }
}
```

**API Key Setup**:

1. Visit [NIST NVD API Key Request](https://nvd.nist.gov/developers/request-an-api-key)
2. Complete the registration form with organizational details
3. Receive API key via email (typically within minutes)
4. Configure rate limiting awareness in your workflows

> [!TIP]
> The NVD API has rate limits (50 requests per 30 seconds with a key, 5 without). Design your agentic workflows to respect these limits and cache results when appropriate.

**Common Use Cases**:

- Researching specific CVEs during vulnerability triage
- Validating security scanner findings against authoritative sources
- Enriching security advisories with official CVE data
- Assessing vulnerability severity and exploitability

## Configuring MCP Servers

MCP servers are configured through the `.vscode/mcp.json` file in your workspace. This file should be committed to source control (without secrets) to ensure consistent configuration across team members.

### Complete Configuration Example

```json
{
  "inputs": [
    {
      "id": "azureDevOpsPat",
      "type": "promptString",
      "description": "Azure DevOps Personal Access Token",
      "password": true
    },
    {
      "id": "ado_org",
      "type": "promptString",
      "description": "Azure DevOps Organization",
      "default": "ensonodigitaluk"
    },
    {
      "id": "nvdApiKey",
      "type": "promptString",
      "description": "Enter your NVD API Key",
      "password": true
    }
  ],
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    },
    "azure-devops": {
      "command": "npx",
      "args": ["-y", "@azure-devops/mcp", "${input:ado_org}"],
      "env": {
        "AZURE_DEVOPS_PAT": "${input:azureDevOpsPat}"
      }
    },
    "mcp-nvd": {
      "command": "uvx",
      "args": ["mcp-nvd"],
      "env": {
        "NVD_API_KEY": "${input:nvdApiKey}"
      }
    }
  }
}
```

### Security Considerations

When configuring MCP servers, consider the following security practices:

1. **Credential Management**

   - Use `"password": true` for sensitive inputs to mask them in the UI
   - Store credentials in environment variables or secrets managers
   - Configure credential rotation policies
   - Audit credential usage regularly

2. **Scope Limitation**

   - Grant MCP servers minimum required permissions
   - Use read-only access where possible
   - Limit server access to specific organizations or repositories
   - Review and remove unused MCP server configurations

3. **Network Security**

   - Ensure MCP servers connect over HTTPS
   - Verify SSL/TLS certificates are valid
   - Consider network egress controls for sensitive environments
   - Monitor MCP server traffic for anomalies

4. **Audit and Compliance**
   - Log all MCP server interactions
   - Include MCP configuration in security audits
   - Document which teams use which MCP servers
   - Track API usage against rate limits and quotas

## Extending with Custom MCP Servers

Organizations may develop custom MCP servers for internal tools and data sources. When creating custom MCP servers for security operations:

- Follow the [Model Context Protocol specification](https://modelcontextprotocol.io/)
- Implement proper authentication and authorization
- Design for rate limiting and error handling
- Provide comprehensive logging and audit trails
- Document security implications and access patterns

> [!INFO]
> Custom MCP servers should undergo security review before deployment, particularly if they access sensitive security data or production systems.
