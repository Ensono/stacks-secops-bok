# Copilot Instructions for Stacks SecOps BOK

## Project Overview

This is a **Hugo-based documentation site** for Ensono's Security Operations Body of Knowledge (BOK). It uses the **Relearn theme** with custom Ensono branding and provides security operations guidance across multiple technology ecosystems.

> **Security Notice**: This project documents security operations practices. When working with this codebase, always follow the comprehensive security guidelines in [`copilot-security-instructions.md`](./copilot-security-instructions.md).

## Architecture & Structure

### Content Organization

- `content/`: Hugo content with **front matter weights** for ordering (e.g., `weight = 1.2`)
- Use **archetype** front matter: `"home"`, `"chapter"` for section indexes
- Nested structure: `basics/`, `ecosystems/`, `scope/` with `_index.md` files as section roots

### Key Patterns

- **Hugo shortcodes** in `layouts/shortcodes/`: Use `{{< logo-jdk >}}`, `{{< tick >}}` etc. for consistent iconography
- **Custom themes** in `assets/css/`: `theme-ensono-light.css` and `theme-ensono-dark.css` override Relearn defaults
- **Markdown extensions**: Use Hugo alerts syntax `> [!INFO]`, `> [!IMPORTANT]`, `> [!TIP]`

### Content Conventions

- Each ecosystem page (Java, NodeJS, Python) follows pattern: overview → local testing → update procedures
- Maven projects use centralized version properties in `pom.xml` for dependency management
- Include specific commands and code examples (e.g., `./mvnw clean install test -Plocal`)

## Development Workflow

### Local Development

```bash
hugo serve  # Start development server
```

### Content Creation

- New pages need proper front matter with `title`, `weight`, `archetype`
- Use relative weights (e.g., 3.1, 3.2, 3.3) for ordering within sections
- Reference ecosystem logos via shortcodes: `{{< logo-dotnet >}}`, `{{< logo-python >}}` etc.

### Deployment

- **GitHub Actions** auto-deploys to GitHub Pages on main branch pushes
- Uses Hugo Extended v0.134.3 with Dart Sass for CSS processing
- **Go modules** manage theme dependency (McShelby/hugo-theme-relearn)

## Key Configuration Files

- `hugo.toml`: Base URL, theme config, custom theme variants `['auto', 'ensono-light', 'ensono-dark']`
- `go.mod`: Theme dependency management
- `.github/dependabot.yml`: Weekly Go module updates, monthly GitHub Actions updates
- `.github/workflows/hugo.yml`: Automated deployment pipeline

## Content Guidelines

- **Informative over prescriptive**: Provide knowledge, not strict rules
- Include **technology-specific sections** with practical examples
- Reference specific Ensono Stacks repositories by name
- Use tables with shortcode icons for repository matrices (see `content/scope/_index.md`)
- Include both manual processes and automation recommendations

## Security Requirements

This project is subject to strict security controls as it documents security operations practices. **All code contributions must comply with:**

- **GPG commit signing** - Never bypass or disable signing requirements
- **Branch protection rules** - Use feature branches and pull requests for protected branches
- **Change control processes** - Follow formal change management for production configurations
- **Security standards compliance** - Adhere to ISO 27001, NIST, PCI DSS, FIPS, and other applicable standards

For complete security guidelines, reference [`copilot-security-instructions.md`](./copilot-security-instructions.md) before making any changes.

## Custom Elements

- Logo shortcodes available: `azdo`, `docker`, `dotnet`, `go`, `jdk`, `js`, `pwsh`, `python`, `tf`
- `{{< tick >}}` shortcode for checkmarks in tables
- Custom CSS variables in theme files for Ensono branding (`--PRIMARY-color: #6941eb`)
