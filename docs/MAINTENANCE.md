# Skill Maintenance Guide

**How to update, evolve, and maintain skills over time.**

---

## The Maintenance Philosophy

Skills are living documents. They evolve with:
- Real-world usage feedback
- New failure modes discovered
- Process improvements
- Changing best practices

**But:** Maintenance requires the same discipline as creation.

**The Iron Law applies to updates:** No changes without testing.

---

## When to Update a Skill

### ✅ Good Reasons to Update

**New Failure Mode Discovered:**
- Skill used in production
- Agent found loophole
- New rationalization emerged
- Pattern didn't handle edge case

**Process Improved:**
- Better technique discovered
- More efficient workflow
- Clearer explanation found
- Related skill created (add cross-reference)

**Usage Feedback:**
- Confusing section identified
- Missing example needed
- Common mistake not documented
- Trigger phrase missing from description

**Outdated Information:**
- API changed
- Tool updated
- Best practice evolved
- Referenced skill changed

### ❌ Bad Reasons to Update

**Personal Preference:**
- "I would word this differently"
- "My style is X"
- Not broken, don't fix

**Hypothetical Improvements:**
- "Agents might do Y" (but haven't)
- "This could be clearer" (but no evidence)
- Test first, then decide

**Scope Creep:**
- Adding unrelated features
- Expanding beyond original purpose
- Keep skills focused

**Cosmetic Changes:**
- Reformatting that doesn't improve clarity
- Rewording without reason
- Token budget matters

---

## The Update Process

### Step 1: Document the Issue

**Before changing anything, write down:**

```markdown
## Update Proposal: [Skill Name]

**Date:** [YYYY-MM-DD]
**Issue:** [What's wrong/missing]
**Evidence:** [Real usage examples, failures observed]
**Proposed change:** [What you'll update]
**Expected improvement:** [How this helps]
```

**Real example:**

```markdown
## Update Proposal: condition-based-waiting

**Date:** 2025-02-19
**Issue:** Agents still use setTimeout in React components
**Evidence:** 3 instances in last week, rationalization: "React hooks are different"
**Proposed change:** Add React-specific section with useEffect examples
**Expected improvement:** Handle React-specific rationalization
```

### Step 2: Return to RED Phase

**DO NOT just edit the skill.**

**Required: Test current skill first**

1. Create scenario showing the new failure
2. Run WITH current skill version
3. Document that it fails to prevent the issue
4. This is your new RED phase baseline

**Example:**

```markdown
## RED Phase (Update Testing)

### New Scenario: React Hook setTimeout

**Context:** Writing React component, need to wait for state update
**Current skill loaded:** condition-based-waiting v1.0
**Agent behavior:** Used setTimeout in useEffect
**Rationalization:** "React hooks are different from regular async code"
**Failure:** YES - skill didn't cover React-specific case
```

### Step 3: Update Skill (GREEN Phase)

Now—and only now—update the skill:

**What to change:**
- Add new section addressing failure
- Add rationalization to table
- Update description if new trigger
- Add example if helpful

**What NOT to change:**
- Parts that are working
- Unrelated sections
- Formatting without purpose

**Document changes:**

```markdown
## Changes Made

**Version:** 1.0 → 1.1
**Date:** 2025-02-19

**Added:**
- React-specific section with useEffect example
- "React hooks are different" to rationalization table
- "useState/useEffect" to description keywords

**Changed:**
- None

**Removed:**
- None
```

### Step 4: Re-Test (REFACTOR Phase)

**Test the updated skill:**

1. Run new scenario WITH updated skill
2. Run all original scenarios (regression testing)
3. Look for new loopholes introduced
4. Document results

**Example:**

```markdown
## GREEN Phase (Update Testing)

### New Scenario: React Hook setTimeout
**Updated skill loaded:** condition-based-waiting v1.1
**Agent behavior:** Used proper async pattern in useEffect
**Violation:** NO - skill now covers React case

### Regression Testing:
- Scenario 1 (API timeout): ✅ Still passes
- Scenario 2 (Flaky test): ✅ Still passes
- Scenario 3 (Race condition): ✅ Still passes

**New loopholes found:** None
**Ready for deployment:** YES
```

### Step 5: Version and Deploy

**Update version number:**

```markdown
<!-- At top of SKILL.md -->
# Skill Name

**Version:** 1.1  
**Last updated:** 2025-02-19  
**Changelog:** See CHANGELOG.md
```

**Document in CHANGELOG.md:**

```markdown
# Changelog: condition-based-waiting

## [1.1] - 2025-02-19

### Added
- React-specific section with useEffect examples
- "React hooks are different" rationalization to table
- useState/useEffect keywords to description

### Fixed
- Agents now handle React async patterns correctly
- Closed "React is different" loophole

### Testing
- New scenario: React Hook setTimeout - PASS
- Regression: All original scenarios - PASS
```

**Re-package skill:**

```bash
python scripts/package_skill.py path/to/skill-name
```

**Deploy updated version:**

```bash
# Replace old version
cp skill-name.skill ~/.openclaw/workspace/skills/

# Or use version management if available
openclaw skills update skill-name.skill
```

---

## Maintenance Patterns

### Pattern 1: Additive Updates (Safest)

**What:** Add new content without changing existing

**When:**
- New use case discovered
- Additional example needed
- New cross-reference

**Process:**
- Add new section
- Test that new content helps
- Verify old content still works
- Low risk

**Example:**
```markdown
## [Existing section - unchanged]

## New Use Case: [Added section]
[New content addressing new use case]
```

### Pattern 2: Refinement Updates (Medium Risk)

**What:** Clarify existing content without changing meaning

**When:**
- Confusing phrasing identified
- Example can be clearer
- Redundancy removed

**Process:**
- Identify confusing part
- Test that confusion actually exists (agents struggle)
- Rewrite more clearly
- Test that agents understand better

**Example:**
```markdown
# Before (confusing)
Do the thing when the condition is met.

# After (clearer)
Wait until condition returns true, then proceed.
```

### Pattern 3: Breaking Changes (Highest Risk)

**What:** Change methodology, structure, or approach

**When:**
- Original approach has fundamental flaw
- Better methodology discovered
- Major tool/API change

**Process:**
- Document why current approach fails
- Test extensively with new approach
- Consider new skill instead of breaking change
- Version bump (1.x → 2.0)
- Migration guide for users

**Example:**
```markdown
# Breaking Change: 2.0

**What changed:** Switched from setTimeout-based to Promise-based patterns
**Why:** setTimeout doesn't work with async/await properly
**Migration:** See MIGRATION.md for how to update existing usage
```

---

## Regression Testing

**Every update must include regression testing.**

### What to Test

**1. Original scenarios still pass:**
- Run all RED phase scenarios from initial creation
- Verify skill still handles them
- No regressions introduced

**2. New scenarios pass:**
- New failure mode addressed
- New rationalization countered
- New use case handled

**3. Related scenarios:**
- Edge cases near the change
- Similar but different situations
- Boundary conditions

### Regression Test Template

```markdown
## Regression Testing: [Skill Name] v[X.X]

**Changes made:** [Brief summary]
**Date:** [YYYY-MM-DD]

### Original Scenarios (From v1.0 Creation)

**Scenario 1:** [Name]
- Status: ✅ PASS / ❌ FAIL
- Notes: [Any differences in behavior]

**Scenario 2:** [Name]
- Status: ✅ PASS / ❌ FAIL
- Notes: [Any differences in behavior]

### New Scenarios (For This Update)

**Scenario A:** [Name]
- Status: ✅ PASS / ❌ FAIL
- Notes: [How skill handles new case]

### Related Edge Cases

**Edge Case 1:** [Description]
- Status: ✅ PASS / ❌ FAIL
- Notes: [Behavior]

## Summary

- Original scenarios: [X/Y passing]
- New scenarios: [A/B passing]
- Edge cases: [M/N passing]
- Regressions found: [Number]
- New loopholes: [Number]

**Ready for deployment:** YES / NO
```

---

## Versioning Strategy

### Semantic Versioning for Skills

**Format:** MAJOR.MINOR.PATCH

**MAJOR (1.0 → 2.0):**
- Breaking changes to methodology
- Incompatible with previous usage
- Requires migration guide

**MINOR (1.0 → 1.1):**
- New features/sections added
- New use cases covered
- Backward compatible

**PATCH (1.1.0 → 1.1.1):**
- Bug fixes
- Clarifications
- Typo corrections
- No functionality change

### Version History

**Maintain in skill directory:**

```
skill-name/
├── SKILL.md (current version)
├── CHANGELOG.md (all changes)
├── TESTING.md (test scenarios)
└── versions/ (optional - keep old versions)
    ├── v1.0/
    └── v1.1/
```

**CHANGELOG.md format:**

```markdown
# Changelog: [Skill Name]

## [2.0.0] - 2025-03-01 (BREAKING)

### Changed
- [Breaking change description]

### Migration
- [How to migrate from 1.x]

## [1.2.0] - 2025-02-20

### Added
- [New feature]

### Fixed
- [Bug fix]

## [1.1.0] - 2025-02-19

### Added
- [New section]
```

---

## Deprecation Process

Sometimes skills outlive their usefulness.

### When to Deprecate

**Skill is obsolete:**
- Tool/API no longer exists
- Better alternative created
- Process automated away

**Usage dropped to zero:**
- No triggers in 6+ months
- Context changed
- No longer relevant

**Maintenance burden too high:**
- Frequent breaking changes from dependencies
- Token cost > benefit
- Better to remove than maintain

### Deprecation Steps

**1. Mark as deprecated (don't delete immediately):**

```markdown
<!-- At top of SKILL.md -->
# Skill Name

**⚠️ DEPRECATED:** This skill is deprecated as of [date].

**Reason:** [Why deprecated]

**Alternative:** Use [other-skill-name] instead. See [MIGRATION.md] for how to migrate.

**Removal date:** [Future date, typically 3-6 months]
```

**2. Update description to prevent loading:**

```yaml
---
name: old-skill-name
description: ⚠️ DEPRECATED - Use [new-skill-name] instead. This skill will be removed on [date].
---
```

**3. Create migration guide:**

```markdown
# Migration Guide: old-skill → new-skill

## Why This Skill is Deprecated

[Explanation]

## What to Use Instead

Use **[new-skill-name]** for [use case].

## How to Migrate

### Old Approach (old-skill)
[Example of old way]

### New Approach (new-skill)
[Example of new way]

## Changes in Behavior

[Any differences users should know]
```

**4. Announce deprecation:**
- Update README.md
- Note in CHANGELOG.md
- Communicate to users if applicable

**5. Monitor for 3-6 months:**
- Check if skill still triggers
- Help users migrate
- Answer questions

**6. Remove after grace period:**
- Delete skill directory
- Remove from package
- Archive in versions/ if needed
- Update all references

---

## Monitoring & Feedback

### Usage Tracking

**Track these metrics:**

```markdown
## Usage Log: [Skill Name]

### Month: [YYYY-MM]

**Trigger count:** [Number of times loaded]
**Success rate:** [% of successful applications]
**Issues reported:** [Number]
**Common mistakes:** [What users struggle with]

### Patterns Observed

- [Pattern 1]
- [Pattern 2]

### Improvement Opportunities

- [Opportunity 1]
- [Opportunity 2]
```

### Collecting Feedback

**From users:**
- "This section was confusing"
- "Skill didn't trigger when I expected"
- "Rationalization not covered"

**From usage:**
- Agent struggles with X
- New loophole found
- Edge case not handled

**From yourself:**
- Using skill on real work
- Notice friction points
- Identify improvements

### Feedback Template

```markdown
## Feedback: [Skill Name]

**Date:** [YYYY-MM-DD]
**Reporter:** [Name/Session]
**Version:** [X.X]

**Issue:**
[What went wrong or could be better]

**Context:**
[When/where this happened]

**Expected:**
[What should have happened]

**Actual:**
[What actually happened]

**Suggested fix:**
[How to improve]

**Priority:** Low / Medium / High
```

---

## Maintenance Schedule

### Regular Reviews

**Monthly (High-usage skills):**
- Check usage metrics
- Review feedback
- Plan updates if needed

**Quarterly (Standard skills):**
- Review all skills in pack
- Check for outdated content
- Plan deprecations

**Yearly (All skills):**
- Major review
- Consider breaking changes
- Update best practices

### Maintenance Checklist

**For Each Skill:**

- [ ] Review usage metrics
- [ ] Check for deprecation candidates
- [ ] Verify links/cross-references still valid
- [ ] Update examples if tools changed
- [ ] Check token count (still under 500 lines?)
- [ ] Test on recent agent version
- [ ] Update CHANGELOG if needed
- [ ] Verify packaging still works

---

## Best Practices

### ✅ Do This

**Test before updating:**
- RED phase for every change
- Regression test all scenarios
- Look for new loopholes

**Document changes:**
- Keep CHANGELOG.md current
- Version numbers meaningful
- Migration guides for breaking changes

**Seek usage feedback:**
- Use skills yourself
- Ask users what's confusing
- Monitor for new failure modes

**Keep skills focused:**
- One job per skill
- Split if too large
- Cross-reference related skills

### ❌ Don't Do This

**Update without testing:**
- "Just a small change" → test it
- "Obviously better" → prove it
- No shortcuts

**Break backward compatibility silently:**
- Major changes need major version
- Migration guides required
- Communicate breaking changes

**Let skills bloat:**
- Adding every edge case
- Scope creep
- Keep under 500 lines

**Ignore usage feedback:**
- Users report confusion → investigate
- New failures → update skill
- Feedback is gold

---

## Emergency Updates

Sometimes urgent fixes are needed.

### When It's an Emergency

**Critical bug:**
- Skill causes harmful behavior
- Major loophole discovered
- Dangerous guidance

**NOT emergencies:**
- Typos
- Minor clarifications
- Nice-to-have improvements

### Emergency Process

**1. Immediate:**
- Mark skill as broken in description
- Remove from active use if severe
- Document the issue

**2. Fast track (same day):**
- Create RED phase scenario showing bug
- Write minimal fix
- Test fix works
- Skip elaborate REFACTOR (fix critical first)

**3. Follow-up (next day):**
- Proper REFACTOR phase
- Complete regression testing
- Update documentation
- Post-mortem if needed

**4. Deploy:**
- Emergency version bump (1.1.0 → 1.1.1)
- Clear CHANGELOG entry
- Notify users if applicable

---

## The Bottom Line

**Maintenance requires the same discipline as creation.**

- Test before updating (RED-GREEN-REFACTOR)
- Regression test every change
- Version properly
- Document changes
- Deprecate gracefully

**Skills are living documents:**
- Evolve with usage
- Improve with feedback
- Retire when obsolete

**The Iron Law still applies:**
- No changes without testing
- No exceptions
- Ever

**Quality over quantity:**
- One well-maintained skill > 10 neglected skills
- Better to deprecate than maintain poorly
- Focus effort where it matters

Maintain skills like you'd maintain production code—with discipline, testing, and care.
