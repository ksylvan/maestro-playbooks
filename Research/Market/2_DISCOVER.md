# Entity Discovery - Find Research Targets

## Context
- **Playbook:** Market Research
- **Agent:** {{AGENT_NAME}}
- **Auto Run Folder:** {{AUTORUN_FOLDER}}
- **Loop:** {{LOOP_NUMBER}}

## Objective

Discover specific entities to research within the target market. Use web search to find companies, products, people, and other relevant entities that should be included in the knowledge vault.

## Instructions

1. **Read the market analysis** from `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_MARKET_ANALYSIS.md`
2. **Read existing entities** from `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_ENTITIES.md` (if it exists)
3. **Focus on ONE entity category** that needs more discovery
4. **Use web search** to find specific entities in that category
5. **Document discoveries** in `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_ENTITIES.md`

## Discovery Checklist

- [ ] **Discover entities**: Read the market analysis to understand priority categories. Pick ONE category that needs more entities discovered. Use web search to find 3-5 specific entities in that category. Append findings to `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_ENTITIES.md` with basic info and discovery source.

## Search Strategies by Entity Type

### Companies
- Search: "[market] companies", "[market] startups", "[market] market leaders"
- Check: Crunchbase, LinkedIn, industry reports, news articles
- Look for: Funding announcements, product launches, partnerships

### Products/Services
- Search: "[market] products", "[market] solutions", "[market] platforms"
- Check: G2, Capterra, Product Hunt, company websites
- Look for: Product comparisons, reviews, feature lists

### People
- Search: "[market] CEO", "[market] founder", "[market] analyst"
- Check: LinkedIn, Twitter/X, conference speakers, podcast guests
- Look for: Thought leaders, frequent commentators, industry veterans

### Technologies
- Search: "[market] technology", "[market] standards", "[market] protocols"
- Check: Technical blogs, GitHub, academic papers, patents
- Look for: Emerging tech, dominant platforms, open standards

### Trends
- Search: "[market] trends 2024", "[market] predictions", "[market] future"
- Check: Industry reports, analyst commentary, news roundups
- Look for: Growth areas, disruptions, shifting dynamics

### Investors
- Search: "[market] investors", "[market] VC", "[market] funding"
- Check: Crunchbase, PitchBook, funding announcements
- Look for: Active investors, fund sizes, portfolio companies

## Output Format

Append to `{{AUTORUN_FOLDER}}/LOOP_{{LOOP_NUMBER}}_ENTITIES.md`:

```markdown
---

## [Category Name] - Discovered [YYYY-MM-DD]

### [Entity Name 1]
- **Type:** [Company | Product | Person | Technology | Trend | Investor]
- **Brief:** [One sentence description]
- **Why Notable:** [Why this entity matters in the market]
- **Discovery Source:** [URL where you found this]
- **Status:** PENDING

### [Entity Name 2]
- **Type:** [Type]
- **Brief:** [Description]
- **Why Notable:** [Relevance]
- **Discovery Source:** [URL]
- **Status:** PENDING

### [Entity Name 3]
...

### Discovery Summary
- **Category:** [Category researched]
- **Entities Found:** [count]
- **Search Queries Used:**
  - "[query 1]"
  - "[query 2]"
- **Sources Checked:**
  - [Source 1]
  - [Source 2]
```

## Entity Status Values

| Status | Meaning |
|--------|---------|
| `PENDING` | Discovered, not yet researched |
| `RESEARCHED` | Full profile created in vault |
| `SKIP` | Not relevant enough to research |
| `DUPLICATE` | Already covered under another entity |

## Guidelines

- **One category per run** - Focus discovery efforts for better results
- **Quality over quantity** - 3-5 well-chosen entities beat 20 marginal ones
- **Note why each matters** - Helps prioritization later
- **Include discovery source** - Enables verification and deeper research
- **Check for duplicates** - Don't re-discover entities already in the list
- **Diversify within category** - Mix of leaders, challengers, and emerging players
