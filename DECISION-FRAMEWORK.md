# Decision Framework

## Core Principle

**Clearer instructions always produce better output, regardless of task size.**

Not every task needs every step. This framework helps you match workflow rigor to task complexity.

---

## The 7-Stage Software Development Lifecycle

Every piece of software ever built goes through some version of this lifecycle. AI doesn't replace it — it accelerates every step.

```mermaid
flowchart LR
    R[Requirements] --> D[Technical Design]
    D --> P[Task Breakdown]
    P --> B[Build]
    B --> Rv[Review]
    Rv --> Dp[Deploy]
    Dp --> M[Monitor]

    style R fill:#1a1a2e,color:#fff
    style D fill:#1a1a2e,color:#fff
    style P fill:#1a1a2e,color:#fff
    style B fill:#4ade80,color:#000
    style Rv fill:#1a1a2e,color:#fff
    style Dp fill:#1a1a2e,color:#fff
    style M fill:#1a1a2e,color:#fff
```

**Key insight:** Most developers use AI for one stage (Build). The best engineers use AI at every step.

---

## The Rigor Scale

Match your workflow depth to task complexity:

| Level | Example | Workflow |
|-------|---------|----------|
| 1. Trivial fix | Typo, comment | Direct fix → `/conventional-commit` |
| 2. Bug fix | Login error | `/bugfix-fast-path` |
| 3. Small feature | Add filter | Lightweight `/spec` → `/implement-task` |
| 4. Complex feature | Auth system | Full `/spec` + design notes → `/implement-task` |
| 5. New product | Greenfield | Full PRD → Design doc → Stakeholder approval → implementation plan |

---

## Make It Executable

Use `/workflow-triage` when the path is not obvious. It should answer:

1. **What class of work is this?**
2. **What is the minimum safe workflow?**
3. **What additional questions must be answered before work starts?**

Default routing:

- **Trivial fix** → direct change, then `/conventional-commit`
- **Bug fix** → `/bugfix-fast-path`
- **Approved Beads task** → `/implement-task`
- **Feature or ambiguous scoped change** → `/spec`
- **Standalone review request** → `/review-task`

---

## Decision Questions

Before starting, ask yourself:

1. **Does this touch user data?**
2. **Is it security-related?**
3. **Is it hard to roll back?**
4. **Does it affect multiple systems?**
5. **Is the requirements scope clear?**
6. **Could this introduce regressions?**

If **yes** to any question: increase your rigor level.

---

## Fast Classification Heuristics

### Trivial fix

Use the direct path when the change is low-risk and non-behavioral.

Examples:
- typo fixes
- comment cleanup
- obvious docs corrections

### Bug fix

Use `/bugfix-fast-path` when the behavior is wrong and the expected behavior is already known.

Examples:
- null handling bug
- regression in validation
- broken edge case in an existing feature

Escalate if the "bug fix" actually changes product behavior or architecture.

### Small feature

Use a lightweight `/spec` when the change is new behavior but still bounded.

Examples:
- add a filter
- add an export button
- add a small admin control

### Complex feature

Use a full `/spec` when the work involves architecture, migrations, rollout, or multiple systems.

Examples:
- auth redesign
- billing workflow changes
- background jobs with retries and operational impact

---

## Stage Details

### 1. Requirements (PRD)

Before you write a single line of code, get clear on what you're building, why, and what's out of scope.

**Produce a short requirements doc:**

```
What: User authentication system
Why: Users need accounts to save preferences
Who: End users of the web app
In scope: Email/password login, signup page
Out of scope: OAuth, password reset (v1), admin roles
```

AI can help you draft this, challenge assumptions, and even prototype quickly to test ideas before committing.

### 2. Technical Design

Requirements are *what* and *why*. Technical design is *how*.

Decide upfront:
- Database choice and data model
- Architecture (monolith, microservices, serverless)
- Auth strategy
- Key constraints and tradeoffs

These decisions are hard to undo. Don't let an agent make them unchecked. Use AI to think through tradeoffs, but own the decisions yourself.

### 3. Task Breakdown

Break the project into small, clear, bounded tasks. This is project management — and it maps directly onto how you should work with coding agents.

**Don't:** Hand an agent your entire application and say "build this."

**Do:** Give it one specific task with all the context it needs.

```
Task: Create the login API endpoint
Context: We're using FastAPI with SQLAlchemy async.
Auth is JWT tokens in httpOnly cookies.
User model is already defined in app/models/user.py.
Follow the existing pattern in app/routers/health.py.
```

### 4. Build

Brief the agent properly. Give it:
- background on what you're building
- your technical design decisions
- the specific task
- constraints and preferences

### 5. Review

This is the step most people skip. Don't.

**Self-review:** Ask the agent to review its own code. Generation and review are different cognitive tasks. Almost every time, the agent finds issues it missed on the first pass.

Common things caught on review:
- edge cases and error handling
- security vulnerabilities
- missing input validation
- performance issues
- weak or missing verification

### 6. Deploy

Get your code to production. AI can help you set up deployment pipelines, CI/CD, and infrastructure if you're not familiar with them.

### 7. Monitor

Set up monitoring before your users tell you something is broken.

**Minimum monitoring stack:**
- **Error tracking** (e.g., Sentry) — real-time errors with stack traces
- **Uptime monitoring** — know when your app goes down
- **Alerts** — get notified immediately
- **Logs** — understand what happened and why

---

## Anti-Patterns

Avoid these common mistakes:

| Anti-Pattern | Problem | Instead |
|--------------|---------|---------|
| Over-engineering simple fixes | Typo → full spec | Match rigor to complexity |
| Under-planning complex features | Auth system → "just build it" | Use appropriate rigor level |
| Skipping review | "It looks fine" | Always review, even for small changes |
| Handing agents too much scope | "Build the whole app" | One bounded task at a time |
| First output acceptance | Taking generated code as-is | Ask for self-review |
| Invisible verification gaps | Closing work with no evidence | Record what was actually verified |

---

## Integration with Repo Workflow

This framework complements the repo's `/workflow-triage` → `/spec` → `/implement-task` → `/conventional-commit` workflows:

- **Trivial fixes:** Direct prompt, direct fix, then commit
- **Bug fixes:** `/bugfix-fast-path`
- **Features:** `/spec` with depth proportional to complexity
- **Complex features:** Full spec with design notes, impact, constraints, and thorough review
- **Standalone review:** `/review-task`

When in doubt, start with more rigor. It is easier to reduce ceremony than to add missing controls retroactively.
