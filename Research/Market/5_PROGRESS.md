# Research Progress Gate

## Context
- **Playbook:** Market Research
- **Agent:** {{AGENT_NAME}}
- **Auto Run Folder:** {{AUTORUN_FOLDER}}
- **Loop:** {{LOOP_NUMBER}}

## Purpose

This document is the **progress gate** for the market research pipeline. It checks whether there are still entities to research and whether coverage targets have been met. **This is the only document with Reset ON** - it controls loop continuation by resetting tasks in documents 1-4 when more work is needed.

## Instructions

1. **Read the research plan** from `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_PLAN.md`
2. **Count entities by status** (PENDING vs RESEARCHED)
3. **Check coverage** against targets from market analysis
4. **Decide whether to continue or exit**
5. **If continuing**: Reset all tasks in documents 1-4
6. **If exiting**: Do NOT reset - finalize the vault

## Progress Check

- [ ] **Check progress and decide**: Read `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_PLAN.md` and count PENDING vs RESEARCHED entities. Check if minimum coverage targets are met (from market analysis). If PENDING entities with CRITICAL/HIGH importance remain, reset documents 1-4 to continue. If coverage is sufficient or only LOW importance items remain, do NOT reset - allow pipeline to exit.

## Reset Tasks (Only if more research needed)

If the progress check determines we need to continue, reset all tasks in the following documents:

- [ ] **Reset 1_ANALYZE.md**: Uncheck all tasks in `{{AUTORUN_FOLDER}}/1_ANALYZE.md`
- [ ] **Reset 2_DISCOVER.md**: Uncheck all tasks in `{{AUTORUN_FOLDER}}/2_DISCOVER.md`
- [ ] **Reset 3_EVALUATE.md**: Uncheck all tasks in `{{AUTORUN_FOLDER}}/3_EVALUATE.md`
- [ ] **Reset 4_RESEARCH.md**: Uncheck all tasks in `{{AUTORUN_FOLDER}}/4_RESEARCH.md`

**IMPORTANT**: Only reset documents 1-4 if there are PENDING entities with CRITICAL or HIGH importance. If all high-priority entities are RESEARCHED, leave these reset tasks unchecked to allow the pipeline to exit.

## Decision Logic

```
IF LOOP_{{LOOP_NUMBER}}_PLAN.md doesn't exist:
    → Do NOT reset anything (PIPELINE JUST STARTED - LET IT RUN)

ELSE IF no PENDING entities with CRITICAL or HIGH importance:
    → Do NOT reset anything (COVERAGE SUFFICIENT - EXIT)
    → Finalize the vault (update INDEX.md, create summary)

ELSE:
    → Reset documents 1-4 (CONTINUE RESEARCHING)
```

## How This Works

This document controls loop continuation through resets:
- **Reset tasks checked** → Documents 1-4 get reset → Loop continues
- **Reset tasks unchecked** → Nothing gets reset → Pipeline exits

### Exit Conditions (Do NOT Reset)

1. **Coverage Met**: All entity categories have minimum coverage
2. **High Priority Done**: All CRITICAL and HIGH importance entities researched
3. **Max Loops**: Hit the loop limit in Batch Runner
4. **Diminishing Returns**: Only LOW importance or VERY HARD entities remain

### Continue Conditions (Reset Documents 1-4)

1. There are PENDING entities with CRITICAL or HIGH importance
2. Entity categories are below minimum coverage targets
3. We haven't hit max loops

## Current Status

Before making a decision, assess the vault:

| Metric | Value |
|--------|-------|
| **Total Entities Discovered** | ___ |
| **Entities Researched** | ___ |
| **PENDING (CRITICAL/HIGH)** | ___ |
| **PENDING (MEDIUM/LOW)** | ___ |
| **SKIP** | ___ |

### Coverage by Category

| Category | Target | Researched | Status |
|----------|--------|------------|--------|
| Companies | [X] | [Y] | [MET/BELOW] |
| Products | [X] | [Y] | [MET/BELOW] |
| People | [X] | [Y] | [MET/BELOW] |
| Technologies | [X] | [Y] | [MET/BELOW] |
| Trends | [X] | [Y] | [MET/BELOW] |

## Research History

Track progress across loops:

| Loop | Entities Researched | Total in Vault | Decision |
|------|---------------------|----------------|----------|
| 1 | ___ | ___ | [CONTINUE / EXIT] |
| 2 | ___ | ___ | [CONTINUE / EXIT] |
| ... | ... | ... | ... |

## Finalization Tasks (On Exit Only)

If exiting, perform these finalization tasks:

- [ ] **Update INDEX.md**: Ensure all researched entities are linked
- [ ] **Create vault summary**: Add research statistics to INDEX.md
- [ ] **Review connections**: Check that inter-page links are working
- [ ] **Note gaps**: Document any entities that couldn't be researched

## Vault Summary Template

Add to INDEX.md on exit:

```markdown
## Research Summary

**Research Period:** [Start Date] - {{DATE}}
**Total Loops:** {{LOOP_NUMBER}}
**Agent:** {{AGENT_NAME}}

### Coverage Statistics
| Category | Count |
|----------|-------|
| Companies | [X] |
| Products | [X] |
| People | [X] |
| Technologies | [X] |
| Trends | [X] |
| **Total Entities** | [X] |

### Research Notes
[Any important notes about coverage gaps or limitations]
```

## Manual Override

**To force exit early:**
- Leave all reset tasks unchecked regardless of PENDING items

**To continue despite meeting targets:**
- Check the reset tasks to keep researching

**To pause for review:**
- Leave unchecked
- Review the vault contents
- Restart when ready

## Notes

- This playbook focuses on building breadth first, then depth
- CRITICAL/HIGH entities should be researched before expanding to MEDIUM/LOW
- Quality of research matters more than hitting exact coverage numbers
- The vault should be useful and navigable, not exhaustive
