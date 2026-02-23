# BMAD Implementation Loop for Kilo Code

> **Plug-and-play autonomous coding loop for overnight sessions**

This setup combines BMAD Method planning with Kilo Code's Orchestrator mode to create a fully autonomous implementation loop that can run for hours until complete.

---

## 🚀 Quick Start

### 1. Copy Files to Your Project

```bash
# Copy these files to your project root:
# - AGENTS.md
# - .kilocode/
# - _bmad-output/
```

### 2. Create Planning Artifacts

Before running the loop, you need:

1. **PRD** (`_bmad-output/planning-artifacts/prd.md`)
   - Product requirements document
   - Can create manually or use BMAD Method

2. **Architecture** (`_bmad-output/planning-artifacts/architecture.md`)
   - Technical architecture decisions
   - Can create manually or use BMAD Method

3. **Stories** (`_bmad-output/planning-artifacts/stories.md`)
   - Story backlog with acceptance criteria
   - Template provided - fill in your stories

### 3. Initialize Loop State

Edit `_bmad-output/loop-artifacts/loop.md`:
- Update Epic name
- Add your story IDs to the progress table
- Set story count in metrics

### 4. Run in Kilo Code

1. Open Kilo Code in your project
2. Switch to **Orchestrator mode** (or use custom `bmad-orchestrator` mode)
3. Say:
   ```
   Run the implementation loop from .kilocode/rules/implementation-loop.md
   ```
4. Approve subtask delegations as needed
5. Walk away - it runs until complete!

---

## 📁 File Structure

```
your-project/
├── AGENTS.md                          # Kilo Code reads this
├── .kilocode/
│   ├── modes.json                     # Custom BMAD modes
│   └── rules/
│       └── implementation-loop.md     # Loop instructions
├── _bmad-output/
│   ├── planning-artifacts/
│   │   ├── prd.md                     # ← YOU CREATE THIS
│   │   ├── architecture.md            # ← YOU CREATE THIS
│   │   └── stories.md                 # ← YOU FILL THIS IN
│   └── loop-artifacts/
│       ├── loop.md                    # Loop state ledger
│       ├── story-template.md          # Template for stories
│       ├── stories/                   # Generated story files
│       ├── tests/                     # Generated test plans
│       └── trace/                     # Generated traceability
└── README.md                          # This file
```

---

## 🔄 How It Works

### The Story Loop (A → J)

```
A) PM: Select Story      → Pick next story from backlog
B) SM: Create Story      → Generate detailed story file
C) SM: Validate Story    → Check story is actionable
D) TEA: Create Tests     → Design test plan (ATDD)
E) DEV: Implement        → Write code (TDD: test first)
F) TEA: Run Tests        → Execute all tests
G) DEV: Code Review      → Review for quality
H) TEA: QA Signoff       → Pass/Fail gate
I) TEA: Traceability     → Create audit trail
J) PM: Mark Done         → Update workflow status
     │
     └─→ NEXT STORY (back to A)
```

### Quality Gates (Zero Tolerance)

Before proceeding:
- ✅ Tests = 100% passing
- ✅ Issues = 0
- ✅ Blockers = 0

If gates fail, loop back to fix.

---

## 🎯 Custom Modes

The `.kilocode/modes.json` defines 5 custom BMAD modes:

| Mode | Purpose | Kilo Base |
|------|---------|-----------|
| `bmad-orchestrator` | Run the loop | Orchestrator |
| `bmad-pm` | Select stories, track status | Architect |
| `bmad-sm` | Create/validate stories | Architect |
| `bmad-tea` | Tests, QA, traceability | Debug |
| `bmad-dev` | Implementation | Code |

You can use these OR native Kilo modes (Architect, Code, Debug).

---

## 💡 Pro Tips

### Model Selection
- Use **smart models** (Claude 4, GPT-4) for planning steps (A-D)
- Use **fast models** (Gemini Flash, MiniMax) for implementation (E-G)
- Configure per-mode in Kilo Code settings

### Recovery
If interrupted:
```
Resume implementation loop
```
It reads `loop.md` and continues from where it left off.

### Parallel Execution
For independent stories:
```
Run implementation loop for STORY-001 and STORY-002 in parallel
```

### Monitoring Progress
Check `loop.md` anytime to see:
- Current step
- Stories completed
- Issues/blockers
- Execution log

---

## 🔧 Customization

### Add Your Own Stories

Edit `_bmad-output/planning-artifacts/stories.md`:

```markdown
### STORY-001: User Authentication

**As a** user,
**I want** to log in with email and password,
**So that** I can access my account securely.

**Acceptance Criteria**:
- [ ] Login form with email/password fields
- [ ] Validation error messages
- [ ] Successful login redirects to dashboard
- [ ] Failed login shows error message

**Technical Notes**: Use JWT for session management
```

### Modify Quality Gates

Edit `.kilocode/rules/implementation-loop.md` to change:
- Test coverage threshold
- Max allowed issues
- Circuit breaker conditions

### Add New Steps

Add custom steps to the loop by:
1. Adding step definition to `implementation-loop.md`
2. Creating prompt template
3. Adding mode configuration if needed

---

## 🚨 Troubleshooting

### "Cannot find planning artifacts"
Create PRD, architecture, and stories files first.

### "Tests failing, loop stuck"
Check `loop.md` for which story is failing. Either:
- Fix the issue manually
- Mark story as blocked
- Skip story temporarily

### "Mode not available"
Make sure `.kilocode/modes.json` is valid JSON. Kilo Code should auto-detect custom modes.

### "Loop going in circles"
Circuit breaker should catch this after 3 failures. Check:
- `loop.md` for repeated failures
- Test output for root cause
- Consider simplifying story

---

## 📊 Comparison

| Feature | This Setup | Kilo Native |
|---------|------------|-------------|
| Overnight autonomy | ✅ Yes | ⚠️ Manual |
| Fresh context per step | ✅ Yes | ✅ Yes |
| Zero tolerance gates | ✅ Yes | ❌ No |
| Setup effort | ⚠️ Copy files | ✅ Built-in |
| Planning phases | ✅ BMAD phases | ✅ Architect |
| Traceability | ✅ Auto-generated | ❌ No |

---

## 📚 Resources

- [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD) - Full planning framework
- [Kilo Code Docs](https://kilo.ai/docs/) - Kilo documentation
- [Orchestrator Mode](https://kilo.ai/docs/code-with-ai/agents/orchestrator-mode) - How delegation works

---

## 📝 License

MIT - Use freely, modify as needed.

---

*Built for developers who want to code while they sleep.* 🌙

