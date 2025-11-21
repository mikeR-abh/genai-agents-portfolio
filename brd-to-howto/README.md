# How-To Doc Generator – Atlassian Rovo AI Agent

**One-click BRD → perfect How-To guide · Deployed organization-wide · Saves 80-90% documentation time**

I built this custom agent in Atlassian Rovo (Confluence/Jira) that turns any BRD/FRD into a clean, user-ready How-To document in seconds.

Best part: I fed the agent its own BRD and it wrote its own perfect "How to Use" guide with zero edits.

### Live Demo (Meta!)

**Input** → My own BRD (6 pages)  
[Download BRD input here](./brd-input.pdf)

**Output** → 100% AI-generated How-To (zero manual edits)  
[Download generated How-To here](./how-to-output.pdf)

### Screenshots

![Rovo sidebar – agent appears here](./screenshot-rovo-sidebar.png)
![Agent generating the document](./screenshot-generation.png)
![Final Confluence page created by the agent](./screenshot-confluence-output.png)

### Exact Prompt Used in Rovo (system instructions)

See file: [`prompts/system.txt`](./prompts/system.txt)

### Prompt for ChatGPT / Grok / Claude (copy-paste ready)

See file: [`prompts/primary_prompt.txt`](./prompts/primary_prompt.txt)

### How I built it (step-by-step with screenshots)

[See full creation guide](./how-i-created-the-agent.pdf)

### Conversation Starters I added in Rovo
- Create a how-to document based on this page
- Generate user guide from this BRD
- Convert this workflow into step-by-step instructions
- Make it beginner-friendly

Built & deployed: November 2025  
Used every day by PMs, technical writers, and engineering teams.

Want this agent in your Confluence? Just copy the prompt from `prompts/system.txt` → create new Rovo agent → paste → activate. Done in 2 minutes.
