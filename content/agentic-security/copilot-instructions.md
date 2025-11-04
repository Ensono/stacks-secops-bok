+++
title = "Copilot Instructions"
weight = 4.2
+++

## Overview

GitHub Copilot Instructions (`.github/copilot-instructions.md`) provide context-aware guidance to AI coding assistants, enabling them to understand your project's architecture, conventions, and requirements. When properly configured with security guidelines, copilot instructions become a powerful tool for maintaining code quality and security posture across your codebase.

> [!IMPORTANT]  
> Copilot instructions should **complement**, not replace, security policies. Always pair project-specific instructions with comprehensive security guidelines like those in `copilot-security-instructions.md`.

## When to Create Copilot Instructions

Consider adding copilot instructions when your project:

- **Uses specific frameworks or tools** with unique patterns (e.g., Hugo, React, Django)
- **Has established conventions** for code organization, naming, or documentation
- **Requires compliance** with security standards or regulatory requirements
- **Involves multiple technologies** that need coordinated guidance
- **Has custom tooling** or build processes that AI agents should understand
- **Maintains architectural patterns** that should be preserved

## Structure and Content

### Essential Components

A well-structured copilot-instructions.md file should include:

1. **Project Overview** - Brief description of the project type and purpose
2. **Architecture & Structure** - Key directories, file organization, and patterns
3. **Development Workflow** - Commands, tools, and processes developers use
4. **Key Configuration Files** - Important configs and their purposes
5. **Content/Code Guidelines** - Conventions, standards, and best practices
6. **Custom Elements** - Project-specific components or extensions
7. **Security Notice** - Reference to comprehensive security instructions

### Example Structure

From this project's own `copilot-instructions.md`:

```markdown
# Copilot Instructions for [Project Name]

## Project Overview

This is a **Hugo-based documentation site** for [purpose].

> **Security Notice**: [Reference to security instructions]

## Architecture & Structure

### Content Organization
- `content/`: [Description]
- Use **[key pattern]** for [purpose]

### Key Patterns
- [Pattern 1]: [Explanation]
- [Pattern 2]: [Explanation]

## Development Workflow

### Local Development
```bash
hugo serve  # Start development server
```

### Deployment
- [Deployment process description]

## Custom Elements
- [Custom component 1]
- [Custom component 2]
```

## Integrating Security Guidelines

### Layered Security Approach

Use a **two-file approach** for comprehensive security:

1. **`copilot-instructions.md`** - Project-specific technical guidance
2. **`copilot-security-instructions.md`** - Comprehensive security framework

This separation provides:

- ✅ **Maintainability** - Security policies update independently from technical docs
- ✅ **Reusability** - Security instructions can be shared across projects
- ✅ **Clarity** - Developers see technical guidance first, security details on-demand
- ✅ **Compliance** - Security framework remains authoritative reference

### Referencing Security Instructions

Include a prominent security notice in your copilot-instructions.md:

```markdown
> **Security Notice**: This project [security context statement]. 
> When working with this codebase, always follow the comprehensive security 
> guidelines in [`copilot-security-instructions.md`](./copilot-security-instructions.md).
```

**Example from this project:**

```markdown
> **Security Notice**: This project documents security operations practices. 
> When working with this codebase, always follow the comprehensive security 
> guidelines in [`copilot-security-instructions.md`](./copilot-security-instructions.md).
```

### Cross-Referencing Patterns

Within your copilot instructions, reference specific security requirements where relevant:

```markdown
## Deployment Workflow

- **GitHub Actions** auto-deploys to GitHub Pages on main branch pushes
- Uses Hugo Extended v0.152.3 with Dart Sass for CSS processing

**Security Requirements:**
- All commits must be GPG signed (see `copilot-security-instructions.md` §1)
- Use feature branches and pull requests for protected branches (see §2)
- Follow change control for production configurations (see §3)
```

## Security Instructions Framework

The `copilot-security-instructions.md` file should establish mandatory security controls. This project's security instructions include 10 comprehensive sections:

### 1. GPG Commit Signing Requirements

**Mandatory behaviors:**
- Never disable, bypass, or suggest disabling GPG signing
- Never use flags like `--no-gpg-sign`, `-n`, or `git config commit.gpgsign false`
- Always preserve existing GPG signing configurations

**Failure handling:**
- Halt operations immediately if signing fails
- Provide specific diagnostic commands (not workarounds)
- Wait for user confirmation before retrying

### 2. Branch Protection and Pull Request Requirements

**Mandatory behaviors:**
- Never suggest force pushing to protected branches
- Never bypass branch protection rules
- Never commit directly to protected branches
- Always follow standard SDLC workflows

**Required workflow:**
1. Create feature branch with descriptive naming
2. Implement changes on feature branch
3. Create Pull Request with detailed description
4. Request required reviews based on change type
5. Wait for CI/CD checks to pass
6. Merge via approved PR process

### 3. Production Configuration Change Control

**Mandatory behaviors:**
- Never make direct changes to production configurations
- Never skip change control processes
- Always follow formal Change Management process
- Always assist in producing required documentation

**Change control process:**
1. Document change request with risk assessment
2. Validate in non-production environments
3. Obtain required approvals
4. Implement during approved change window
5. Document results and verify rollback procedures

### 4. Authentication and Authorization Controls

**Prohibited actions:**
- Disabling authentication mechanisms
- Removing authorization checks
- Hard-coding credentials or bypassing credential management
- Reducing security levels to "fix" access issues
- Creating overly permissive access policies

**Required approaches:**
- Use established authentication middleware
- Implement proper authorization checks
- Secure credential management via environment variables/secrets managers
- Apply principle of least privilege
- Add audit logging for security events

### 5. Security Standards Compliance

**Applicable standards:**
- ISO 27001:2013 - Information Security Management
- NIST SP 800-53 - Security and Privacy Controls
- FIPS 140-2/140-3 - Cryptographic Standards
- PCI DSS v4.0 - Payment Card Industry Data Security
- CIS Controls - Center for Internet Security
- Cyber Essentials (UK) - UK Government Cybersecurity
- OWASP Top 10 - Web Application Security Risks
- GDPR - Data Protection Regulation

**Automatic compliance scanning:**
- Cryptographic standards violations (FIPS, ISO 27001)
- Data protection violations (PCI DSS, GDPR)
- Injection vulnerabilities (OWASP Top 10, NIST)
- Access control violations (ISO 27001, NIST, CIS)
- Insecure configuration (CIS Benchmarks, NIST)

### 6. Audit and Logging Requirements

**Mandatory behaviors:**
- Always include audit logging for security-relevant actions
- Never suggest disabling or circumventing audit logs
- Always log to immutable storage where available

**Required audit events:**
- Authentication: login, logout, failed_login, password_change
- Authorization: access_granted, access_denied, privilege_escalation
- Data access: read_sensitive, update_sensitive, delete_data, export_data
- Configuration: config_change, feature_flag_toggle, deployment
- Security: encryption_key_rotation, certificate_renewal, security_scan

### 7. Incident Response

**Required process when security incidents occur:**
1. Declare incident to Security Team
2. Activate Incident Response Plan
3. Obtain emergency change approval
4. Document all actions in incident log
5. Implement changes with audit trail
6. Schedule post-incident review

**Prohibited actions:**
- Bypassing security controls during incidents
- Skipping approval processes
- Implementing insecure workarounds

### 8. Code Review Checklist

**Pre-submission verification:**
- No security controls disabled or bypassed
- No hard-coded credentials or secrets
- No weak cryptographic algorithms
- Proper input validation and sanitization
- Proper authentication and authorization checks
- Secure configuration (cookies, CORS, headers)
- No sensitive data in logs
- Audit logging for security events
- Compliance with applicable standards
- No OWASP Top 10 vulnerabilities
- Proper error handling without information disclosure
- Secure dependencies (no known CVEs)
- Data encryption at rest and in transit
- Principle of least privilege applied

### 9. Security Documentation Requirements

**Required documentation for code changes:**
1. Security Impact Statement (authentication, authorization, data protection, audit logging, compliance)
2. Threat Model Considerations (mitigated threats, new attack surfaces, security assumptions)
3. Compliance Mapping (satisfied standards, implemented controls, audit evidence)

### 10. Escalation Procedures

**When users insist on bypassing security controls:**
- Document the violation attempts
- Explain required approvals (CISO, risk acceptance, compensating controls)
- Note conversation may be subject to security audit
- Provide contact information for security team
- Refuse to proceed without proper authorization

## Real-World Example: This Project

This documentation site uses both instruction files to guide AI agents:

### copilot-instructions.md (67 lines)

**Focus:** Hugo-specific technical guidance

```markdown
## Architecture & Structure

### Content Organization
- `content/`: Hugo content with **front matter weights** for ordering
- Use **archetype** front matter: `"home"`, `"chapter"` for section indexes
- Nested structure: `basics/`, `ecosystems/`, `scope/` with `_index.md` files

### Key Patterns
- **Hugo shortcodes** in `layouts/shortcodes/`: Use `{{< logo-jdk >}}`, `{{< tick >}}`
- **Custom themes** in `assets/css/`: `theme-ensono-light.css` override Relearn defaults
- **Markdown extensions**: Use Hugo alerts syntax `> [!INFO]`, `> [!IMPORTANT]`

## Development Workflow

### Local Development
```bash
hugo serve  # Start development server
```

### Content Creation
- New pages need proper front matter with `title`, `weight`, `archetype`
- Use relative weights (e.g., 3.1, 3.2, 3.3) for ordering within sections
```

### copilot-security-instructions.md (600+ lines)

**Focus:** Comprehensive security framework

```markdown
# GitHub Copilot Security and Compliance Instructions

## Core Security Principles

GitHub Copilot MUST operate within established security controls and 
compliance frameworks. Under NO circumstances should Copilot suggest, 
implement, or assist in circumventing security measures.

## 1. GPG Commit Signing Requirements

### Mandatory Behavior
- **NEVER** disable, bypass, or suggest disabling GPG signing requirements
- **NEVER** use flags like `--no-gpg-sign`, `-n`, or `git config commit.gpgsign false`
- **ALWAYS** preserve existing GPG signing configurations

[600+ more lines of detailed security requirements]
```

### How They Work Together

When an AI agent works on this project:

1. **Reads copilot-instructions.md first** - Gets Hugo-specific context (shortcodes, front matter, archetype patterns)
2. **Sees security notice** - Understands security requirements apply
3. **Consults copilot-security-instructions.md** - When making commits, branch changes, or security-relevant modifications
4. **Applies both contexts** - Suggests Hugo content changes that comply with security requirements

**Example interaction:**

```text
User: "Can you add a new page about Docker security?"

Agent (applies copilot-instructions.md):
- Creates file with proper front matter (title, weight, archetype)
- Uses Hugo alert syntax for callouts
- Includes relevant shortcodes ({{< logo-docker >}})
- Follows content conventions for ecosystem pages

Agent (applies copilot-security-instructions.md):
- Verifies GPG signing is enabled before committing
- Creates feature branch instead of committing to main
- Includes security impact statement in commit message
- Ensures no sensitive information in code examples
```

## Best Practices

### Keep Instructions Focused

**Do:**
- ✅ Document project-specific patterns and conventions
- ✅ Explain architectural decisions and structure
- ✅ Provide examples of common tasks
- ✅ Reference security instructions for comprehensive policies

**Don't:**
- ❌ Duplicate security policies in both files
- ❌ Include overly detailed API documentation
- ❌ Write instructions that conflict with security guidelines
- ❌ Create instructions that become outdated quickly

### Maintain Both Files

**copilot-instructions.md updates when:**
- Project architecture changes
- New frameworks or tools are adopted
- Conventions evolve
- Custom components are added

**copilot-security-instructions.md updates when:**
- Security standards change
- Compliance requirements update
- New threats emerge
- Organizational policies evolve

### Test Instructions

Periodically verify your copilot instructions by:

1. **Starting a fresh conversation** with your AI assistant
2. **Requesting common tasks** to see if guidance is followed
3. **Checking security compliance** in suggested code
4. **Verifying conventions** are preserved
5. **Testing edge cases** that might confuse the AI

### Version Control

Treat copilot instructions as code:

- ✅ Commit changes with descriptive messages
- ✅ Review updates in pull requests
- ✅ Test changes before merging
- ✅ Document major revisions
- ✅ Tag stable versions

## Common Patterns

### Framework-Specific Instructions

**For React projects:**

```markdown
## Component Patterns

- Use **functional components** with hooks
- Follow **container/presentational** pattern
- Store shared state in **Context** or **Redux**
- Use **TypeScript** for type safety

### File Organization
- `components/`: Reusable UI components
- `containers/`: Connected components with business logic
- `hooks/`: Custom React hooks
- `utils/`: Utility functions and helpers
```

**For Python projects:**

```markdown
## Code Conventions

- Follow **PEP 8** style guide
- Use **type hints** for function signatures
- Prefer **dataclasses** over dictionaries for structured data
- Use **pathlib** instead of `os.path`

### Project Structure
- `src/`: Source code modules
- `tests/`: Unit and integration tests (pytest)
- `docs/`: Sphinx documentation
- `scripts/`: Utility scripts
```

### Multi-Technology Projects

For projects combining multiple technologies:

```markdown
## Technology Stack

- **Backend:** Django REST Framework (Python 3.11+)
- **Frontend:** React 18 with TypeScript
- **Database:** PostgreSQL 15
- **Infrastructure:** Terraform on AWS

## Technology-Specific Guidance

### Django Backend
- Use **class-based views** for complex logic
- Implement **custom permissions** for authorization
- Follow **12-factor app** configuration principles

### React Frontend
- Use **React Query** for server state management
- Implement **code splitting** for performance
- Follow **accessibility best practices** (WCAG 2.1 AA)

### Integration Points
- Backend exposes **REST API** at `/api/v1/`
- Frontend uses **JWT tokens** for authentication
- WebSocket connections via **Django Channels** for real-time features
```

## Measuring Effectiveness

Your copilot instructions are effective when:

- ✅ AI agents consistently follow project conventions
- ✅ Generated code matches architectural patterns
- ✅ Security requirements are automatically applied
- ✅ New team members onboard faster with AI assistance
- ✅ Code reviews find fewer convention violations
- ✅ Compliance audits show consistent adherence to standards

## Further Resources

- **GitHub Copilot Documentation**: [docs.github.com/copilot](https://docs.github.com/copilot)
- **Security Best Practices**: See [`copilot-security-instructions.md`](./copilot-security-instructions.md)
- **MCP Servers**: See [MCP Servers](./mcp-servers.md) for AI-assisted research and automation
- **Dependency Management**: See [Dependency Management](./dependency-management.md) for agentic update workflows
