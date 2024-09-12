+++
archetype = "chapter"
title = "Basics"
weight = 1
+++

The documents contained within this microsite are intended to provide a shared body of knowledge (BOK) for everyone involved in managing security operations for Ensono Stacks.

The Security Operations (SecOps) role is a limited set of activities that are easy to overlook, however if left unattended can create substantial problems down the line:

- Large numbers of vulnerabilities that need to be resolved to bring a project back into compliance.
- Vulnerabilities making their way into production lead to operational risk on behalf our our teams and clients.
- Vulnerabilities in local, development and testing environments may give an attacker a foothold within an organisation.
- Vulnerabilities in any code, may result in a successful supply chain attack which is both disruptive and damaging within the organisation.

For the purposes of these documents the role of Security Operations for Ensono Stacks focuses on the following disciplines:

- **Vulnerability Management** - the act of identifying vulnerabilities in Ensono Stacks and submitting pull requests (changes) that mitigate these vulnerabilities.
- **Software Currency** - the act of proactively updating dependencies to recent versions, this could be considered proactive Vulnerability Management, however the primary purpose is to ensure that when a new project is created it has current versions of software.
- **Security Configuration Management** - the act of identifying areas of configuration key to the security of systems and identifying configuration errors that might lead to compromise or disclosure.

## Principles

A body of knowledge is not intended to be prescriptive, it aims to institutionalise and socialize knowledge and best practice approaches to security operations. The following principles are important in creating a body of knowledge:

1. **Information** over **Prescription** - this means recommendations should be informative, not telling an engineer how to act. Practice and tools evolve, this document should change with the times.
2. **Living** over **Static** - this document should readily accept pull requests from people living the role on a daily basis.
3. **Automated** over **Manual** - processes should be as automated as possible, only relying on humans to review actions where there is value.

## How to read this document

You can read this document from beginning to end, or simply use it as a reference. Both approaches are equally valid.
