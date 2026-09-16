# How to Build Software with AI — Architecture

The key idea is to stop thinking of AI as just a **code generator**. Build an **AI Software Engineering System** where the model has context, rules, tools, reusable skills, commands, agents, and verification.

```text
                         AI SOFTWARE FACTORY
┌─────────────────────────────────────────────────────────────────┐
│                         Developer / User                        │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                      AI Development Interface                   │
│       Claude Code / Codex / OpenCode / Cursor / etc.            │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                ┌───────────────┼────────────────┐
                ▼               ▼                ▼
        ┌─────────────┐ ┌──────────────┐ ┌───────────────┐
        │   Commands  │ │    Skills    │ │     Rules     │
        │ /plan       │ │ React Skill  │ │ coding rules  │
        │ /implement  │ │ API Skill    │ │ security      │
        │ /debug      │ │ DB Skill     │ │ architecture  │
        └─────────────┘ └──────────────┘ └───────────────┘
                │               │                │
                └───────────────┼────────────────┘
                                ▼
                     ┌─────────────────────┐
                     │   Agent / Planner   │
                     └──────────┬──────────┘
                                │
              ┌─────────────────┼─────────────────┐
              ▼                 ▼                 ▼
       ┌─────────────┐  ┌─────────────┐  ┌──────────────┐
       │   Explorer   │  │   Coder     │  │   Reviewer   │
       │ Understand   │  │ Implement   │  │ Check code   │
       └─────────────┘  └─────────────┘  └──────────────┘
              │                 │                 │
              └─────────────────┼─────────────────┘
                                ▼
                     ┌─────────────────────┐
                     │       Tools         │
                     │ Git • Terminal      │
                     │ Files • Browser     │
                     │ DB • APIs • Cloud   │
                     └──────────┬──────────┘
                                │
                                ▼
                     ┌─────────────────────┐
                     │     Verification    │
                     │ Tests • Build       │
                     │ Typecheck • Security│
                     └──────────┬──────────┘
                                │
                                ▼
                     ┌─────────────────────┐
                     │      Software       │
                     │    Production App   │
                     └─────────────────────┘
```

## 1. The Architecture Layers

### 1. Context

The AI first needs to understand the project.

```text
/context
```

Typical context:

```text
Project requirements
Architecture
Tech stack
Database schema
Existing source code
API documentation
Business rules
Environment variables
Previous decisions
```

A project should have a persistent source of truth such as:

```text
docs/
├── PRD.md
├── ARCHITECTURE.md
├── DATABASE.md
├── API.md
├── DECISIONS.md
└── REQUIREMENTS.md
```

---

# 2. Rules

Rules define **how the AI must work**.

```text
.ai/rules/
├── coding.md
├── architecture.md
├── security.md
├── testing.md
├── git.md
└── documentation.md
```

Example:

```text
Rule:
Never modify production infrastructure without explaining
the impact and verifying the configuration.

Rule:
Every new API endpoint must have validation and tests.

Rule:
Do not introduce a new dependency when existing project
dependencies can solve the problem.
```

Rules are essentially the **engineering standards** for your AI.

---

# 3. Agent Commands

Commands are your **high-level interface** to the AI.

```text
.ai/commands/
├── analyze.md
├── plan.md
├── implement.md
├── debug.md
├── test.md
├── review.md
├── security.md
├── optimize.md
└── verify.md
```

Example:

```text
/plan

Analyze the requirements and repository.

Produce:
1. Problem definition
2. Existing architecture
3. Proposed solution
4. Files to change
5. Dependencies
6. Database changes
7. API changes
8. Testing strategy
9. Risks

Do not modify files.
```

So:

```text
Command = WHAT the developer asks the AI to do
```

---

# 4. Agent Skills

Skills contain **specialized knowledge and procedures**.

```text
.ai/skills/
├── frontend/
│   ├── react.md
│   ├── nextjs.md
│   └── accessibility.md
│
├── backend/
│   ├── fastapi.md
│   ├── node.md
│   └── api-design.md
│
├── database/
│   ├── postgres.md
│   └── migrations.md
│
├── devops/
│   ├── docker.md
│   ├── kubernetes.md
│   └── aws.md
│
└── ai/
    ├── agents.md
    ├── rag.md
    ├── mcp.md
    └── evals.md
```

Example:

```text
Skill: PostgreSQL

The agent should know:

- schema design
- indexes
- transactions
- migrations
- query optimization
- connection pooling
- security
- backup/recovery
```

So:

```text
Skill = HOW the AI should perform specialized work
```

---

# 5. Agents

Agents are specialized AI workers.

```text
.ai/agents/
├── planner.md
├── architect.md
├── researcher.md
├── frontend.md
├── backend.md
├── database.md
├── devops.md
├── security.md
├── tester.md
└── reviewer.md
```

For example:

```text
Planner Agent
      ↓
Architecture Agent
      ↓
Frontend Agent + Backend Agent
      ↓
Database Agent
      ↓
Testing Agent
      ↓
Security Agent
      ↓
Reviewer Agent
```

Each agent can have:

```text
Role
Responsibilities
Skills
Tools
Rules
Inputs
Outputs
Verification criteria
```

---

# 6. Tools

Agents need tools to actually **do work**.

```text
                    AGENT
                      │
        ┌─────────────┼──────────────┐
        ▼             ▼              ▼
      Files         Terminal         Git
        │             │              │
        ▼             ▼              ▼
      Read/Write     Run code       Commit
        │
        ├── Browser
        ├── Database
        ├── APIs
        ├── Docker
        ├── Kubernetes
        └── Cloud
```

Typical tool categories:

```text
Filesystem
Terminal
Git
GitHub/GitLab
Browser
Database
HTTP/API
Docker
Cloud
Monitoring
Search
MCP servers
```

---

# 7. Memory / Project Knowledge

The AI should not rediscover the project every session.

```text
.ai/
├── memory/
│   ├── architecture.md
│   ├── decisions.md
│   ├── known-issues.md
│   └── conventions.md
```

For larger projects:

```text
Knowledge
    │
    ├── PRD
    ├── ADRs
    ├── API docs
    ├── Database docs
    ├── Codebase
    ├── Tickets
    └── Previous decisions
```

This becomes the AI's **project memory**.

---

# 8. Verification Layer

This is one of the most important parts.

Never use:

```text
AI → Code → Done
```

Use:

```text
AI → Code → Verify → Fix → Verify → Done
```

Verification should include:

```text
Typecheck
Lint
Unit tests
Integration tests
E2E tests
Build
Security scan
Git diff review
Runtime checks
```

Example:

```text
/verify

Run:

npm run typecheck
npm run lint
npm test
npm run build

Then inspect the git diff and identify:
- regressions
- missing tests
- security issues
- incomplete implementation
```

---

# 9. The Full AI Development Loop

This is the architecture I'd recommend for you:

```text
                  ┌───────────────┐
                  │   Requirement │
                  └───────┬───────┘
                          ▼
                  ┌───────────────┐
                  │   /analyze     │
                  └───────┬───────┘
                          ▼
                  ┌───────────────┐
                  │    /plan       │
                  └───────┬───────┘
                          ▼
                  ┌───────────────┐
                  │  Architecture  │
                  └───────┬───────┘
                          ▼
                  ┌───────────────┐
                  │    /tasks      │
                  └───────┬───────┘
                          ▼
              ┌────────────────────────┐
              │ Specialized AI Agents  │
              │ Frontend / Backend / DB│
              └────────────┬───────────┘
                           ▼
                    ┌─────────────┐
                    │ /implement  │
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
                    │    /test    │
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
                    │   /review   │
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
                    │  /security  │
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
                    │  /optimize  │
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
                    │   /verify   │
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
                    │   /deploy   │
                    └─────────────┘
```

# 10. Recommended Project Structure

For your **AI FDE / Agentic Software Factory** work, I would structure projects roughly like this:

```text
my-project/
│
├── .ai/
│   │
│   ├── commands/
│   │   ├── analyze.md
│   │   ├── plan.md
│   │   ├── implement.md
│   │   ├── debug.md
│   │   ├── test.md
│   │   ├── review.md
│   │   ├── security.md
│   │   ├── optimize.md
│   │   └── verify.md
│   │
│   ├── skills/
│   │   ├── frontend/
│   │   ├── backend/
│   │   ├── database/
│   │   ├── devops/
│   │   └── ai/
│   │
│   ├── agents/
│   │   ├── planner.md
│   │   ├── architect.md
│   │   ├── coder.md
│   │   ├── tester.md
│   │   ├── security.md
│   │   └── reviewer.md
│   │
│   ├── rules/
│   │   ├── coding.md
│   │   ├── security.md
│   │   ├── testing.md
│   │   └── architecture.md
│   │
│   └── memory/
│       ├── decisions.md
│       ├── conventions.md
│       └── known-issues.md
│
├── docs/
│   ├── PRD.md
│   ├── ARCHITECTURE.md
│   ├── API.md
│   └── DATABASE.md
│
├── frontend/
├── backend/
├── tests/
├── infrastructure/
│
├── Dockerfile
├── docker-compose.yml
└── README.md
```

## The Mental Model

Think about the system like this:

```text
COMMANDS
"What should I do?"

        +

SKILLS
"How should I do it?"

        +

RULES
"What constraints must I follow?"

        +

AGENTS
"Who specializes in this work?"

        +

TOOLS
"What can I actually operate?"

        +

MEMORY
"What does the project already know?"

        +

VERIFICATION
"How do I prove the work is correct?"
```

That gives you:

> **AI Software Engineer = Model + Context + Commands + Skills + Rules + Agents + Tools + Memory + Verification**

This is much closer to an **Agentic Software Factory** than simply using ChatGPT to generate code.

____

For everyday AI-assisted software development, I’d keep the command set to these **10 core commands**:

| Command      | What it does                                                 |
| ------------ | ------------------------------------------------------------ |
| `/analyze`   | Understand and inspect the existing codebase                 |
| `/plan`      | Create a step-by-step implementation plan                    |
| `/implement` | Write and integrate the actual code                          |
| `/debug`     | Find the root cause and fix errors                           |
| `/test`      | Create and run relevant tests                                |
| `/review`    | Review code for bugs, quality, and maintainability           |
| `/refactor`  | Improve structure without changing behavior                  |
| `/security`  | Check vulnerabilities and security risks                     |
| `/optimize`  | Improve performance, efficiency, and resource usage          |
| `/verify`    | Final validation: build, tests, types, and expected behavior |

### Recommended workflow

```text
/analyze
→ /plan
→ /implement
→ /test
→ /debug
→ /review
→ /security
→ /optimize
→ /verify
```

For **AI FDE / agentic development**, I'd add one more:

```text
/agent
```

Use it for designing and implementing AI agents, tools, workflows, memory, MCP integrations, and human-approval flows.
