# Project Configuration for Kilo Code

This file configures AI agent behavior for this project.

---

## 🔄 BMAD Implementation Loop

This project uses a BMAD-style implementation loop for autonomous development.

### Activation

In Kilo Code, switch to **Orchestrator mode** and say:
```
Run the implementation loop from .kilocode/rules/implementation-loop.md
```

### How It Works

1. **Orchestrator** reads the loop state from `loop.md`
2. Delegates steps to specialized roles via subtasks
3. Each subtask runs in **fresh context**
4. Results are captured in `loop.md` ledger
5. Quality gates enforce: Tests=100%, Issues=0
6. Loop continues until all stories complete

### Required Files

| File | Purpose |
|------|---------|
| `_bmad-output/planning-artifacts/prd.md` | Product Requirements |
| `_bmad-output/planning-artifacts/architecture.md` | Technical Architecture |
| `_bmad-output/planning-artifacts/stories.md` | Story Backlog |
| `_bmad-output/loop-artifacts/loop.md` | Loop State Ledger |

### Quick Commands

| Command | Action |
|---------|--------|
| `Run implementation loop` | Start autonomous loop |
| `Resume implementation loop` | Continue after interruption |
| `Show loop status` | Display current progress |
| `Stop loop` | Halt execution |

---

## 📋 Project-Specific Instructions

<!-- Add your project-specific instructions below -->

### Coding Standards
- Follow existing code patterns
- Write tests for all new code
- Update documentation as needed

### Commit Convention
- Use conventional commits: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`
- One commit per story
- Reference story ID in commit message

---

## 🔧 Mode Configuration

See `.kilocode/rules/` for detailed mode configurations.

