+++
archetype = "chapter"
title = "NodeJS"
weight = 3.2
+++

NodeJS projects using [`npm`](https://npmjs.com/) can use `npm audit` to generate a list of vulnerable packages based upon the [National Vulnerability Database (NVD)](https://nvd.nist.gov/). You can subsequently run npm `npm audit fix` to safely upgrade package versions within the dependency range specified in `package.json`. The NPM documentation have complete articles on [Auditing package dependencies for security vulnerabilities](https://docs.npmjs.com/auditing-package-dependencies-for-security-vulnerabilities).

> [!IMPORTANT]
> Certain frameworks don't cope well with `npm audit fix` as they expect every package in the solution to be upgraded to the same version (lock-step upgrade) and in some cases include code migrations to automate the upgrade across breaking versions. There are two such frameworks in use within Ensono Stacks, [NX](https://nx.dev/) and [Docusaurus](https://docusaurus.io/), in the case that these frameworks need to be upgraded according to their own methodology, see the section on [Framework Specific Guidance](#framework-specific-guidance).

This means that a specification of `^1.0.0` in the `package.json` would update the `package-lock.json` from `1.0.0` to `1.0.1` but not `2.0.0`, this is typically safe as major and patch revisions don't contain breaking changes. However not all packages adhere to [SemVer](https://semver.org/) which can result in errors and deprecated API calls. Testing is advised.

It's possible to ignore version constraints entirely with `npm audit fix --force` however this frequently results in broken dependencies. Additionally the there is an Apache-2.0 Licensed external tool called [NPM Check Updates](https://www.npmjs.com/package/npm-check-updates) which can iteratively install packages to their latest version, regardless of a Major version, executing tests within each update:

```shell
npm install -g npm-check-updates
ncu --doctor -u
```

Two additional parameters `--doctorInstall` allow you to customise the install command (i.e. `npm install` vs. `yarn install`) and `--doctorTest` allows you to specify the test command, it defaults to `npm test`. This can be a long-running operation however provides a way to identify breaking changes without installing packages manually with `npm install packagename@latest`.

## Update Run Book

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

Finally run nvm to install an appropriate version of Node and NPM:

```shell
nvm install
```

#### Install Hooks

Some projects will have [git hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) setup to enforce code style, spelling and commit messages. To configure this it's important that the `prepare` scripts referenced in `package.json` executes, check the output of `npm install` and the contents of `package.json` and if required execute the script with:

`npm run {{ Insert Setup Script Name Here }}`

### Update Packages

Before updating packages it's worth running `npm install` followed by `npm test` to ensure that the project builds correctly. If either of those commands you will likely need to inspect `package.json` to identify the appropriate script to execute with `npm run {script name}`.

## Framework Specific Guidance

### Next.js

Next.js [provides a codemod](https://nextjs.org/docs/app/guides/upgrading/version-15) to upgrade between versions of Next.js.

> [!TIP]
> If you get a peer dependencies warning, you may need to upgrade `react` and `react-dom`.

### Nx

> [!NOTE]
> Ensono provides a [Migrate GitHub Action](https://github.com/Ensono/stacks-nx-plugins/blob/main/.github/workflows/migrate.yml) which will update Nx in `stacks-nx-plugins` on a weekly basis. These changes are surfaced to maintenance engineers as [Pull Requests on the stacks-nx-plugins](https://github.com/Ensono/stacks-nx-plugins/pulls) repository for review and merging.

Nx has a concept of [automated dependency updates](https://nx.dev/features/automate-updating-dependencies) which includes not only updating `package.json` and `package-lock.json` but additionally executing code migrations to modify code to account for breaking changes.

The first step is understanding the current version of `Nx`:

```shell
./node_modules/nx/bin/nx.js --version
```

This version number can be compared to the [Nx Changelog](https://nx.dev/changelog) to understand if there are multiple major version numbers between the current and `latest` state.

> [!IMPORTANT]
> It's best practice to upgrade major / breaking changes, i.e. `18.2.2` to `19.0.0`, in single increments. For example if the current version is `v18` and the latest version is `v20` then you would upgrade to `v19` first then upgrade to `v20`.

To upgrade the `package.json` and update `package-json.lock` in addition to generating a `migrations.json` file:

```shell
nx migrate latest
```

To run the migrations:

```shell
nx migrate --run-migrations
```

This will run the migrations, however it will not delete the `migrations.json` this may be required to upgrade changes in other branches.

At this point you can build and test the project:

```shell
npm run build
npm run test
```

Because Nx test runs are cached, it won't be necessary to execute them again when committing.

### Docusaurus

Older projects may still be on Docusaurus v2, in which case there is specific guidance for [upgrading from v2 to v3](https://docusaurus.io/docs/migration/v3).

When upgrading between minor versions, it's essential that each package starting with `@docusaurus` is upgraded to the target version:

```shell
npm install @docusaurus/core@latest @docusaurus/mdx-loader@latest @docusaurus/plugin-google-analytics@latest @docusaurus/plugin-google-gtag@latest @docusaurus/plugin-sitemap@latest @docusaurus/preset-classic@latest
```

Other packages can be updated using a similar approach. Alternatively `npm audit fix` can be used.
