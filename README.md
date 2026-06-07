# AI301_Capstone

# Contribution [#]: Replace Every strncpy_s with util::platform

**Contribution Number:** [1 ]  
**Student:** [Amanda Orozco]  
**Issue:** [[GitHub issue link](https://github.com/LunarG/gfxreconstruct/issues/1358)]  
**Status:** [Phase I ]

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

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

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
