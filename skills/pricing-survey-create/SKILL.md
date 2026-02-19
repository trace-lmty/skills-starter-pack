---
name: pricing-survey-create
description: Create pricing research surveys using Price Intelligently methodology. Use when the user wants to create a pricing survey, Van Westendorp survey, value metric survey, or needs to research pricing for a product. Covers Survey 1 (qualification, segmentation, relative preference, price sensitivity) and Survey 2 (value metric discovery). Based on Patrick Campbell's proven framework used with 30+ million survey responses.
---

# Pricing Survey Creation (Price Intelligently Method)

Create research-backed pricing surveys following the proven PI methodology.

## Two Survey System

### Survey 1: Primary Pricing Survey
**Purpose:** Discover willingness to pay, feature preferences, pain points
**Questions:** 15-20 questions
**Sample size:** 100-150 responses minimum per segment

### Survey 2: Value Metric Survey  
**Purpose:** Determine what customers should pay for (usage metric)
**Questions:** 8-12 questions
**Sample size:** 100-150 responses minimum per segment

## Survey 1 Structure

### 1. Qualification Questions (1-3 questions)
**Purpose:** Filter out non-target respondents

**Format:** Multiple choice, single select
**Rule:** Include "Other" option to disqualify non-targets

**Template:**
```
In which industry is your company?
○ Technology/Software
○ Marketing/Advertising  
○ Finance/Banking
○ Healthcare
○ Education
○ E-commerce/Retail
○ Professional Services
○ Other (disqualifies)
```

**Guidance:**
- Early-stage products: Be broader (1 qualification question)
- Established products: Be specific (2-3 questions)
- Always include at least ONE qualification question

### 2. Segmentation Questions (4-6 questions)
**Purpose:** Slice data by buyer persona for analysis

**Standard B2B segmentation:**
1. Job title/seniority
2. Company size (employees)
3. Decision-making authority
4. Company revenue
5. Department
6. Competing tools used

**Template questions:**

```
Which of the following most closely matches your job title?
○ Individual Contributor
○ Manager
○ Director
○ VP/SVP
○ C-Level/Executive
○ Founder/Owner
```

```
Counting all locations where your employer operates, what is the total number of employees?
○ 1 (Solo)
○ 2-10
○ 11-50
○ 51-200
○ 201-1,000
○ 1,001-5,000
○ 5,000+
```

```
What level of decision-making authority do you have on purchasing software for your organization?
○ Final decision-making authority
○ Significant input in decision
○ Some input in decision
○ No input in decision
```

```
What is your company's annual revenue?
○ $0 - $100K
○ $100K - $1M
○ $1M - $10M
○ $10M - $50M
○ $50M - $100M
○ $100M+
○ Don't know / N/A
```

```
In which department do you work?
○ Engineering/Product
○ Marketing
○ Sales
○ Operations
○ Finance
○ HR
○ Executive/Leadership
○ Other
```

```
What tools do you currently use for [relevant category]?
☐ Tool A
☐ Tool B
☐ Tool C
☐ None of the above
```

### 3. Product Description (if pre-launch)
**Purpose:** Give context before asking about value

**Include:**
- Value proposition (1-2 sentences)
- Screenshot or mockup
- Key differentiators

**Attention check:**
```
Based on what you just read, this product allows you to [core function]. True or False?
○ True
○ False (disqualifies - not paying attention)
```

### 4. Relative Preference Questions (3 questions)
**Purpose:** Force tradeoffs to discover true priorities

**Formula:** (# MOST - # LEAST) / Total Responses = Preference Score
**Interpretation:** +0.30 or higher = very strong preference

**Question format:** "Select which is MOST and LEAST important"

**Three required questions:**

**A. Value Propositions (4-6 options)**
```
Which of the following options align LEAST and MOST with the value you would derive from [Product]?

                                    LEAST    MOST
○ [Value prop 1]                     ○        ○
○ [Value prop 2]                     ○        ○
○ [Value prop 3]                     ○        ○
○ [Value prop 4]                     ○        ○
○ [Value prop 5]                     ○        ○
```

**B. Features (4-6 options)**
```
Of the following features, which is LEAST and MOST important to you?

                                    LEAST    MOST
○ [Feature 1]                        ○        ○
○ [Feature 2]                        ○        ○
○ [Feature 3]                        ○        ○
○ [Feature 4]                        ○        ○
○ [Feature 5]                        ○        ○
```

**C. Pain Points (4-6 options)**
```
Of the following options, which do you find to be the LEAST and MOST painful?

                                    LEAST    MOST
○ [Pain point 1]                     ○        ○
○ [Pain point 2]                     ○        ○
○ [Pain point 3]                     ○        ○
○ [Pain point 4]                     ○        ○
○ [Pain point 5]                     ○        ○
```

### 5. Price Sensitivity Questions (Van Westendorp)
**Purpose:** Triangulate acceptable price range

**Four required questions:**
```
At what monthly price would you consider [Product] to be so expensive that you would never consider buying it?
$[___]

At what monthly price would you consider [Product] to be getting expensive, but you would still consider buying it?
$[___]

At what monthly price would you consider [Product] to be a great deal—a bargain for what you get?
$[___]

At what monthly price would [Product] be so inexpensive that you would question the quality?
$[___]
```

**Optional excitement gauge:**
```
On a scale from 1-7, how likely are you to buy [Product] based on what you've learned?
1 ○ ○ ○ ○ ○ ○ ○ 7
Not at all likely          Extremely likely
```

---

## Survey 2: Value Metric Survey

**When to run:** After Survey 1, to determine pricing model

### Structure (Shorter than Survey 1)

**1. Abbreviated segmentation (1-3 questions)**
Use learnings from Survey 1 to pick most important segments

**2. Product reminder** (if pre-launch)
Brief description + attention check

**3. Value Metric Preference Question**
```
Which of the following pricing models would you MOST and LEAST prefer?

                                        LEAST    MOST
○ Pricing per user/month                  ○        ○
○ Pricing per [usage unit]                ○        ○
○ Pricing based on features/tiers         ○        ○
○ Pricing based on [metric 1]             ○        ○
○ Pricing based on [metric 2]             ○        ○
```

**Value metric examples:**
- Per user (Slack, Salesforce)
- Per hour/minute (Otter, transcription)
- Per video (Wistia)
- Per contact (HubSpot, Drift)
- Per storage (Dropbox)
- Per integration (Zapier)
- Per API call (Stripe)
- Per document (DocuSign)
- Per team size tier

**Include per-user as benchmark** — helps understand if customers prefer per-user or alternative metrics

**4. Price Sensitivity (per chosen metric)**
```
At what monthly price PER USER would you consider [Product] to be...
```

---

## Survey Best Practices

### Quality Control
- Strip responses where "too expensive" < "getting expensive" (invalid)
- Strip responses with $0 or absurd values ($1,000,000)
- Strip responses that fail attention check
- Strip outlier response times (<30 seconds total, or <1 second per question)

### Sample Sizes
- Quick directional check: 50-150 responses
- Solid data: 150 responses per segment
- High-stakes decision: 500+ responses

### Panel vs Customer Survey
- **Use panels when:** New market, no customers yet, exploring new segment
- **Use customers when:** Existing product, want brand-aware data
- **Use both when:** Compare brand-aware vs brand-unaware willingness to pay

---

## Implementation Checklist

Before sending Survey 1:
- [ ] Qualification question(s) defined
- [ ] 4-6 segmentation questions included
- [ ] Product description ready (if pre-launch)
- [ ] Attention check question included
- [ ] 3 relative preference questions (value, features, pain)
- [ ] 4 Van Westendorp price questions
- [ ] Total questions: 15-20

Before sending Survey 2:
- [ ] 1-3 abbreviated segmentation questions
- [ ] 4-6 value metric options defined
- [ ] Per-user included as benchmark
- [ ] Price sensitivity adjusted for value metric
- [ ] Total questions: 8-12

---

## Output Formats

When asked to create a survey, output in requested format:
- **Google Forms:** Provide question text and options
- **Typeform:** Provide structured question list
- **CSV/Excel:** Provide spreadsheet-ready format
- **JSON:** Provide structured data for programmatic use

See [references/survey-templates.md](references/survey-templates.md) for complete examples.
