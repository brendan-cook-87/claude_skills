### Executing Action Plans

## Skill Style
This skill is a generic guideline for how you, claude code, should operate.

## General Guidelines
You should ALWAYS follow the process of test driven development. 
If you think that it is unimportant for this task (prototyping etc. that won't reach production) STOP.  
Ask the human developer if they agree that it is unimportant for this feature.

You should ALWAYS write tests before implementing any new code, or attempting a fix.
You should ALWAYS confirm that a test (or tool to generate a finding) fails first, before attempting a fix.
You should ALWAYS confirm that the test (or tool to generate a finding) succeeds after the change has been attempted.
You should ALWAYS prioritise fixing the code to fix a failing test.  You should NEVER disable or modify a failing test.  If 
you think there is an exception to this, you may ask the human developer to confirm this case.

You should present a test plan with clearly documented cases and edge cases that need testing to the human developer 
for review before implementing any new functionality.  This test plan should be neatly formatted.  Use formatting like 
tables to clearly present combinations of inputs for different cases and the expected results.

When writing an action plan, this test plan should be included in the ** how to test ** section.