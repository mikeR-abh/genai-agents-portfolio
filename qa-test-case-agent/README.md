# QA Test Case & Edge Case Generator

**Problem it solves**: Manual test case writing takes hours and always misses edge cases → delays releases and causes bugs in production.

**What it does**: I paste any user story or requirement → agent instantly outputs:
- Positive test cases
- Negative test cases
- Edge cases (boundary values, nulls, special characters, concurrency, etc.)
- Data-driven test ideas
- Expected results + priority

**Tool used**: Atlassian Rovo + Claude 3.5 / Grok

**Prompt I used** (copy your real one here or use this one):
"You are a senior QA engineer with 15 years experience. Given this requirement: [paste requirement], generate a complete test case suite including:
- Test case ID, description, preconditions, steps, test data, expected result
- All edge cases and boundary conditions
- Negative scenarios
- Priority (P0/P1/P2)
Output in markdown table format."

**Example**:

**Input requirement**:  
"User login with email and password. Password must be 8–64 characters, contain uppercase, lowercase, number and special char."

**Agent instantly generated 47 test cases including**:
- Valid login
- Password exactly 8 chars / 64 chars
- Missing special char → fail
- SQL injection attempt
- Concurrent logins from 2 devices
- Account lock after 5 failed attempts
- etc.

Adopted by entire QA team — now used on every single user story.

Saved ~8–10 hours per sprint in test planning.
