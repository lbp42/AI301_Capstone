# AI301_Capstone

# Contribution [#]: Replace Every strncpy_s with util::platform

**Contribution Number:** [1 ]  
**Student:** [Amanda Orozco]  
**Issue:** [[GitHub issue link](https://github.com/LunarG/gfxreconstruct/issues/1358)]  
**Status:** [Phase III ]

---

## Why I Chose This Issue
---

I chose this issue as the find and replace issue is across the entire codebase, so I'm given the opprotunity to contribute to an open source but without the complexicity of learning every 
aspect of the codebase. String handling is a common vulnerability especially in the cybersecurity world. A project like this can demonstrate a real-world issue that can have serious 
security repercussions. 

## Understanding the Issue

### Problem Description

The issue is that strncpy_s is still being used rather than the wrapper.  A string copy wrapper named util::platform::StringCopy is inconsistent within the codebase,
thus causing compatibility issues on Mac and Linux. 

### Expected Behavior

Every instance of strncpy_s in the codebase should be replaced with util::platform::StringCopy so the code is consistent.
### Current Behavior

strncpy_s is used directly in multiple places throughout the codebase instead 
of going through the wrapper.

### Affected Components

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

- This refactor has no behavior change so no new unit tests were required
- Existing build passing confirms correctness of the replacement
  
### Integration Tests

- Not applicable for this change — this is a pure refactor with no 
  behavior change. The existing build passing serves as integration 
  validation since the project compiles and links successfully with 
  the new function calls.

### Manual Testing

- Ran `grep -r "strncpy_s" --include="*.cpp" --include="*.h" .` before 
  and after changes to confirm all instances were replaced
- Rebuilt project with `make -j4` — compiled successfully with zero errors
- Only remaining `strncpy_s` is inside `platform.h` which is the wrapper 
  definition itself and is expected

---

## Implementation Notes

### Week [3] Progress

Replaced all 12 instances of `strncpy_s` with `gfxrecon::util::platform::StringCopy` 
across 3 files using VSCode's Find and Replace. Rebuilt the project successfully 
with no errors after the changes.


### Code Changes

- **Files modified:** 
  - `framework/util/file_path.cpp` (8 replacements)
  - `framework/util/driver_info.cpp` (2 replacements)
  - `framework/encode/d3d12_capture_manager.cpp` (2 replacements)
- **Key commits:** https://github.com/lbp42/gfxreconstruct/tree/fix-issue-1358
- **Approach decisions:** Used VSCode Find and Replace for accuracy. 
  Confirmed `platform.h` wrapper takes identical arguments so no 
  additional changes were needed.

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

- [GFXReconstruct GitHub Issue #1358](https://github.com/LunarG/gfxreconstruct/issues/1358)
- [GFXReconstruct CONTRIBUTING.md](https://github.com/LunarG/gfxreconstruct/blob/dev/CONTRIBUTING.md)
