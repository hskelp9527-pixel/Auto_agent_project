# Self-Evolution System

A file-driven autonomous development system based on [autoresearch](https://github.com/karpathy/autoresearch) (Andrej Karpathy).

## Core Philosophy

AI collaborates through files (PRD.md → task.md → progress.md → evaluation.md), developing one task at a time from task.md, with an independent evaluator scoring each iteration. Points gained → confirm complete, points lost → direct rewrite, iterating until 95 points.

**Core Features**:
- 📁 **File-Driven**: AI communicates through files, not context-dependent
- 🔄 **Autonomous Iteration**: Points gained confirm completion, points lost trigger rewrite, continue to 95 points
- 🤖 **Independent Evaluation**: Independent agent evaluates and scores, results written to evaluation.md
- 📝 **Complete Records**: task.md, progress.md, evaluation.md, ERROR_LOG.md

## When to Use

- ✅ "Help me develop a XXX project"
- ✅ "Use self-evolution-system to build from scratch"
- ✅ "Refactor this project"
- ✅ "Develop based on PRD"

## Core Workflow

```
Phase 1: Requirements → Generate PRD.md
   ↓
Phase 2: Task Breakdown → PRD.md → task.md
   ↓
Phase 3: Autonomous Iteration
   Read task.md → Get task → Develop → Evaluate →
   Points gained → Confirm complete, update progress.md
   Points lost → Direct rewrite
   No change → Next task
   ↓ Repeat until 95 points
Phase 4: Delivery (≥95 points)
```

## File Structure

```
project/
├── PRD.md              # Requirements document
├── task.md             # Task list
├── progress.md         # Development progress
├── evaluation.md       # Evaluation results
├── ERROR_LOG.md        # Error history
└── [project code]
```

## Scoring Criteria

| Dimension | Points | Description |
|-----------|--------|-------------|
| Feature Completeness | 60 | Pages, forms, data, charts, CRUD, usability |
| Design System | 20 | Dependencies, variants, tokens, accessibility, responsive |
| Code Quality | 10 | TypeScript, components, error handling, standards |
| Feature Verification | 10 | Startup, navigation, interaction, responsive |
| **Total** | **100** | **Target: ≥95** |

## Key Principles

1. **File-Driven Communication** ⚠️ **Highest Priority**
   - AI communicates through files, not context-dependent
   - Different AI can continue by reading files

2. **ERROR_LOG.md Mandatory**
   - Must read before each development
   - Avoid repeating errors

3. **Iterative Improvement**
   - Points gained → confirm completion
   - Points lost → direct rewrite
   - Continue to 95 points

4. **Independent Evaluation**
   - Independent agent evaluates
   - Results written to evaluation.md

5. **No Mid-Process Interruption**
   - Don't ask user at 75 points or other intermediate scores
   - Continue iterating to 95 points

## References

- `SKILL.md` - Main documentation
- `references/evaluation-standard.md` - Evaluation criteria
- `references/common-traps.md` - Common pitfalls
- `references/example-workflow.md` - Example workflow
- `references/key-principles.md` - Key principles

## Origin

Based on [autoresearch](https://github.com/karpathy/autoresearch) (Andrej Karpathy).

**autoresearch original**:
- Domain: Machine learning research
- Method: AI modifies train.py → trains → evaluates → keep or discard

**This system adaptation**:
- Domain: General development projects
- Method: AI reads task.md → develops → evaluates → points gained confirm/points lost rewrite

## Version History

- **v2.0.0** (2025-03-29): **Major Refactor - File-Driven Autonomous Development System**
  - Refactored from "verification tool" to "development system"
  - Core workflow: PRD → task → develop → evaluate → iterate
  - File-driven: AI communicates through files
  - Points gained confirm, points lost rewrite
  - 95-point delivery standard

- **v1.3.0** (2025-03-29): Optimized per official skill-creator
  - Split file structure (732→200 lines)
  - More pushy description

- **v1.2.0** (2025-03-29): Added Round 0 ERROR_LOG.md mandatory check

- **v1.1.0** (2025-03-29): Initial version

## 🙏 Acknowledgments

Thanks to Andrej Karpathy's [autoresearch](https://github.com/karpathy/autoresearch) project for the core idea and inspiration.
