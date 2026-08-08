Open Engineering Sandcastles

Safe places for engineering agents to experiment, build, test, and learn.

Welcome to Open Engineering Sandcastles — the home of the definitions, contracts, conventions, and lifecycle model for Sandcastles within the Open Engineering ecosystem.

A Sandcastle is a temporary, isolated engineering environment.

It gives an engineer or autonomous agent somewhere safe to inspect source code, modify artifacts, execute tools, run builds, test hypotheses, validate changes, and collect evidence — without granting unrestricted access to the environment around it.

Like a real sandcastle, it is designed to be created, shaped, inspected, and eventually washed away.

⸻

What is a Sandcastle?

A Sandcastle is an isolated and disposable execution environment created for a bounded engineering activity.

A Sandcastle may contain:

* a source repository
* a dedicated Git branch or worktree
* an engineering agent
* development tools
* build dependencies
* test infrastructure
* validators
* rules and conventions
* credentials with explicitly limited scope
* execution logs
* generated artifacts
* evidence

A typical lifecycle is:

Engineering Task
       │
       ▼
Create Sandcastle
       │
       ▼
Prepare Environment
       │
       ▼
Execute Agent
       │
       ├── inspect
       ├── investigate
       ├── modify
       ├── build
       ├── test
       └── validate
       │
       ▼
Collect Evidence
       │
       ▼
Review Changes
       │
       ▼
Commit / Pull Request
       │
       ▼
Destroy Sandcastle

The Sandcastle itself is temporary.

The engineering evidence it produces is not.

⸻

Why Sandcastles?

Autonomous engineering agents are powerful because they can act.

That is also why they need boundaries.

Giving an agent unrestricted access to an engineer’s workstation, credentials, repositories, network, and infrastructure creates unnecessary risk.

Open Engineering therefore treats isolation as part of the engineering model rather than an implementation detail.

A Sandcastle establishes an explicit boundary around agent execution.

                 Host Environment
┌───────────────────────────────────────────────┐
│                                               │
│        ┌────────── Sandcastle ──────────┐     │
│        │                                │     │
│        │          Engineering Agent     │     │
│        │                 │              │     │
│        │                 ▼              │     │
│        │              Source            │     │
│        │                 │              │     │
│        │        ┌────────┼────────┐     │     │
│        │        ▼        ▼        ▼     │     │
│        │      Build     Test   Validate │     │
│        │                                │     │
│        └────────────────────────────────┘     │
│                                               │
└───────────────────────────────────────────────┘

The agent gets the environment it needs.

Nothing more.

⸻

Sandcastles and Open Engineering

Sandcastles form part of the Open Engineering execution architecture.

Goal
 │
 ▼
Investigation
 │
 ▼
Decision
 │
 ▼
Plan
 │
 ▼
Task
 │
 ▼
Agent Fabric
 │
 ▼
Sandcastle
 │
 ▼
Execution
 │
 ├── Build
 ├── Test
 ├── Rules
 ├── Conventions
 └── Validation
 │
 ▼
Evidence
 │
 ▼
Review
 │
 ▼
Change

This creates a controlled path from intent to execution to evidence.

A Sandcastle does not decide what should be built.

It provides the safe place in which that work can happen.

⸻

Definition, Not Implementation

This organization defines what a Sandcastle is.

It does not prescribe one particular sandbox technology.

The Open Engineering model deliberately separates the abstraction from its implementations.

Sandcastle
    │
    ├── Local container
    │
    ├── Rootless container
    │
    ├── MicroVM
    │
    ├── Kubernetes workload
    │
    ├── Remote sandbox
    │
    └── Future providers

This allows Open Engineering workflows to request a Sandcastle without needing to know how that environment is implemented.

For example:

sandcastle:
  isolation: ephemeral
  source:
    repository: example/source
    strategy: isolated-branch
  agent:
    capability: engineering
  permissions:
    network: restricted
    filesystem: workspace
    credentials: scoped
  verification:
    - build
    - test
    - lint
    - validate
  evidence:
    - execution-log
    - changes
    - commits
    - test-results
    - validation-results

The implementation is free to satisfy that contract using whichever execution technology is appropriate.

⸻

Sandcastle Providers

A Sandcastle Provider realizes a Sandcastle definition as an executable environment.

Sandcastle Definition
         │
         ▼
Sandcastle Provider
         │
   ┌─────┼──────────────┐
   │     │              │
   ▼     ▼              ▼
Container   MicroVM   Kubernetes

Providers may differ in:

* isolation strength
* startup time
* available compute
* persistence
* networking
* filesystem semantics
* credential handling
* cost
* execution location

The Sandcastle contract allows these differences while preserving a common Open Engineering execution model.

⸻

Agent Isolation

A Sandcastle should follow the principle of least privilege.

An agent should receive only the capabilities required for its assigned engineering task.

Examples include:

Filesystem
├── workspace        read/write
├── dependencies     read
└── host             unavailable
Network
├── package registry allowed
├── required APIs    allowed
└── arbitrary access restricted
Git
├── repository       scoped
├── branch           isolated
└── credentials      limited
Infrastructure
├── test resources   permitted
└── production       unavailable

The exact security policy depends on the Sandcastle implementation and the engineering task.

The boundary, however, should always be explicit.

⸻

Parallel Engineering

Sandcastles make parallel agent execution a natural part of the Open Engineering model.

                     Plan
                      │
                 Task Breakdown
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
       Task A       Task B      Task C
          │           │           │
          ▼           ▼           ▼
    Sandcastle A Sandcastle B Sandcastle C
          │           │           │
          ▼           ▼           ▼
       Agent A      Agent B      Agent C
          │           │           │
          └───────────┼───────────┘
                      │
                      ▼
                    Review
                      │
                      ▼
                   Evidence
                      │
                      ▼
                    Change

Each task receives its own execution boundary.

Agents can therefore work independently without unintentionally sharing mutable working state.

⸻

Evidence

A Sandcastle should make execution observable.

An engineering change should be accompanied by evidence describing how it was produced and verified.

Evidence may include:

Sandcastle Execution
        │
        ├── agent session
        ├── prompts
        ├── tool executions
        ├── source changes
        ├── commits
        ├── build output
        ├── test results
        ├── rule results
        ├── convention validation
        └── execution metadata
                 │
                 ▼
              Evidence

This allows Open Engineering to answer not merely:

What changed?

but also:

Why did it change?

Who or what changed it?

In which environment?

Which rules were applied?

Which tests passed?

What evidence supports accepting the change?

⸻

Sandcastle Lifecycle

A Sandcastle has a deliberately bounded lifecycle.

1. Define

Describe the required environment, source, permissions, agent capabilities, and verification policy.

2. Provision

Create the isolated execution environment.

3. Prepare

Retrieve source code, dependencies, rules, conventions, and required tools.

4. Execute

Allow the engineer or agent to perform the assigned work.

5. Verify

Run builds, tests, linters, rules, policy checks, and convention validation.

6. Capture

Record changes, execution metadata, results, and evidence.

7. Review

Evaluate the proposed engineering change.

8. Publish

Where appropriate, create commits, branches, artifacts, or pull requests.

9. Destroy

Remove the temporary execution environment and revoke its temporary access.

Define
  ↓
Provision
  ↓
Prepare
  ↓
Execute
  ↓
Verify
  ↓
Capture
  ↓
Review
  ↓
Publish
  ↓
Destroy

⸻

Declarative Sandcastles

Open Engineering aims to make Sandcastles declarative.

Instead of scripting the mechanics of creating an agent environment, an engineer should be able to describe the desired environment.

apiVersion: open-engineering.io/v1alpha1
kind: Sandcastle
metadata:
  name: implement-hello-world
spec:
  source:
    repository: open-engineering-example/source
  workspace:
    strategy: isolated
  agent:
    capability: software-engineering
  permissions:
    filesystem: workspace
    network: restricted
    credentials: scoped
  verification:
    required:
      - build
      - test
      - conventions
  evidence:
    capture: true
  lifecycle:
    persistence: ephemeral

A provider can then turn this definition into an executable environment.

This creates the possibility of eventually composing Sandcastles through infrastructure orchestration technologies such as Kubernetes and Crossplane.

⸻

Sandcastles as Composable Engineering Infrastructure

A Sandcastle is not merely a container.

It is a composition of engineering capabilities.

Sandcastle
 │
 ├── Source
 ├── Agent
 ├── Runtime
 ├── Tools
 ├── Rules
 ├── Conventions
 ├── Credentials
 ├── Network Policy
 ├── Verification
 ├── Observability
 └── Evidence

This makes Sandcastles suitable for much more than code generation.

They can support:

* software implementation
* refactoring
* dependency upgrades
* documentation generation
* architecture experiments
* migrations
* debugging
* security analysis
* rule validation
* code review
* release preparation
* reproducibility investigations

⸻

Relationship to Sandcastle

The concept is inspired in part by Matt Pocock’s Sandcastle project, which provides isolated environments for running AI coding agents.

Open Engineering generalizes the idea into a technology-independent engineering primitive.

The external project can therefore serve as an implementation technology while the Open Engineering Sandcastle definition remains independent of any particular agent, sandbox provider, runtime, or vendor.

This distinction is intentional:

Open Engineering Sandcastle
          │
          │ defines
          ▼
   execution contract
          │
          │ implemented by
          ▼
  sandbox technologies

Open Engineering owns the abstraction.

Implementations provide the machinery.

⸻

Design Principles

Isolated by Default

Agent execution happens inside an explicit boundary.

Ephemeral by Default

Execution environments should exist only as long as they are needed.

Least Privilege

Agents receive the minimum capabilities required for their task.

Declarative

Describe the desired engineering environment rather than scripting its construction.

Observable

Execution should produce structured telemetry and evidence.

Reproducible

A Sandcastle definition should contain enough information to recreate an equivalent engineering environment.

Verifiable

Changes should be tested and validated before leaving the Sandcastle.

Composable

Sandcastles combine reusable Open Engineering capabilities rather than duplicating them.

Provider Independent

The definition must not depend on a particular container, VM, cloud, agent, or sandbox implementation.

AI Native

Autonomous agents are first-class participants in the engineering process.

Human Compatible

Anything an agent can execute should remain understandable, inspectable, and reviewable by engineers.

⸻

Vision

Software engineering is moving from tools that merely assist engineers toward systems that can increasingly perform engineering work.

That transition requires more than capable agents.

It requires safe places for those agents to work.

Open Engineering Sandcastles provide those places.

Intent
  ↓
Plan
  ↓
Agent
  ↓
┌─────────────────────────┐
│       SANDCASTLE        │
│                         │
│  investigate            │
│  experiment             │
│  build                  │
│  test                   │
│  validate               │
│  learn                  │
│                         │
└─────────────────────────┘
  ↓
Evidence
  ↓
Review
  ↓
Engineering Change

Give agents freedom to engineer — inside boundaries we understand.

⸻

Open Engineering

Open by design.
AI native.
Composable by default.
Evidence driven.

A Sandcastle is where engineering agents are free to build — safely.
