# Contribution [1]: [Enable automatic precommit-ci TypeScript code formatting]

**Contribution Number:** [1]  
**Student:** [Alexander Chakmakian]  
**Issue:** [https://github.com/maplibre/maplibre-tile-spec/issues/842]  
**Status:** [Phase I]

---

## Why I Chose This Issue

I chose this issue because it sparked my interest in the maplibre project and founder Nyurik who created the project. Nyurik also has an impressive background which caught my eye in being a contributor. This issue also seems to match my interest in both JavaScript/TypeScript development and software development workflows which I've been getting more experience in.
I also chose this issue because I wanted to contribute to a project that I find interesting even if my contribution is something small. I like that this issue improves the developer workflow while it's on something that is being used widely by many today.

---

## Understanding the Issue

### Problem Description

The TypeScript code in the /ts folder was not being automatically formatted in a reliable way. Because of that, code with inconsistent formatting kept getting merged into the project. The maintainer pointed out that they already use pre-commit to handle formatting in other languages, but the tool they were using for TypeScript (Prettier) got archived, so they needed a different approach.

While digging into this, I found that someone had already started fixing it in an earlier pull request. They switched TypeScript over to a formatter called Biome and added it to the pre-commit setup. So the core idea was already in place, but it was not fully finished, which is where my work comes in.

### Expected Behavior

When a contributor commits or opens a pull request, the TypeScript code should automatically get checked for formatting, and badly formatted code should not be able to make it into the main branch. There should also be a simple command a developer can run locally to format their code before pushing.

### Current Behavior

Formatting was only being enforced through the local pre-commit hook. That only works if a contributor actually installs and runs pre-commit on their machine. If they skip that step, unformatted TypeScript can still get merged, which is exactly the problem the issue is complaining about.

On top of that, the project's own command for formatting TypeScript (just ts::fmt) was broken. It pointed to an npm script called "format" that did not exist in package.json, so running it just gave an error. Biome was also not listed as a project dependency, so there was nothing tying the formatter to the project itself.

### Affected Components

- ts/package.json (the scripts and dependency list for the TypeScript package)
- ts/package-lock.json (npm's lock file, which gets updated automatically when a dependency is added)
- ts/mod.just (the just commands for linting and formatting the TypeScript code)
- The TypeScript CI workflow, since that is what actually runs the lint step on pull requests

---

## Reproduction Process

### Environment Setup

I forked the project, cloned it to my local machine, and set up two remotes: origin pointing to my fork and upstream pointing to the real maplibre repo. The TypeScript part of the project only needs Node and npm, which I already had, so I did not have to build the whole repo (which also has Rust, Java, and C++ parts). I just went into the ts folder and ran npm ci to install the dependencies. That was really the only setup needed to start working on this issue.

### Steps to Reproduce

1. Go into the ts folder and run npm ci to install dependencies.
2. Try to run the project's formatting command, npm run format. This is the command that just ts::fmt relies on.
3. Observed result: npm errors out with "Missing script: format" because that script was never actually added to package.json.

I also confirmed the bigger problem behind the issue: formatting is only checked by the local pre-commit hook, not by CI. So if a contributor never installs pre-commit, there is nothing stopping unformatted TypeScript from being merged.

### Reproduction Evidence

- **Branch link:** https://github.com/AlexChakmakian/maplibre-tile-spec/tree/enable-ts-biome-formatting
- **My findings:** The existing fix had set up Biome and a pre-commit hook, and all the current TypeScript files were already formatted correctly. But the format command was broken, Biome was not listed as a dependency, and formatting was never enforced in CI. So the issue was not really "start from scratch," it was "finish what was started and close the gap."

---

## Solution Approach

### Analysis

The root cause is that the earlier fix only got halfway there. Biome was set up as the formatter and the pre-commit hook was added, but pre-commit is optional, so it does not guarantee anything. The actual enforcement was missing in two places: there was no working command for a developer to format their code, and there was no check in the CI pipeline to reject unformatted code on a pull request. So formatting was basically on the honor system.

### Proposed Solution

My plan is to finish the setup so formatting is actually enforced, not just suggested. I want to add Biome as a real project dependency, add working format commands to package.json, and then make the CI lint step also run a formatting check so that badly formatted TypeScript fails the build and cannot be merged. I am keeping the changes small and focused since most of the foundation is already there.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** TypeScript formatting is not being enforced. The format command is broken and CI never checks formatting, so unformatted code can still be merged.

**Match:** The project already uses Biome for TypeScript (set up in an earlier PR) and uses just commands plus npm scripts to run things in each language. I am following those same patterns instead of introducing anything new. I am also pinning Biome to the same version the pre-commit hook uses so local, CI, and pre-commit all format the exact same way.

**Plan:**
1. Add @biomejs/biome as a dev dependency in ts/package.json (which also updates package-lock.json).
2. Add two npm scripts: format to actually fix formatting, and format:check to check it without changing files. Both are pointed at the src folder so they cover all the TypeScript files but skip the JSON config files, which matches how the pre-commit hook already behaves.
3. Update ts/mod.just so the lint command also runs format:check. Since CI runs just ts::lint, this is what makes formatting an actual gate on pull requests.

**Implement:** Work is on my branch here: https://github.com/AlexChakmakian/maplibre-tile-spec/tree/enable-ts-biome-formatting

**Review:** Check that the changes are small and follow the project's existing style, that I only touched the files I needed to, and that I did not reformat or change any of the actual source code.

**Evaluate:** Run npm run format:check (should pass since the code is already clean), run npm run format (should change nothing), and then purposely break the formatting in one file to confirm format:check fails like it should. Also make sure the existing lint and tests still pass.

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

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
