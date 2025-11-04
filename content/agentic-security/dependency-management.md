+++
title = "Dependency Management"
weight = 4.3
+++

## Overview

Dependency management is a critical security practice that traditionally requires significant manual effort: researching updates, reading changelogs, testing changes, and coordinating deployments. Agentic coding with AI assistants and MCP servers transforms this into an assisted, systematic process that maintains security while reducing toil.

> [!IMPORTANT]  
> Agentic dependency management doesn't remove human oversight - it enhances decision-making with automation, research, and validation. Always review AI-suggested changes before committing.

## The Agentic Approach

Traditional dependency management follows a manual, time-consuming workflow:

```text
❌ Traditional Workflow
1. Manually check each dependency for updates
2. Search documentation for breaking changes
3. Read through changelogs across multiple sites
4. Test changes locally
5. Write commit messages
6. Document changes for team
```

Agentic dependency management leverages AI and MCP servers:

```text
✅ Agentic Workflow
1. AI queries MCP servers for latest versions
2. AI retrieves and summarizes release notes
3. AI identifies breaking changes and security fixes
4. AI suggests update plan with risk assessment
5. AI implements changes with proper testing
6. AI generates comprehensive commit messages
```

## MCP Servers for Dependency Research

### GitHub MCP Server

The GitHub MCP server provides authoritative version information and release details.

**Query latest releases:**

```text
User: "What's the latest version of Hugo?"

AI (uses github_get_latest_release):
- Repository: gohugoio/hugo
- Latest release: v0.152.2
- Release date: 2024-11-15
- Breaking changes: [AI summarizes from release notes]
```

**Check multiple dependencies:**

```text
User: "Check for updates to all GitHub Actions in .github/workflows/"

AI (uses grep_search + github_get_latest_release):
1. actions/checkout: v4 → v5 available
2. actions/upload-pages-artifact: v3 → v4 available
3. actions/configure-pages: v4 → v5 available
```

### National Vulnerability Database (NVD) MCP Server

The NVD MCP server identifies known vulnerabilities in dependencies.

**Check for CVEs:**

```text
User: "Check if our dependencies have any CVEs"

AI (uses mcp_nvd for each dependency):
- Hugo v0.134.3: No known CVEs
- actions/checkout@v4: CVE-2024-XXXXX (Medium severity)
  - Recommendation: Upgrade to v5 (fixed)
```

**Vulnerability triage:**

```text
User: "What's the severity of CVE-2024-XXXXX?"

AI (uses mcp_nvd):
- CVSS Base Score: 5.3 (Medium)
- Attack Vector: Network
- Impact: Information Disclosure
- Fixed in: v5.0.0
- Recommendation: Upgrade to v5+ (available now)
```

### Azure DevOps MCP Server

For projects using Azure DevOps, the Azure DevOps MCP server manages work items and pipelines.

**Create security patch work items:**

```text
User: "Create a work item for patching CVE-2024-XXXXX"

AI (uses mcp_azure_devops):
- Creates: Security Patch work item
- Title: "Patch CVE-2024-XXXXX in actions/checkout"
- Priority: High (based on CVSS score)
- Links: CVE details, affected pipeline, fix version
```

## Real-World Example: Updating This Project

This project's dependencies were recently updated using the agentic approach. Here's how it worked:

### Step 1: Discovery

**User request:**

```text
"Can you use the MCP servers and review the content of this repository 
updating any dependencies to their latest version?"
```

**AI analysis:**

```text
AI: I'll use the GitHub MCP server to check for updates.

Checking dependencies:
1. Hugo (from .github/workflows/hugo.yml): Currently 0.134.3
2. GitHub Actions (from .github/workflows/hugo.yml): Various v4 actions
3. Pre-commit hooks (from .pre-commit-config.yaml): Various versions
4. Hugo theme (from go.mod): Specific commit hash
```

### Step 2: Version Research

**AI queries GitHub MCP server:**

```text
github_get_latest_release(owner="gohugoio", repo="hugo")
→ Latest: v0.152.2 (released Nov 2024)
→ Current: v0.134.3 (released June 2024)
→ Gap: 18 minor versions

github_get_release_by_tag(owner="gohugoio", repo="hugo", tag="v0.152.2")
→ Release notes retrieved
→ Breaking changes identified: [none affecting this project]
→ New features: Enhanced shortcode processing, improved theme support
```

### Step 3: Risk Assessment

**AI evaluation:**

```text
Hugo Update (0.134.3 → 0.152.2):
- Risk Level: LOW
- Reason: No breaking changes in release notes affecting our usage
- Testing Required: Run `hugo serve` and verify shortcodes render
- Rollback Plan: Pin to 0.134.3 in workflow if issues arise

GitHub Actions Updates (v4 → v5):
- Risk Level: LOW
- Reason: GitHub provides compatibility guarantees
- Testing Required: Verify workflow runs successfully
- Rollback Plan: Revert workflow file commit

Pre-commit Hooks Updates:
- Risk Level: MEDIUM (pre-commit-hooks v4.6.0 → v6.0.0)
- Reason: Major version bump, Python 3.9+ required
- Testing Required: Run `pre-commit run --all-files`
- Rollback Plan: Pin to v4.6.0 if compatibility issues
```

### Step 4: Implementation

**AI-generated update plan:**

```yaml
Updates to implement:
1. .github/workflows/hugo.yml:
   - Hugo: 0.134.3 → 0.152.2
   - actions/checkout: v4 → v5
   - actions/configure-pages: v4 → v5
   - actions/upload-pages-artifact: v3 → v4

2. .pre-commit-config.yaml:
   - pre-commit-hooks: v4.6.0 → v6.0.0
   - markdownlint: v0.41.0 → v0.45.0
   - codespell: v2.3.0 → v2.4.1
   - detect-secrets: v1.5.0 (already latest)

3. go.mod:
   - hugo-theme-relearn: Update to latest commit
```

**AI applies changes:**

```text
AI: Updating .github/workflows/hugo.yml...
✅ Hugo version updated to 0.152.2
✅ GitHub Actions updated to v5

AI: Updating .pre-commit-config.yaml...
✅ Pre-commit hooks updated
⚠️  Note: Requires Python 3.9+

AI: Updating go.mod...
✅ Theme updated to latest commit
```

### Step 5: Validation

**AI-orchestrated testing:**

```bash
# Run pre-commit validation
pre-commit run --all-files
✅ All hooks passed

# Test Hugo build
hugo --minify
✅ Build successful, no errors

# Verify theme rendering
hugo serve
✅ All shortcodes render correctly
✅ Custom themes intact
```

### Step 6: Documentation

**AI-generated commit message:**

```text
chore: Update dependencies to latest versions

Updates all dependencies across the project to their latest stable releases.

Hugo:
- Updated from 0.134.3 to 0.152.2 (18 minor versions)
- No breaking changes affecting this project
- Includes enhanced shortcode processing and theme support

GitHub Actions:
- actions/checkout: v4 → v5
- actions/configure-pages: v4 → v5  
- actions/upload-pages-artifact: v3 → v4
- All actions maintain backward compatibility

Pre-commit Hooks:
- pre-commit-hooks: v4.6.0 → v6.0.0
- markdownlint: v0.41.0 → v0.45.0
- codespell: v2.3.0 → v2.4.1
- Note: pre-commit-hooks v6+ requires Python 3.9+

Hugo Theme:
- Updated hugo-theme-relearn to latest commit
- Commit hash: 52ebe5c56c2b (Nov 2024)

Testing:
- ✅ Pre-commit hooks validated successfully
- ✅ Hugo build completes without errors  
- ✅ All shortcodes and custom themes render correctly
- ✅ GitHub Actions workflow syntax validated

Security:
- GPG signing verified and maintained
- No known CVEs in updated dependencies
- All changes follow change management policies
```

## Dependency Update Workflows

### Regular Maintenance Updates

**Scheduled review cycle:**

```text
User: "Review dependencies for updates (monthly maintenance)"

AI workflow:
1. Enumerate all dependency sources:
   - GitHub Actions in .github/workflows/
   - Pre-commit hooks in .pre-commit-config.yaml
   - Go modules in go.mod
   - npm packages in package.json (if present)
   - Python packages in requirements.txt (if present)

2. Query each dependency via MCP servers:
   - Check latest release
   - Retrieve release notes
   - Identify breaking changes

3. Categorize updates:
   - Patch updates (1.2.3 → 1.2.4): LOW risk, apply automatically
   - Minor updates (1.2.0 → 1.3.0): LOW risk, review release notes
   - Major updates (1.x → 2.x): MEDIUM/HIGH risk, detailed assessment

4. Generate update plan with risk levels

5. Implement changes with testing

6. Create commit with comprehensive documentation
```

### Security Patch Workflow

**Responding to vulnerability disclosures:**

```text
User: "CVE-2024-XXXXX was just published affecting [dependency]. 
       Please patch it."

AI workflow:
1. Query NVD MCP server for CVE details:
   - CVSS score and severity
   - Affected versions
   - Fixed versions
   - Attack vector and impact

2. Check current version against CVE:
   - Determine if project is affected
   - Identify minimum fixed version

3. Query GitHub MCP server for fixed release:
   - Retrieve release notes
   - Check for breaking changes
   - Assess update path (direct vs. multi-step)

4. Risk assessment:
   - Vulnerability severity: [CVSS score]
   - Update risk: [breaking changes analysis]
   - Recommendation: [patch immediately / scheduled / needs planning]

5. If immediate patch required:
   - Create feature branch
   - Apply security update
   - Run automated tests
   - Generate security-focused commit message
   - Create pull request with CVE details
   - Alert security team

6. If scheduled patch acceptable:
   - Create security work item
   - Document in security tracking
   - Schedule for next maintenance window
   - Monitor for exploit activity
```

### Breaking Change Management

**Handling major version updates:**

```text
User: "Update [dependency] to v2.0 (major version bump)"

AI workflow:
1. Retrieve comprehensive release notes:
   - All changes between current and target version
   - Migration guides
   - Deprecation notices

2. Identify breaking changes affecting this project:
   - Search codebase for affected patterns
   - List files requiring modification
   - Estimate scope of changes

3. Create migration plan:
   - Step-by-step update sequence
   - Required code changes
   - Test cases to validate
   - Rollback procedures

4. Implement in stages:
   - Update dependency version
   - Fix compilation errors
   - Refactor deprecated patterns
   - Update tests
   - Validate functionality

5. Document migration:
   - What changed and why
   - Breaking changes encountered
   - Solutions implemented
   - Future maintenance notes
```

## Automation and Tooling Integration

### Dependabot Integration

Combine Dependabot automation with AI assistance:

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "gomod"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
      - "go"
```

**AI-enhanced Dependabot workflow:**

```text
1. Dependabot creates PR for dependency update

2. User: "Review dependabot PR #123"

3. AI workflow:
   - Reads PR description and changelog
   - Queries NVD for security implications
   - Retrieves detailed release notes via GitHub MCP
   - Analyzes breaking changes
   - Checks for affected code patterns
   - Runs test suite
   - Provides recommendation: APPROVE / REQUEST CHANGES / NEEDS REVIEW

4. AI generates review comment:
   "✅ Safe to merge
   
   - No breaking changes affecting our usage
   - No known CVEs
   - Test suite passes
   - Release notes reviewed: [summary]
   
   Recommendation: Approve and merge"
```

### Pre-commit Hook Automation

Use pre-commit hooks to enforce dependency policies:

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: check-dependency-versions
        name: Check for outdated critical dependencies
        entry: python scripts/check_deps.py
        language: python
        pass_filenames: false
```

**AI-generated dependency checking script:**

```python
#!/usr/bin/env python3
"""Check for outdated dependencies with known CVEs."""

import sys
import requests

def check_dependencies():
    """Query NVD for CVEs in current dependencies."""
    # AI generates script that:
    # 1. Parses go.mod, package.json, requirements.txt
    # 2. Queries NVD API for each dependency
    # 3. Reports any HIGH/CRITICAL CVEs
    # 4. Fails commit if critical vulnerabilities found
    pass

if __name__ == "__main__":
    sys.exit(check_dependencies())
```

### CI/CD Pipeline Integration

**Automated dependency checks in pipelines:**

```yaml
# .github/workflows/dependency-check.yml
name: Dependency Security Check

on:
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Monday
  workflow_dispatch:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      
      - name: Check for vulnerabilities
        run: |
          # AI-generated script checks all dependencies
          python scripts/vulnerability_scan.py
          
      - name: Create issues for CVEs
        if: failure()
        uses: actions/github-script@v7
        with:
          script: |
            // AI generates script to create GitHub issues
            // for each discovered vulnerability
```

## Software Currency Best Practices

### Maintain Update Cadence

**Recommended schedule:**

| Dependency Type | Update Frequency | Risk Assessment |
|----------------|------------------|----------------|
| Security patches | Immediate | Always apply (test first) |
| Patch versions (x.y.Z) | Weekly | Low risk, batch updates |
| Minor versions (x.Y.0) | Monthly | Review release notes |
| Major versions (X.0.0) | Quarterly | Detailed planning required |
| Development tools | Monthly | Lower priority, test locally |

### Version Pinning Strategy

**Balance stability with security:**

```toml
# hugo.toml - Pin major version, allow minor/patch
[module]
  [[module.imports]]
    path = "github.com/McShelby/hugo-theme-relearn"
    # Version: ~0.x allows 0.x.y updates, blocks 1.0.0
```

```yaml
# .github/workflows/hugo.yml - Pin minor, allow patch
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v3
  with:
    hugo-version: '0.152.x'  # Allow patch updates
    extended: true
```

```yaml
# .pre-commit-config.yaml - Pin major, specify minimum minor
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0  # Pin to major version 6, update minor/patch as available
```

### Dependency Lifecycle Management

**Track dependency health:**

```text
User: "Generate a dependency health report"

AI generates report:

# Dependency Health Report
Generated: 2024-11-15

## Summary
- Total dependencies: 25
- Up to date: 18 (72%)
- Minor updates available: 5 (20%)
- Major updates available: 2 (8%)
- Known CVEs: 0 (0%)

## Outdated Dependencies

### Minor Updates Available (Low Risk)
1. markdownlint: v0.45.0 → v0.46.1
   - Release: 2024-10-20
   - Changes: Bug fixes, performance improvements
   - Recommendation: Update in next maintenance window

### Major Updates Available (Review Required)
1. Hugo: v0.152.2 → v1.0.0
   - Release: 2024-11-01
   - Changes: [breaking changes listed]
   - Recommendation: Schedule for Q1 2025, requires migration planning

## Dependency Age Analysis

### Critical Path Dependencies
- Hugo: 15 days old (excellent)
- actions/checkout: 30 days old (excellent)
- pre-commit-hooks: 45 days old (good)

### Development Dependencies
- markdownlint: 25 days old (good)
- codespell: 60 days old (acceptable)

## Recommendations
1. Schedule batch update for 5 minor version bumps (1-2 hours)
2. Begin planning for Hugo v1.0.0 migration (Q1 2025)
3. All dependencies within acceptable currency window
```

## Vulnerability Management

### Proactive Scanning

**Regular vulnerability assessments:**

```text
User: "Scan all dependencies for known vulnerabilities"

AI workflow:
1. Enumerate all dependencies from:
   - go.mod / go.sum
   - package.json / package-lock.json
   - requirements.txt / Pipfile.lock
   - .github/workflows/*.yml (GitHub Actions)
   - Dockerfile (base images)

2. Query NVD MCP server for each dependency:
   - Check against CVE database
   - Retrieve CVSS scores
   - Identify affected versions

3. Generate vulnerability report:
   - HIGH/CRITICAL: Immediate action required
   - MEDIUM: Schedule for next sprint
   - LOW: Include in regular maintenance

4. Create remediation plan:
   - Update paths for each vulnerability
   - Risk assessment for each update
   - Testing requirements
   - Rollback procedures
```

### Incident Response

**Responding to zero-day vulnerabilities:**

```text
⚠️ Zero-day disclosed: Critical RCE in [dependency] v1.2.3

User: "Emergency patch for CVE-2024-XXXXX"

AI incident response workflow:
1. Immediate assessment:
   - Query NVD for CVE details (CVSS 9.8 CRITICAL)
   - Check if project uses affected dependency
   - Identify all locations where dependency is used
   - Determine exploitation potential in our context

2. Mitigation options:
   a) Update to patched version (if available)
   b) Apply vendor-provided hotfix
   c) Implement temporary compensating controls
   d) Disable affected functionality (last resort)

3. Emergency patch implementation:
   - Create emergency branch: hotfix/cve-2024-xxxxx
   - Apply security update
   - Run critical path tests only (skip full suite for speed)
   - Generate security-focused commit message
   - Create PR with "SECURITY" label
   - Notify security team with details

4. Verification:
   - Confirm vulnerability no longer exploitable
   - Verify no functionality regression
   - Document remediation in security log
   - Schedule full test suite for next business day

5. Post-incident:
   - Update vulnerability tracking
   - Document lessons learned
   - Review detection capabilities
   - Improve monitoring for similar issues
```

## Compliance and Audit

### Audit Trail

**Maintain comprehensive dependency change logs:**

```text
AI-generated audit log:

# Dependency Change Log - 2024

## 2024-11-15: Routine Maintenance Update
- **Type:** Scheduled maintenance
- **Approver:** Engineering Lead
- **Changes:**
  - Hugo: 0.134.3 → 0.152.2 (18 minor versions)
  - actions/checkout: v4 → v5
  - pre-commit-hooks: v4.6.0 → v6.0.0
- **Risk Assessment:** Low (no breaking changes)
- **Testing:** ✅ All tests passed
- **CVEs Addressed:** None
- **Commit:** 71eb796

## 2024-10-22: Security Patch
- **Type:** Emergency security patch
- **Approver:** CISO
- **Changes:**
  - actions/checkout: v4.1.0 → v4.1.1
- **Risk Assessment:** Low (patch only)
- **CVEs Addressed:** CVE-2024-XXXXX (Medium)
- **Testing:** ✅ Critical path verified
- **Commit:** a3f9823
```

### Compliance Reporting

**Generate compliance documentation:**

```text
User: "Generate dependency compliance report for ISO 27001 audit"

AI generates:

# Dependency Management Compliance Report
Audit Period: Q4 2024
Standard: ISO 27001:2013 Clause 12.6.2 (Technical Vulnerability Management)

## Control Objectives Met

✅ **Regular Vulnerability Scanning**
- Frequency: Weekly automated scans
- Tool: NVD MCP server + GitHub Dependabot
- Last scan: 2024-11-15
- Findings: 0 HIGH/CRITICAL vulnerabilities

✅ **Timely Patching Process**
- CRITICAL vulnerabilities: Patched within 24 hours
- HIGH vulnerabilities: Patched within 7 days
- MEDIUM vulnerabilities: Patched within 30 days
- Evidence: Dependency change log shows all patches applied within SLA

✅ **Risk Assessment for Updates**
- All dependency updates include risk assessment
- Breaking change analysis documented
- Rollback procedures defined
- Evidence: Commit messages show risk evaluation

✅ **Audit Trail Maintained**
- Complete change log for all dependency updates
- Approver recorded for each change
- Testing results documented
- Evidence: Git history with GPG-signed commits

## Dependencies Inventory
- Total: 25 dependencies
- High-risk: 5 (internet-facing, security-critical)
- Medium-risk: 12 (build/deployment)
- Low-risk: 8 (development only)

## Vulnerability Metrics
- Total CVEs discovered: 3
- CVEs remediated: 3 (100%)
- Average time to remediation: 4.2 days
- Target SLA: 7 days (ACHIEVED)

## Recommendations
1. Continue current scanning cadence
2. Consider adding SBOM generation for supply chain visibility
3. Implement automated dependency approval workflow
```

## Summary

Agentic dependency management combines AI assistance with MCP servers to create an efficient, secure, and auditable update process:

**Key Benefits:**
- ✅ **Automated research** - MCP servers provide authoritative version and vulnerability data
- ✅ **Risk assessment** - AI analyzes release notes and breaking changes
- ✅ **Comprehensive documentation** - Detailed commit messages and audit trails
- ✅ **Security-first** - Immediate vulnerability patching with proper testing
- ✅ **Compliance-ready** - Complete audit trails for regulatory requirements

**Implementation Checklist:**
- [ ] Configure MCP servers (GitHub, NVD, Azure DevOps)
- [ ] Set up Dependabot for automated PR creation
- [ ] Implement pre-commit hooks for dependency validation
- [ ] Define update cadence (weekly/monthly/quarterly)
- [ ] Establish version pinning strategy
- [ ] Create emergency patch procedures
- [ ] Document audit trail requirements
- [ ] Train team on AI-assisted update workflows

**Continuous Improvement:**
- Monitor AI suggestions for accuracy
- Refine risk assessment criteria based on experience
- Update MCP server queries for better results
- Share successful patterns across projects
- Document edge cases and handling procedures

By combining AI agents with MCP servers and established security practices, dependency management becomes a proactive, efficient process that maintains security posture while reducing manual toil.

## Further Resources

- **MCP Servers**: See [MCP Servers](./mcp-servers.md) for configuration details
- **Copilot Instructions**: See [Copilot Instructions](./copilot-instructions.md) for AI guidance patterns
- **Security Guidelines**: See `.github/copilot-security-instructions.md` for security requirements
- **National Vulnerability Database**: [nvd.nist.gov](https://nvd.nist.gov/)
- **GitHub Security Advisories**: [github.com/advisories](https://github.com/advisories)
