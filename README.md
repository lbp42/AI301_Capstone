# AI301_Capstone

# Contribution [#]: Replace Every strncpy_s with util::platform

**Contribution Number:** [1 ]  
**Student:** [Amanda Orozco]  
**Issue:** [[GitHub issue link](https://github.com/LunarG/gfxreconstruct/issues/1358)]  
**Status:** [Phase II ]

---

## Why I Chose This Issue

[1-2 paragraphs explaining why this issue interests you, how it matches your skills/learning goals, what you hope to learn]

---

I chose this issue as the find and replace issue is across the entire codebase, so I'm given the opprotunity to contribute to an open source but without the complexicity of learning every 
aspect of the codebase. String handling is a common vulnerability especially in the cybersecurity world. A project like this can demonstrate a real-world issue that can have serious 
security repercussions. 

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

The issue is that strncpy_s is still being used rather than the wrapper.  A string copy wrapper named uitil::platform::StringCopy is inconsistent within the codebase,
thus causing compatibility issues on Mac and Linux. 

### Expected Behavior

[What should happen?]
Every instance of strncpy_s in the codebase should be replaced with util::platform::StringCopy so the code is consistent.
### Current Behavior

[What actually happens?]

strncpy_s is used directly in multiple places throughout the codebase instead 
of going through the wrapper.

### Affected Components

[Which parts of the codebase are involved?]
Various C++ source files across the codebase wherever strncpy_s appears.
---

## Reproduction Process

### Environment Setup

- Cloned fork on macOS (Apple Silicon)
- Installed Xcode Command Line Tools, CMake via Homebrew
- Initialized git submodules with `git submodule update --init`
- Built project with `cmake . -Bbuild -DCMAKE_OSX_ARCHITECTURES="arm64"` and `make -j4`
- Set up SSH authentication for GitHub

### Steps to Reproduce

1. Clone the repository
2. Run `grep -r "strncpy_s" --include="*.cpp" --include="*.h" .` from the project root
3. Observed 12 instances of `strncpy_s` across 4 files instead of the wrapper `util::platform::StringCopy`

### Reproduction Evidence

- **Commit showing reproduction:** (https://github.com/lbp42/gfxreconstruct/tree/fix-issue-1358)
- **Screenshots/logs:** [If applicable]
- **My findings:** - When running `grep -r "strncpy_s" --include="*.cpp" --include="*.h" .` from  
 root it revealed 12 instances of `strncpy_s` across 4 files:
    - `framework/util/file_path.cpp` — 8 instances
    - `framework/util/driver_info.cpp` — 2 instances  
    - `framework/encode/d3d12_capture_manager.cpp` — 2 instances
    - `framework/util/platform.h` — 1 instance ( wrapper defined here DO NOT be change)

---

## Solution Approach

### Analysis

The issue is the inconsistent use of the project's own cross-platform string 
copy wrapper. `strncpy_s` is a Windows-specific function, but it is being 
called directly in several files. 
Instead of using the existing `util::platform::StringCopy` wrapper (defined in `framework/util/platform.h`). 
This wrapper was created specifically to handle cross-platform compatibility but is not being used consistently.

### Proposed Solution

Replace all direct calls to `strncpy_s` with `gfxrecon::util::platform::StringCopy` 
across the affected files. This is a direct 1:1 swap as noted by the maintainer 
in the issue.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** The codebase has a cross-platform string copy wrapper 
`util::platform::StringCopy` but 12 places in the code still use the 
Windows-only `strncpy_s` directly. 

**Match:** The wrapper already exists in `framework/util/platform.h` and 
takes the same arguments as `strncpy_s`, making this a direct replacement.

**Plan:** [Step-by-step implementation plan]
1. Modify `framework/util/file_path.cpp` and replace 8 instances of `strncpy_s`
2. Modify `framework/util/driver_info.cpp` and replace 2 instances 
3. Modify `framework/encode/d3d12_capture_manager.cpp` and replace 2 instances
4. Leave `framework/util/platform.h` as is (this is the wrapper def itself)

**Implement:** [[Link to your branch/commits as you work](https://github.com/lbp42/gfxreconstruct/tree/fix-issue-1358)]]

**Review:** Will follow CONTRIBUTING.md guidelines for commit messages and format.

**Evaluate:** Rebuild the project with `make -j4` after changes and confirm 
it compiles with no errors. Run grep again to confirm zero remaining instances 
of `strncpy_s` outside of `platform.h`.

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
