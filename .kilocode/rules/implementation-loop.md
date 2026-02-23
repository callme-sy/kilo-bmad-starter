# BMAD Implementation Loop for Kilo Code Orchestrator

> **Purpose:** Autonomous implementation loop that runs BMAD-style workflows through Kilo Code's Orchestrator mode. Each step runs in fresh context via subtask delegation.

---

## 🎯 Orchestrator Instructions

When activated, you are the **BMAD Loop Orchestrator**. Your job is to:
1. Read the current state from `loop.md` ledger
2. Execute the next step in the STORY LOOP
3. Delegate to specialized roles via subtasks
4. Append results to `loop.md`
5. Enforce quality gates before proceeding
6. Loop until ALL STORIES DONE

---

## 📋 Role Legend

| Color | Role | Kilo Mode | Responsibility |
|-------|------|-----------|----------------|
| 🔵 Blue | PM (Product) | Architect | Select stories, workflow status |
| 🟢 Green | SM (Scrum) | Architect | Create/validate stories, sprint planning |
| 🟠 Orange | TEA (Quality) | Debug | Test design, ATDD, test review, traceability |
| 🔴 Red | DEV (Code) | Code | Implementation, code review |
| 🔵 Cyan | Architect | Architect | Architecture, implementation readiness |

---

## 🚨 Critical Orchestrator Gates

Before ANY subtask delegation, verify:

| Gate | Requirement | Action if Failed |
|------|-------------|------------------|
| **Context Guard** | Must be on MAIN branch. No drift. | Stop. Alert user to sync/merge. |
| **Ledger Update** | Verbatim subtask return appended to `loop.md` | Cannot proceed without ledger entry. |
| **Zero Tolerance** | Tests=100%, Issues=0, Blockers=0 | Loop back to fix before forward progress. |
| **Artifacts** | Return Capsule + Ready Flag required | Re-run subtask with incomplete flag. |

---

## 🔄 The Loop Flowchart

```
START
  │
  ▼
┌─────────────────────────────────────────┐
│  PHASE 3: SYS READY (Optional)          │
│  • Architect: Check impl-readiness      │
│  • TEA: Check test-design exists        │
└─────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────┐
│  PHASE 4: EPIC START                    │
│  • SM: Sprint planning                  │
│  • TEA: Test design for epic            │
└─────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────┐
│  STEP 1: STORY LOOP (A → J)             │
│                                         │
│  A) PM: Select Story [Determine ID]     │
│  B) SM: create-story [Story File]       │
│  C) SM: validate [Validation]           │
│  D) TEA: atdd [Test Plan]               │
│  E) DEV: dev-story [Implementation]     │
│  F) TEA: automate [Tests]               │
│  G) DEV: code-review [Review]           │
│  H) TEA: test-review [QA Signoff]       │
│  I) TEA: trace [Traceability]           │
│  J) PM: workflow-status [Done]          │
│                                         │
└─────────────────────────────────────────┘
  │
  ├─── NEXT STORY ───┐
  │                  │
  │                  ▼ (loop back to A)
  │
  │  ALL STORIES DONE
  │         │
  ▼         ▼
┌─────────────────────────────────────────┐
│  STEP 2: EPIC COMPLETION                │
│  • TEA: trace (epic-level)              │
│  • SM: epic-retrospective               │
└─────────────────────────────────────────┘
  │
  ▼
DONE
```

---

## 📝 Subtask Delegation Protocol

For each step (A-J), follow this pattern:

### 1. Pre-Delegation
```
Read loop.md for current state
Identify next step (A-J)
Prepare context for subtask
```

### 2. Subtask Creation
Use `new_task` with:
- **mode**: Appropriate Kilo mode (see Role Legend)
- **instructions**: Step-specific prompt + relevant artifacts
- **context**: Minimal - just what's needed for this step

### 3. Post-Delegation
```
Wait for subtask completion
Append Return Capsule to loop.md
Verify Ready Flag
Apply quality gates
Proceed to next step or loop back
```

---

## 📦 Return Capsule Format

Each subtask must return:

```markdown
## Return Capsule: [STEP ID]

**Status**: ✅ Ready / ⚠️ Blocked / ❌ Failed
**Story ID**: [STORY-ID]
**Artifacts Created**:
- [ ] `[filename]` - [description]
- [ ] `[filename]` - [description]

**Test Results**:
- Tests Run: [N]
- Passed: [N]
- Failed: [N]

**Issues Found**: [N]
**Blockers**: [N]

**Summary**: [1-2 sentence summary]

**Ready Flag**: [TRUE/FALSE]
```

---

## 🔧 Step-Specific Prompts

### Step A: PM Select Story
```
Mode: Architect
Task: Select next story from backlog

Read: _bmad-output/planning-artifacts/stories.md
Output: Story ID for this iteration
Return: Story ID + brief description
```

### Step B: SM Create Story
```
Mode: Architect
Task: Create detailed story file

Input: Story ID from Step A
Read: Architecture doc, PRD
Output: _bmad-output/loop-artifacts/stories/[STORY-ID].md
Template: story-template.md
```

### Step C: SM Validate
```
Mode: Architect
Task: Validate story is complete and actionable

Input: Story file from Step B
Checklist:
- [ ] Clear acceptance criteria
- [ ] Technical approach defined
- [ ] Dependencies identified
- [ ] Test scenarios listed
Output: Validation report
```

### Step D: TEA ATDD
```
Mode: Debug
Task: Create Acceptance Test-Driven Design plan

Input: Validated story file
Output: _bmad-output/loop-artifacts/tests/[STORY-ID]-test-plan.md
Include:
- Test cases for each acceptance criteria
- Edge cases
- Integration test needs
```

### Step E: DEV Story Implementation
```
Mode: Code
Task: Implement the story following TDD

Input: Story file + Test plan
Process:
1. Write failing tests first
2. Implement minimum code to pass
3. Refactor if needed
4. Run all tests
Output: Working code + passing tests
```

### Step F: TEA Automate Tests
```
Mode: Debug
Task: Automate and run all tests for story

Input: Implemented story
Process:
1. Run unit tests
2. Run integration tests
3. Capture results
Output: Test execution report
Gate: Must be 100% passing to proceed
```

### Step G: DEV Code Review
```
Mode: Code
Task: Review implemented code

Input: All code changes for story
Checklist:
- [ ] Code follows project standards
- [ ] No security vulnerabilities
- [ ] Adequate test coverage
- [ ] Documentation updated
Output: Code review report
```

### Step H: TEA Test Review
```
Mode: Debug
Task: QA Signoff on story

Input: Test plan + Test results + Code review
Process:
1. Verify all tests passing
2. Verify acceptance criteria met
3. Check for regressions
Output: QA Signoff (PASS/FAIL)
Gate: Must PASS to proceed
```

### Step I: TEA Trace
```
Mode: Debug
Task: Create traceability matrix

Input: All story artifacts
Output: _bmad-output/loop-artifacts/trace/[STORY-ID]-trace.md
Include:
- Requirement → Test → Code mapping
- Coverage report
```

### Step J: PM Workflow Status
```
Mode: Architect
Task: Update workflow status

Input: All completed artifacts
Output: Update loop.md with story completion
Check: More stories in backlog?
- YES → Return to Step A
- NO → Proceed to Epic Completion
```

---

## 🏁 Epic Completion

When ALL STORIES DONE:

### TEA: Epic-Level Trace
```
Mode: Debug
Task: Create epic-level traceability

Input: All story traces
Output: _bmad-output/loop-artifacts/trace/epic-trace.md
```

### SM: Epic Retrospective
```
Mode: Architect
Task: Document learnings and improvements

Input: All loop.md entries
Output: _bmad-output/loop-artifacts/retrospective.md
Include:
- What went well
- What could improve
- Process improvements for next epic
```

---

## 🛑 Circuit Breaker

Stop the loop if:
- Same story fails 3 consecutive times
- Total blockers > 5
- User sends STOP signal
- Context drift detected (not on MAIN)

---

## 📁 Required File Structure

```
project/
├── _bmad-output/
│   ├── planning-artifacts/
│   │   ├── prd.md
│   │   ├── architecture.md
│   │   └── stories.md
│   └── loop-artifacts/
│       ├── loop.md              # Ledger (CRITICAL)
│       ├── stories/             # Individual story files
│       ├── tests/               # Test plans
│       └── trace/               # Traceability matrices
├── .kilocode/
│   └── rules/
│       └── implementation-loop.md  # This file
└── AGENTS.md                    # Kilo reads this
```

---

## 🚀 Quick Start

1. Ensure planning artifacts exist (PRD, Architecture, Stories)
2. Initialize `loop.md` with epic/story list
3. In Kilo Code, switch to Orchestrator mode
4. Say: "Run the implementation loop from .kilocode/rules/implementation-loop.md"
5. Approve subtask delegations as needed
6. Walk away - it will run until complete

---

## 💡 Pro Tips

- **Model Selection**: Use powerful models (Claude 4, GPT-4) for planning steps (A-D), faster models for implementation (E-G)
- **Checkpoint**: Review `loop.md` periodically to see progress
- **Recovery**: If interrupted, just say "Resume implementation loop" - it reads `loop.md` for state
- **Parallel**: For independent stories, can run multiple loops in parallel

