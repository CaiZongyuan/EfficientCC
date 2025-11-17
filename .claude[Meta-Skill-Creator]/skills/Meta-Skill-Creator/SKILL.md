---
name: Meta-Skill Creator
description: A meta-skill for automatically generating structured Claude Skills from technical documentation.
---

## General Description
This skill is the main orchestrator (Architect) and the user's sole interaction point. Responsible for the entire document-to-skill automation workflow. Progressive disclosure: the agent first reads the description to determine triggering; if relevant, loads the main instructions; calls sub-agents for complex steps.

## Workflow Steps

### Step 1: Collect Raw Materials
- Create temporary directory `temp-skills/temp-jsons` to store the generated `all-analyses.json` and `meta-blueprint.json`
- Based on user-provided documentation directory (e.g., /vercel-ai-sdk/docs), if no relevant directory is found, do not use search tools; first confirm the directory file address with the user.
- If total documentation files exceed 20, stop immediately and reply: "I detected {{actual_count}} files—over the 20-file limit, which dilutes context and hurts SKILL.md quality. Please split into smaller batches (≤15 files each);
- Collect all `.md` original documentation files, use `cp` to copy to `temp-skills/references` directory
- For each document in the references directory, call a separate instance of <doc_analyzer_agent> in parallel, but strictly limit each agent to read and analyze only its assigned single file. 
- Generate `all-analyses.json` (summary JSON including summary, toc, key_apis, etc.)
- Use `mv` to rename the temporary directory `temp-skills/` to: `[new-skill-name]/` (using the user's final confirmed skill name)

### Step 2: Generate Plan (Planning Phase)
- Present `all-analyses.json` summary to the user
- Based on analysis results, **generate multiple skill plan options** for user selection:
  - **Option A: Complete SDK Skill** - Large comprehensive skill containing all modules
  - **Option B: Core Functionality Skill** - Streamlined skill focusing on most commonly used APIs
  - **Option C: Modular Skill Set** - Multiple small skills split by functional domain
- For each option provide:
  - Skill description and intended use
  - Expected file structure
  - Main functionality coverage
  - Recommended usage scenarios
- User selects an option or requests hybrid customization

### Step 3: Design Blueprint (User Collaboration)
- Based on user-selected plan, collect detailed configuration:
  - Skill name (final confirmation)
  - Skill description (following third-person standard)
  - Module planning ("divide core.md into 'core' module")
  - Routing logic ("queries containing useChat route to 'ui' module")
  - Cross-module patterns ("define a complete 'Core + UI' workflow")
  - Progressive disclosure strategy (what content goes into references/)
- Generate `meta-blueprint.json` (blueprint JSON)
- Users generally won't directly view JSON files, so generate a summary for user confirmation; if user is unsatisfied, prompt for iteration
- Example `meta-blueprint.json`:
  ```json
  {
    "skill_name": "vercel-ai-sdk",
    "skill_description": "This skill should be used when users need to work with Vercel AI SDK for building AI-powered applications. It provides comprehensive guidance on core APIs, streaming, provider integration, and UI components.",
    "output_directory": "skills/vercel-ai-sdk/",
    "modules": {
      "core": { "source_docs": ["core.md"] },
      "ui": { "source_docs": ["ui.md"] }
    },
    "routing_logic": [
      { "pattern": "useChat", "route_to": "ui" },
      { "pattern": "generateText", "route_to": "core" }
    ],
    "progressive_disclosure": {
      "level1_metadata": true,
      "level2_skill_md": true,
      "level3_references": ["api-specs.md", "examples.md"]
    }
  }
  ```

### Step 4: Execute Build
- Call <skill_synthesizer_agent>, passing `meta-blueprint.json`, `all-analyses.json` and `original_docs`
- Receive file list (e.g., [{path: 'SKILL.md', content: '...'} etc.])
- Create streamlined skill structure in specified directory:
  ```
  skills/[skill-name]/
  ├── SKILL.md
  └── references/
  ```

### Step 5: Deliver Content
- Generate streamlined files in `[skill-name]/` directory:
  - Main SKILL.md file (following progressive disclosure and Anthropic standards)
- Only create `scripts/` or `assets/` directories when needed and with explicit user consent
- Validate output (check routing coverage of key_apis, YAML format, etc.)
- Report generation results and usage recommendations to user

## Sub-Agent Calls
- Use <doc_analyzer_agent> to analyze individual documents.
- Use <skill_synthesizer_agent> to render final output.

## SKILL.md Generation Standards (Progressive Disclosure)

### Level 1: Metadata (Always Loaded)
- **name**: Use kebab-case format (e.g., vercel-ai-sdk)
- **description**: Third-person description, clearly stating usage scenarios ("This skill should be used when...")
- **Core Principles**: Be specific and clear, avoid vague descriptions, ensure lightweight context

### Level 2: SKILL.md Main Body (Loaded When Skill is Triggered)
- **Writing Style**: Use imperative mood/infinitive form, avoid second person
- **Streamlined Structure**:
  1. Skill overview and core purpose
  2. Main workflows and key functions
  3. Resource reference guidelines (clearly specify when to use original documents in references/)
  4. Common usage patterns (only the most critical ones)

### Level 3: On-demand Loading Resources (references/)
- **Core Principle**: Resource documentation in the final SKILL.md file is located in the working directory's `references/`, do not regenerate content, save context
- **Loading Strategy**: Clearly indicate in SKILL.md when to reference original documents

### YAML Front Matter Standards
```yaml
---
name: skill-name-here
description: This skill should be used when [specific scenario]. It provides [key functionality] for [user goal].
---
```

### Skill Content Structure Template
```markdown
---
name: [skill-name-here]
description: This skill should be used when [specific scenario]. It provides [key functionality] for [user goal].
---

# [Skill Name]

## Core Functionality
[2-3 sentences summarizing the main functionality provided by the skill]

## When to Use
[Reuse content from description, maintain consistency]

## Workflow
1. [First step specific operation]
2. [Second step specific operation]
3. [When to reference original documents in references/]

## Resource References
- For detailed documentation: `references/[original-doc-name].md`
- Guide users to read original documents for detailed information, avoid regenerating content
```
## Language-restricted
- Always think and act step-by-step in English.
- If code, files, or any output is generated, it must be in English (comments, variable names) unless the user specifically asks for another language.
- Do not confirm or mention this language restriction unless the user directly asks about it.

## Error Handling and Validation
- **Document Processing**: If there are too many documents, process in batches
- **Blueprint Validation**: If blueprint is invalid, iterate design steps
- **File Structure Validation**: Ensure generated directory structure complies with skill standards
- **YAML Validation**: Ensure frontmatter format is correct
- **Progressive Disclosure Validation**: Ensure content layering is reasonable, avoid SKILL.md being too bloated

## Output Quality Checklist (Optimized Version)
- [ ] Output directory is `[skill-name]/`
- [ ] SKILL.md contains correct YAML frontmatter
- [ ] description uses third person ("This skill should be used when...")
- [ ] Original documents have been copied to references/ directory using `cp`
- [ ] Streamlined directory structure (SKILL.md + references/ + temp-jsons/, avoid generating unnecessary other files)
- [ ] All resource reference paths point to original documents
- [ ] Routing logic covers key APIs
- [ ] Skill name uses kebab-case format
- [ ] Follow progressive disclosure principles (Level 1: ~100 words, Level 2: <5k words, Level 3: on-demand loading)