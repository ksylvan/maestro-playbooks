# Maestro-Playbooks

Playbooks for [RunMaestro.ai](https://github.com/pedramamini/Maestro) Agent Orchestrator.

## What Are Playbooks?

Playbooks are saved Auto Run configurations that automate multi-step workflows. Each playbook defines a sequence of markdown documents with task checkboxes that Maestro processes through AI agents, looping until exit conditions are met.

## Available Playbooks

### Development Playbooks

Code improvement workflows that work with Maestro's **default agent prompt**.

| Playbook | Purpose | Exit Condition |
|----------|---------|----------------|
| `Development/Performance/` | Find and fix performance issues | No PENDING items remain |
| `Development/Security/` | Audit and remediate vulnerabilities | No CRITICAL/HIGH severity issues |
| `Development/Refactor/` | Simplify code, eliminate duplication | No LOW risk + HIGH benefit items |
| `Development/Documentation/` | Achieve 90% doc coverage | Coverage >=90% or no HIGH importance gaps |
| `Development/Testing/` | Achieve 80% test coverage | Coverage >=80% or no auto-testable work |
| `Development/Usage/` | Update README to match actual features | No CRITICAL/HIGH importance gaps |

### Research Playbooks

Knowledge-building workflows that require **custom agent prompts** with user configuration.

| Playbook | Purpose | Exit Condition |
|----------|---------|----------------|
| `Research/Market/` | Build Obsidian-style knowledge vault about a market | Coverage targets met or no HIGH importance entities remain |

## Playbook Architecture

Each playbook follows a 5-document chain pattern:

```
1_ANALYZE.md     -> Survey target, identify what to research/fix
2_FIND_*.md      -> Find specific issues/gaps/entities
3_EVALUATE.md    -> Rate candidates by priority criteria
4_IMPLEMENT.md   -> Execute one item (fix code or research entity)
5_PROGRESS.md    -> Loop gate: resets 1-4 if work remains, exits if done
```

### Loop Control Mechanism

- Documents 1-4 have `Reset: OFF` (don't auto-reset when completed)
- Document 5 has `Reset: ON` and controls the loop by conditionally resetting 1-4
- Each loop iteration creates `LOOP_N_*` working files with incremented loop number

### Agent Prompt Requirements

- **Development playbooks**: Use Maestro's default agent prompt
- **Research playbooks**: Require custom `Agent-Prompt.md` with user configuration (see playbook README)

## Status Values

Items in `LOOP_N_PLAN.md` use these statuses:

| Status | Meaning |
|--------|---------|
| `PENDING` | Ready for automated implementation/research |
| `IMPLEMENTED` / `RESEARCHED` | Completed in current loop |
| `WON'T DO` / `SKIP` | Skipped intentionally (with reason) |
| `PENDING - MANUAL REVIEW` | Requires human decision |

## Using These Playbooks

### Setup in Maestro

1. Open Maestro and select Auto Run mode
2. Choose a playbook folder (e.g., `Development/Performance/`)
3. **For Research playbooks**: Configure `Agent-Prompt.md` with your target topic
4. Configure settings:
   - **Loop Mode**: ON (for continuous iteration)
   - **Max Loops**: Set a reasonable limit (5-10 for Development, 20-30 for Research)
   - Documents 1-4: `Reset: OFF`
   - Document 5: `Reset: ON`

### Review-First Approach (Recommended)

1. Run once with Loop Mode OFF
2. Review the generated `LOOP_1_PLAN.md`
3. Manually adjust statuses if needed:
   - Change `PENDING` to `WON'T DO` / `SKIP` to skip items
   - Change to `PENDING - MANUAL REVIEW` for risky items
4. Run again with Loop Mode ON

### Template Variables

These variables are substituted by Maestro at runtime:

| Variable | Description |
|----------|-------------|
| `{{AGENT_NAME}}` | Name of the Maestro agent |
| `{{AGENT_PATH}}` | Root path of the target project |
| `{{AUTORUN_FOLDER}}` | Path to the Auto Run folder |
| `{{LOOP_NUMBER}}` | Current loop iteration (1, 2, 3...) |
| `{{DATE}}` | Today's date (YYYY-MM-DD) |
| `{{CWD}}` | Current working directory |

## Customization

### Adjusting Aggressiveness

Edit `4_IMPLEMENT.md` in any playbook to change which items get auto-processed:
- Default: LOW complexity/risk + HIGH gain/benefit only
- More aggressive: Include MEDIUM complexity
- Conservative: Require VERY HIGH gain

### Adding Custom Tactics

Edit `1_ANALYZE.md` to add domain-specific investigation patterns.

### Creating New Playbooks

Copy an existing playbook folder and modify:
1. Update the Context section with `**Playbook:** [Name]`
2. Adjust the discovery patterns in step 2
3. Modify rating criteria in step 3
4. Change exit conditions in step 5
5. For Research playbooks: Create an `Agent-Prompt.md` with configuration placeholders

## Tips

1. **Start without loop mode** - Review what it finds before enabling automation
2. **Set max loops** - Prevent runaway iterations
3. **Check the logs** - Each playbook maintains a cumulative log file
4. **Commit frequently** - Each loop iteration is a good commit point
5. **Run tests** - After Development playbook changes, verify nothing broke

## License

See [LICENSE](LICENSE) for details.
