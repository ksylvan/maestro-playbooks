# Market Research Playbook

A systematic Auto Run playbook for building comprehensive knowledge vaults about specific markets using web research.

## Overview

This playbook creates an automated pipeline that:
1. **Analyzes** a target market to understand its structure and key entity types
2. **Discovers** specific entities (companies, products, people, technologies, trends)
3. **Evaluates** each entity by importance and research effort
4. **Researches** high-priority entities using web search
5. **Loops** until coverage targets are met or all high-priority entities are researched

The output is an **Obsidian-style knowledge vault** with interlinked markdown files.

## Requirements

### Custom Agent Prompt Required

**This playbook requires a custom agent prompt.** Unlike the Development playbooks which work with Maestro's default agent prompt, this playbook needs the `Agent-Prompt.md` file configured with:

1. **MARKET_TOPIC**: The specific market you want to research
2. **OUTPUT_FOLDER**: Where the vault files should be created

**Setup Steps:**
1. Open `Agent-Prompt.md` in this folder
2. Replace the example MARKET_TOPIC with your target market
3. Optionally adjust the OUTPUT_FOLDER path
4. In Maestro, set the agent prompt to use this file's contents

### Web Search Access

This playbook relies heavily on web search to gather market information. Ensure your Maestro agent has web search capabilities enabled.

## Document Chain

| Document | Purpose | Reset on Completion? |
|----------|---------|---------------------|
| `Agent-Prompt.md` | Custom agent prompt with market configuration | N/A |
| `1_ANALYZE.md` | Survey market, identify entity types, create templates | No |
| `2_DISCOVER.md` | Find specific entities in each category | No |
| `3_EVALUATE.md` | Prioritize entities by importance and effort | No |
| `4_RESEARCH.md` | Research one entity, create vault profile | No |
| `5_PROGRESS.md` | Coverage gate - resets 1-4 if targets not met | **Yes** |

## Generated Files

### Working Documents (per loop)
- `LOOP_N_MARKET_ANALYSIS.md` - Market overview and entity templates
- `LOOP_N_ENTITIES.md` - Discovered entities with basic info
- `LOOP_N_PLAN.md` - Prioritized research targets
- `RESEARCH_LOG_{{AGENT_NAME}}_{{DATE}}.md` - Cumulative research log

### Vault Output
```
vault/
├── INDEX.md           # Launch page with navigation
├── Companies/         # Company profiles
│   ├── Company-A.md
│   └── Company-B.md
├── Products/          # Product profiles
├── People/            # Key people profiles
├── Technologies/      # Technology overviews
├── Trends/            # Trend analyses
└── Resources/         # References and data sources
```

## Entity Status Values

| Status | Meaning |
|--------|---------|
| `PENDING` | Discovered, awaiting research |
| `RESEARCHED` | Full profile created in vault |
| `SKIP` | Not important enough to research |
| `DUPLICATE` | Covered under another entity |

## How It Works

### The Loop Control Mechanism

- **Documents 1-4** have `Reset: OFF` - they don't auto-reset
- **Document 5** has `Reset: ON` - it controls loop continuation

When `5_PROGRESS.md` runs:
1. It checks how many PENDING entities with CRITICAL/HIGH importance remain
2. **If high-priority PENDING entities exist**: Reset docs 1-4 → continue
3. **If no high-priority PENDING entities**: Don't reset → exit and finalize vault

### Single Loop Iteration

Each loop iteration:
1. Analyzes/refines understanding of the market
2. Discovers new entities in one category
3. Evaluates one newly discovered entity
4. Researches one PENDING entity thoroughly
5. Checks if more research is needed

### Exit Conditions

The pipeline exits when ANY of these are true:
1. All CRITICAL/HIGH importance entities are RESEARCHED
2. Coverage targets from market analysis are met
3. Max loop limit reached
4. Only LOW importance or VERY HARD entities remain

## Recommended Setup

### In Maestro Batch Runner
```
Loop Mode: ON
Max Loops: 20-30 (market research takes many iterations)
Documents:
  1_ANALYZE.md    [Reset: OFF]
  2_DISCOVER.md   [Reset: OFF]
  3_EVALUATE.md   [Reset: OFF]
  4_RESEARCH.md   [Reset: OFF]
  5_PROGRESS.md   [Reset: ON]
```

### Agent Prompt Configuration

**Critical**: Set your Maestro agent to use the contents of `Agent-Prompt.md` as its system prompt.

## Template Variables Used

| Variable | Description |
|----------|-------------|
| `{{AGENT_NAME}}` | Name of your Maestro agent |
| `{{AUTORUN_FOLDER}}` | Path to this playbook folder |
| `{{LOOP_NUMBER}}` | Current loop iteration (1, 2, 3...) |
| `{{DATE}}` | Today's date (YYYY-MM-DD) |

## Example Markets

This playbook works well for researching:

- **Technology Markets**: "AI Code Assistants", "Kubernetes Platforms", "No-Code Tools"
- **Consumer Markets**: "Plant-Based Foods", "Smart Home Devices", "Electric Vehicles"
- **B2B Markets**: "Sales Enablement Software", "Cloud Security", "HR Tech"
- **Emerging Markets**: "Carbon Capture", "Quantum Computing", "Synthetic Biology"

## Output: The Knowledge Vault

The vault uses Obsidian-compatible markdown with:

- **`[[wiki-style]]` links** connecting related entities
- **Consistent templates** for each entity type
- **INDEX.md** as the central navigation hub
- **Source citations** for all factual claims
- **Subfolder organization** by entity type

### Using the Vault

The generated vault can be:
- Opened directly in [Obsidian](https://obsidian.md/) for graph visualization
- Used as reference documentation
- Exported to other formats
- Extended with manual research

## Tips

1. **Start specific** - Narrow markets yield better results than broad ones
2. **Set realistic targets** - 5-10 entities per category is usually sufficient
3. **Review after first loop** - Check LOOP_1_MARKET_ANALYSIS.md before continuing
4. **Adjust importance** - Manually change entity priorities if needed
5. **Check the vault** - Open INDEX.md periodically to see progress
6. **Use max loops** - Market research benefits from more iterations than code playbooks

## Customization

### Adjusting Entity Types
Edit `1_ANALYZE.md` to change which entity categories are researched.

### Changing Templates
Modify the entity templates in `4_RESEARCH.md` to capture different information.

### Coverage Targets
Adjust the target counts in the market analysis output to control when the playbook exits.
