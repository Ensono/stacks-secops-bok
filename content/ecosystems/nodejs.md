+++
archetype = "chapter"
title = "NodeJS"
weight = 3.2
+++

NodeJS projects using [`npm`](https://npmjs.com/) can use `npm audit` to generate a list of vulnerable packages based upon the [National Vulnerability Database (NVD)](https://nvd.nist.gov/). You can subsequently run npm `npm audit fix` to safely upgrade package versions within the dependency range specified in `package.json`. The NPM documentation have complete articles on [Auditing package dependencies for security vulnerabilities](https://docs.npmjs.com/auditing-package-dependencies-for-security-vulnerabilities).

> [!IMPORTANT]
> Certain frameworks don't cope well with `npm audit fix` as they expect every package in the solution to be upgraded to the same version (lock-step upgrade). There are two such frameworks in use within Ensono Stacks, [NX](https://nx.dev/) and [Docusaurus](https://docusaurus.io/), in the case that these frameworks need to be upgraded according to their own methodology, see the section on [Framework Specific Guidance](#framework-specific-guidance).

This means that a specification of `^1.0.0` in the `package.json` would update the `package-lock.json` from `1.0.0` to `1.0.1` but not `2.0.0`, this is typically safe as major and patch revisions don't contain breaking changes. However not all packages adhere to [SemVer](https://semver.org/) which can result in errors and deprecated API calls. Testing is advised.

It's possible to ignore version constraints entirely with `npm audit fix --force` however this frequently results in broken dependencies. Additionally the there is an Apache-2.0 Licensed external tool called [NPM Check Updates](https://www.npmjs.com/package/npm-check-updates) which can iteratively install packages to their latest version, regardless of a Major version, executing tests within each update:

```shell
npm install -g npm-check-updates
ncu --doctor -u
```

Two additional parameters `--doctorInstall` allow you to customise the install command (i.e. `npm install` vs. `yarn install`) and `--doctorTest` allows you to specify the test command, it defaults to `npm test`. This can be a long-running operation however provides a way to identify breaking changes without installing packages manually with `npm install packagename@latest`.

## Update Runbook

### Setup Development Environment

It's not uncommon for NodeJS project to include hints to the use of the versions of NPM an Node that are recommended for the project, these may either be included in the `project.json` included as a separate `.nvmrc`.

> [!TIP]
> While it is possible to use `node`, `npm` and other parts of the NodeJS ecosystem on Windows shells, it's not recommended, there are a small number of packages which take a dependency on a working C compiler and other packages that expect a case-sensitive file system. It's strongly advised to use a POSIX compliant shell such as [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install).

The best way to consume these is through the [`nvm`](https://github.com/nvm-sh/nvm) package:

First install `nvm`:

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

Re-source your shell:

```shell
source ~/.bashrc # for bash
source ~/.zshrc # for zsh
```

Finally run run nvm to install an appropiate version of Node and NPM:

```shell
nvm install
```
### Update Packages

Before updating packages it's worth running `npm install` followed by `npm test` to ensure that the project builds correctly. If either of those commands you will likely need to inspect `package.json` to identify the appropriate script to execute with `npm run {script name}`.

## Framework Specific Guidance

### Nx

### Docusaurus
`
