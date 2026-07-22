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

This is incomplete docs, not a code bug. `recipes/cursor.md` should be about as detailed as `recipes/gemini-cli.md`. Rome-1 gave me the Cursor recipe and said it should cover the same six sections: Prerequisites, Setup, Verify, MCP tools reference, Troubleshooting, and Uninstall.

### Expected Behavior

Someone using Cursor should be able to follow `recipes/cursor.md` and get everything they need - setup, verify, what the MCP tools do (with examples), troubleshooting, and how to uninstall.

### Current Behavior

Setup and verify are already in good shape. The four MCP tools are only named in one sentence though. There is no Troubleshooting section and no real tool usage examples. Rome-1 also said the file grew since the issue was filed (~36 to ~58 lines), so the old gap table in the issue is outdated.

### Affected Components

- `recipes/cursor.md` - the file I will change
- `recipes/gemini-cli.md` - gold standard for the six sections
- `recipes/opencode.md` - good example for how the MCP tools section should look
- Docs only, so no app code / functions to change

---

## Reproduction Process

### Environment Setup

I followed the repo README and looked at the recipes folder. No dev container or heavy setup - it is markdown only. I forked `Raftersecurity/rafter-cli`, cloned my fork, and made a branch called `docs-issue-33-cursor-recipe` off `main`.

One challenge: the issue still describes the old shorter Cursor recipe, so I almost planned work that was already partly done. Rome-1 cleared that up. I fixed it by re-reading the current `recipes/cursor.md` on main and using his six-section list plus `opencode.md` as my checklist instead of the old issue table.

### Steps to Reproduce

1. Clone `https://github.com/Raftersecurity/rafter-cli` (or my fork) and check out `main`.
2. Open `recipes/cursor.md`, `recipes/gemini-cli.md`, and `recipes/opencode.md`.
3. Check Cursor against these six sections: Prerequisites, Setup, Verify, MCP tools reference, Troubleshooting, Uninstall.
4. You should see all six at a similar depth to Gemini (tools explained, troubleshooting present).
5. What you get instead: Setup and Verify are there, tools are only named (`scan_secrets`, `evaluate_command`, `read_audit_log`, `get_config`), and Troubleshooting / tool usage examples are missing.

### Reproduction Evidence

- Commit showing reproduction: N/A (docs gap, not a failing test)
- Branch: `docs-issue-33-cursor-recipe` on my fork
- My findings: `git log` on `recipes/cursor.md` shows it was added around Mar 2026 with the other agent recipes, then updated May 29, 2026 to lead with local `--with-cursor` setup. That is why setup looks stronger than the original issue table, but Troubleshooting and examples still never got added.

---

## Solution Approach

### Analysis

The real issue is that Cursor was written mainly as a setup guide and later improved for local install, but it never got brought up to the full recipe style that Gemini and OpenCode use. Missing sections are the symptom; uneven docs is the cause. The Cursor integration itself is fine.

### Proposed Solution

Only edit `recipes/cursor.md`. Keep the setup/verify content. Add an MCP tools reference (like `opencode.md`) with short usage examples, add Troubleshooting, and fill any other thin sections from the six-section list.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Cursor needs the same six sections as Gemini. Biggest holes are tool examples and Troubleshooting.

**Match:** Use `recipes/gemini-cli.md` for overall coverage and `recipes/opencode.md` for the MCP tools section shape (same four tools).

**Plan:**
1. Diff current `cursor.md` against the six sections and note what is still missing.
2. Expand the MCP tools reference with short usage examples for `scan_secrets`, `evaluate_command`, `read_audit_log`, and `get_config`.
3. Add Troubleshooting (restart Cursor, PATH, local vs global sandbox, re-running init is safe).
4. Add or finish Prerequisites / Uninstall only if they are still thin after the diff.
5. Watch for local vs global uninstall paths (`./.cursor/mcp.json` vs `~/.cursor/mcp.json`), do not invent tools beyond the four Rome-1 listed, and do not rewrite setup that already works.

**Implement:** [Phase III - write the docs on `docs-issue-33-cursor-recipe`]

**Review:** Before the PR, check the six-section list again and make sure only `recipes/cursor.md` changed.

**Evaluate:** Read the finished recipe like a new Cursor user; optionally run `rafter agent verify` after install to check the verify / troubleshooting wording.
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
