+++
archetype = "chapter"
title = "Usage of Git"
weight = 1.1
+++

## Branches

All stacks projects use the `git` version control system, typically all repositories have either a `main` or `master` branch which represents the production release. All work is then done in either forked repositories or in feature branches within the main repository.

Feature branches should be short lived as conveyed by [Trunk Based Development](https://trunkbaseddevelopment.com/) and [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow). Whilst no absolute naming convention exists we recommend that security branches are created with the `hotfix/` prefix.

### Example

If the updates are for a wide range of vulnerabilities, or simply maintaining version currency:

```shell
git checkout -b hotfix/updates-2024-09-24
```

If the update is specific to one vulnerability then:

```shell
git checkout -b hotfix/cve-2024-45590
```

If no CVE is available, then you can use a reference from the GitHub Advisory Database or another well recognised advisory service.

## Commits

We prefer the use of [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) this is enforced on some projects through the use of hooks. Ensure that the commit message succinctly describes the purpose of the updates and uses an appropriate commit type such as:

- `fix():` for a hotfix for a know security vulnerability
- `chore():` for simply bumping versions

> [!NOTE]
> GitHub will automatically link CVEs and GHSA references in commit messages.

### Example

```
fix(axios-plugin): mitigate CVE-2024-45590

- bumps the version of `axios` to 1.7.8 to mitigate ReDOS attack.
```
