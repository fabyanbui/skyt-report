# Problem Definition: Unverified OSS and AI-Assisted Code Reuse

> **Related VDT proposal:** [Source Code Compliance Gate (SCCG)](./vdt-source-code-compliance-gate.md)  
> **Purpose:** Define the user and engineering pain points before discussing the proposed solution.

## 1. Context and motivation

While developing an AI OCR pipeline, I experimented with a wide range of open-source (OSS) models, repositories, and supporting tools to compare their capabilities and find a practical implementation path. In that process, my attention was mainly on technical factors—accuracy, speed, hardware requirements, integration effort, and whether the model could solve the OCR problem. I realized that I could easily overlook a separate but important question: **under what license and conditions may this model, repository, code, or associated asset be used in our intended product?**

This is not unique to AI OCR. In general software development, engineers increasingly reuse OSS libraries, copy examples from public repositories, install developer tools, and use AI coding assistants to generate or modify code. Yet the development workflow may not include a consistent, visible checkpoint to answer questions such as:

- Has the license of this model, repository, package, or tool been identified and reviewed?
- Do its license conditions fit the intended use, modification, deployment, and distribution model?
- Have the tool's origin, maintainer, release, integrity, and security posture been assessed to the level required by the project?
- When AI generates code, is there a process to review provenance where relevant, dependency additions, license concerns, security risks, and policy compliance before the code is merged?
- Can the team later show what was checked and who approved a decision?

The observation is a gap in the **development process and its controls**, not an allegation that any particular OSS project or AI-generated code is unlawful or unsafe.

## 2. Core problem statement

Engineering teams need to make rapid build-versus-reuse decisions, but the information required to make those decisions responsibly is fragmented across repositories, package registries, model cards, license files, documentation, security advisories, and internal policies. Developers may be able to install or integrate a component before anyone has established whether its license, provenance, security posture, and operational terms are acceptable for the target project.

At the same time, AI-assisted coding can increase the volume and speed of code changes and dependency additions. Unless existing review and CI/CD controls explicitly cover these risks, a conventional code review may focus on functionality, maintainability, and tests without consistently checking licensing, approved-tool status, provenance, or supply-chain security.

The resulting pain is not simply “developers forget to check licenses.” It is that **the workflow does not reliably make the required checks visible, timely, repeatable, and auditable**.

## 3. Pain points

### P1. License and usage conditions are easy to overlook

- Technical evaluation often prioritizes model quality, latency, resource consumption, and ease of integration.
- License information may be in a separate file, model card, package page, or upstream dependency rather than in the code being evaluated.
- “Open source,” “publicly available,” and “free to download” can be incorrectly treated as equivalent to “approved for any commercial or enterprise use.”
- AI models may have separate terms for code, model weights, training data, datasets, or other assets. A repository's top-level license may not cover every asset.

**Impact:** Late discovery can force component replacement, rework, a release delay, or a legal review under time pressure.

### P2. No consistent policy checkpoint for AI-generated or AI-modified code

- AI-assisted changes may introduce new packages, copy patterns from sources that are not disclosed, or add code whose origin is difficult to establish from the diff alone.
- Teams may have code review and automated tests but no explicit checklist or automated control for license, dependency, provenance, and security policy.
- A code-generation tool's output is not automatically compliant merely because the tool produced it.

**Impact:** Review quality depends heavily on individual awareness and reviewer time; checks may be inconsistent across teams and repositories.

### P3. OSS tools and models may be technically tested but not sufficiently vetted

- A quick proof of concept can establish that a tool runs without establishing whether its source, release, dependencies, maintainer practices, known vulnerabilities, or distribution terms are suitable.
- Model and package metadata may be incomplete, stale, inconsistent, or hard to verify.
- An unfamiliar project can be useful technically but still require additional security, privacy, license, or operational review before adoption.

**Impact:** Teams may adopt components without a documented, risk-based decision or may repeat the same research independently.

### P4. Dependency chains hide additional obligations and risks

- Direct dependencies can pull in transitive packages with different licenses or known vulnerabilities.
- Installing a model or tool can also bring scripts, plugins, native binaries, container images, model weights, or other artifacts that need separate consideration.
- Manual review of only the top-level repository does not provide a complete view of what is included in the delivered system.

**Impact:** The project may have an incomplete view of its software and model supply chain.

### P5. Evidence and ownership of decisions are fragmented

- A developer may inspect a license or security advisory informally without recording the source, version, date, conclusion, or reviewer.
- Teams may not know who can approve a restricted license, accept a vulnerability, or grant a project-specific exception.
- Repeating the same checks across projects wastes effort, while undocumented exceptions can become permanent by accident.

**Impact:** Compliance reviews are slower, decisions are difficult to reproduce, and teams cannot easily demonstrate due diligence.

### P6. Checks happen too late or are too disruptive

- If license and tool approval happen only near release, remediation may require replacing a dependency or redesigning an integration after significant work has accumulated.
- If a policy is introduced as a strict blocking gate without validation, false positives and ambiguous metadata can frustrate developers and encourage workarounds.
- A useful control must distinguish clear policy violations from incomplete evidence that needs human review.

**Impact:** Either risk is discovered late, or overly broad controls add friction without improving decisions.

### P7. Enterprise and client delivery increase the cost of uncertainty

- The acceptable risk can vary by deployment model: internal experimentation, hosted service, on-premises deployment, redistribution of binaries, or delivery to a customer.
- Client contracts, confidentiality obligations, security requirements, and internal policies may add restrictions beyond the OSS license itself.
- A generic license scanner cannot decide every question about a particular combination, use case, or contract.

**Impact:** Teams need a traceable escalation path that involves the right engineering, security, procurement, and legal owners instead of relying on an automated verdict alone.

## 4. Scope boundaries: related but distinct checks

The problem should not be reduced to license scanning alone. There are at least four related control areas:

| Control area | Key question | Examples of evidence |
|---|---|---|
| License and legal terms | Are the component's terms understood and compatible with the intended use and distribution? | License identifier and text, notices, model card, approval record |
| Provenance and integrity | Do we know where this component came from and which version/artifact is used? | Upstream URL, version, checksum, lockfile, source record |
| Security and maintenance | Is the component acceptable under project security requirements? | Vulnerability scan, release activity, maintainer/security policy review |
| AI-assisted change governance | Are generated or modified changes reviewed under the same engineering and policy controls? | PR review, dependency diff, tests, scanner output, documented exceptions |

These controls overlap but are not interchangeable. A permissive license does not prove a component is secure; a vulnerability-free scan does not grant permission to use copyrighted code; and a public repository does not prove that its uploader had authority to publish every included asset.

## 5. Why current informal practices are insufficient

Informal checks can be appropriate for low-risk experimentation, but they are unreliable as the sole control when components move into production or client-facing products. They depend on each developer remembering every relevant question, knowing where to find the evidence, interpreting different license families correctly, and documenting the decision in a consistent place.

The proposed improvement is not to add a legal review meeting to every code change. It is to **automate repeatable evidence collection and policy checks**, surface uncertainty early, and reserve human review for ambiguous or high-impact decisions.

## 6. What success should look like

Before choosing tools or setting hard enforcement rules, a pilot should establish whether the workflow can:

1. Identify the relevant direct and transitive dependencies and, where feasible, model/tool artifacts.
2. Record license and provenance evidence with clear uncertainty when metadata is incomplete.
3. Flag missing evidence and policy-defined high-risk conditions before merge or release.
4. Give developers actionable remediation guidance and a clear path for human review.
5. Record the policy version, scan result, decision owner, and any exception.
6. Keep CI overhead and false-positive rates within agreed limits.
7. Avoid sending proprietary code, confidential documents, or client data to unapproved external services.

## 7. Questions to validate during discovery

These are hypotheses to validate with engineering, DevSecOps, procurement, and Legal/Compliance stakeholders, rather than assumptions about every team:

- Which repositories and AI/OSS workflows currently have mandatory license or security checks?
- Are model weights, datasets, plugins, container images, and non-code artifacts covered by current procedures?
- Who maintains the approved/restricted/prohibited license policy and the approved-tool inventory?
- How are exceptions requested, approved, time-limited, and audited today?
- At what point in the SDLC do teams first review license, provenance, and security evidence?
- What proportion of current findings are actionable, ambiguous, or false positives?
- Which repositories must be scanned entirely inside Viettel-controlled infrastructure?

## 8. Conclusion

The central pain point is a **process-control gap between fast reuse of OSS/AI assets and the evidence needed to approve them for a specific engineering context**. The proposed VDT initiative should address this gap with a risk-based workflow that combines automated inventory and evidence collection, policy checks in PR/CI, human review for uncertain cases, and auditable exceptions.

The goal is responsible reuse—not preventing teams from using OSS or AI. Legal interpretation, license compatibility decisions, and exceptions must remain subject to the organization's approved governance and qualified human review.
