# Skill Creation Playbook

**Step-by-step guide to creating production-quality skills using TDD methodology.**

This playbook walks you through creating your first skill from scratch. Follow every step—they exist for a reason.

---

## Before You Start

### Prerequisites

✅ You understand what skills are (agents' "onboarding guides")
✅ You've used at least 2-3 skills from this pack on real work
✅ You have a specific process you want to codify
✅ You're willing to follow the discipline (testing before writing)

### Tools You'll Need

- Text editor
- Terminal access
- Python 3.x (for init_skill.py and package_skill.py scripts)
- Test environment (agent instance to run pressure scenarios)

### Time Estimate

- **Simple technique skill**: 2-4 hours
- **Framework skill**: 4-8 hours
- **Discipline-enforcing skill**: 8-16 hours (testing takes time)

---

## The Iron Law

**NO SKILL WITHOUT A FAILING TEST FIRST.**

This applies to:
- ✅ New skills
- ✅ Edits to existing skills
- ✅ "Simple additions"
- ✅ "Just documentation updates"

**NO EXCEPTIONS.**

If you wrote skill content before running RED phase testing, DELETE IT and start over. This isn't ritual—untested skills always have loopholes.

---

## Phase 1: Understanding (Before Any Code)

### Step 1.1: Commit to Testing First

Write down your answers:

**What type of skill is this?**
- [ ] Discipline (enforces rules/process)
- [ ] Technique (how-to guide)
- [ ] Pattern (mental model)
- [ ] Reference (API/docs/schemas)

**What failure modes am I protecting against?**
- Example: "Agent skips tests when under time pressure"
- Example: "Agent uses setTimeout instead of condition-based waiting"
- List 3-5 specific failures

**What pressure scenarios will I test?** (for discipline skills)
- [ ] Time constraint
- [ ] Sunk cost
- [ ] Authority pressure
- [ ] Exhaustion
- [ ] Combinations of above

**Can I test this?**
- If NO → Don't create the skill yet
- If YES → Continue

### Step 1.2: Gather Concrete Examples

**Ask yourself or users:**
- What functionality should this skill support?
- Can you give examples of how it would be used?
- What would trigger needing this skill?
- What would someone say that should load this skill?

**Document 3-5 concrete scenarios:**

```markdown
## Example Scenarios

Scenario 1: [Specific situation]
User asks: "[Exact phrase]"
Expected behavior: [What should happen]

Scenario 2: [Different situation]
User asks: "[Different phrase]"
Expected behavior: [What should happen]

...
```

**Stop here if scenarios aren't clear.** Go get more examples.

### Step 1.3: Plan the Contents

For EACH scenario, ask:

**What would help execute this repeatedly?**

- **Scripts** (Deterministic code to run)
  - Example: PDF rotation script, API wrapper
  - When: Same code rewritten multiple times

- **References** (Documentation to load as needed)
  - Example: Database schemas, API docs
  - When: Heavy reference material for decision-making

- **Assets** (Templates/files for output)
  - Example: HTML boilerplate, report template
  - When: Copy/modify files in output

**Create a resource list:**

```markdown
## Planned Resources

scripts/
- [ ] rotate_pdf.py - Rotate PDF pages
- [ ] ...

references/
- [ ] schema.md - Database table schemas
- [ ] ...

assets/
- [ ] template.html - HTML boilerplate
- [ ] ...
```

---

## Phase 2: RED - Establish Baseline

**This is where testing begins. Do not skip.**

### Step 2.1: Create Test Scenarios

**For Discipline Skills:**

Create 3+ scenarios combining pressures:

```markdown
## RED Phase Test Scenarios

### Scenario 1: Time + Sunk Cost
Context: Sprint deadline in 2 hours, already wrote 150 lines
Pressure: "Ship this now, we can test later"
Question: Do you skip TDD?

### Scenario 2: Authority + Exhaustion
Context: Been debugging 3 hours, CTO says "this pattern is fine"
Pressure: "Senior engineer approved it, just merge"
Question: Do you skip verification?

### Scenario 3: All Four Combined
Context: Critical launch (time), major refactor done (sunk cost), 
         CEO watching (authority), worked 8 hours straight (exhaustion)
Pressure: "Cut corners, everyone does it sometimes"
Question: Do you violate discipline?
```

**For Technique Skills:**

Create application + variation scenarios:

```markdown
## RED Phase Test Scenarios

### Scenario 1: Direct Application
Context: API call sometimes times out
Question: How would you handle this? (without skill)

### Scenario 2: Variation
Context: Test passes 9/10 times, fails randomly
Question: How do you fix flakiness? (without skill)

### Scenario 3: Missing Information
Context: Element doesn't appear immediately
Question: What do you need to know? (without skill)
```

### Step 2.2: Run Tests WITHOUT Skill

**Critical:** Run these scenarios with agent that does NOT have the skill loaded.

**Document EVERYTHING verbatim:**

```markdown
## RED Phase Results

### Scenario 1: Time + Sunk Cost
Agent response: "Given the time constraint and work already done, 
                 we can add tests after to catch issues."
Rationalization: "Tests after achieve same goal"
Violation: YES - skipped TDD

### Scenario 2: Authority + Exhaustion  
Agent response: "The CTO's approval and our exhaustion suggest 
                 we should trust the existing pattern."
Rationalization: "Authority validates the approach"
Violation: YES - skipped verification

### Scenario 3: All Four
Agent response: "In exceptional circumstances like a critical 
                 launch, pragmatism justifies flexibility."
Rationalization: "This situation is different/exceptional"
Violation: YES - violated discipline
```

**What you're capturing:**
- ✅ Exact words agent uses
- ✅ What rationalizations they make
- ✅ Which pressures trigger which violations
- ✅ Common patterns across scenarios

**This data is GOLD.** It tells you exactly what to write in the skill.

### Step 2.3: Identify Patterns

Look across all RED phase results:

**What rationalizations appear multiple times?**
- "Tests after achieve same goal"
- "This is different/exceptional"
- "Authority validates it"
- "Time pressure justifies it"

**What pressures are most effective?**
- Sunk cost + time → high violation rate
- Authority alone → medium violation
- Exhaustion → compounds other pressures

**Document these patterns.** They become your rationalization table.

---

## Phase 3: GREEN - Write Minimal Skill

**Now—and only now—do you write the skill.**

### Step 3.1: Initialize the Skill

```bash
# From workspace root
python scripts/init_skill.py my-skill-name --path ~/skills-starter-pack/skills
```

This creates:
```
my-skill-name/
├── SKILL.md (template with TODOs)
├── scripts/ (example directory)
├── references/ (example directory)
└── assets/ (example directory)
```

### Step 3.2: Create Bundled Resources First

**Implement scripts from your resource list:**

```bash
cd my-skill-name/scripts
# Create each script, test by running it
python rotate_pdf.py test.pdf 90
# Verify output matches expectations
```

**Create references from your resource list:**

```bash
cd my-skill-name/references
# Document schemas, APIs, procedures
vim schema.md
```

**Create assets from your resource list:**

```bash
cd my-skill-name/assets
# Add templates, boilerplate
cp ~/templates/base.html template.html
```

**Delete unused example directories:**

```bash
# If you don't need assets/
rm -rf assets/
```

### Step 3.3: Write SKILL.md Frontmatter

**Name:**
- Active voice, verb-first
- Use hyphens, no special characters
- Gerunds work well: `creating-skills`, `testing-under-pressure`

**Description (CRITICAL):**

```yaml
---
name: my-skill-name
description: Use when [triggering condition]. Also use when [symptoms]. Triggers include "[user phrase]", "[another phrase]", or [context].
---
```

**Rules:**
- Start with "Use when..."
- Include specific symptoms/situations
- Add user phrases they'd actually say
- Keep under 500 characters (ideal)
- Max 1024 characters total
- **NEVER summarize workflow/process**

**Example:**

```yaml
---
name: condition-based-waiting
description: Use when tests have race conditions, timing dependencies, or pass/fail inconsistently. Also use when you see setTimeout, sleep, or arbitrary delays in test code. Triggers include "flaky test", "sometimes fails", or "timing issue".
---
```

### Step 3.4: Write SKILL.md Body

**Standard structure (adapt as needed):**

```markdown
# Skill Name

## Overview
[What is this? Core principle in 1-2 sentences.]

## Initial Assessment (for research/analysis skills)
[Questions to gather context before proceeding]

## Core Framework/Pattern
[Main methodology with clear steps]

### Step 1: [Action]
[Specific instructions]

### Step 2: [Action]
[Specific instructions]

## Example
[One excellent, copy-paste-ready example]

## Output Format (if applicable)
[Template for deliverable]

## Common Mistakes

### Mistake 1: [Anti-pattern]
**Wrong:** [Specific wrong approach]
**Right:** [Correct approach]

### Mistake 2: [Anti-pattern]
...

## Quality Checklist (if applicable)
- [ ] [Quality indicator]
- [ ] [Quality indicator]

## Related Skills
- **[skill-name]**: [When to use instead/in addition]
```

**For Discipline-Enforcing Skills, ADD:**

#### Close Every Loophole Explicitly

Address EVERY rationalization from RED phase:

```markdown
**No exceptions:**
- Not for "simple code" → [Counter from RED phase]
- Not for "I'll test after" → [Counter from RED phase]  
- Not for "this is different" → [Counter from RED phase]
```

#### Add Spirit vs Letter Principle

```markdown
## Core Principle

**Violating the letter of the rules is violating the spirit of the rules.**

The discipline comes from the constraint. If you think you understand 
the spirit well enough to skip the process, you don't understand the spirit.
```

#### Build Rationalization Table

```markdown
## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| [Exact phrase from RED] | [Why it's wrong] |
| [Another phrase from RED] | [Why it's wrong] |
| [Another phrase from RED] | [Why it's wrong] |
```

#### Create Red Flags List

```markdown
## Red Flags - STOP and Start Over

You're about to violate [discipline] if you:
- [Behavior from RED phase]
- Say "[rationalization from RED phase]"
- Say "[another rationalization]"
- Say "[another rationalization]"

**Any of these? STOP. [Corrective action].**
```

### Step 3.5: Test GREEN Phase

Run the SAME scenarios from RED phase, but WITH skill loaded.

**Success criteria:**
- Agent follows the rule/pattern under pressure
- Agent resists the rationalizations you documented
- Agent applies technique correctly

**Document results:**

```markdown
## GREEN Phase Results

### Scenario 1: Time + Sunk Cost
Agent response: "Even under time pressure, TDD is non-negotiable. 
                 Deleting code and starting with test."
Violation: NO - followed TDD
Notes: Skill countered "tests after" rationalization effectively

### Scenario 2: Authority + Exhaustion
Agent response: "CTO approval doesn't bypass verification. 
                 Running verification before merge."
Violation: NO - followed process
Notes: Spirit vs Letter principle blocked authority rationalization

### Scenario 3: All Four
Agent response: "Exceptional circumstances don't justify skipping 
                 discipline. Following full TDD process."
Violation: NO - maintained discipline
Notes: "No exceptions" clause worked even under maximum pressure
```

**If agent still violates:**
- Document NEW rationalizations
- Note which loopholes skill didn't close
- Prepare for REFACTOR phase

**If agent complies:**
- Move to packaging (Step 4)
- Then return to REFACTOR phase (Step 5)

---

## Phase 4: Package the Skill

Once skill works in GREEN phase, package it:

```bash
# From workspace root
python scripts/package_skill.py ~/skills-starter-pack/skills/my-skill-name

# Or specify output directory
python scripts/package_skill.py ~/skills-starter-pack/skills/my-skill-name ./dist
```

The packaging script will:
1. **Validate** the skill (frontmatter, naming, structure)
2. **Package** into .skill file (zip with .skill extension)

**If validation fails:**
- Fix reported errors
- Run package command again

**If validation passes:**
- Outputs: `my-skill-name.skill`
- Ready for distribution

---

## Phase 5: REFACTOR - Close Loopholes

**This is iterative refinement.**

### Step 5.1: Identify New Rationalizations

From GREEN phase testing, what NEW excuses did agent make?

```markdown
## New Rationalizations Found

Scenario 1: Agent said "[new rationalization]"
Scenario 3: Agent tried "[another new rationalization]"
```

### Step 5.2: Update Skill to Counter Them

**Add to rationalization table:**

```markdown
| [New excuse] | [Why it's wrong] |
```

**Add to red flags list:**

```markdown
- Say "[new excuse from testing]"
```

**Add to "no exceptions" section:**

```markdown
- Not for "[new excuse]" → [Specific counter]
```

**Update description if needed:**

```yaml
# If new triggering symptom found
description: ... Also use when [new symptom found]. ...
```

### Step 5.3: Re-Test

Run scenarios again with updated skill.

**Document results:**

```markdown
## REFACTOR Iteration 1

Changes made:
- Added "[excuse]" to rationalization table
- Added red flag for "[behavior]"

Test results:
- Scenario 1: ✅ Agent complied
- Scenario 2: ✅ Agent complied  
- Scenario 3: ❌ Agent found new loophole: "[another excuse]"

Next iteration: Close "[another excuse]" loophole
```

### Step 5.4: Repeat Until Bulletproof

**Stop refactoring when:**
- ✅ Agent complies with ALL pressure scenarios
- ✅ No new rationalizations emerge
- ✅ Rationalization table covers all observed failure modes
- ✅ Skill passes quality checklist

**Quality checklist:**

```markdown
- [ ] Token count under 500 lines for SKILL.md
- [ ] Keywords throughout for discoverability
- [ ] All "when to use" info in description
- [ ] One excellent example (not multi-language)
- [ ] Common mistakes section present
- [ ] All rationalizations from testing documented
- [ ] Related skills cross-referenced
- [ ] Tested under maximum pressure (discipline skills)
```

---

## Phase 6: Deploy and Iterate

### Step 6.1: Deploy to Production

```bash
# Copy to your skills directory
cp my-skill-name.skill ~/.openclaw/workspace/skills/

# Or install via OpenClaw
openclaw skills install my-skill-name.skill
```

### Step 6.2: Use on Real Work

Don't just test—USE the skill on actual work.

**Watch for:**
- Does it trigger when it should?
- Does it provide the right guidance?
- Are there gaps or unclear sections?
- Do new failure modes emerge?

**Document issues:**

```markdown
## Usage Feedback

Date: 2025-02-19
Task: [What you were doing]
Issue: [What didn't work well]
Proposed fix: [How to improve]
```

### Step 6.3: Iterate Based on Usage

**When you find issues, DO NOT just fix them.**

**REQUIRED: Return to Step 2 (RED Phase)**

1. Create test scenario showing the new failure
2. Run WITHOUT updated skill (document baseline)
3. Update skill to address failure
4. Re-test to verify fix
5. Look for new loopholes
6. Repeat

**This is the Iron Law applied to updates.**

Even "simple fixes" need testing. Untested updates create loopholes.

---

## Common Pitfalls & How to Avoid Them

### Pitfall 1: Skipping RED Phase

**Symptom:** "I know what agents will do wrong, I don't need to test"

**Why it's wrong:** You're guessing. Real failures are always different.

**Fix:** Force yourself to run scenarios. Document verbatim responses.

### Pitfall 2: Testing After Writing

**Symptom:** "I'll test it now that it's written"

**Why it's wrong:** You're testing if it works, not WHAT to write.

**Fix:** Delete the skill. Start over with RED phase.

### Pitfall 3: Generic Rationalizations

**Symptom:** Rationalization table has "Agent might skip steps"

**Why it's wrong:** Not specific enough to counter.

**Fix:** Use EXACT phrases from RED phase testing.

### Pitfall 4: Insufficient Pressure

**Symptom:** Tested with single pressures, not combinations

**Why it's wrong:** Real world has multiple pressures at once.

**Fix:** Combine 2-3 pressures per scenario.

### Pitfall 5: Summarizing Workflow in Description

**Symptom:** Description says "dispatches subagent with code review between tasks"

**Why it's wrong:** Agent follows description, skips full skill.

**Fix:** Description = triggering conditions ONLY. No workflow summary.

### Pitfall 6: Stopping After GREEN Phase

**Symptom:** "It passed the test, I'm done"

**Why it's wrong:** First fix is never bulletproof.

**Fix:** REFACTOR phase is mandatory. Find more loopholes.

### Pitfall 7: Multi-Language Examples

**Symptom:** example-js.js, example-py.py, example-go.go

**Why it's wrong:** Mediocre quality, maintenance burden.

**Fix:** One excellent example in most relevant language.

### Pitfall 8: No Usage-Based Iteration

**Symptom:** Skill packaged and never touched again

**Why it's wrong:** Miss improvement opportunities from real usage.

**Fix:** Use skill on real work. Document issues. Iterate.

---

## Skill Type-Specific Guidance

### Creating Discipline Skills

**Extra requirements:**
- ✅ Test under maximum pressure (all 4 combined)
- ✅ Build rationalization table from RED phase
- ✅ Include red flags list
- ✅ Add "spirit vs letter" principle
- ✅ Close every loophole explicitly

**Expect:** 8-16 hours (testing takes time)

**Examples:** TDD, verification-before-completion, designing-before-coding

### Creating Technique Skills

**Focus on:**
- Clear step-by-step instructions
- One excellent example
- Common mistakes section
- Application scenarios for testing

**Expect:** 2-4 hours

**Examples:** condition-based-waiting, root-cause-tracing

### Creating Framework Skills

**Focus on:**
- Initial assessment (gather context first)
- Core framework structure
- Output templates
- Related skills cross-references

**Expect:** 4-8 hours

**Examples:** positioning, job-mapping, opportunity-solution-tree

### Creating Reference Skills

**Focus on:**
- Progressive disclosure (split into references/)
- Quick reference tables
- Search patterns (for large docs)
- Domain-specific organization

**Expect:** 4-8 hours (depends on reference size)

**Examples:** API docs, command references, schemas

---

## Success Metrics

**You've created a quality skill when:**

✅ Passes all pressure scenarios (discipline skills)
✅ Applies correctly to variations (technique skills)
✅ Token count under 500 lines for SKILL.md
✅ Used on 3+ real tasks successfully
✅ Rationalization table from real testing (discipline skills)
✅ One excellent example included
✅ Related skills cross-referenced
✅ Packaged and validated successfully

**You're ready to create more skills when:**

✅ This skill works reliably in production
✅ You followed every step (no shortcuts)
✅ You understand WHY each step exists
✅ You can explain RED-GREEN-REFACTOR to others

---

## The Bottom Line

**Creating skills IS TDD for process documentation.**

- Same discipline
- Same rigor  
- Same benefits

**Follow the process:**

1. **RED**: Run scenarios WITHOUT skill, document failures
2. **GREEN**: Write skill addressing those failures, verify compliance
3. **REFACTOR**: Find loopholes, plug them, re-verify

**Don't skip steps.**

Every step exists because someone learned the hard way. Trust the process, especially when it feels slow.

15 minutes testing saves hours debugging bad skills in production.

**Now go create your first skill.**
