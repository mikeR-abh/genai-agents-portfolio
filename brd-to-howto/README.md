**Prompt I used**:
"You are an expert technical writer. Take this full BRD/FRD [paste document here] and convert it into a clear, step-by-step 'How-To Implementation Guide' for developers. Include: code structure suggestions, edge cases, acceptance criteria, potential risks, and sample API calls. Output in markdown with headings."

**Example Input → Output**:

**Input snippet**: "User must be able to upload profile picture with max 5MB, formats JPG/PNG..."

**Agent output**:
### How to Implement Profile Picture Upload
1. Frontend: Use <input type="file"> with accept="image/jpeg,image/png"
2. Validate size client-side ≤ 5MB
3. Backend endpoint: POST /api/user/profile-picture
4. Edge cases: File >5MB → return 400, wrong format → 415, etc.


How-to Doc Generator — LLM Agent (Confluence / Rovo)

Convert BRDs / FRDs into clear, developer-focused How-To Implementation Guides using an LLM-powered agent. Built and deployed in Confluence using Atlassian Rovo. Ideal for engineering teams who want consistent developer documentation from product artifacts.

➤ Project Overview

This agent ingests Business Requirement Documents (BRD) or Functional Requirement Documents (FRD) and outputs a clear, structured How-To Implementation Guide in Markdown. Guides include:

Code structure suggestions

API examples and sample requests/responses

Edge cases and acceptance criteria

Potential risks and mitigations

Testing checklist and suggested unit/integration tests

Clear headings and step-by-step instructions suitable for developer handoff

Reference sample BRD/FRD used for testing:
/mnt/data/Abhishek Singh- TPMUR.pdf

➤ Features

Single-paste or file upload input (supports pasted text or PDF-to-text pipeline)

Output in Markdown, ready to paste into Confluence or GitHub

Configurable templates for Frontend / Backend / Data / QA sections

Generates API endpoint examples, request/response shapes, status codes

Adds acceptance criteria and test suggestions automatically

Edge-case detection and suggested handling

Confidence score and “review suggested” flags for ambiguous areas

➤ How It Works (high level)

Document ingestion

If PDF, run OCR / text extraction.

For Confluence/Rovo: use Rovo file or pasted content as input to the agent.

Document parsing

Break into sections (features, flows, constraints, non-functional requirements).

Extract explicit requirements (e.g., "max 5MB", "jpg/png").

Template population (Prompting + RAG)

Use Retrieval Augmented Generation (RAG) to fetch related patterns & prior docs (optional).

Prompt LLM to produce structured How-To guide in markdown.

Post-processing

Validate generated code snippets and JSON.

Apply deterministic rules (size validation logic, HTTP status mapping).

Emit confidence and recommended manual review notes.

➤ Deployment / Integration (Confluence + Rovo)

Agent built with Atlassian Rovo (Confluence automation + LLM integration).

Input: Confluence page content (paste FRD/BRD) or attachments (PDF/text).

Output: New Confluence page with the How-To guide (Markdown/Confluence format).

Optional: webhook or CI job to push generated docs to GitHub repo.

Notes:

Ensure the Rovo instance can access the LLM provider (Claude / Grok / OpenAI API) you choose.

Use short-lived API tokens and limit access scopes.

Keep a human review step for high-risk or security-sensitive docs.

➤ Prompts
System prompt (set as agent identity)
You are an expert technical writer and software engineer with deep experience creating engineering runbooks, API references, and implementation guides. Produce developer-focused, concise, and testable How-To Implementation Guides from BRD/FRD inputs. Prefer clarity, explicit acceptance criteria, and code-like examples. When uncertain, surface assumptions and mark them for review.

Primary user prompt (refined)
You are an expert technical writer. Take the full BRD/FRD below (or the pasted section) and convert it into a clear, step-by-step "How-To Implementation Guide" for developers. Output must be valid Markdown with headings and short, actionable steps. Include the following sections where applicable:

1. Overview / Purpose
2. Assumptions & Dependencies (explicit)
3. Data Models & Code Structure Suggestions
4. API Endpoints (method, path, request body, response body, status codes, sample cURL)
5. Frontend Implementation Notes (props, validation, example snippet)
6. Backend Implementation Notes (controllers, validation, error codes, storage)
7. Edge Cases & Handling (explicit list mapped to expected behaviour/code)
8. Acceptance Criteria (pass/fail criteria and test cases)
9. Potential Risks & Mitigations (security, privacy, scale)
10. QA Test Cases / Checklist (unit & integration tests)
11. Example: for any UI or file upload, include client-side validation, server response behaviour, and exact HTTP codes

If the BRD is missing details, make reasonable assumptions and mark them with [ASSUMPTION]. For any numeric constraints (like file size, rate limits), show exact validation logic and sample code. Prioritize correctness, and keep code snippets executable pseudo-code (JavaScript / Node / Python). Return only Markdown.

Short prompt for quick use
Convert the following FRD into a How-To Implementation Guide with API endpoints, validation, edge cases, acceptance criteria, and sample API calls. Output in Markdown.

➤ Example

Input snippet:
"User must be able to upload profile picture with max 5MB, formats JPG/PNG..."

Agent output (Markdown):

## How to Implement Profile Picture Upload

### Overview
Allow users to upload a profile picture. Max size: 5 MB. Allowed formats: image/jpeg, image/png.

### Frontend
- Use `<input type="file" accept="image/jpeg,image/png">`.
- Client validation:
  - Check file.size <= 5 * 1024 * 1024
  - Check file.type in ["image/jpeg","image/png"]
- Sample JS:
```javascript
function validateFile(file) {
  const allowed = ["image/jpeg","image/png"];
  if (!allowed.includes(file.type)) return {ok:false, error:"Unsupported type"};
  if (file.size > 5 * 1024 * 1024) return {ok:false, error:"File too large"};
  return {ok:true};
}

Backend

Endpoint: POST /api/user/profile-picture

Authentication: Bearer token (JWT)

Accepts: multipart/form-data: file

Server-side validation:

If file.mimetype not in allowed → respond 415 Unsupported Media Type

If size > 5MB → respond 400 Bad Request with {error: "File size exceeds 5MB"}

Sample Express.js:

app.post('/api/user/profile-picture', authMiddleware, upload.single('file'), async (req,res) => {
  const file = req.file;
  if (!file) return res.status(400).json({error: "No file"});
  if (!["image/jpeg","image/png"].includes(file.mimetype)) return res.status(415).json({error:"unsupported type"});
  if (file.size > 5 * 1024 * 1024) return res.status(400).json({error:"File too large"});
  // store file in object store, update user record...
  return res.status(200).json({url: uploadedUrl});
});

Edge cases

File > 5MB → 400 Bad Request (message: File too large)

Unsupported format → 415 Unsupported Media Type

Missing file → 400 Bad Request

Corrupted file → 422 Unprocessable Entity

Acceptance criteria

Uploads succeed for valid JPG/PNG ≤ 5MB

Invalid uploads return appropriate status codes

Uploaded images accessible at returned URL

QA Checklist

Unit tests for validation functions

Integration test hitting /api/user/profile-picture with valid/invalid files


---

## ➤ Implementation Tips & Libraries

- **Backend**
  - Node.js + Express + Multer (file upload)
  - Python Flask + Flask-Uploads or FastAPI + python-multipart
  - Store files in S3 / GCS with signed URLs
  - Validate MIME and re-check file signature if security is required

- **Frontend**
  - HTML5 file input, FileReader for preview
  - Show progress bar using `XMLHttpRequest` or `fetch` with streams
  - Use client-side image resizing for large images when allowed

- **LLM / Rovo**
  - Keep prompt length within model limits or use chunking + RAG
  - Use deterministic post-processing rules for numeric constraints
  - Maintain an audit log of generated docs for traceability

---

## ➤ Testing & Evaluation

**Suggested test matrix for generated docs**
- **Completeness:** All sections exist (Overview, API, Edge Cases, Acceptance Criteria)
- **Accuracy:** Sample code compiles or is syntactically correct pseudo-code
- **Actionability:** Developer can implement from doc with ≤ 1 clarification question
- **Safety:** No leaked secrets or internal-only URLs in output
- **Review rate:** % of auto-generated docs requiring manual correction

**Human-in-loop**: Set a policy that all high-risk or customer-impacting features require manual review before publishing.

---

## ➤ Security & Privacy

- Do not include secrets, API keys, or internal hostnames in generated text.
- When using Rovo/LLM, avoid sending PII to third-party LLMs unless compliance-approved.
- Redact personal data in FRDs before sending to the LLM (or run a de-identification step).
- Use access controls in Confluence; store generated docs in a permissioned space.

---

## ➤ Suggested Repository Structure



howto-agent/
├── README.md # this file
├── LICENSE
├── prompts/
│ ├── system.txt
│ └── primary_prompt.txt
├── examples/
│ ├── sample_brd_1.pdf # reference or link to sample BRD
│ └── sample_output_1.md
├── scripts/
│ └── rovo_integration.md # documentation for integrating with Rovo
└── contributing.md


> Use `/mnt/data/Abhishek Singh- TPMUR.pdf` as the sample input when validating outputs locally.

---

## ➤ Contributing

- Add a new sample BRD in `examples/` and corresponding generated output.
- Add unit tests for post-processing scripts (e.g., validation mapping, status code rules).
- Suggest improvements to prompts in `prompts/` and add JIT templates for different domains (payments, profile, search).

---

## ➤ Example Rovo Integration Notes

- Create a Rovo job that:
  1. Accepts page content or attachment.
  2. Extracts text and chunk it to meet model limits.
  3. Calls the LLM with the `system` + `primary_prompt` templates.
  4. Runs post-processing to ensure code block formatting and no disallowed content.
  5. Writes output back to a new Confluence page or comment and tags for review.

- For reproducibility, save the prompt version and model+temperature used in the Confluence page footer.

---

## ➤ License (MIT)



MIT License

Copyright (c) 2025 Abhishek Singh

Permission is hereby granted, free of charge, to any person obtaining a copy
