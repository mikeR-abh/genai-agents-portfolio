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
