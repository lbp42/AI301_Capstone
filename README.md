# AI301_Capstone

# Contribution [#]: Replace Every strncpy_s with util::platform

**Contribution Number:** [1 ]  
**Student:** [Amanda Orozco]  
**Issue:** [[GitHub issue](https://github.com/LunarG/gfxreconstruct/issues/1358)]  
**Status:** [Phase IV ]

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

- **Commit showing reproduction:** [[Reproduction](https://github.com/lbp42/gfxreconstruct/tree/fix-issue-1358)]
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

**Implement:** [[Branch](https://github.com/lbp42/gfxreconstruct/tree/fix-issue-1358)]

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

  
**Before — 12 instances of `strncpy_s` found across 4 files:**
<img width="1010" height="189" alt="Before fix - grep showing 12 instances of strncpy_s" src="https://github.com/user-attachments/assets/430f3098-66a1-4d42-96a2-766553e4c461" />

**After — only the wrapper definition in `platform.h` remains:**
<img width="773" height="777" alt="After fix - grep showing only the platform.h wrapper" src="https://github.com/user-attachments/assets/27e980d8-60df-4eb6-8096-7091cf60bf42" />


**Successful rebuild confirming no errors were introduced:**
<img width="1216" height="54" alt="Successful rebuild with no errors" src="https://github.com/user-attachments/assets/dae654b9-49f4-41c3-a918-dc7077363ccd" />


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
- **Key commits:** [[Commits](https://github.com/lbp42/gfxreconstruct/tree/fix-issue-1358)]
- **Approach decisions:** Used VSCode Find and Replace for accuracy. 
  Confirmed `platform.h` wrapper takes identical arguments so no 
  additional changes were needed.

---

## Pull Request

**PR Link:** [[PR](https://github.com/LunarG/gfxreconstruct/pull/3056)]

**PR Description:** 
Replaced all direct calls to `strncpy_s` with the existing cross-platform wrapper `gfxrecon::util::platform::StringCopy` across 3 files to address issue #1358.

**Maintainer Feedback:**
- [06/28/2026]: Awaiting review 
- [Date]: [How you addressed it]

**Status:** Awaiting review 

---

## Learnings & Reflections

### Technical Skills Gained

I got more comfortable setting up a C++ build environment from scratch on Apple Silicon — installing Xcode Command Line Tools, CMake via Homebrew, initializing submodules, and configuring the right architecture flags. I also practiced verifying a refactor's correctness through tooling (grep) rather than unit tests, since this change had no behavior difference to test directly.

### Challenges Overcome

Getting the build working locally took some troubleshooting, since a few dependencies (Vulkan, Metal tools, jsoncpp) weren't found by CMake on macOS — though these turned out to be pre-existing warnings unrelated to my change, not blockers. The other challenge was confirming all 12 replacements matched the wrapper's exact argument order and types so nothing broke, which I verified by rebuilding after each batch of changes.

### What I'd Do Differently Next Time

I'd run the "before" grep and save that output right away, instead of having to check out an earlier commit later to reconstruct it — that would've saved time when documenting the change afterward.


---

## Resources Used

- [GFXReconstruct GitHub Issue #1358](https://github.com/LunarG/gfxreconstruct/issues/1358)
- [GFXReconstruct CONTRIBUTING.md](https://github.com/LunarG/gfxreconstruct/blob/dev/CONTRIBUTING.md)
