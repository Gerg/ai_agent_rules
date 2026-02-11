# Code Review

Conduct a thorough, multi-round code review using the `pr-review` and `agent-issue-tracking` skills.

## Prerequisites

Enable these skills:
- `pr-review` - Core review process and validation techniques
- `agent-issue-tracking` - Track findings and organize work

## Instructions

- Schedule and perform multiple rounds of focused rounds using the `pr-review` skill. 
- The default number of rounds is 5, unless otherwise specified.
- The default code under review is the most recent commit, unless otherwise specified
- After each round, decide if additional rounds are needed based on findings.
- The rounds of review should eventually converge on a stable set of issues
- If there is any uncertainty about scope, criteria, etc. Prompt the user to clarify. Do not make unfounded assumptions.

### Additional Rounds (as needed)

Schedule additional rounds if there are aspects of the code that are not covered by existing review rounds. For example:
- If in-depth review is needed on a particular element
- If there is a relevant agent skill for an aspect of the code under review
- If there is external reference is relevant to the code under review
- If an excessive number of issues are discovered during a review
- If there is conflicting feedback from multiple rounds of review
- If the user asks for additional rounds
- And so on...

### Final: Summary
- Generate comprehensive summary of all findings
- Categorize and prioritize issues
- Provide overall recommendation

## Success Criteria

- [ ] The scope of the review is well understood
- [ ] The review criteria are well understood
- [ ] The desired number of review rounds are complete
- [ ] No scheduled review rounds were skipped
- [ ] All findings tracked and categorized
- [ ] Summary generated with clear recommendation
