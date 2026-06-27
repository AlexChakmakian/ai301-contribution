# Contribution [1]: [Enable automatic precommit-ci TypeScript code formatting]

**Contribution Number:** [1]  
**Student:** [Alexander Chakmakian]  
**Issue:** [https://github.com/maplibre/maplibre-tile-spec/issues/842]  
**Status:** [Merged]

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

This issue was more about developer tooling than changing TypeScript logic, so I did not add new unit tests. I still ran the existing TypeScript test suite to make sure my tooling change did not break the package.

### Integration Tests

I tested this through the same commands the project and CI already use. The main thing I wanted to confirm was that formatting is now part of the normal TypeScript lint path, instead of only depending on pre-commit being installed locally.

### Manual Testing

I tested the change locally from the ts folder.

Commands I ran:

1. npm ci
2. npm run format:check
3. npm run format
4. npm run lint
5. npm run test
6. npm run build

Results:

- npm ci installed the TypeScript dependencies successfully.
- npm run format:check checked 104 TypeScript files and passed with no fixes needed.
- npm run format ran successfully and did not change any source files.
- npm run lint passed.
- npm run test passed with 27 test files passing and 1011 tests passing, with 5 skipped tests.
- npm run build passed with tsc.

I also tested the failure case by temporarily adding badly formatted code to one TypeScript file. npm run format:check failed like expected, which proved the formatting gate actually works. After that, I restored the file so no source code changes were included in the PR.

---

## Implementation Notes

### Phase III / IV Progress

For Phase III, I moved from planning the fix to actually implementing it and opening the pull request. My branch is here:

https://github.com/AlexChakmakian/maplibre-tile-spec/tree/enable-ts-biome-formatting

The implementation ended up being small, but it fixes the missing part of the previous Biome setup. The earlier PR had already added Biome and pre-commit support, but the TypeScript package still did not have a working npm format command, and CI was not checking formatting through the normal lint command.

I added Biome as a TypeScript dev dependency, added format scripts, and updated the TypeScript just lint command so it also checks formatting. That means contributors can run the formatter locally, and CI can catch unformatted TypeScript before it gets merged.

After opening the PR, GitHub Copilot left a review comment saying that my first version used biome check, which runs more than formatting. That feedback made sense because this issue is specifically about formatting, not adding another linter. I updated the scripts to use biome format instead, so the PR stays focused on formatting only.

During review, the maintainer also asked if the formatting could be fixed automatically instead of only failing the check and making contributors fix it manually. That was a good point because the original issue is about automatic formatting. To address that, I added a TypeScript formatting job to the existing autofix.ci workflow. The job runs just ts::fmt on pull requests and uses autofix-ci/action to commit formatting fixes back to the PR branch when needed.

After those updates, the maintainer accepted and merged the PR. This completed my first contribution to the MapLibre Tile Spec project.

### Code Changes

- **Files modified:** ts/package.json, ts/package-lock.json, ts/mod.just, .github/workflows/autofix.yml
- **Branch:** https://github.com/AlexChakmakian/maplibre-tile-spec/tree/enable-ts-biome-formatting
- **Approach decisions:** I kept the change focused on the TypeScript tooling instead of changing source code. I also scoped the Biome commands to ./src because all the TypeScript source files are there, and this avoids changing JSON config files that the existing pre-commit setup already excludes.

---

## Pull Request

**PR Link:** [https://github.com/maplibre/maplibre-tile-spec/pull/1456]

**PR Summary:** This PR builds on the existing Biome setup for the TypeScript code. It adds Biome as a TypeScript dev dependency, adds working format scripts, makes the TypeScript lint step check formatting, and adds an autofix.ci job so formatting can be applied automatically on pull requests. The main goal is to prevent unformatted TypeScript from getting merged while also reducing manual work for contributors.

**Maintainer Feedback:**
- June 19: Copilot suggested using biome format instead of biome check because the issue is only about formatting. I agreed with the feedback and updated the scripts so Biome only runs as a formatter.
- June 20: A maintainer asked if the PR could add an autofix or pre-commit hook so contributors do not have to manually fix formatting. I updated the PR by adding a TypeScript job to the existing autofix.ci workflow. It runs just ts::fmt and lets autofix-ci/action commit formatting fixes automatically.
- June 22: The maintainer merged the PR and thanked me for the contribution.

**Status:** Merged

---

## Learnings & Reflections

### Technical Skills Gained

I learned more about how npm scripts, package-lock.json, just commands, and CI all connect together. Before this, I knew formatting tools existed, but this helped me understand how a project actually enforces formatting so it is not just optional on each developer's computer.

I also learned the difference between a CI check that only fails and an autofix workflow that can actually apply the fix for a contributor. That was an important distinction for this issue because the maintainer wanted the formatting process to be automatic, not just enforced.

### Challenges Overcome

The main challenge was figuring out what part of the issue was already fixed and what part was still missing. At first it looked like Biome had already solved the issue, but after testing locally I found that the format command was missing and CI was not checking formatting. I also learned from the Copilot review that biome format was a better fit than biome check because this PR is only about formatting.

Another challenge was understanding the maintainer's feedback about autofix. I first thought the PR was done once CI checked formatting, but the maintainer wanted the workflow to fix formatting automatically when possible. I updated the PR to add that automatic step.

The most rewarding part was seeing the PR get accepted and merged after responding to feedback. It helped me understand that open source contribution is not just about the first version of the code, but also about being willing to iterate with maintainers.

### What I'd Do Differently Next Time

Next time I would check the existing history and CI setup earlier before assuming the issue needs to be solved from scratch. I would also test the exact commands mentioned in the repo first, because that is how I found the broken npm run format script.

---

## Resources Used

- Issue #842: https://github.com/maplibre/maplibre-tile-spec/issues/842
- Pull request #1456: https://github.com/maplibre/maplibre-tile-spec/pull/1456
- Biome formatting docs: https://biomejs.dev/formatter/
- pre-commit docs: https://pre-commit.com/
