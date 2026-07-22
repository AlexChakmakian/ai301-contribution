# Contribution [2]: [docs: expand minimal platform recipes (aider, cursor, windsurf)]

**Contribution Number:** [2]  
**Student:** Alexander Chakmakian
**Issue:** [https://github.com/Raftersecurity/rafter-cli/issues/33]
**Status:** [Phase II]

---

## Why I Chose This Issue

I chose this issue because it is a clear documentation task with a gold-standard example (`recipes/gemini-cli.md`) and a scoped path. I can expand just the Cursor recipe. Cursor is the AI coding tool I use most, so this is a good first contribution where I can verify the docs myself and learn how Rafter integrates with agent platforms which seems interesting.

---

## Understanding the Issue

### Problem Description

This is incomplete docs, not a code bug. `recipes/cursor.md` should match the detail level of `recipes/gemini-cli.md`. Rome-1 assigned me the Cursor recipe and said it needs the same six sections: Prerequisites, Setup, Verify, MCP tools reference, Troubleshooting, and Uninstall.

### Expected Behavior

A Cursor user should be able to set up Rafter, verify it, understand the MCP tools, troubleshoot common problems, and uninstall - all from `recipes/cursor.md`.

### Current Behavior

Rome-1 noted the file has grown since the issue was filed (~36 → ~58 lines), so the old issue table is outdated. I checked the current file against the six sections. Setup and verify are already solid. Tools are named in one line but not really explained. Biggest gaps right now:

- tool usage examples
- Troubleshooting

I may still need light Prerequisites / Uninstall / MCP tools reference work after a careful pass, but I will not rewrite setup that already works.

### Affected Components

- `recipes/cursor.md` (the file I will edit)
- `recipes/gemini-cli.md` (gold standard)
- `recipes/opencode.md` (Rome-1 said use this for the MCP tools section shape)

---

## Reproduction Process

### Environment Setup

Forked and cloned `Raftersecurity/rafter-cli`, made a branch off main. Compared `cursor.md` to `gemini-cli.md` and `opencode.md`. Docs-only, so no build step.

### Steps to Reproduce

1. Open current `recipes/cursor.md` (not the old version from the issue).
2. Check it against the six sections Rome-1 listed.
3. Note what is missing vs Gemini / OpenCode.

Observed result: setup/verify are fine. Tools are listed but not described with examples. No Troubleshooting section.

### Reproduction Evidence

- Commit showing reproduction: N/A (docs gap, not a failing test)
- My findings: trust the current file + Rome-1's section list, not the original issue table

---

## Solution Approach

### Analysis

Cursor got good setup docs first; Gemini and OpenCode show the fuller recipe shape. My job is to close the remaining gaps in `cursor.md`.

### Proposed Solution

Edit only `recipes/cursor.md`. Keep existing setup/verify. Add tool usage examples and Troubleshooting, and fill any other thin sections from the six-section list. Shape the MCP tools reference like `opencode.md` (`scan_secrets`, `evaluate_command`, `read_audit_log`, `get_config`).

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Bring Cursor docs up to the six-section bar without rewriting what already works.

**Match:** Gemini for overall coverage; OpenCode for the MCP tools section shape.

**Plan:**
1. Diff current `cursor.md` against the six sections and list what is still missing.
2. Expand the MCP tools reference with short usage examples.
3. Add Troubleshooting for common Cursor issues.
4. Fill any other thin sections (Prerequisites, Uninstall) if needed.
5. Open a docs PR for `#33` (Phase III/IV).

**Implement:** [Phase III]

**Review:** [Phase III]

**Evaluate:** [Phase III]
---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Issue #33](https://github.com/Raftersecurity/rafter-cli/issues/33) (including Rome-1's guidance on Cursor scope)
- [Current `recipes/cursor.md`](https://github.com/Raftersecurity/rafter-cli/blob/main/recipes/cursor.md)
- [Gold standard `recipes/gemini-cli.md`](https://github.com/Raftersecurity/rafter-cli/blob/main/recipes/gemini-cli.md)
- [`recipes/opencode.md`](https://github.com/Raftersecurity/rafter-cli/blob/main/recipes/opencode.md) (shape for MCP tools reference)
