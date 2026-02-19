---
name: tradeoff-framework
description: When the user needs to make a structured decision or frame a tradeoff. Use for product decisions, feature prioritization, strategic choices, or any scenario where clear decision-making frameworks help. Also use when the user mentions "decision," "tradeoff," "should we," "options," or "recommendations."
---

# Tradeoff Framework

## When to Use This Skill

Use when:
- Making product or strategic decisions
- Evaluating multiple options
- Need to frame a choice clearly
- Decision needs to be durable (not revisited constantly)
- Team needs alignment on criteria and tradeoffs
- User asks "should we..." or "which option..."

## The Problem This Solves

Decisions often get revisited or reversed because they weren't clearly framed initially. The strongest voice or most recent argument wins instead of the best reasoning. This creates thrash, direction changes, and hurts execution.

**Solution:** Better framing = more durable decisions.

## The 5-Step Framework

### 1. State the Problem Clearly

**Why:** If we're not clear on what we're solving, we'll argue in circles.

**What to write:**
- **What decision needs to be made** (1 sentence)
- **Why it matters** (tie to a metric or goal)
- **How we'll know it's solved** (success criteria)

**Example:**
```
What decision: Should newly created Stacks be public (visible to team by default), 
or private (visible only to creator unless explicitly shared)?

Why it matters: Default visibility affects virality, collaboration, and perceived utility. 
Public Stacks could drive network effects but only if content is also accessible.

We'll know it's solved: Public Stacks increase cross-user engagement, with minimal 
user confusion or access complaints, and no spike in unintended document exposure.
```

### 2. Define the Ideal Outcome

**Why:** Without clear criteria, we default to personal preferences or loud opinions.

**What to write:**
- List key attributes of a great solution
- Clarify what's **essential** vs. **nice-to-have**
- **Rank by importance**

**Example:**
```
Must-have:
- Promotes discovery and engagement across users
- Preserves trust by respecting document-level ACLs

High:
- Minimizes user confusion about what is visible and accessible

Medium:
- Simple, intuitive mental model
- Technically implementable with limited edge-case complexity
```

**Tips:**
- If criteria is ambiguous (e.g., "scalability"), rephrase clearly (e.g., "handle >1M queries/day with <500ms latency")
- Directional > perfect

### 3. List the Options

**Why:** You can't make a tradeoff with one idea.

**What to write:**
- Capture **2-5 viable options**
- **Name them clearly** so people can reference them in discussion

**Example:**
```
1. Default Private – Stack and contents are private unless explicitly shared
2. Default Public to Team – Stack is public; content access relies on existing doc ACLs
3. Prompt at Creation – User decides Stack visibility at creation and is guided to update ACLs
4. Hybrid Visibility Mode – Stack is public; only shows content already accessible to viewer
5. View-Only Override – Stack is public; all added content is made "view-only" to team/workspace
```

### 4. Score the Options

**Why:** This is where decisions go from gut feel to structured judgment.

**What to do:**
- Create a **matrix**: options on one axis, attributes (from step 2) on the other
- Score each cell: **High / Medium / Low** (or actual data if available)
- Explain edge cases if needed

**Example output:**

| Criteria | 1. Private | 2. Public | 3. Prompt | 4. Hybrid | 5. Override |
|----------|-----------|-----------|-----------|-----------|-------------|
| Promotes discovery | Low | High | Medium | High | High |
| Preserves trust/ACLs | Yes | Risk | Yes | Yes | With consent |
| Minimizes confusion | High | Medium | Low | Medium | Medium |
| Simple mental model | Easiest | Simple | Complex | Medium | Medium |
| Implementation complexity | Low | Low | Medium | High | Medium |

**Key insight:** You're not looking for math. You're looking for insight. Use the matrix to spot where tradeoffs bite.

### 5. Make a Recommendation

**Why:** No one wants to debate from scratch. Start from a point of view.

**What to write:**
- Say **what you recommend and why**
- **Acknowledge what we give up**
- Identify **where the risks are**

**Example:**
```
Recommendation: Option 5 – View-Only Override

Why: Balances discovery (high) with trust (consent-based). More viral than Private, 
less risky than fully Public. Medium implementation complexity but clear mental model.

What we give up: Some flexibility in per-document permissions (all become view-only by default).

Risks: Users may not understand why their edit permissions change when adding to Stack. 
Need clear messaging at add-time.
```

## Output Format

When applying this framework, structure your response like this:

```markdown
## Decision: [Title of Decision]

### 1. Problem Statement
- **What:** [Decision to be made]
- **Why:** [Why it matters]
- **Success:** [How we'll know it's solved]

### 2. Ideal Outcome
**Must-have:**
- [Criterion 1]
- [Criterion 2]

**High:**
- [Criterion 3]

**Medium:**
- [Criterion 4]

### 3. Options
1. **[Option Name]** – [Brief description]
2. **[Option Name]** – [Brief description]
3. **[Option Name]** – [Brief description]

### 4. Scoring Matrix
[Table as shown above]

**Key Tradeoffs:**
- [Insight 1]
- [Insight 2]

### 5. Recommendation
**Recommend:** [Option Name]

**Why:** [Reasoning]

**What we give up:** [Tradeoffs]

**Risks:** [Known risks]
```

## Tips for Better Framing

1. **Steel man the alternatives** - Present each option in its best light before scoring
2. **Make criteria measurable** - "Fast" is vague; "<2s load time" is clear
3. **Rank criteria** - Not everything is equally important
4. **Expose assumptions** - Write them down so they can be challenged
5. **Time-box** - This is for decisions, not research projects
6. **Update if learning** - If new info changes the frame, update it

## Anti-Patterns to Avoid

- ❌ Listing options without clear criteria → debate becomes subjective
- ❌ Too many criteria (>7) → analysis paralysis
- ❌ Hiding your recommendation → forces others to guess your thinking
- ❌ Perfect scoring → you're looking for insights, not precision
- ❌ Skipping "what we give up" → decision will be revisited when downsides appear

## When NOT to Use This

- User just needs information (not a decision)
- Decision is trivial (don't over-engineer simple choices)
- Only one viable option exists (no tradeoff to frame)
- Decision is purely emotional/personal (framework won't help)

---

**Source:** Hiten Shah's decision-making framework (extracted 2026-02-05)
