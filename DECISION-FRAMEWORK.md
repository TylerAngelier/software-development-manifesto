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
| 1. Trivial fix | Typo, comment | Fix → Commit |
| 2. Bug fix | Login error | Clear prompt → Implement → Review → Commit |
| 3. Small feature | Add filter | Lightweight spec (What/Why) → Implement → Review |
| 4. Complex feature | Auth system | Full spec + design notes → Implement → Thorough review |
| 5. New product | Greenfield | Full PRD → Design doc → Stakeholder approval → Implement |

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

AI can help you draft this, challenge your assumptions, and even prototype quickly to test ideas before committing.

**Key insight:** Prototyping is now fast enough to be a planning tool. Build throwaway versions to learn, not to ship.

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

The difference in output quality is dramatic.

### 4. Build

Brief the agent properly. Give it:
- Background on what you're building
- Your technical design decisions
- The specific task
- Constraints and preferences

An LLM makes hundreds of small decisions as it writes code. Context makes those decisions well-informed. Without it, the agent guesses.

### 5. Review

This is the step most people skip. Don't.

**Self-review:** Ask the agent to review its own code. Generation and review are different cognitive tasks. Almost every time, the agent finds issues it missed on the first pass.

```
Look at the code you just wrote. Find any bugs, edge cases,
security issues, or potential problems.
```

Common things caught on review:
- Edge cases and error handling
- Security vulnerabilities
- Missing input validation
- Performance issues

**Human review:** You don't need to read every line, but understand what was built. Does it match your design? Are there obvious issues?

**Automated review:** Layer in CI/CD checks, static analysis, and AI-powered PR review tools.

### 6. Deploy

Get your code to production. AI can help you set up deployment pipelines, CI/CD, and infrastructure if you're not familiar with them.

**Example prompt:**

```
Commit and save these changes with a clear commit message.
Then push the latest version to GCP Cloud Run.
```

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

---

## Integration with Repo Workflow

This framework complements the repo's `/spec` → `/implement-task` → `/conventional-commit` workflow:

- **Trivial/Bug fixes:** Skip `/spec`, use direct prompts with review
- **Features:** Use `/spec` with depth proportional to complexity
- **Complex features:** Full spec with design notes, constraints, thorough review

When in doubt, start with more rigor. It is easier to reduce ceremony than to add missing controls retroactively.
