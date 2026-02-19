# Skills Starter Pack

**The complete package for building world-class AI agent skills.**

## What's Inside

This package contains everything you need to create production-ready skills for AI agents:

1. **skill-creator-pro** - The meta skill for creating and testing skills using TDD methodology
2. **13 exceptional example skills** - Production-tested, representing diverse use cases and patterns
3. **Design pattern documentation** - What makes these skills 11/10 and how to replicate it
4. **Creation playbook** - Step-by-step guides for different skill types
5. **Maintenance guides** - How to update, test, and evolve skills over time
6. **Real-world case studies** - How these skills delivered results in production

## Quick Start

### 1. Install skill-creator-pro

The meta skill that teaches agents how to create other skills:

```bash
# Copy to your skills directory
cp -r skills/skill-creator-pro ~/.openclaw/workspace/skills/

# Or install via OpenClaw
openclaw skills install skills/skill-creator-pro.skill
```

### 2. Explore Example Skills

Browse the `skills/` directory to see 11 production-ready examples covering:
- Research & discovery (JTBD, customer interviews, opportunity mapping)
- Strategic frameworks (positioning, pricing, product strategy)
- Execution (copywriting, copy editing, storytelling)
- Meta-skills (tradeoffs, customer empathy, creative intelligence)

### 3. Learn the Patterns

Read `docs/DESIGN_PATTERNS.md` to understand what makes these skills exceptional and how to apply the same patterns to your own skills.

## What Makes These Skills 11/10

Every skill in this pack meets these criteria:

✅ **Test-driven** - Created using RED-GREEN-REFACTOR methodology  
✅ **Bulletproof** - Resists rationalization under pressure  
✅ **Token-efficient** - Progressive disclosure, under 500 lines  
✅ **Production-ready** - Used in real work, battle-tested  
✅ **Well-documented** - Clear triggers, examples, common mistakes  
✅ **Discoverable** - Rich descriptions with specific keywords  

## Skills Included (13 Total)

### Meta & Strategy (4 skills)
- **skill-creator-pro** - TDD methodology for creating skills
- **positioning** - April Dunford's positioning framework
- **tradeoff-framework** - Structured decision-making
- **customer-empathy** - Bernadette Jiwa's empathy-first marketing

### Research & Discovery (5 skills)
- **jtbd-research** - Jobs to be Done core methodology
- **outcome-discovery** - Uncover desired outcome statements
- **job-mapping** - Universal Job Map framework
- **continuous-discovery-interview** - Teresa Torres interview technique
- **opportunity-solution-tree** - OST creation and maintenance

### Product & Validation (2 skills)
- **product-discovery-inspired** - INSPIRED validation methodology
- **pricing-survey-create** - Patrick Campbell pricing research

### Execution & Communication (2 skills)
- **copywriting** - Marketing copy creation framework
- **storytelling-science** - Brain science narrative framework

## Documentation (115KB Total)

### For Skill Creators (72KB)
- **docs/DESIGN_PATTERNS.md** (24.5KB) - What makes skills 11/10
- **docs/CREATION_PLAYBOOK.md** (19.5KB) - Step-by-step skill creation
- **docs/TESTING_GUIDE.md** (14.9KB) - How to test skills under pressure
- **docs/MAINTENANCE.md** (15KB) - Updating and evolving skills

### For Skill Users (42KB)
- **docs/SKILL_SELECTION.md** (13KB) - When to use which skill
- **docs/SKILL_VS_CHAT.md** (9.6KB) - When to create a skill vs use chat
- **SKILLS_CATALOG.md** (14.9KB) - Complete skill index with metadata
- **examples/REAL_WORLD_USAGE.md** (19.3KB) - Production case studies with metrics

## Philosophy

**Skills are TDD for process documentation.**

The same discipline that makes code bulletproof makes documentation bulletproof:

1. **RED**: Run scenarios WITHOUT skill, document failures
2. **GREEN**: Write skill addressing those failures
3. **REFACTOR**: Close loopholes, make bulletproof

This pack teaches that methodology and demonstrates it through exceptional examples.

## Use Cases

**For Companies:**
- Codify institutional knowledge
- Standardize processes across teams
- Onboard AI agents to company-specific workflows
- Build competitive moats through specialized capabilities

**For Developers:**
- Extend AI capabilities systematically
- Create reusable process libraries
- Ship deterministic behavior in AI systems
- Build skills marketplace offerings

**For Researchers:**
- Document research methodologies
- Make tacit knowledge explicit
- Enable reproducible research workflows
- Share domain expertise programmatically

## What's Different About This Pack

**Not just templates** - These are production-tested skills used in real work

**Not just documentation** - Complete TDD methodology for creating skills

**Not just examples** - Design patterns extracted from 70+ skills

**Not just theory** - Battle-tested approaches that resist rationalization

## Support & Community

- **Documentation**: Full guides in `docs/`
- **Examples**: Real-world usage in `examples/`
- **Updates**: Track improvements in `CHANGELOG.md`

## License

[TBD - Specify license for the skills and documentation]

---

**Built by the tinylab team**  
Creating reliable units of agent labor, one skill at a time.
