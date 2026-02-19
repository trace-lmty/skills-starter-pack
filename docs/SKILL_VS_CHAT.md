# When to Create a Skill vs Just Use Chat

**The decision framework for skill creation.**

## TL;DR

✅ **Create a skill when:**
- You'll do this repeatedly (3+ times)
- Pattern isn't intuitively obvious
- Others would benefit
- Process needs discipline/consistency

❌ **Just use chat when:**
- One-off task
- Standard practice well-documented
- Project-specific convention
- Mechanical constraint (automate instead)

---

## The Decision Tree

### Question 1: Will this be repeated?

**One-time task?** → Chat
- "Analyze this specific dataset"
- "Help debug this particular bug"
- "Research this competitor once"

**Regular task (3+ times)?** → Consider skill
- "Analyze customer interview transcripts" (ongoing)
- "Debug race conditions" (recurring pattern)
- "Research competitors" (continuous activity)

### Question 2: Is the pattern intuitively obvious?

**Obvious to you and others?** → Chat
- "Search the web for X"
- "Summarize this article"
- "Translate to Spanish"

**Non-obvious, requires specific knowledge?** → Skill
- "Conduct JTBD switch interviews"
- "Apply Van Westendorp pricing methodology"
- "Structure outcome statements correctly"

### Question 3: Does it need discipline/consistency?

**Flexible, context-dependent?** → Chat
- "Help me brainstorm ideas"
- "Explain this concept"
- "Draft an email"

**Needs consistent application?** → Skill
- "Follow TDD strictly" (discipline)
- "Always use condition-based waiting" (consistency)
- "Structure all JTBD outcomes the same way" (standard)

### Question 4: Is it broadly applicable?

**Project-specific or personal preference?** → AGENTS.md or chat
- "In OUR codebase, use kebab-case"
- "I prefer Oxford commas"
- "This project's git workflow"

**Applies across projects/teams?** → Skill
- "How to conduct customer interviews"
- "Framework for making tradeoff decisions"
- "Method for positioning products"

---

## The Cost-Benefit Analysis

### Costs of Creating a Skill

**Time:**
- Initial creation: 2-16 hours (depending on type)
- Testing: Must follow RED-GREEN-REFACTOR
- Iteration: Updates based on usage
- Maintenance: Keep relevant as process evolves

**Cognitive:**
- Learning the methodology
- Following TDD discipline
- Testing under pressure
- Closing loopholes

**Token:**
- Description always loaded (~100 tokens)
- SKILL.md loaded when triggered (200-500 lines typical)
- Must justify token cost with value

### Benefits of Creating a Skill

**Consistency:**
- Same process every time
- Bulletproof against rationalization
- No "I'll just skip this once"

**Leverage:**
- Write once, use forever
- Others can use it
- Compounds with other skills

**Quality:**
- Tested failure modes addressed
- Best practices encoded
- Continuous improvement through iteration

**Knowledge Transfer:**
- Onboards new agents instantly
- Codifies institutional knowledge
- Makes tacit knowledge explicit

---

## Real-World Examples

### ✅ Good Skill Candidates

**Research Methodology:**
- Why: Used repeatedly, non-obvious structure, needs consistency
- Example: continuous-discovery-interview skill
- Alternative: Explain interview technique every time

**Strategic Framework:**
- Why: Applies broadly, has specific steps, common mistakes
- Example: positioning skill
- Alternative: Re-research positioning framework each time

**Discipline Enforcement:**
- Why: Needs strict consistency, easy to rationalize away
- Example: TDD skill
- Alternative: Try to remember TDD under pressure (doesn't work)

**Technical Pattern:**
- Why: Recurring problem, specific solution, others face it too
- Example: condition-based-waiting skill
- Alternative: Debug race conditions repeatedly

### ❌ Bad Skill Candidates

**One-Off Analysis:**
- Why: Not repeated
- Wrong: "Analyze Q4 2024 survey results" skill
- Right: Just ask agent to analyze in chat

**Well-Documented Standard:**
- Why: Agent already knows this
- Wrong: "How to use git" skill
- Right: Chat (agent knows git)

**Project-Specific Convention:**
- Why: Not broadly applicable
- Wrong: "Our team's code review checklist" skill
- Right: Put in AGENTS.md or project docs

**Mechanical Constraint:**
- Why: Should be automated, not documented
- Wrong: "Filename must match regex [a-z0-9-]" skill
- Right: Script that validates/renames files

---

## The Gray Zone

### Could Go Either Way

**Company-Specific Process:**

Example: "Our customer onboarding workflow"

**Skill if:**
- Complex multi-step process
- Used by multiple teams
- Needs strict consistency
- Non-obvious best practices

**Chat/AGENTS.md if:**
- Simple reference
- Only your team uses it
- Frequently changes
- Mostly links/pointers

**Domain Knowledge:**

Example: "Healthcare compliance requirements"

**Skill if:**
- Reference document with structure
- Agents need to actively use knowledge
- Process for applying knowledge
- Used across multiple projects

**Chat if:**
- Quick fact-checking
- Rarely needed
- Simpler to search web
- No special process

**Tool Wrapper:**

Example: "Using internal API"

**Skill if:**
- API is complex/non-standard
- Common mistakes to avoid
- Authentication/setup nuances
- Example workflows help

**Chat if:**
- API is self-documenting
- Straightforward REST/GraphQL
- Just need endpoint URLs
- Better to read API docs directly

---

## The Skill Creation Threshold

**Create a skill when ALL are true:**

1. ✅ Used 3+ times OR will be used by others
2. ✅ Non-obvious or has non-intuitive nuances
3. ✅ Broadly applicable (not project-specific)
4. ✅ Would benefit from testing/consistency
5. ✅ Justifies token cost (loaded selectively)

**If ANY are false, reconsider.**

---

## Alternative: Start with Chat, Graduate to Skill

**Progressive approach:**

### Phase 1: Chat (First Time)
- Use chat to figure out the process
- Document what works in AGENTS.md
- Note difficulties/surprises

### Phase 2: Chat with Notes (Times 2-3)
- Reference your AGENTS.md notes
- Refine the process
- Document more nuances

### Phase 3: Create Skill (Time 4+)
- Now you know what works
- You have real failure modes documented
- You understand non-obvious parts
- Worth the investment

**Benefits:**
- Don't over-invest upfront
- Learn what actually matters
- Real RED phase data (actual failures)
- Better skill from experience

---

## When to STOP Using a Skill

Skills can outlive their usefulness:

### Retire a skill when:

**Pattern is now intuitively obvious:**
- Everyone does it naturally
- No longer needed as guardrail
- Internalized the practice

**Better alternative exists:**
- Tool/automation replaces process
- New methodology is superior
- Skill's approach is outdated

**Usage dropped to zero:**
- No one triggered it in 6+ months
- Process no longer relevant
- Context changed

**Token cost > benefit:**
- Skill is too large (>1000 lines)
- Triggered too often (always loaded)
- Information available elsewhere

**How to retire:**
1. Archive skill (don't delete immediately)
2. Document why retired (CHANGELOG)
3. Point to replacement (if any)
4. Monitor for 1-2 months
5. Delete if truly unused

---

## The AGENTS.md Alternative

**Some things belong in AGENTS.md, not skills:**

### Put in AGENTS.md when:

**Personal/Team Preferences:**
- "I prefer X style"
- "Our team uses Y convention"
- "Default to Z approach"

**Environment-Specific:**
- "Camera names and locations"
- "SSH hosts and aliases"
- "File paths and directories"

**Meta-Context:**
- "How to work with me"
- "Decision-making norms"
- "Communication preferences"

**Quick References:**
- "Commands I use frequently"
- "Links to resources"
- "Project structure notes"

### Put in Skill when:

**Teachable Process:**
- Multi-step methodology
- Has failure modes
- Needs testing/validation

**Broadly Applicable:**
- Works for any team/project
- Not personal preference
- Others would benefit

**Requires Consistency:**
- Easy to do wrong
- Needs discipline
- Has quality bar

---

## Decision Flowchart

```
Start: Should I create a skill?
↓
Will I do this 3+ times? 
  NO → Use chat
  YES ↓
  
Is the pattern intuitively obvious?
  YES → Use chat (or AGENTS.md if preference)
  NO ↓
  
Is it broadly applicable (not project-specific)?
  NO → Put in AGENTS.md
  YES ↓
  
Does it need discipline/consistency?
  NO → Try chat first, graduate later if needed
  YES ↓
  
Justifies token cost?
  NO → Simplify or use chat
  YES ↓
  
✅ CREATE SKILL
```

---

## Examples from This Pack

### Why These Are Skills

**positioning** - Not obvious how to position, used repeatedly, benefits from structure
**jtbd-research** - Complex methodology, many failure modes, needs consistency
**skill-creator-pro** - Enforces discipline (TDD), easy to rationalize away
**outcome-discovery** - Specific syntax non-obvious, used across many projects
**continuous-discovery-interview** - Easy to ask leading questions, needs practice

### What's NOT a Skill (Correctly)

**"Analyze this competitor"** - One-off, use chat
**"Our brand voice"** - Team-specific, belongs in AGENTS.md
**"Git workflow"** - Standard practice, agent knows it
**"File naming rules"** - Mechanical, automate with script

---

## The Bottom Line

**Default to chat.**

Skills are an investment. Only invest when:
- Pattern repeats
- Benefit > cost
- Broadly applicable
- Non-obvious or needs discipline

**Start simple, graduate to skill when justified.**

Most things should stay in chat. Skills are for the patterns that matter enough to codify and maintain.

Ask yourself: "Is this process worth 2-16 hours to document and test?"

If yes → Create skill with TDD
If no → Use chat, maybe add to AGENTS.md
If unsure → Use chat 3 times first, then decide
