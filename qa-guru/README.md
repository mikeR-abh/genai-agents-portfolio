# QA Guru – Test Case Generator (Atlassian Rovo AI Agent)

**Requirements (BRD/FRD/User Stories) → 50+ complete test cases with edge cases, negative scenarios, data tables, security & performance tests in seconds · Used daily by the QA team · 90% faster test planning**

Custom Atlassian Rovo agent that instantly generates thorough, professional test cases in perfect Confluence-ready markdown tables. Covers everything: happy path, negative, boundary, security, performance, accessibility, compliance.

The most-loved agent on the QA team — no one writes manual test cases anymore.

##  Example Input/Output (Live Demo)

### Input Snippet (from BRD)
> Users must be able to upload a profile picture with max 5MB, formats JPEG/PNG/JPG. System must validate file size client-side and server-side. Invalid files must show clear error message. Virus scanning must be performed.

### Agent Output (excerpt – full output has 45+ test cases)

| ID       | Title                              | Preconditions                  | Steps                                      | Test Data                  | Expected Result                                      | Type       | Priority |
|----------|------------------------------------|--------------------------------|--------------------------------------------|----------------------------|------------------------------------------------------|------------|----------|
| QA-001   | Valid JPEG upload (happy path)     | User logged in                 | 1. Select 3MB JPEG → Click Upload        | valid-image.jpg (3MB)      | Upload success, new picture shown                    | Positive   | High     |
| QA-002   | Valid PNG upload                   | User logged in                 | 1. Select 4.5MB PNG → Click Upload        | valid-image.png (4.5MB)    | Upload success                                       | Positive   | High     |
| QA-007   | File size exactly 5MB (boundary)   | User logged in                 | 1. Select exactly 5MB JPEG → Click Upload | boundary-5mb.jpg           | Upload success                                       | Boundary   | High     |
| QA-008   | File size 5.1MB (failure)          | User logged in                 | 1. Select 5.1MB PNG → Click Upload        | oversized.png (5.1MB)      | Error: "File must not exceed 5MB"                    | Negative   | High     |
| QA-012   | Invalid format GIF                 | User logged in                 | 1. Select 2MB GIF → Click Upload          | invalid.gif                | Error: "Only JPEG/PNG/JPG allowed"                   | Negative   | High     |
| QA-021   | No file selected                   | User logged in                 | 1. Click Upload without selecting file    | —                          | Error: "Please select a file"                        | Negative   | Medium   |
| QA-035   | SQL injection via filename         | User logged in                 | 1. Upload file named "'; DROP TABLE users;--.jpg" | malicious-name.jpg   | File rejected or sanitized, no DB error              | Security   | Critical |
| QA-041   | Upload during slow network         | User logged in, throttled net  | 1. Upload 4.9MB file                      | large-valid.jpg            | Progress bar shown, upload succeeds or graceful timeout | Performance| Medium   |

(Full real output contains 45+ rows + dedicated sections for Security Tests, Data-Driven Tests, Accessibility, etc.)

### Exact System Prompt (used in Rovo)
[`prompts/system.txt`](./prompts/system.txt)

### Copy-paste prompt for ChatGPT / Grok / Claude
[`prompts/primary_prompt.txt`](./prompts/primary_prompt.txt)

### Conversation Starters in Rovo
- Generate test cases from this BRD/FRD
- Include edge cases and negative scenarios
- Create full test suite with data tables
- Add security and performance tests

Built & deployed: November 2025  
Replaced 100% of manual test case writing for the team.

Want this agent? Copy `prompts/system.txt` → create new Rovo agent → paste → activate. Done in 2 minutes.
