---
name: skill-creator-pro
description: Use when creating new skills, editing existing skills, or verifying skills work before deployment. Also use when the user says "create a skill," "write a skill," "test this skill," or "improve this skill." For general coding TDD, see test-driven-development.
---

# Skill Creator Pro: The Ultimate Skill Creation Framework

## Overview

**Creating skills IS Test-Driven Development applied to process documentation.**

Skills are modular, self-contained packages that extend Claude's capabilities by providing specialized knowledge, workflows, and tools. Think of them as "onboarding guides" for specific domains or tasks—they transform Claude from a general-purpose agent into a specialized agent equipped with procedural knowledge that no model can fully possess.

**Core principle:** If you didn't watch an agent fail without the skill, you don't know if the skill teaches the right thing. The same RED-GREEN-REFACTOR cycle that makes code bulletproof makes documentation bulletproof.

**What Skills Provide:**
1. Specialized workflows - Multi-step procedures for specific domains
2. Tool integrations - Instructions for working with specific file formats or APIs
3. Domain expertise - Company-specific knowledge, schemas, business logic
4. Bundled resources - Scripts, references, and assets for complex and repetitive tasks

**When to Create a Skill:**
- Technique wasn't intuitively obvious to you
- You'd reference this again across projects
- Pattern applies broadly (not project-specific)
- Others would benefit

**Don't create for:**
- One-off solutions
- Standard practices well-documented elsewhere
- Project-specific conventions (put in AGENTS.md)
- Mechanical constraints (if enforceable with regex/validation, automate it)

---

## The Testing-First Mindset

### Why Testing Matters for Skills

Skills that enforce discipline (like TDD, code review, documentation standards) need to resist rationalization. Agents are smart and will find loopholes when under pressure.

**The Iron Law (No Exceptions):**

```
NO SKILL WITHOUT A FAILING TEST FIRST
```

This applies to NEW skills AND EDITS to existing skills.

- Write skill before testing? **Delete it. Start over.**
- Edit skill without testing? **Same violation.**
- No exceptions for "simple additions," "just adding a section," or "documentation updates"

**Why this is non-negotiable:**
- Untested skills have loopholes. Always.
- Testing reveals actual failure modes, not hypothetical ones
- 15 minutes testing saves hours debugging bad skills in production
- Clear to you ≠ clear to other agents. Test it.

### TDD Mapping for Skills

| TDD Concept | Skill Creation |
|-------------|----------------|
| **Test case** | Pressure scenario with subagent |
| **Production code** | Skill document (SKILL.md) |
| **Test fails (RED)** | Agent violates rule without skill (baseline) |
| **Test passes (GREEN)** | Agent complies with skill present |
| **Refactor** | Close loopholes while maintaining compliance |
| **Write test first** | Run baseline scenario BEFORE writing skill |
| **Watch it fail** | Document exact rationalizations agent uses |
| **Minimal code** | Write skill addressing those specific violations |
| **Watch it pass** | Verify agent now complies |
| **Refactor cycle** | Find new rationalizations → plug → re-verify |

The entire skill creation process follows RED-GREEN-REFACTOR.

### Testing Different Skill Types

**Discipline-Enforcing Skills** (rules/requirements):
- Examples: TDD, verification-before-completion, designing-before-coding
- Test with: Academic questions + pressure scenarios + combined pressures
- Pressures: Time constraints, sunk cost, authority pressure, exhaustion
- Success criteria: Agent follows rule under maximum pressure

**Technique Skills** (how-to guides):
- Examples: condition-based-waiting, root-cause-tracing, defensive-programming
- Test with: Application scenarios + variations + missing information tests
- Success criteria: Agent successfully applies technique to new scenario

**Pattern Skills** (mental models):
- Examples: reducing-complexity, information-hiding concepts
- Test with: Recognition scenarios + application + counter-examples
- Success criteria: Agent correctly identifies when/how to apply pattern

**Reference Skills** (documentation/APIs):
- Examples: API documentation, command references, library guides
- Test with: Retrieval scenarios + application + gap testing
- Success criteria: Agent finds and correctly applies reference information

---

## Core Principles

### 1. Concise is Key

The context window is a public good. Skills share it with everything else Claude needs: system prompt, conversation history, other skills' metadata, and the actual user request.

**Default assumption: Claude is already very smart.** Only add context Claude doesn't already have. Challenge each piece of information: "Does Claude really need this explanation?" and "Does this paragraph justify its token cost?"

**Token Efficiency Targets:**
- Frequently-loaded skills: <200 words total
- Standard skills: <500 words (still be concise)
- Heavy reference material: Split to separate files

**Techniques:**

**Move details to tool help:**
```markdown
# ❌ BAD: Document all flags in SKILL.md
search-conversations supports --text, --both, --after DATE, --before DATE, --limit N

# ✅ GOOD: Reference --help
search-conversations supports multiple modes and filters. Run --help for details.
```

**Use cross-references:**
```markdown
# ❌ BAD: Repeat workflow details
When searching, dispatch subagent with template...
[20 lines of repeated instructions]

# ✅ GOOD: Reference other skill
Always use subagents (50-100x context savings). REQUIRED: Use [other-skill-name] for workflow.
```

**Compress examples:**
```markdown
# ❌ BAD: Verbose example (42 words)
your human partner: "How did we handle authentication errors in React Router before?"
You: I'll search past conversations for React Router authentication patterns.
[Dispatch subagent with search query: "React Router authentication error handling 401"]

# ✅ GOOD: Minimal example (20 words)
Partner: "How did we handle auth errors in React Router?"
You: Searching...
[Dispatch subagent → synthesis]
```

Prefer concise examples over verbose explanations.

### 2. Set Appropriate Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

**High freedom (text-based instructions)**: Use when multiple approaches are valid, decisions depend on context, or heuristics guide the approach.

**Medium freedom (pseudocode or scripts with parameters)**: Use when a preferred pattern exists, some variation is acceptable, or configuration affects behavior.

**Low freedom (specific scripts, few parameters)**: Use when operations are fragile and error-prone, consistency is critical, or a specific sequence must be followed.

Think of Claude as exploring a path: a narrow bridge with cliffs needs specific guardrails (low freedom), while an open field allows many routes (high freedom).

### 3. Progressive Disclosure Design

Skills use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<500 lines ideal)
3. **Bundled resources** - As needed by Claude (unlimited because scripts can be executed without reading)

Keep SKILL.md body to essentials and under 500 lines. Split content into separate files when approaching this limit. When splitting, reference files from SKILL.md and describe clearly when to read them.

---

## Anatomy of a Skill

Every skill consists of a required SKILL.md file and optional bundled resources:

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter metadata (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── Markdown instructions (required)
└── Bundled Resources (optional)
    ├── scripts/          - Executable code (Python/Bash/etc.)
    ├── references/       - Documentation loaded into context as needed
    └── assets/           - Files used in output (templates, icons, fonts, etc.)
```

### SKILL.md (required)

Every SKILL.md consists of:

- **Frontmatter** (YAML): Contains `name` and `description` fields. These are the ONLY fields that Claude reads to determine when the skill gets used. Be clear and comprehensive in describing what the skill is and when it should be used.
- **Body** (Markdown): Instructions and guidance for using the skill. Only loaded AFTER the skill triggers.

### Bundled Resources (optional)

#### Scripts (`scripts/`)

Executable code (Python/Bash/etc.) for tasks that require deterministic reliability or are repeatedly rewritten.

- **When to include**: When the same code is being rewritten repeatedly or deterministic reliability is needed
- **Example**: `scripts/rotate_pdf.py` for PDF rotation tasks
- **Benefits**: Token efficient, deterministic, may be executed without loading into context
- **Note**: Scripts may still need to be read by Claude for patching or environment-specific adjustments

#### References (`references/`)

Documentation and reference material intended to be loaded as needed into context to inform Claude's process and thinking.

- **When to include**: For documentation that Claude should reference while working
- **Examples**: `references/finance.md` for financial schemas, `references/api_docs.md` for API specifications
- **Use cases**: Database schemas, API documentation, domain knowledge, company policies, detailed workflow guides
- **Benefits**: Keeps SKILL.md lean, loaded only when Claude determines it's needed
- **Best practice**: If files are large (>10k words), include grep search patterns in SKILL.md
- **Avoid duplication**: Information should live in either SKILL.md or references files, not both. Prefer references files for detailed information unless it's truly core to the skill.

#### Assets (`assets/`)

Files not intended to be loaded into context, but rather used within the output Claude produces.

- **When to include**: When the skill needs files that will be used in the final output
- **Examples**: `assets/logo.png` for brand assets, `assets/slides.pptx` for PowerPoint templates
- **Use cases**: Templates, images, icons, boilerplate code, fonts, sample documents that get copied or modified
- **Benefits**: Separates output resources from documentation, enables Claude to use files without loading them

### What to NOT Include in a Skill

A skill should only contain essential files that directly support its functionality. Do NOT create extraneous documentation or auxiliary files, including:

- README.md
- INSTALLATION_GUIDE.md
- QUICK_REFERENCE.md
- CHANGELOG.md
- etc.

The skill should only contain the information needed for an AI agent to do the job at hand. Creating additional documentation files adds clutter and confusion.

---

## Skill Creation Process: RED-GREEN-REFACTOR

Follow these steps in order. Skill creation is TDD applied to documentation.

### Step 0: Testing-First Mindset

**Before writing any skill content, commit to testing.**

Write down:
1. What type of skill is this? (Discipline/Technique/Pattern/Reference)
2. What pressure scenarios will I use for testing?
3. What failure modes am I protecting against?

If you can't answer these questions, you're not ready to write the skill.

### Step 1: Understanding with Examples

Skip this step only when the skill's usage patterns are already clearly understood.

To create an effective skill, clearly understand concrete examples of how the skill will be used. This understanding can come from either direct user examples or generated examples validated with user feedback.

**Example questions for an image-editor skill:**
- "What functionality should the image-editor skill support? Editing, rotating, anything else?"
- "Can you give some examples of how this skill would be used?"
- "I can imagine users asking for things like 'Remove the red-eye from this image' or 'Rotate this image'. Are there other ways you imagine this skill being used?"
- "What would a user say that should trigger this skill?"

**Avoid overwhelming users** - Start with the most important questions and follow up as needed.

**Conclude this step** when there is a clear sense of the functionality the skill should support.

### Step 2: Planning Contents

To turn concrete examples into an effective skill, analyze each example by:

1. Considering how to execute on the example from scratch
2. Identifying what scripts, references, and assets would be helpful when executing these workflows repeatedly

**Example: `pdf-editor` skill**
- Query: "Help me rotate this PDF"
- Analysis: Rotating a PDF requires re-writing the same code each time
- Resource: A `scripts/rotate_pdf.py` script would be helpful

**Example: `frontend-webapp-builder` skill**
- Queries: "Build me a todo app" or "Build me a dashboard to track my steps"
- Analysis: Writing a frontend webapp requires the same boilerplate HTML/React each time
- Resource: An `assets/hello-world/` template containing boilerplate files would be helpful

**Example: `big-query` skill**
- Query: "How many users have logged in today?"
- Analysis: Querying BigQuery requires re-discovering table schemas each time
- Resource: A `references/schema.md` file documenting table schemas would be helpful

**Output:** Create a list of the reusable resources to include: scripts, references, and assets.

### Step 3: RED Phase - Establish Baseline

**This is where testing begins. Do not skip.**

Run pressure scenarios with subagent WITHOUT the skill. Document exact behavior:

**For Discipline-Enforcing Skills:**
1. Create 3+ pressure scenarios combining:
   - Time pressure ("You have 10 minutes")
   - Sunk cost ("You already wrote 200 lines")
   - Authority pressure ("Senior engineer says this is fine")
   - Exhaustion ("You've been debugging for 3 hours")

2. Run scenarios WITHOUT skill loaded
3. Document VERBATIM what the agent says:
   - What choices did they make?
   - What rationalizations did they use?
   - Which pressures triggered violations?
   - What loopholes did they find?

**For Technique Skills:**
1. Create application scenarios (can they apply the technique?)
2. Create variation scenarios (do they handle edge cases?)
3. Run WITHOUT skill, document gaps

**For Reference Skills:**
1. Create retrieval scenarios (can they find the right info?)
2. Run WITHOUT skill, document what's missing

**Critical:** Write down the EXACT rationalizations agents use. These become your rationalization table later.

**Example baseline documentation:**

```markdown
## RED Phase Results - condition-based-waiting skill

### Scenario 1: Flaky test with setTimeout
**Without skill:** Agent used sleep(5000) and said "this should be enough time"
**Rationalization:** "A 5 second wait covers most cases"

### Scenario 2: Race condition under time pressure
**Without skill:** Agent added arbitrary delay and moved on
**Rationalization:** "We can tune the timing later"
**Pressure that triggered violation:** Time constraint ("only 15 minutes left")

### Scenario 3: Test passes 9/10 times
**Without skill:** Agent said "good enough, move on"
**Rationalization:** "90% reliability is acceptable for development"
```

These exact rationalizations inform what you write in the skill.

### Step 4: Initialize the Skill

Skip this step only if the skill being developed already exists, and iteration or packaging is needed.

When creating a new skill from scratch, always run the `init_skill.py` script. The script generates a new template skill directory that automatically includes everything a skill requires.

**Usage:**

```bash
scripts/init_skill.py <skill-name> --path <output-directory>
```

The script:
- Creates the skill directory at the specified path
- Generates a SKILL.md template with proper frontmatter and TODO placeholders
- Creates example resource directories: `scripts/`, `references/`, and `assets/`
- Adds example files in each directory that can be customized or deleted

After initialization, customize or remove the generated SKILL.md and example files as needed.

### Step 5: GREEN Phase - Write Minimal Skill

**Now write the skill, informed by your baseline testing.**

When editing the skill, remember that the skill is being created for another instance of Claude to use. Include information that would be beneficial and non-obvious to Claude.

#### Start with Reusable Skill Contents

Begin with the reusable resources identified in Step 2: `scripts/`, `references/`, and `assets/` files. 

**Note:** This step may require user input. For example, when implementing a `brand-guidelines` skill, the user may need to provide brand assets or templates.

**Test scripts** by actually running them to ensure there are no bugs and output matches expectations. If there are many similar scripts, only a representative sample needs testing.

**Delete** any example files and directories not needed for the skill. The initialization script creates example files to demonstrate structure, but most skills won't need all of them.

#### Update SKILL.md

**Writing Guidelines:** Always use imperative/infinitive form.

##### Frontmatter (YAML)

Write the YAML frontmatter with `name` and `description`:

**Name:**
- Use only letters, numbers, and hyphens (no parentheses, special chars)
- Active voice, verb-first: `creating-skills` not `skill-creation`
- Gerunds (-ing) work well for processes: `testing-skills`, `debugging-with-logs`

**Description (CRITICAL FOR DISCOVERY):**

The description is the primary triggering mechanism for your skill. It helps Claude understand when to use the skill.

**Format:** Start with "Use when..." to focus on triggering conditions

**CRITICAL: Description = When to Use, NOT What the Skill Does**

The description should ONLY describe triggering conditions. Do NOT summarize the skill's process or workflow in the description.

**Why this matters:** Testing revealed that when a description summarizes the skill's workflow, Claude may follow the description instead of reading the full skill content. A description saying "code review between tasks" caused Claude to do ONE review, even though the skill's flowchart showed TWO reviews.

When the description was changed to just "Use when executing implementation plans with independent tasks" (no workflow summary), Claude correctly read the flowchart and followed the two-stage review process.

**The trap:** Descriptions that summarize workflow create a shortcut Claude will take. The skill body becomes documentation Claude skips.

```yaml
# ❌ BAD: Summarizes workflow - Claude may follow this instead of reading skill
description: Use when executing plans - dispatches subagent per task with code review between tasks

# ❌ BAD: Too much process detail
description: Use for TDD - write test first, watch it fail, write minimal code, refactor

# ❌ BAD: Too abstract, vague
description: For async testing

# ✅ GOOD: Just triggering conditions, no workflow summary
description: Use when executing implementation plans with independent tasks in the current session

# ✅ GOOD: Specific triggers and symptoms
description: Use when tests have race conditions, timing dependencies, or pass/fail inconsistently

# ✅ GOOD: Multiple trigger phrases
description: Use when creating new skills, editing existing skills, or verifying skills work before deployment. Also use when the user says "create a skill," "write a skill," or "test this skill."
```

**Content guidelines:**
- Include both what the skill does and specific triggers/contexts for when to use it
- Use concrete triggers, symptoms, and situations that signal this skill applies
- Describe the *problem* (race conditions, inconsistent behavior) not *language-specific symptoms* (setTimeout, sleep)
- Keep triggers technology-agnostic unless the skill itself is technology-specific
- Write in third person (injected into system prompt)
- Include specific phrases users might say
- Keep under 500 characters if possible
- Max 1024 characters total for entire frontmatter
- **NEVER summarize the skill's process or workflow**

**Include all "when to use" information here** - Not in the body. The body is only loaded after triggering, so "When to Use This Skill" sections in the body are not helpful.

**Do not include any other fields in YAML frontmatter.** Only `name` and `description` are supported.

##### Body

Write instructions for using the skill and its bundled resources.

**Standard Structure (adapt as needed):**

```markdown
# Skill Name

## Overview
[What is this? Core principle in 1-2 sentences.]

## Initial Assessment (if applicable)

Before [taking action], identify:

1. **[Context Category]**
   - [Question to gather context]

## Core Framework/Pattern

[Main methodology with clear steps or decision points]

### Step 1: [Action]
[Specific instructions]

**Example:**
[Concrete, copy-paste-ready example]

## Output Format / Deliverable Template (if applicable)

[Copy-paste template for expected output]

## Common Mistakes to Avoid

### Mistake 1: [Common Error]
**Wrong:** [Specific anti-pattern]
**Right:** [Correct approach]

## Quality Checklist (if applicable)

- [ ] [Quality indicator 1]
- [ ] [Quality indicator 2]

## Related Skills

- **[skill-name]**: [When to use that instead/in addition]
```

**For Discipline-Enforcing Skills, Add:**

##### Close Every Loophole Explicitly

Address the specific rationalizations found in your RED phase:

```markdown
**No exceptions:**
- Not for "simple additions"
- Not for "just adding a section"
- Don't keep untested changes as "reference"
- Don't "adapt" while running tests
- Delete means delete
```

##### Address "Spirit vs Letter" Arguments

Add foundational principle early:

```markdown
**Violating the letter of the rules is violating the spirit of the rules.**
```

##### Build Rationalization Table

Every excuse agents make from your RED phase testing goes in the table:

```markdown
## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
```

##### Create Red Flags List

Make it easy for agents to self-check when rationalizing:

```markdown
## Red Flags - STOP and Start Over

- Code before test
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**
```

#### Learn Proven Design Patterns

Consult these helpful guides based on your skill's needs:

- **Multi-step processes**: See references/workflows.md for sequential workflows and conditional logic
- **Specific output formats or quality standards**: See references/output-patterns.md for template and example patterns

These files contain established best practices for effective skill design.

#### Test: Run GREEN Phase

Run the same pressure scenarios from RED phase, but now WITH the skill loaded.

**Success criteria:**
- Agent follows the rule/pattern under pressure
- Agent applies the technique correctly
- Agent resists the rationalizations you documented

**If agent still violates:** Document the NEW rationalizations. You need them for REFACTOR phase.

**If agent complies:** Move to Step 6 (Packaging), then return to Step 7 (REFACTOR).

### Step 6: Package the Skill

Once development of the skill is complete, it must be packaged into a distributable .skill file. The packaging process automatically validates the skill first to ensure it meets all requirements:

```bash
scripts/package_skill.py <path/to/skill-folder>
```

Optional output directory specification:

```bash
scripts/package_skill.py <path/to/skill-folder> ./dist
```

The packaging script will:

1. **Validate** the skill automatically, checking:
   - YAML frontmatter format and required fields
   - Skill naming conventions and directory structure
   - Description completeness and quality
   - File organization and resource references

2. **Package** the skill if validation passes, creating a .skill file named after the skill (e.g., `my-skill.skill`) that includes all files and maintains the proper directory structure for distribution. The .skill file is a zip file with a .skill extension.

If validation fails, the script will report the errors and exit without creating a package. Fix any validation errors and run the packaging command again.

### Step 7: REFACTOR Phase - Close Loopholes

**This is the iterative refinement phase.**

Agent found a new rationalization in GREEN phase? Add explicit counter. Re-test until bulletproof.

**Process:**

1. **Identify new rationalizations** from GREEN phase testing
   - What did the agent say this time?
   - What new loophole did they find?
   - What pressure triggered this violation?

2. **Add explicit counters** to skill
   - Add to rationalization table
   - Add to red flags list
   - Add explicit "no exceptions" clauses
   - Update description if new trigger identified

3. **Re-test** with same scenarios
   - Does agent comply now?
   - Did fixing this create new loopholes?

4. **Repeat** until agent is bulletproof under all pressures

**When to stop refactoring:**
- Agent complies with all pressure scenarios
- No new rationalizations emerge from testing
- Rationalization table covers all observed failure modes
- Skill passes quality checklist (see below)

**Example REFACTOR cycle:**

```markdown
## Iteration 1 (GREEN)
Agent violated under time pressure: "I'll add tests after"
Added to skill: "Not for 'I'll test after'"
Re-tested: Agent complied

## Iteration 2 (REFACTOR)
Agent found new loophole: "I already manually tested it"
Added to rationalization table with counter
Re-tested: Agent complied

## Iteration 3 (REFACTOR)
Agent tried: "This is different because it's a trivial change"
Added to red flags: "This is different because..."
Re-tested: Agent complied

## Final State
All pressure scenarios pass
Rationalization table complete
Ready for deployment
```

### Step 8: Iterate Based on Usage

After deploying the skill, users may request improvements. Often this happens right after using the skill, with fresh context of how the skill performed.

**Iteration workflow:**

1. Use the skill on real tasks
2. Notice struggles or inefficiencies
3. Identify how SKILL.md or bundled resources should be updated
4. **Return to Step 3 (RED Phase)** - Test the updated skill before deploying changes
5. Implement changes following GREEN and REFACTOR phases
6. Test again and deploy

**Remember:** Editing an existing skill without testing violates the Iron Law. Treat updates like new skills - establish baseline, implement changes, close loopholes.

---

## Progressive Disclosure Patterns

Keep SKILL.md body to essentials and under 500 lines to minimize context bloat. Split content into separate files when approaching this limit. When splitting out content into other files, reference them from SKILL.md and describe clearly when to read them.

**Key principle:** When a skill supports multiple variations, frameworks, or options, keep only the core workflow and selection guidance in SKILL.md. Move variant-specific details (patterns, examples, configuration) into separate reference files.

### Pattern 1: High-level guide with references

```markdown
# PDF Processing

## Quick start

Extract text with pdfplumber:
[code example]

## Advanced features

- **Form filling**: See [FORMS.md](FORMS.md) for complete guide
- **API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
- **Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns
```

Claude loads FORMS.md, REFERENCE.md, or EXAMPLES.md only when needed.

### Pattern 2: Domain-specific organization

For Skills with multiple domains, organize content by domain to avoid loading irrelevant context:

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

When a user asks about sales metrics, Claude only reads sales.md.

Similarly, for skills supporting multiple frameworks or variants, organize by variant:

```
cloud-deploy/
├── SKILL.md (workflow + provider selection)
└── references/
    ├── aws.md (AWS deployment patterns)
    ├── gcp.md (GCP deployment patterns)
    └── azure.md (Azure deployment patterns)
```

When the user chooses AWS, Claude only reads aws.md.

### Pattern 3: Conditional details

Show basic content, link to advanced content:

```markdown
# DOCX Processing

## Creating documents

Use docx-js for new documents. See [DOCX-JS.md](DOCX-JS.md).

## Editing documents

For simple edits, modify the XML directly.

**For tracked changes**: See [REDLINING.md](REDLINING.md)
**For OOXML details**: See [OOXML.md](OOXML.md)
```

Claude reads REDLINING.md or OOXML.md only when the user needs those features.

### Important Guidelines

- **Avoid deeply nested references** - Keep references one level deep from SKILL.md. All reference files should link directly from SKILL.md.
- **Structure longer reference files** - For files longer than 100 lines, include a table of contents at the top so Claude can see the full scope when previewing.
- **Cross-reference without force-loading** - Use skill name only: `See test-driven-development` not `@skills/tdd/SKILL.md` (@ syntax force-loads immediately)

---

## Bulletproofing Against Rationalization

Skills that enforce discipline need to resist rationalization. This section provides systematic techniques.

### Psychology of Persuasion

Understanding WHY persuasion techniques work helps you apply them systematically. Key principles:

- **Authority**: "This is the standard" carries weight
- **Commitment**: "You already committed to this rule" creates consistency pressure
- **Scarcity**: "No exceptions" makes the rule absolute
- **Social proof**: "Other agents follow this" creates normative pressure
- **Unity**: "We all follow this discipline" creates shared identity

### Systematic Loophole Closing

**1. Close Every Loophole Explicitly**

Don't just state the rule - forbid specific workarounds:

```markdown
# ❌ BAD
Write code before test? Delete it.

# ✅ GOOD
Write code before test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete
```

**2. Address "Spirit vs Letter" Arguments**

Add foundational principle early:

```markdown
**Violating the letter of the rules is violating the spirit of the rules.**
```

This cuts off entire class of "I'm following the spirit" rationalizations.

**3. Build Rationalization Table**

Capture rationalizations from baseline testing (Step 3: RED Phase). Every excuse agents make goes in the table:

```markdown
| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
```

**4. Create Red Flags List**

Make it easy for agents to self-check when rationalizing:

```markdown
## Red Flags - STOP and Start Over

- Code before test
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**
```

**5. Update Description for Violation Symptoms**

Add to description: symptoms of when you're ABOUT to violate the rule:

```yaml
description: Use when implementing any feature or bugfix, before writing implementation code
```

---

## Claude Search Optimization (CSO)

**Critical for discovery:** Future Claude needs to FIND your skill.

### 1. Rich Description Field

**Purpose:** Claude reads description to decide which skills to load for a given task. Make it answer: "Should I read this skill right now?"

**Format:** Start with "Use when..." to focus on triggering conditions

See "Update SKILL.md > Frontmatter" section above for detailed guidelines. Key points:

- Description = When to Use, NOT What the Skill Does
- Include specific symptoms, situations, contexts
- Write in third person
- **NEVER summarize the skill's process or workflow**

### 2. Keyword Coverage

Use words Claude would search for throughout your skill:

- **Error messages**: "Hook timed out", "ENOTEMPTY", "race condition"
- **Symptoms**: "flaky", "hanging", "zombie", "pollution"
- **Synonyms**: "timeout/hang/freeze", "cleanup/teardown/afterEach"
- **Tools**: Actual commands, library names, file types
- **User phrases**: What would users actually say?

### 3. Descriptive Naming

**Use active voice, verb-first:**
- ✅ `creating-skills` not `skill-creation`
- ✅ `condition-based-waiting` not `async-test-helpers`
- ✅ `using-skills` not `skill-usage`
- ✅ `flatten-with-flags` over `data-structure-refactoring`

**Gerunds (-ing) work well for processes:**
- `creating-skills`, `testing-skills`, `debugging-with-logs`
- Active, describes the action you're taking

**Name by what you DO or core insight:**
- ✅ `root-cause-tracing` > `debugging-techniques`

### 4. Cross-Referencing Other Skills

Use skill name only, with explicit requirement markers:
- ✅ Good: `**REQUIRED BACKGROUND:** You MUST understand test-driven-development`
- ✅ Good: `See outcome-segmentation for detailed statistical methods`
- ❌ Bad: `See skills/testing/test-driven-development` (unclear if required)
- ❌ Bad: `@skills/testing/test-driven-development/SKILL.md` (force-loads, burns context)

**Why no @ links:** `@` syntax force-loads files immediately, consuming 200k+ context before you need them.

---

## Quality Patterns from Research

These patterns emerge from analyzing high-quality skills in the ecosystem:

### Pattern 1: Outcome-First Design

Best skills focus on OUTCOMES not METHODS in their descriptions:

```yaml
# ❌ BAD: Describes the method
description: Creates personas using demographic data and user interviews

# ✅ GOOD: Describes the outcome and when to use
description: When the user wants to create outcome-based personas built around desired outcomes and job characteristics—not demographics
```

**Why it works:** Claude searches by *problem to solve*, not *tool to use*

### Pattern 2: Initial Assessment Before Action

Every audit/analysis skill starts with context gathering:

```markdown
## Initial Assessment

Before [providing recommendations], identify:

1. **[Context Category 1]**
   - Question 1
   - Question 2

2. **[Context Category 2]**
   - Question 1
   - Question 2
```

**Why it works:** Prevents generic advice, forces customization

### Pattern 3: Framework-First, Then Details

High-quality skills follow this structure:

1. **Overview** - What is this, core principle (1-2 sentences)
2. **When to Use** - Triggering conditions and scope
3. **Core Framework** - The main structure/methodology
4. **Implementation Details** - How to execute
5. **Common Mistakes** - What goes wrong + fixes
6. **Related Skills** - Cross-references

### Pattern 4: Actionable Output Templates

Best skills provide copy-paste templates:

```markdown
## Deliverable Template

[Copy-paste structure for expected output]
```

**Why it works:** Removes "what format?" friction, ensures consistency

### Pattern 5: Conditional Details with Inline Decisions

Show basic path, link to advanced:

```markdown
## Basic Operation

[Simple instructions]

**For advanced features**: See [ADVANCED.md](ADVANCED.md)
```

Claude reads advanced docs only when needed.

### Pattern 6: Real-World Grounding

Include:
- **Concrete examples** (not abstract templates)
- **Common mistakes sections** (what actually goes wrong)
- **Red flags lists** (self-check before violating rules)
- **Rationalization tables** (documented from real testing)

### Pattern 7: Hierarchical Information Architecture

- Most important info first (scanning-friendly)
- Tables for quick reference
- Expandable details in separate files
- Clear headings and navigation

### Pattern 8: One Excellent Example

One excellent example beats many mediocre ones:

```markdown
# ✅ GOOD: One well-commented, runnable example in most relevant language

# ❌ BAD: Multiple implementations in different languages
```

### Pattern 9: Flowcharts Only for Non-Obvious Decisions

Use flowcharts ONLY for:
- Non-obvious decision points
- Process loops where you might stop too early
- "When to use A vs B" decisions

**Never use flowcharts for:**
- Reference material → Tables, lists
- Code examples → Markdown blocks
- Linear instructions → Numbered lists

### Pattern 10: Self-Contained Skill or Modular with Purpose

**Self-Contained (most skills):**
```
skill-name/
  SKILL.md    # Everything inline
```
When: All content fits, no heavy reference needed

**Modular with Purpose:**
```
skill-name/
  SKILL.md       # Core workflow
  scripts/       # Reusable tools
  references/    # Heavy documentation
  assets/        # Output templates
```
When: Reusable code, heavy reference, or output assets needed

Don't create files "just in case" - every file must serve a purpose.

---

## Common Anti-Patterns

### ❌ Narrative Example
"In session 2025-10-03, we found empty projectDir caused..."

**Why bad:** Too specific, not reusable

**Fix:** Extract the general principle or pattern

### ❌ Multi-Language Dilution
example-js.js, example-py.py, example-go.go

**Why bad:** Mediocre quality, maintenance burden

**Fix:** One excellent example in most relevant language. Claude can port.

### ❌ Code in Flowcharts
```dot
step1 [label="import fs"];
step2 [label="read file"];
```

**Why bad:** Can't copy-paste, hard to read

**Fix:** Use markdown code blocks for code

### ❌ Generic Labels
helper1, helper2, step3, pattern4

**Why bad:** Labels should have semantic meaning

**Fix:** Use descriptive names: validateInput, parseResponse, handleError

### ❌ Verbose Explanations
[200 words explaining what Claude already knows]

**Why bad:** Wastes tokens, slows reading

**Fix:** Challenge each paragraph: "Does Claude really need this?"

### ❌ Workflow Summary in Description
```yaml
description: Use for TDD - write test first, watch fail, write code, refactor
```

**Why bad:** Claude follows description instead of reading full skill

**Fix:** Describe triggering conditions only, not workflow

### ❌ Untested Skills
Writing skill without running RED-GREEN-REFACTOR cycle

**Why bad:** Always has loopholes, missing failure modes

**Fix:** Follow the Iron Law. No skill without failing test first.

### ❌ Extraneous Documentation
README.md, INSTALLATION.md, CHANGELOG.md

**Why bad:** Clutter, confusion, not for AI agents

**Fix:** Only SKILL.md and purposeful bundled resources

### ❌ Deeply Nested References
SKILL.md → references/main.md → references/sub/detail.md

**Why bad:** Hard to discover, confusing navigation

**Fix:** Keep references one level deep from SKILL.md

### ❌ Force-Loading with @ Links
`@skills/some-skill/SKILL.md`

**Why bad:** Loads 200k+ tokens immediately, even if not needed

**Fix:** Reference by name only: `See some-skill for details`

---

## Skill Creation Checklist

**IMPORTANT: Create todos for EACH checklist item.**

This checklist follows the RED-GREEN-REFACTOR cycle for skills.

### Step 0: Testing-First Mindset
- [ ] Committed to testing before writing
- [ ] Identified skill type (Discipline/Technique/Pattern/Reference)
- [ ] Planned pressure scenarios (3+ combined pressures for discipline skills)
- [ ] Identified failure modes to protect against

### Step 1: Understanding with Examples
- [ ] Gathered concrete examples of skill usage
- [ ] Validated examples with user (if applicable)
- [ ] Clear sense of functionality skill should support

### Step 2: Planning Contents
- [ ] Analyzed each example for execution approach
- [ ] Identified reusable scripts needed
- [ ] Identified references documentation needed
- [ ] Identified assets/templates needed
- [ ] Created list of bundled resources to include

### Step 3: RED Phase - Establish Baseline
- [ ] Created 3+ pressure scenarios (combine time/sunk cost/authority/exhaustion)
- [ ] Ran scenarios WITHOUT skill - documented baseline behavior
- [ ] Documented VERBATIM rationalizations agents used
- [ ] Identified patterns in failures/rationalizations
- [ ] Captured which pressures triggered which violations

### Step 4: Initialize the Skill
- [ ] Ran init_skill.py script (if new skill)
- [ ] OR verified existing skill structure (if updating)

### Step 5: GREEN Phase - Write Minimal Skill

**Bundled Resources:**
- [ ] Implemented scripts identified in Step 2
- [ ] Tested scripts by running them (representative sample)
- [ ] Created references files identified in Step 2
- [ ] Created assets identified in Step 2
- [ ] Deleted unused example files from initialization

**Frontmatter:**
- [ ] Name uses only letters, numbers, hyphens (no parentheses/special chars)
- [ ] YAML frontmatter with ONLY name and description (max 1024 chars)
- [ ] Description starts with "Use when..." and includes specific triggers/symptoms
- [ ] Description includes multiple user phrases ("create a skill", "test this skill")
- [ ] Description written in third person
- [ ] Description does NOT summarize workflow/process
- [ ] Description kept under 500 characters (if possible)

**Body:**
- [ ] Clear overview with core principle (1-2 sentences)
- [ ] Initial Assessment section (if applicable)
- [ ] Core Framework/Pattern with clear steps
- [ ] Address specific baseline failures identified in RED phase
- [ ] One excellent example (not multi-language)
- [ ] Output format/deliverable template (if applicable)
- [ ] Common Mistakes section with wrong/right examples
- [ ] Quality Checklist (if applicable)
- [ ] Related Skills section with cross-references

**For Discipline-Enforcing Skills:**
- [ ] Explicit loophole closing ("No exceptions" list)
- [ ] "Spirit vs Letter" principle stated early
- [ ] Rationalization table from RED phase testing
- [ ] Red flags list for self-checking
- [ ] Updated description with violation symptoms

**Testing:**
- [ ] Ran scenarios WITH skill - verified agents now comply
- [ ] Documented any NEW rationalizations for REFACTOR phase

### Step 6: Package the Skill
- [ ] Ran package_skill.py script
- [ ] Validation passed (frontmatter, naming, structure)
- [ ] Generated .skill file successfully

### Step 7: REFACTOR Phase - Close Loopholes
- [ ] Identified NEW rationalizations from GREEN phase
- [ ] Added explicit counters to skill
- [ ] Updated rationalization table
- [ ] Updated red flags list
- [ ] Updated "no exceptions" clauses (if applicable)
- [ ] Re-tested with same scenarios
- [ ] Repeated until agent bulletproof under all pressures
- [ ] Verified no new loopholes created

### Quality Checks (Final)
- [ ] Token count within targets (<500 lines for SKILL.md)
- [ ] Keywords throughout for search (errors, symptoms, tools)
- [ ] Small flowchart only if decision non-obvious (optional)
- [ ] Quick reference table (if applicable)
- [ ] No narrative storytelling
- [ ] Supporting files only for tools or heavy reference
- [ ] Cross-references validated (skill names exist)
- [ ] All "when to use" info in description, not body
- [ ] Skill passes all pressure scenarios

### Deployment
- [ ] Committed skill to git and pushed (if applicable)
- [ ] Tested in real usage scenario
- [ ] Documented any iteration needs for future updates
- [ ] Considered contributing back via PR (if broadly useful)

---

## Discovery Workflow

How future Claude finds your skill:

1. **Encounters problem** ("tests are flaky")
2. **Finds SKILL** (description matches)
3. **Scans overview** (is this relevant?)
4. **Reads patterns** (quick reference table)
5. **Loads example** (only when implementing)

**Optimize for this flow** - put searchable terms early and often.

---

## Skill Types Reference

Different skills serve different purposes. Choose the right structure:

### Technique Skills
**Purpose:** Concrete method with steps to follow

**Examples:** condition-based-waiting, root-cause-tracing, defensive-programming

**Structure:**
- Overview (what is this technique?)
- When to use (symptoms/triggers)
- Step-by-step implementation
- Example (copy-paste ready)
- Common mistakes

**Testing:** Application scenarios, variations, edge cases

### Pattern Skills
**Purpose:** Way of thinking about problems

**Examples:** flatten-with-flags, test-invariants, reducing-complexity

**Structure:**
- Overview (core insight)
- When to recognize pattern applies
- Before/after comparison
- When NOT to apply
- Related patterns

**Testing:** Recognition scenarios, application, counter-examples

### Reference Skills
**Purpose:** API docs, syntax guides, tool documentation

**Examples:** API documentation, command references, library guides

**Structure:**
- Overview (what does this document?)
- Quick reference table
- Detailed sections (or link to references/)
- Search patterns (for large docs)

**Testing:** Retrieval scenarios, application, gap testing

### Discipline Skills
**Purpose:** Enforce process/rules under pressure

**Examples:** TDD, verification-before-completion, designing-before-coding

**Structure:**
- Overview (core principle + Iron Law)
- The process (clear steps)
- **Rationalization table** (from RED phase)
- **Red flags list** (self-check)
- **No exceptions list** (explicit loopholes closed)
- Common mistakes

**Testing:** Pressure scenarios (time/sunk cost/authority/exhaustion combined)

**Special requirements:**
- Must include bulletproofing techniques
- Must be tested under pressure
- Must document actual rationalizations from testing

---

## The Bottom Line

**Creating skills IS TDD for process documentation.**

- **Same Iron Law:** No skill without failing test first
- **Same cycle:** RED (baseline) → GREEN (write skill) → REFACTOR (close loopholes)
- **Same benefits:** Better quality, fewer surprises, bulletproof results

**The process:**

1. **RED**: Run pressure scenarios WITHOUT skill, document exact failures
2. **GREEN**: Write skill addressing those specific failures, verify compliance
3. **REFACTOR**: Find new loopholes, plug them, re-verify

**Testing matters because:**
- Untested skills have loopholes. Always.
- Testing reveals actual failure modes, not hypothetical ones.
- 15 minutes testing saves hours debugging bad skills in production.
- Clear to you ≠ clear to other agents. Test it.

**Progressive disclosure matters because:**
- Context window is shared resource
- Load only what's needed, when it's needed
- Token efficiency = faster, cheaper, better results

**Bulletproofing matters because:**
- Agents are smart and find loopholes under pressure
- Explicit counters resist rationalization
- Rationalization tables document real failure modes
- Red flags enable self-checking

If you follow TDD for code, follow it for skills. It's the same discipline applied to documentation.

**Now go create world-class skills.**
