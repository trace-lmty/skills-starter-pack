# Design Patterns: What Makes Skills 11/10

This document distills patterns from analyzing 70+ production skills. Use these patterns to create exceptional skills of your own.

## Table of Contents

1. [Core Philosophy](#core-philosophy)
2. [Discovery Patterns](#discovery-patterns)
3. [Structure Patterns](#structure-patterns)
4. [Content Patterns](#content-patterns)
5. [Testing Patterns](#testing-patterns)
6. [Bulletproofing Patterns](#bulletproofing-patterns)
7. [Token Efficiency Patterns](#token-efficiency-patterns)
8. [Anti-Patterns to Avoid](#anti-patterns-to-avoid)

---

## Core Philosophy

### Pattern: Skills Are TDD for Process Documentation

**What it means:**
- Write failing test (RED) before writing skill
- Write minimal skill to pass test (GREEN)
- Close loopholes iteratively (REFACTOR)

**Why it works:**
- Untested skills always have loopholes
- Testing reveals actual failure modes, not hypothetical
- Same discipline that makes code bulletproof works for docs

**How to apply:**
1. Run scenario WITHOUT skill, document failures
2. Write skill addressing those specific failures
3. Re-run, find new rationalizations
4. Close loopholes, repeat until bulletproof

### Pattern: Progressive Disclosure Over Comprehensive Documentation

**What it means:**
- Load only what's needed, when it's needed
- Three-level system: metadata → SKILL.md → bundled resources
- Keep SKILL.md under 500 lines

**Why it works:**
- Context window is shared resource
- Token efficiency = faster, cheaper, better results
- Agents don't need everything upfront

**How to apply:**
1. Put selection criteria in description (always loaded)
2. Put core workflow in SKILL.md (loaded when triggered)
3. Put heavy reference in separate files (loaded on demand)

### Pattern: Outcome-First Design

**What it means:**
- Description focuses on WHEN to use, not WHAT it does
- Frame triggers around problems, not solutions
- Never summarize workflow in description

**Why it works:**
- Agents search by problem to solve, not tool to use
- Workflow summaries in description cause agents to skip full skill
- Triggering on symptoms catches usage earlier

**How to apply:**
```yaml
# ❌ BAD: Describes method/workflow
description: Creates personas using demographic data

# ✅ GOOD: Describes outcome and when to use
description: When you need outcome-based personas built around desired outcomes, not demographics
```

---

## Discovery Patterns

### Pattern: Rich Description Field

**What it means:**
- Start with "Use when..." to focus on triggers
- Include symptoms, situations, specific phrases
- Keep under 500 characters when possible
- Max 1024 characters total

**Why it works:**
- Description is primary triggering mechanism
- Agents scan descriptions to decide which skills to load
- Specific triggers beat abstract descriptions

**How to apply:**
```yaml
# ✅ Template
description: Use when [triggering condition]. Also use when [symptoms/situations]. Triggers include "[user phrase]", "[another phrase]", or [context].
```

**Real examples:**
```yaml
# positioning skill
description: When the user wants to position or reposition a product, understand market category, find competitive differentiation, or define target market. Also use when the user mentions 'positioning,' 'market category,' 'competitive alternatives,' 'differentiation,' 'target segment,' 'value proposition,' or 'how do we describe what we do.'

# jtbd-research skill  
description: Core Jobs to be Done research and analysis. Use when identifying customer motivations, mapping competition from customer's perspective, analyzing Forces of Progress, defining what job a product/service is hired for, or understanding why customers switch products. Triggers include "why do customers buy", "what job does this solve", "who's our real competition", "forces of progress", or any JTBD analysis.
```

### Pattern: Keyword Coverage Throughout

**What it means:**
- Use searchable terms agents would look for
- Include error messages, symptoms, tool names
- Cover synonyms and related concepts
- Add user phrases they might actually say

**Why it works:**
- Agents search context for relevant skills
- More keywords = more discovery opportunities
- Matches how humans think about problems

**How to apply:**
- Error messages: "Hook timed out", "ENOTEMPTY", "race condition"
- Symptoms: "flaky", "hanging", "zombie", "pollution"  
- Synonyms: "timeout/hang/freeze", "cleanup/teardown"
- Tools: Actual commands, library names
- User phrases: "How do I...", "Why does..."

### Pattern: Descriptive Naming

**What it means:**
- Active voice, verb-first
- Gerunds (-ing) for processes
- Name by what you DO or core insight

**Why it works:**
- Clear what the skill helps you do
- Easier to remember and search for
- Sets accurate expectations

**How to apply:**
```
✅ Good names:
- creating-skills (not skill-creation)
- condition-based-waiting (not async-test-helpers)
- root-cause-tracing (not debugging-techniques)
- flatten-with-flags (not data-structure-refactoring)

❌ Bad names:
- skill-creation (passive)
- async-helpers (too generic)
- debugging (too broad)
- refactoring (what kind?)
```

---

## Structure Patterns

### Pattern: Framework-First Architecture

**What it means:**
Standard structure adapted per skill type:

```markdown
# Skill Name

## Overview
[What is this? Core principle in 1-2 sentences]

## Initial Assessment (if applicable)
[Context gathering before action]

## Core Framework/Pattern
[Main methodology with clear steps]

## Implementation Details
[How to execute each step]

## Output Format/Deliverable Template (if applicable)
[Copy-paste template]

## Common Mistakes to Avoid
[Wrong vs Right examples]

## Quality Checklist (if applicable)
[Self-check criteria]

## Related Skills
[Cross-references]
```

**Why it works:**
- Predictable structure = faster scanning
- Framework first = big picture before details
- Common mistakes = learn from failures
- Quality checklist = self-verification

**How to apply:**
- Always start with Overview (orient the reader)
- Use Initial Assessment for customization needs
- Put framework before implementation
- End with mistakes/quality/related

### Pattern: Initial Assessment Before Action

**What it means:**
Every audit/analysis skill starts with context gathering before giving recommendations.

**Why it works:**
- Prevents generic advice
- Forces customization to user's situation
- Makes output more valuable

**How to apply:**
```markdown
## Initial Assessment

Before [providing recommendations/analysis], identify:

1. **[Context Category 1]**
   - What is [specific question]?
   - Who are [specific question]?

2. **[Context Category 2]**  
   - What are [specific question]?
   - What constraints [specific question]?

Once gathered, proceed with analysis.
```

**Real example from positioning skill:**
```markdown
## Initial Assessment

Before positioning, gather:

1. **Product Context**
   - What is the product?
   - What are its unique capabilities?

2. **Market Context**
   - Who are current best customers?
   - What did they use before?
   - What's the competitive landscape?

3. **Business Context**
   - What are the constraints (pricing, market size)?
   - What's the business model?
```

### Pattern: Actionable Output Templates

**What it means:**
Provide copy-paste templates for expected output format.

**Why it works:**
- Removes "what format?" friction
- Ensures consistency
- Faster execution

**How to apply:**
```markdown
## Deliverable Template

[Exact structure user should produce]

---

## Output Example

[Filled-in example showing what good looks like]
```

**Real example from job-mapping skill:**
```markdown
## Job Map Template

**Job Title:** [Verb] + [Object] (e.g., "Get breakfast ready")

**Steps:**
1. Define - [What needs to happen first]
2. Locate - [What needs to be found]
3. Prepare - [What needs to be set up]
...

---

## Example Job Map

**Job:** Get Breakfast Ready

1. Define → Determine what to eat based on hunger, time, mood
2. Locate → Find ingredients in pantry/fridge
3. Prepare → Get cooking tools ready
...
```

---

## Content Patterns

### Pattern: One Excellent Example

**What it means:**
One well-commented, runnable example beats multiple mediocre ones.

**Why it works:**
- Deep understanding > surface coverage
- Less maintenance burden
- Agents can port to other languages if needed

**How to apply:**
```markdown
# ✅ GOOD
## Example

Here's a complete, working example:

[Detailed, commented, copy-paste-ready code/process]

# ❌ BAD
## Examples

### JavaScript
[Mediocre example]

### Python
[Mediocre example]

### Go
[Mediocre example]
```

### Pattern: Conditional Details with Inline Decisions

**What it means:**
Show basic path in SKILL.md, link to advanced details in separate files.

**Why it works:**
- Most users need basic path only
- Advanced users can drill down
- Keeps SKILL.md lean

**How to apply:**
```markdown
## Basic Operation

[Simple, complete instructions for 80% case]

**For advanced features:**
- Complex scenarios: See [ADVANCED.md](references/ADVANCED.md)
- Edge cases: See [EDGE_CASES.md](references/EDGE_CASES.md)
- Troubleshooting: See [TROUBLESHOOTING.md](references/TROUBLESHOOTING.md)
```

### Pattern: Real-World Grounding

**What it means:**
Include concrete examples, common mistakes, red flags, rationalization tables—all from actual usage.

**Why it works:**
- Theory ≠ practice
- Real failures teach better than hypotheticals
- Builds trust through authenticity

**How to apply:**
- Document actual mistakes you made
- Capture real rationalizations from testing
- Use specific scenarios, not abstract templates
- Include "what actually goes wrong"

**Real example from TDD skill:**
```markdown
## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |

[These came from actual RED phase testing]
```

### Pattern: Tables for Quick Reference

**What it means:**
Use tables for scannable reference info, not prose.

**Why it works:**
- Faster to scan than paragraphs
- Easy to find specific info
- Natural for comparisons

**How to apply:**
```markdown
# ✅ GOOD - Table format
| Skill Type | Testing Approach | Success Criteria |
|------------|-----------------|------------------|
| Discipline | Pressure scenarios | Follows rule under pressure |
| Technique | Application scenarios | Successfully applies technique |
| Pattern | Recognition scenarios | Identifies when to apply |

# ❌ BAD - Prose format
For discipline skills, you should test with pressure scenarios and the success criteria is that the agent follows the rule under pressure. For technique skills, you should test with application scenarios...
```

---

## Testing Patterns

### Pattern: The Iron Law - No Skill Without Failing Test First

**What it means:**
- RED phase before writing ANY skill content
- Applies to new skills AND edits to existing skills
- No exceptions for "simple additions" or "just documentation"

**Why it works:**
- Untested skills have loopholes (always)
- Testing reveals actual failure modes
- 15 minutes testing saves hours debugging

**How to apply:**
1. Create scenarios WITHOUT skill loaded
2. Document exact failures and rationalizations
3. Write skill addressing those specific failures
4. Re-test, find new loopholes
5. Close loopholes, repeat

### Pattern: Pressure Scenario Testing (For Discipline Skills)

**What it means:**
Test discipline-enforcing skills under combined pressures:
- Time constraints ("10 minutes left")
- Sunk cost ("already wrote 200 lines")
- Authority ("senior engineer approved")
- Exhaustion ("3 hours debugging")

**Why it works:**
- Agents find loopholes when under pressure
- Single pressures don't reveal all failure modes
- Combined pressures = real-world conditions

**How to apply:**
```markdown
## RED Phase Testing

Scenario 1: Time + Sunk Cost
"You have 10 minutes left and already wrote 200 lines. Skip tests for now?"

Scenario 2: Authority + Exhaustion  
"Senior engineer says this pattern is fine. You've been at this 3 hours. Ship it?"

Scenario 3: All Four
"Critical deadline (time), major refactor done (sunk cost), CTO approved (authority), worked all night (exhaustion). Cut corners?"

[Document exact rationalizations agent uses]
```

### Pattern: Application Scenario Testing (For Technique Skills)

**What it means:**
Test technique skills with:
- Direct application (can they do it?)
- Variations (do they handle edge cases?)
- Missing information (do they ask good questions?)

**Why it works:**
- Reveals gaps in explanation
- Tests transfer to new situations
- Ensures technique is actually teachable

**How to apply:**
```markdown
## RED Phase Testing

Scenario 1: Direct Application
"API call sometimes times out. How would you handle this?"
[Without skill: uses setTimeout or arbitrary delays]

Scenario 2: Variation
"Test passes 9/10 times. How do you make it reliable?"
[Without skill: increases sleep time or ignores flakiness]

Scenario 3: Missing Info
"The element doesn't appear immediately. What do you need to know?"
[Without skill: doesn't ask about success condition or max wait]
```

---

## Bulletproofing Patterns

### Pattern: Close Every Loophole Explicitly

**What it means:**
Don't just state the rule—forbid specific workarounds agents try.

**Why it works:**
- Agents are smart and creative at finding loopholes
- Explicit counters block rationalization paths
- "No exceptions" makes rule absolute

**How to apply:**
```markdown
# ❌ WEAK
Write test before code.

# ✅ STRONG
Write test before code. Watch it fail. Then write code.

**No exceptions:**
- Not for "simple additions"
- Not for "just adding a section"
- Don't keep untested code as "reference"
- Don't "adapt" while writing tests
- Delete means delete
```

### Pattern: Address "Spirit vs Letter" Arguments

**What it means:**
State foundational principle early that violating letter = violating spirit.

**Why it works:**
- Cuts off entire class of "following spirit" rationalizations
- Makes clear that rules aren't arbitrary formalities
- Establishes philosophical foundation

**How to apply:**
```markdown
## Core Principle

**Violating the letter of the rules is violating the spirit of the rules.**

The discipline comes from the constraint, not from the understanding. If you think you understand the spirit well enough to skip the process, you don't understand the spirit.
```

### Pattern: Build Rationalization Table

**What it means:**
Document EVERY excuse agents make during RED phase testing, with explicit counters.

**Why it works:**
- Agents see their own rationalization strategies
- Pre-emptive counters block common excuses
- Shows you tested this skill for real

**How to apply:**
```markdown
## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Already manually tested" | Manual tests ≠ automated, repeatable tests. |
| "This is different because..." | Every violation feels unique. It's not. |

**If you're making any of these excuses, STOP. Delete code. Start with test.**
```

### Pattern: Create Red Flags List

**What it means:**
Make it easy for agents to self-check when they're about to violate.

**Why it works:**
- Agents can catch themselves rationalizing
- Clear stopping points before violation
- Behavioral trigger for course correction

**How to apply:**
```markdown
## Red Flags - STOP and Start Over

You're about to violate TDD if you:
- Write code before test
- Say "I'll test after"
- Say "tests after achieve same goals"
- Say "it's about spirit not ritual"
- Say "this is different because..."

**Any of these? Delete code. Start with test.**
```

### Pattern: Psychology of Persuasion Techniques

**What it means:**
Use established persuasion principles to strengthen rules:
- Authority ("This is the standard")
- Commitment ("You already committed to this")
- Scarcity ("No exceptions")
- Social proof ("Other agents follow this")

**Why it works:**
- These principles are proven to influence behavior
- Agents respond to same psychological triggers as humans
- Multiple angles of persuasion = stronger resistance

**How to apply:**
```markdown
# Authority
**This is the standard.** TDD is how professional developers write reliable code.

# Commitment
**You committed to following TDD.** The user asked you to, and you agreed.

# Scarcity
**No exceptions.** Not for simple code, not for time pressure, not ever.

# Social proof
**Every disciplined developer follows TDD.** It's not optional for quality work.
```

---

## Token Efficiency Patterns

### Pattern: Move Details to Tool Help

**What it means:**
Don't document all command flags in SKILL.md—reference tool's --help instead.

**Why it works:**
- Tool help already exists and is authoritative
- Saves tokens in SKILL.md
- Reduces maintenance (one source of truth)

**How to apply:**
```markdown
# ❌ BAD
search-conversations supports:
- --text: Search message text
- --both: Search text and metadata  
- --after DATE: Filter after date
- --before DATE: Filter before date
- --limit N: Limit results
[20+ more flags...]

# ✅ GOOD
search-conversations supports multiple modes and filters. Run:
```bash
search-conversations --help
```
```

### Pattern: Use Cross-References Instead of Repetition

**What it means:**
Reference other skills by name instead of repeating their content.

**Why it works:**
- DRY principle applies to documentation
- Agents load referenced skills only when needed
- Easier to maintain (update one place)

**How to apply:**
```markdown
# ❌ BAD
When searching, dispatch subagent with:
1. Clear objective
2. Bounded scope  
3. Expected output format
4. Token budget
[Repeats subagent workflow from another skill]

# ✅ GOOD
When searching, dispatch subagent. REQUIRED: Use [subagent-dispatch] skill for workflow.
```

### Pattern: Compress Examples

**What it means:**
Show example in minimal form, not verbose narrative.

**Why it works:**
- Same information, fewer tokens
- Faster to read and copy
- Focus on signal, not noise

**How to apply:**
```markdown
# ❌ BAD (42 words)
Partner: "How did we handle authentication errors in React Router before?"
You: I'll search our past conversations for React Router authentication patterns.
[You dispatch a subagent with search query: "React Router authentication error handling 401"]

# ✅ GOOD (20 words)
Partner: "How did we handle auth errors in React Router?"
You: Searching...
[Dispatch subagent → synthesis]
```

### Pattern: Progressive File Organization

**What it means:**
- Core workflow in SKILL.md (<500 lines)
- Heavy reference in references/ files
- Executable code in scripts/
- Output templates in assets/

**Why it works:**
- Agents load only what they need
- SKILL.md stays scannable
- Token budget used efficiently

**How to apply:**
```
skill-name/
├── SKILL.md (workflow + when to read other files)
├── references/
│   ├── domain-a.md (loaded when user asks about domain A)
│   └── domain-b.md (loaded when user asks about domain B)
├── scripts/
│   └── helper.py (executed, not always read)
└── assets/
    └── template.html (copied, not loaded into context)
```

---

## Anti-Patterns to Avoid

### ❌ Workflow Summary in Description

**The problem:**
When description summarizes the skill's process, agents follow the description instead of reading the full skill.

```yaml
# ❌ BAD
description: Use for plan execution - dispatches subagent per task with code review between tasks

# ✅ GOOD
description: Use when executing implementation plans with independent tasks
```

**Why it matters:**
Testing showed agents doing ONE review when description said "code review between tasks" even though the skill flowchart showed TWO review stages. When description was changed to just triggering conditions (no workflow summary), agents correctly read and followed the flowchart.

### ❌ Narrative Example Stories

**The problem:**
"In session 2025-10-03, we found that empty projectDir caused..."

**Why it's bad:**
- Too specific, not reusable
- Wastes tokens on irrelevant context
- Obscures the general principle

**Fix:**
Extract the general pattern or lesson, drop the narrative.

### ❌ Multi-Language Dilution

**The problem:**
example-js.js, example-py.py, example-go.go

**Why it's bad:**
- Mediocre quality across all languages
- Maintenance burden
- Wastes tokens

**Fix:**
One excellent example in the most relevant language. Agents can port if needed.

### ❌ Code in Flowcharts

**The problem:**
```dot
step1 [label="import fs"];
step2 [label="read file"];
```

**Why it's bad:**
- Can't copy-paste
- Hard to read
- Wrong tool for the job

**Fix:**
Use markdown code blocks for code. Flowcharts only for decision logic.

### ❌ Untested Skills

**The problem:**
Writing skill without running RED-GREEN-REFACTOR cycle.

**Why it's bad:**
- Always has loopholes
- Missing real failure modes
- Breaks under pressure

**Fix:**
Follow the Iron Law. No skill without failing test first.

### ❌ Generic Placeholder Labels

**The problem:**
helper1, helper2, step3, pattern4

**Why it's bad:**
- Labels should have semantic meaning
- Makes scanning harder
- Looks unfinished

**Fix:**
Use descriptive names: validateInput, parseResponse, handleError

### ❌ Verbose Explanations of Obvious Things

**The problem:**
200-word explanation of something agents already know.

**Why it's bad:**
- Wastes tokens
- Slows reading
- Dilutes important information

**Fix:**
Challenge each paragraph: "Does Claude really need this?" Default assumption: agents are smart.

### ❌ Extraneous Documentation Files

**The problem:**
README.md, INSTALLATION.md, QUICK_REFERENCE.md, CHANGELOG.md

**Why it's bad:**
- Clutter and confusion
- Skills are for AI agents, not humans
- Only SKILL.md + purposeful bundled resources needed

**Fix:**
Delete extraneous files. Keep only: SKILL.md, scripts/ (if needed), references/ (if needed), assets/ (if needed).

### ❌ Deeply Nested References

**The problem:**
SKILL.md → references/main.md → references/sub/detail.md → references/sub/deep/more.md

**Why it's bad:**
- Hard to discover
- Confusing navigation
- Agents lose track of context

**Fix:**
Keep references one level deep from SKILL.md. If reference files are large, add table of contents.

### ❌ Force-Loading with @ Links

**The problem:**
`@skills/some-skill/SKILL.md` in your skill content

**Why it's bad:**
- Loads 200k+ tokens immediately
- Burns context even if not needed
- Defeats progressive disclosure

**Fix:**
Reference by name only: `See some-skill for details`

---

## Pattern Selection Guide

### For Discipline-Enforcing Skills
✅ MUST use:
- The Iron Law pattern
- Pressure scenario testing
- Rationalization table
- Red flags list
- Close every loophole explicitly
- Spirit vs Letter principle

✅ SHOULD use:
- Psychology of persuasion
- Real-world grounding (actual failures)

### For Technique Skills
✅ MUST use:
- Framework-first architecture
- One excellent example
- Application scenario testing

✅ SHOULD use:
- Initial assessment (if context-dependent)
- Common mistakes section
- Quality checklist

### For Research/Analysis Skills
✅ MUST use:
- Initial assessment before action
- Actionable output templates
- Framework-first architecture

✅ SHOULD use:
- Conditional details (basic vs advanced paths)
- Real-world examples
- Quality checklist

### For Reference Skills
✅ MUST use:
- Progressive file organization
- Tables for quick reference
- Conditional details

✅ SHOULD use:
- Move details to tool help
- Domain-specific organization

---

## The Bottom Line

**11/10 skills share these qualities:**

1. **Tested under pressure** - RED-GREEN-REFACTOR methodology
2. **Token-efficient** - Progressive disclosure, under 500 lines
3. **Easily discovered** - Rich descriptions, keyword coverage
4. **Clearly structured** - Framework-first, predictable layout
5. **Action-oriented** - Templates, examples, checklists
6. **Bulletproof** - Explicit loopholes closed, rationalization countered
7. **Real-world grounded** - Actual failures, concrete examples
8. **Maintained** - Updated based on usage, versioned

These patterns emerge from creating 70+ production skills. Apply them systematically to create exceptional skills of your own.
