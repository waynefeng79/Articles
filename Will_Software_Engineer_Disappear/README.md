### Requirements, Tests, and the Boundaries of AI: Will Traditional Software Engineers Disappear?

A few days ago, I participated in an intriguing debate. One of the central questions was: In the era of AI, will traditional software engineers disappear? Some argued that we will only need business analysts who can clearly define requirements, while others believed the future belongs exclusively to hybrid professionals fluent in both business and programming.

I would like to analyze this from the perspective of software engineering's core pain points and the current boundaries of AI capabilities, which gives rise to several insightful questions.

---

#### 1. Can Requirements Ever Be Precisely Defined?

Accurately defining requirements is a perennial challenge in software development—one that is nearly impossible to solve perfectly. When clients define software requirements, functional requirements typically receive the most attention, though mostly confined to high-level business descriptions. Translating those broad requirements into functional modules, edge cases, and exception-handling logic depends heavily on a developer's deep comprehension of the underlying business context. Whenever a gap emerges between user intent and developer interpretation, requirement changes naturally follow.

This reality is precisely why Agile methodology superseded the traditional Waterfall model. Its core premise assumes that requirements are inherently incomplete and unreliable, relying on rapid prototyping through iterative cycles to validate true user needs. From this angle, AI can indeed replace developers in rapidly building prototypes to satisfy functional requirements.

However, requirements contain another crucial dimension: **non-functional requirements (NFRs)**—such as performance, scalability, availability, reliability, security, privacy, maintainability, compatibility, and portability. These metrics are notoriously difficult for clients, or even technical managers, to evaluate objectively.

Consider a simple example: A client needs a trading system that supports thousands of concurrent users.

* **Solution A**: Utilizes localized caching with tightly coupled components, achieving target concurrency on an AWS `t3.small` instance.
* **Solution B**: Adopts a stateless architecture, requiring a deployment on a `c7i.large` instance with over three times the operational cost.

Assuming the response time requirement is under 1 second, Solution A meets the benchmark at a fraction of the cost, appearing to be the superior choice. However, if user traffic surges in the future, Solution A—having sacrificed scalability for short-term cost efficiency—would require a complete architectural overhaul, far exceeding the initial savings. Alternatively, if end users find a 1-second delay unacceptable, the initial value proposition collapses.

Even less visible than performance are reliability, security, and privacy. They remain unnoticed during normal operations, but any failure can result in substantial financial loss or severe regulatory penalties.

Can AI address non-functional requirements? At its core, current AI programming relies on context-based token prediction. Agents can execute tasks accurately only when provided with explicit context and reliable test suites. For NFRs, the key obstacles are:

1. **Requirements are notoriously difficult to quantify comprehensively**: Does scalability mean handling more transaction types, higher user volume, or a broader spectrum of client devices?
2. **Non-functional requirements naturally conflict**: Enhancing security often degrades performance, while maximizing high availability inflates infrastructure costs.

The core issue is that clients rarely quantify these trade-offs, leaving AI with no basis for optimal decision-making. Of course, this is not exclusively an AI limitation; human architects cannot mathematically prove their designs to be definitively optimal for every scenario either. Software success relies heavily on human experience, intuition, and iterative refinement.

Unless two business models are 100% identical, AI cannot deliver a single "canonical solution" for software. For complex architectures, Agents rely on a "Plan Mode" to align on proposals with human engineers before generating code. Evaluating these technical trade-offs still demands seasoned engineers.

---

#### 2. Can a "Perfect Test Suite" Guarantee Code Quality?

Since non-functional requirements are hard to formalize, could we discard ambiguous requirements and rely solely on a comprehensive test suite to guarantee AI-generated code quality?

Theoretically, the sheer complexity of modern software renders this impossible. The real-world execution environment—hardware configurations, operating system versions, third-party dependencies, runtime timing, data volume, edge inputs, and even hardware temperature or electromagnetic interference—can introduce variations that lead to a combinatorial explosion of test cases.

Can 100% white-box coverage or passing every black-box system test guarantee a bug-free system? Quality assurance has never relied on testing alone; architectural design plays a decisive role.

Engineers typically address bugs in two ways:

* **Root-Cause Fixes**: Pinpointing the underlying flaw and resolving it at the architectural/logical level.
* **Workarounds**: Bypassing the issue when the root cause is elusive or too costly to fix.

Both approaches can pass an automated test suite, but their underlying quality is drastically different. Recently, I used ChatGPT to build a script inserting large volumes of records into a PostgreSQL database. I noticed data loss occurring whenever a single operation exceeded a certain threshold. After several troubleshooting rounds with ChatGPT, the AI eventually proposed: *"Let's compare the input payload post-insertion and re-insert any missing records."*

Evidently, AI tends to favor workarounds that satisfy test assertions quickly. However, the resulting performance overhead and latent risk of logical flaws are unacceptable in production environments.

---

#### 3. Can AI Accurately Diagnose and Fix Dynamic Bugs?

Unquestionably, AI excels at identifying **static issues** within explicit code contexts—such as syntax errors, typos, obvious logical flaws, or code style violations—and proposing elegant refactorings.

However, AI struggles with **dynamic runtime issues**.

Returning to the database ingestion example: How should an array-handling function be structured? It depends on dynamic parameters, such as array payload size versus the database's single-query insertion limit.

* If actual data volumes remain small, a basic single-statement query suffices.
* If production data frequently exceeds configuration limits, the function must batch data into optimal chunk sizes (balancing throughput and compatibility) within a loop.

If you simply report "data loss" to an AI without full runtime context, it will struggle to find the root cause. Syntactically, the static code appears flawless; the root cause stems from a mismatch between hardware/software configuration and boundary constraints—the exact realm where experienced developers add value. Software configurations exist to accommodate diverse deployment environments, requiring developers to evaluate dynamic edge boundaries. AI generates code based on training probabilities, but how closely does its training data match a client's specific business demands? Can requirements realistically specify bounds down to every single function to provide AI with flawless context?

---

#### 4. Can AI Replace Humans in Long-Term Software Maintenance?

Software maintenance encompasses both bug fixing and new feature development.

For **feature enhancements**, modern AI Agents demonstrate impressive codebase comprehension. Given sufficient context, AI can extend functionality while respecting existing interfaces.

However, the greatest hidden hazard in long-term maintenance is **architectural entropy and technical debt**. AI lacks global architectural guardrails when writing code; it easily falls into the trap of fulfilling isolated requests by introducing code duplication, breaking module encapsulation, or injecting anti-patterns (often termed "AI Slop"). If non-technical personnel rely solely on AI for long-term maintenance, the codebase will rapidly degrade into unmaintainable "spaghetti code." Thus, developers remain indispensable as architectural guardians during the maintenance lifecycle.

---

### Conclusion

Software engineers will continue to exist for the foreseeable future. AI has become a powerful productivity multiplier, vastly amplifying developer capabilities.

However, claiming that software development can be entirely driven by business personnel without developers is premature. Such a model could only materialize in hyper-standardized domains (e.g., basic CRUD applications) with formalizable requirements and unlimited compute resources where architectural inefficiency carries zero cost.