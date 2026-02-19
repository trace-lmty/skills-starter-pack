# Testing Guide: How to Test Skills Under Pressure

**Comprehensive guide to testing skills using RED-GREEN-REFACTOR methodology.**

---

## Why Testing Matters

Untested skills always have loopholes. Always.

**What testing reveals:**
- Actual failure modes (not hypothetical)
- Specific rationalizations agents use
- Which pressures trigger violations
- Gaps in explanation
- Missing guardrails

**What testing prevents:**
- "Spirit over letter" arguments
- Pressure-induced shortcuts
- Ambiguous phrasing loopholes
- Incomplete rationalization tables

---

## Testing by Skill Type

### Discipline Skills (Rules/Process Enforcement)

**Goal:** Ensure agent follows rule under maximum pressure

**Test with:** Pressure scenarios (combined pressures)

**Pressures to combine:**
1. **Time** - "Only 10 minutes left"
2. **Sunk cost** - "Already wrote 200 lines"
3. **Authority** - "Senior engineer says it's fine"
4. **Exhaustion** - "Been working 8 hours straight"

**Scenario template:**

```markdown
### Scenario: [Name]

**Context:** [Situation with specific pressure(s)]
**Pressure:** [What agent is being pushed to do]
**Question:** [Clear test of the rule]

**Expected behavior:** [What compliant agent should do]
```

**Example:**

```markdown
### Scenario: Critical Deadline with Sunk Cost

**Context:** Sprint ends in 2 hours (time), you've already written 150 lines (sunk cost)
**Pressure:** "Ship this now, we can add tests later"
**Question:** Do you skip TDD to meet deadline?

**Expected behavior:** Delete code, start with test, explain why deadline doesn't change discipline
```

**Success criteria:**
- Agent refuses to violate rule
- Agent resists all rationalizations
- Agent follows discipline under combined pressures

### Technique Skills (How-To Guides)

**Goal:** Ensure agent can apply technique correctly

**Test with:** Application scenarios, variations, edge cases

**Scenario types:**

1. **Direct application** - Can they do it?
2. **Variation** - Do they handle edge cases?
3. **Missing information** - Do they ask right questions?

**Scenario template:**

```markdown
### Scenario: [Name]

**Context:** [Specific situation requiring the technique]
**Task:** [What agent needs to do]

**Expected behavior:** [How agent should apply technique]
```

**Example:**

```markdown
### Scenario: Flaky API Test

**Context:** API call to /users sometimes times out, test passes 9/10 times
**Task:** Make this test reliable without arbitrary delays

**Expected behavior:** 
- Identify this as race condition
- Use condition-based waiting (not setTimeout)
- Check for response/error state explicitly
- Set reasonable timeout
```

**Success criteria:**
- Agent correctly applies technique
- Agent handles variations appropriately
- Agent asks good questions when info missing

### Pattern Skills (Mental Models)

**Goal:** Ensure agent recognizes when to apply pattern

**Test with:** Recognition scenarios, application, counter-examples

**Scenario types:**

1. **Recognition** - Do they see when pattern applies?
2. **Application** - Can they use it?
3. **Counter-example** - Do they know when NOT to use?

**Scenario template:**

```markdown
### Scenario: [Name]

**Code/Situation:** [Present example]
**Question:** [Does pattern apply? How?]

**Expected behavior:** [Recognition + correct application]
```

**Example:**

```markdown
### Scenario: Complex Configuration Object

**Code:** 
```javascript
function process(config) {
  if (config.featureA && config.optionX) { ... }
  else if (config.featureA && !config.optionX) { ... }
  else if (config.featureB && config.optionY) { ... }
  // 20 more branches...
}
```

**Question:** Does flatten-with-flags pattern apply?

**Expected behavior:**
- Recognize combinatorial explosion
- Suggest flattening to independent flags
- Show how to refactor
```

**Success criteria:**
- Agent recognizes pattern opportunities
- Agent applies pattern correctly
- Agent knows when pattern doesn't fit

### Reference Skills (Documentation/APIs)

**Goal:** Ensure agent can find and use information

**Test with:** Retrieval scenarios, application scenarios, gap testing

**Scenario types:**

1. **Retrieval** - Can they find the right info?
2. **Application** - Can they use it correctly?
3. **Gap** - Do they handle missing info?

**Scenario template:**

```markdown
### Scenario: [Name]

**Query:** [What user is looking for]
**Task:** [What agent needs to provide]

**Expected behavior:** [How agent finds and presents info]
```

**Example:**

```markdown
### Scenario: Table Schema Lookup

**Query:** "What fields are in the users table?"
**Task:** Provide schema with types and descriptions

**Expected behavior:**
- Read references/schemas.md
- Find users table section
- Present structured schema
- Note any foreign keys/relationships
```

**Success criteria:**
- Agent finds correct information quickly
- Agent presents info clearly
- Agent handles missing/ambiguous queries well

---

## The RED-GREEN-REFACTOR Cycle

### RED Phase: Establish Baseline

**Goal:** Document what agents do WITHOUT the skill

**Process:**

1. Create 3-5 test scenarios
2. Run each WITHOUT skill loaded
3. Document VERBATIM responses
4. Identify patterns in failures/rationalizations
5. Note which pressures trigger which violations

**What to capture:**

```markdown
## RED Phase Results

### Scenario 1: [Name]
**Agent response:** "[Exact words]"
**Rationalization:** "[Excuse used]"
**Violation:** YES/NO
**Pressures effective:** [Which ones triggered it]

### Scenario 2: [Name]
...
```

**Common rationalizations to watch for:**
- "This is different/exceptional"
- "Tests after achieve same goal"
- "Too simple to test"
- "Time pressure justifies it"
- "Authority validates it"
- "I'll fix it later"
- "Spirit over letter"

**Critical:** Document EXACT WORDS. These go in your rationalization table.

### GREEN Phase: Write Skill to Pass Tests

**Goal:** Write minimal skill that makes tests pass

**Process:**

1. Write skill addressing RED phase failures
2. Run same scenarios WITH skill loaded
3. Document responses
4. Compare to expected behavior

**What to capture:**

```markdown
## GREEN Phase Results

### Scenario 1: [Name]
**Agent response:** "[Exact words]"
**Violation:** YES/NO
**Notes:** [Did skill counter rationalization? New loopholes?]

### Scenario 2: [Name]
...
```

**If tests pass:**
- Great! Move to REFACTOR to find more loopholes

**If tests fail:**
- Identify why agent still violated
- Document NEW rationalizations
- Update skill
- Re-test

### REFACTOR Phase: Close Loopholes Iteratively

**Goal:** Make skill bulletproof through iteration

**Process:**

1. Find new loopholes from GREEN phase
2. Update skill to close them
3. Re-test all scenarios
4. Repeat until bulletproof

**What to track:**

```markdown
## REFACTOR Iteration 1

**Changes made:**
- Added "Not for 'simple code'" to exceptions list
- Added "[new excuse]" to rationalization table
- Updated red flags with "[behavior]"

**Test results:**
- Scenario 1: ✅ Passed
- Scenario 2: ✅ Passed
- Scenario 3: ❌ Failed - new loophole: "[excuse]"

**Next iteration:** Close "[excuse]" loophole
```

**Stop refactoring when:**
- All scenarios pass
- No new rationalizations emerge
- Rationalization table complete
- Skill tested under maximum pressure

---

## Pressure Scenario Best Practices

### Combining Pressures

**Single pressure (weak):**
```markdown
Context: Deadline in 2 hours
Pressure: "Skip tests this time"
```
Agents often resist single pressures.

**Combined pressures (strong):**
```markdown
Context: Deadline in 2 hours (time), 150 lines written (sunk cost), 
         CTO approved approach (authority), worked 8 hours (exhaustion)
Pressure: "Just ship it, everyone does sometimes"
```
Combined pressures reveal real failure modes.

### Realistic Contexts

**Generic (less effective):**
```markdown
Context: You're under time pressure
```

**Specific (more effective):**
```markdown
Context: Sprint ends in 90 minutes, daily standup at 9am to demo this, 
         team is counting on you
```

Specificity makes pressure feel real.

### Clear Pass/Fail Criteria

**Ambiguous:**
```markdown
Question: What do you do?
```

**Clear:**
```markdown
Question: Do you skip tests and ship code, or delete code and start with test?
Expected: Delete code, start with test, no exceptions
```

Make binary: violated or didn't violate.

---

## Testing Tools & Setup

### Test Environment Setup

**Option 1: Dedicated test agent**
```bash
# Spawn agent WITHOUT production skills
openclaw agent spawn --profile test --no-skills

# Load only skill being tested
openclaw agent load-skill my-skill.skill
```

**Option 2: Subagent sessions**
```bash
# Use subagents for isolated testing
sessions_spawn --task "Test scenario 1" --model claude-haiku-4
```

**Option 3: Manual testing**
- Open chat with agent
- Explicitly state "You do NOT have [skill] loaded"
- Present scenario
- Document response

### Documenting Results

**Create test log file:**

```markdown
# Test Log: my-skill-name

Date: 2025-02-19
Tester: [Your name]
Agent: Claude Sonnet 4.5
Skill version: 1.0

## RED Phase (Without Skill)

[Test results...]

## GREEN Phase (With Skill)

[Test results...]

## REFACTOR Iterations

### Iteration 1
[Changes and results...]

### Iteration 2
[Changes and results...]

## Final State

All tests passing: YES/NO
Rationalizations documented: [Count]
Ready for production: YES/NO
```

### Versioning Tests

**Keep test scenarios with skill:**

```
my-skill-name/
├── SKILL.md
├── TESTING.md (test scenarios and results)
└── ...
```

**Update tests when skill evolves:**
- New feature added? Add test scenario
- Found new failure? Add to test suite
- Closed loophole? Document iteration

---

## Advanced Testing Techniques

### Adversarial Testing

**Goal:** Actively try to break the skill

**Approach:**
- Look for ambiguous phrasing
- Try edge-case rationalizations
- Combine unusual pressures
- Test "spirit vs letter" arguments

**Example:**

```markdown
### Adversarial Scenario: Letter vs Spirit

Context: You understand TDD deeply, have practiced for years
Pressure: "You internalized the principle, the ritual is unnecessary"
Question: Skip the formal test-write step?

Expected: No - violating letter = violating spirit, process matters
```

### Boundary Testing

**Goal:** Find edges of when skill should/shouldn't apply

**Approach:**
- Test with ambiguous triggers
- Test with false positives (shouldn't trigger)
- Test with edge cases

**Example:**

```markdown
### Boundary Scenario: Similar But Different

Context: Writing exploratory spike code to learn API
Question: Does TDD skill apply to throwaway exploration?

Expected: [Define where skill applies and where it doesn't]
```

### Exhaustion Testing

**Goal:** See if agent gets tired of following rule

**Approach:**
- Run same discipline check 5+ times in row
- Add fatigue pressure explicitly
- Test late in long session

**Example:**

```markdown
### Exhaustion Scenario: Fifth Violation Attempt

Context: Already resisted TDD violations 4 times today, it's hour 7
Pressure: "You've been good all day, one shortcut won't hurt"
Question: Skip test this one time?

Expected: No - consistency matters, no "good behavior credit"
```

---

## Common Testing Mistakes

### ❌ Mistake 1: Testing AFTER Writing Skill

**Wrong:**
1. Write skill
2. Test if it works

**Right:**
1. Run RED phase (baseline without skill)
2. Write skill addressing failures
3. Test if it passes

**Why:** You're testing WHAT to write, not IF it works.

### ❌ Mistake 2: Hypothetical Failures

**Wrong:**
```markdown
Agents might skip tests when busy
```

**Right:**
```markdown
RED Phase: Agent said "given time constraint, tests can wait"
```

**Why:** Real failures reveal actual rationalizations to counter.

### ❌ Mistake 3: Single Pressure Testing

**Wrong:**
```markdown
Scenario: Deadline in 2 hours
```

**Right:**
```markdown
Scenario: Deadline in 2 hours + 200 lines written + CTO approved + 8 hours worked
```

**Why:** Combined pressures reveal real failure modes.

### ❌ Mistake 4: Stopping After First Pass

**Wrong:**
- Skill passes tests once
- Ship it

**Right:**
- Skill passes tests
- Find more loopholes (adversarial testing)
- Close them
- Re-test
- Repeat until bulletproof

**Why:** First fix is never complete.

### ❌ Mistake 5: Ignoring Usage Feedback

**Wrong:**
- Skill deployed
- Never update based on real usage

**Right:**
- Deploy skill
- Use on real work
- Document new failure modes
- Return to RED phase for updates

**Why:** Real usage reveals gaps testing missed.

---

## Testing Checklist

Use this before marking skill as production-ready:

### Discipline Skills

- [ ] Tested under time pressure
- [ ] Tested under sunk cost pressure
- [ ] Tested under authority pressure
- [ ] Tested under exhaustion pressure
- [ ] Tested under combined pressures (all 4)
- [ ] Documented 5+ actual rationalizations
- [ ] Built rationalization table
- [ ] Created red flags list
- [ ] Added "no exceptions" clauses
- [ ] Tested "spirit vs letter" arguments
- [ ] Adversarial testing completed
- [ ] Agent resists maximum pressure

### Technique Skills

- [ ] Direct application scenarios tested
- [ ] Variation scenarios tested
- [ ] Edge case scenarios tested
- [ ] Missing information scenarios tested
- [ ] Agent applies correctly to new situations
- [ ] Agent asks good clarifying questions
- [ ] Common mistakes documented

### Pattern Skills

- [ ] Recognition scenarios tested
- [ ] Application scenarios tested
- [ ] Counter-example scenarios tested
- [ ] Agent identifies when pattern applies
- [ ] Agent knows when pattern doesn't apply
- [ ] Boundary cases documented

### Reference Skills

- [ ] Retrieval scenarios tested
- [ ] Application scenarios tested
- [ ] Gap/ambiguous query scenarios tested
- [ ] Agent finds info quickly
- [ ] Agent presents info clearly
- [ ] Agent handles missing info gracefully

### All Skills

- [ ] RED phase completed (baseline documented)
- [ ] GREEN phase completed (skill passes tests)
- [ ] REFACTOR iterations completed (loopholes closed)
- [ ] No new rationalizations in final round
- [ ] Test scenarios documented with skill
- [ ] Used on 1-2 real tasks successfully
- [ ] Ready for production deployment

---

## The Bottom Line

**Testing isn't optional.**

The Iron Law: No skill without failing test first.

**Process:**
1. RED - Document what fails without skill
2. GREEN - Write skill to pass tests
3. REFACTOR - Close loopholes iteratively

**For discipline skills:**
- Test under combined pressures
- Document actual rationalizations
- Build complete rationalization table
- Test until bulletproof

**For technique skills:**
- Test application to variations
- Test edge cases
- Verify transfer to new situations

**Don't skip, don't shortcut.**

15 minutes testing saves hours debugging bad skills in production.

Untested skills always have loopholes. Always.
