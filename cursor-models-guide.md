# Cursor Models Explained

## What Each Model Is

### Auto

Cursor automatically selects the model based on task complexity.\
**Best for:** Most users, day-to-day coding, mixed workloads.\
**Trade-off:** Less manual control, usually optimal selection.

------------------------------------------------------------------------

### MAX Mode

Not a model, but a behavior switch.\
Enables: - Larger context windows - Deeper repository analysis - More
aggressive multi-step reasoning

**Consumes significantly more tokens**\
**Best for:** Large refactors, migrations, architecture changes.

------------------------------------------------------------------------

### Composer 1.5

Cursor-optimized code composition model.\
Very fast and efficient for: - Editing files - Generating components -
Small refactors

**Lower token usage**\
**Best for:** UI components, CRUD code, Tailwind/React boilerplate.

------------------------------------------------------------------------

### Sonnet 4.5

Balanced reasoning + coding model.\
Strong contextual understanding.

**Moderate token usage**\
**Best for:** - Feature development - Debugging - Backend + frontend
work - Explaining code

------------------------------------------------------------------------

### Opus 4.6

Highest reasoning depth.\
Excels at: - Complex logic - System design - Multi-file reasoning

**Highest token consumption**\
**Slowest & most expensive**\
**Best for:** - Architecture decisions - Hard bugs - Security reviews

------------------------------------------------------------------------

### GPT-5.3 Codex

Specialized agentic coding model.\
Excels at: - Repo-wide changes - Following instructions precisely -
Applying diffs safely

**High token usage, efficient per task**\
**Best for:** - Large refactors - Test generation - Legacy code
modernization

------------------------------------------------------------------------

### GPT-5.2

General-purpose strong model.\
Less specialized than Codex.

**Moderate-high token usage**\
**Best for:** - Mixed tasks - When Codex is unnecessary

------------------------------------------------------------------------

## Token Consumption (Lowest → Highest)

1.  Composer 1.5\
2.  Sonnet 4.5\
3.  GPT-5.2\
4.  GPT-5.3 Codex\
5.  Opus 4.6\
6.  MAX Mode (multiplier on top)

> MAX Mode can increase token usage significantly depending on
> repository size.

------------------------------------------------------------------------

## Effectiveness vs Cost

  Model           Effectiveness            Cost Efficiency
  --------------- ------------------------ -----------------
  Composer 1.5    ⭐⭐⭐                   ⭐⭐⭐⭐⭐
  Sonnet 4.5      ⭐⭐⭐⭐                 ⭐⭐⭐⭐
  GPT-5.2         ⭐⭐⭐⭐                 ⭐⭐⭐
  GPT-5.3 Codex   ⭐⭐⭐⭐⭐ (code)        ⭐⭐⭐
  Opus 4.6        ⭐⭐⭐⭐⭐ (reasoning)   ⭐⭐
  MAX Mode        ⭐⭐⭐⭐⭐               ⭐

------------------------------------------------------------------------

## Recommended Usage

### Daily Development

**Sonnet 4.5 + Auto**\
Best balance of quality, speed, and cost.

### UI / Tailwind / React Components

**Composer 1.5**\
Fast and cost-efficient.

### Hard Problems / Architecture

**Opus 4.6**\
Use selectively due to higher cost.

### Large Refactors / Repo-wide Changes

**GPT-5.3 Codex**\
Use MAX Mode if deep context is required.

### Cost-Sensitive Work

**Composer 1.5** or **Sonnet 4.5**\
Avoid MAX unless necessary.
