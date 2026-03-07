# Kagantic-Vault

## Summary

Kagantic-Vault is a structured knowledge base template designed for Obsidian-style markdown vaults. It uses layered `index.md` files to guide agentic LLMs through content efficiently, while remaining easy for humans to read and edit. It is recommended to use with Git and an LLM agent connected to GitHub.

---

## Scope and Non-Goals

Kagantic-Vault is **optimized** for Obsidian-style markdown vaults stored in Git repositories and used by LLM agents (local or GitHub-connected) for retrieval-augmented workflows. It is designed for human-writable, agent-navigable knowledge bases -- not for high-throughput transactional data, log storage, or raw code repositories without documentation.

---

## Structure

### General

You can check `/example vault` for the structural example, which includes:

- File structure
- Files with internal writing structure planned
- Example tags
- Notes to guide template adoption

### File Structure

File structure relies on `index.md` files, with each one connected to its sub-categories. This acts as a navigation highway for agentic LLMs when they are searching for files.

```
obsidian-vault/
├── Main Index.md
├── Example folder/
│   ├── Example sub index.md
│   └── Example file.md
├── Another Example folder/
│   ├── Another Example sub index.md
│   └── Another Example file.md
```

### File Format

Each file is structured with a summary of 3-6 sentences before the in-depth content begins. This lets custom systems use the summary as a preview, and helps general models produce better answers by front-loading context.

Below the summary is a tag section that improves both RAG retrieval and previewing. At low vault scale the benefit is minor, so tags can be removed to save tokens -- but over time they provide better entry points for agents.

The remaining content is your own notes. Using external/internal links, code blocks, tables, and highlights more than usual is recommended here.

---

## RAG-Friendly Tag Rules

Tags give LLMs higher-quality entry points than the Main Index when queries are niche or the vault is large. They act as lightweight metadata that improves retrieval quality and routing.

**Guidelines**

- Use short, descriptive **keywords**, preferably 1-3 words each (e.g., `#product`, `#onboarding_guide`).
- Use lowercase and underscores for multi-word tags (e.g., `#internal_policy`, `#api_reference`).
- Mix **topic**, **domain**, and **audience** tags (e.g., `#llm_agents`, `#devops`, `#team_leads`).
- Avoid over-tagging; aim for **3-5 tags per file**, focusing on what an agent would actually search for.
- Keep tags stable over time; rename intentionally and update older files when conventions change.

**Example tag sets**

- A product spec: `*Tags:* #product #feature_spec #frontend #v1 #internal`
- An internal policy note: `*Tags:* #policy #hr #remote_work #employees #guidelines`
- A prompt engineering guide: `*Tags:* #llm_agents #prompt_engineering #best_practices #examples #training`

---

## How to Adopt

### Setup Steps

- [ ] Create a `Main Index.md` file. This is the LLM's entry point. Preferably, add an instruction directing agents to start searching from here.
- [ ] Link `Main Index.md` to sub-index files (e.g., `subcategory index`, `another subcategory index`). These are the roads your LLM will follow. For human access, enabling arrows in **Graph View** mode is recommended -- it lets you trace paths the same way an LLM does.
- [ ] Write a 3-5 sentence summary at the top of each file. In custom systems, this summary can be used as a preview so less tokens are spent reading full files. It also improves day-to-day chatbot responses.
- [ ] Add a tag section below the summary with RAG-compatible tags. Keywords provide better start points than `Main Index.md` when the vault is large and the topic is niche.

---

## Summary and Tag Creation Prompt

Use the following prompt on existing files to generate summaries and tags quickly. Paste the file content after `<file>` and fill in any existing keywords you want the model to reuse before running.

```xml
<context>
  You are preparing a file for a Kagantic-Vault knowledge base used by agentic LLMs and humans.
  The vault uses summaries as lightweight previews so agents can decide whether to read a full file.
  Tags act as metadata entry points that allow agents to skip the Main Index and jump directly to
  relevant content -- making tag quality critical for retrieval accuracy at scale.
</context>

<file>
  {paste file content here}
</file>

<existing-keywords>
  {paste current vault keywords here, or leave empty}
</existing-keywords>

<instructions>
  ## Summary
  Write a 3-6 sentence paragraph that functions as an agent-readable preview. A good summary:
  - States what the file IS and what it CONTAINS, not what it argues or concludes
  - Includes the domain, scope, and any key entities (systems, teams, features, decisions) by name
  - Signals what questions this file can answer so an agent can confidently route to it
  - Avoids raw data, lists, or conclusions -- it must read as a preview, not a digest

  ## Tags
  Choose or create exactly 5 tags optimized for retrieval. Apply this priority order:
  1. TOPIC tag -- the primary subject (e.g. #feature_spec, #runbook, #policy, #architecture)
  2. DOMAIN tag -- the area it belongs to (e.g. #backend, #hr, #infrastructure, #product)
  3. ENTITY tag -- a named system, team, feature, or concept central to the file (e.g. #auth_service, #onboarding)
  4. AUDIENCE tag -- who or what would query this (e.g. #llm_agents, #engineers, #team_leads)
  5. STATUS or SCOPE tag -- lifecycle or boundary signal (e.g. #v1, #internal, #deprecated, #draft)

  Prefer reusing existing keywords over inventing new ones to keep the tag namespace stable.
  Use lowercase_underscores only. No compound tags longer than 3 words.
</instructions>

<output-format>
  Output ONLY the markdown block below. No preamble, explanation, or questions.

  # Summary
  {summary here}

  *Tags:* #tag1 #tag2 #tag3 #tag4 #tag5
</output-format>
```

### Benchmark

| Model             | Response Quality | Best For                                         | Notes                                                                 |
| ----------------- | ---------------- | ------------------------------------------------ | --------------------------------------------------------------------- |
| Claude Sonnet 4.6 | Best             | High-quality, low-error migrations               | Rarely misses format; strong at choosing what to highlight.           |
| Kimi K2 Thinking  | Mid              | High-speed, multi-agent swarms                   | Requires manual format fixes; good when speed and scale matter most.  |
| Grok 4.2 Expert   | Excellent        | Large migrations on a budget                     | Extremely fast; needs manual fixes roughly 1 out of 3 runs.           |
| GPT 5.2 Thinking  | Mid              | Users already subscribed to ChatGPT              | Occasionally misses information but keeps format mostly intact.       |
| GPT 5.2           | Mid              | Not recommended                                  | Often misses markdown requirements; breaks strict output formatting.  |
| Gemini 3.1 PRO    | Low              | Very long, book-like documents (edge cases only) | Frequently misses the goal and does not follow markdown instructions. |

---

## Examples

### Main Index

```markdown
# Main Index
This vault is organized around index files that guide both humans and LLM agents through the content.
Start from this page and follow the links into sub-indexes before opening individual notes.
*Tags:* #index #entrypoint #llm_agents #documentation #navigation

## Core Areas

- [[Product Index]] - Product specs, roadmaps, release notes.
- [[Engineering Index]] - Architecture, infrastructure, runbooks.
- [[Operations Index]] - Processes, policies, and playbooks.

## How Agents Should Use This

1. Start here.
2. Choose the most relevant sub-index based on the user query.
3. Prefer files whose summaries and tags match the query before reading full content.

```

### Sub-Index

```markdown
# Product Index
This index groups all product-related files: specs, discovery notes, roadmaps, and user research.
Agents should start from here for any product or feature question.
*Tags:* #index #product #features #user_research #llm_agents

## Sections

- [[Product Overview]] - High-level product vision and positioning.
- [[Feature Specs Index]] - Individual features and technical details.
- [[User Research Index]] - Interview notes and key insights.

```

### Note File

```markdown
# Feature X - User Mention Alerts
## Summary
This note describes Feature X, which sends real-time alerts to users when they are mentioned in
comments across the platform. It covers user stories, acceptance criteria, and edge cases such as
spam prevention and notification throttling. It also documents UI behavior on web and mobile,
including notification center updates. Known limitations and follow-up ideas are listed at the end.
*Tags:* #product #feature_spec #notifications #frontend #backend

---

## Notes

### User Stories

- As a user, I receive an alert when someone @mentions me in a comment.
- As a user, I can configure which notification channels I want to use.

### Acceptance Criteria

- Mentions are processed within 10 seconds.
- Duplicate notifications for the same event are not sent.

### Links

- [[Product Index]]
- [[Notification Infrastructure Overview]]
```