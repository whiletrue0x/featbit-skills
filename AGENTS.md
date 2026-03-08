# FeatBit Skills — Agent Instructions

This repo contains agent skills for FeatBit. The primary task is authoring and maintaining `SKILL.md` files under `skills/`. All other work (releases, versioning) supports that purpose.

## Project Structure

```
featbit-skills/
├── .claude-plugin/
│   ├── plugin.json          # version must stay in sync
│   └── marketplace.json     # version must stay in sync
├── skills/
│   └── featbit-{topic}/
│       ├── SKILL.md         # required, <500 lines
│       ├── references/      # one level deep only
│       ├── scripts/         # deterministic helper CLIs
│       └── assets/          # output templates, schemas
├── package.json             # version must stay in sync
└── README.md                # version badge must stay in sync
```

## Skill Authoring Rules

### Directory and Naming

- Every skill directory must be named `featbit-{topic}`.
- The `name` field in `SKILL.md` frontmatter must exactly match the directory name (e.g., `name: featbit-dotnet-sdk` lives in `skills/featbit-dotnet-sdk/SKILL.md`).
- `references/` is one level deep only. Never create `references/db/v1/schema.md`; use `references/schema.md`.
- Never create `README.md`, `CHANGELOG.md`, or `INSTALLATION.md` inside a skill directory.

### Frontmatter

```yaml
---
name: featbit-{topic}
description: <trigger-optimized, max 1024 chars>
license: MIT
metadata:
  author: FeatBit
  version: 1.0.0
  category: sdk-integration | deployment | api-reference | platform
---
```

`name` rules: 1–64 characters, lowercase letters, numbers, and hyphens, no consecutive hyphens.

`description` rules:
- Third person, describes capability.
- Include explicit positive triggers (user phrases, file extensions, keywords).
- Include at least one negative trigger ("Do not use for...") to prevent false activation.

Bad: `FeatBit SDK help.`

Good: `Expert guidance for integrating FeatBit .NET Server SDK. Use when user asks about ".NET SDK", "C# feature flags", "ASP.NET Core FeatBit", or mentions .cs, .csproj files. Do not use for Node.js, Python, or client-side JavaScript questions.`

### SKILL.md Content

- Hard limit: 500 lines. Move excess content to `references/`.
- `SKILL.md` is the navigation brain — high-level steps and decisions only.
- Do not inline large code blocks, full config files, or exhaustive API schemas. Put them in `references/` or `assets/`.
- Load reference files just-in-time: instruct explicitly when to read them.

Just-in-time loading pattern:
```markdown
Read `references/environment-variables.md` only when the user asks about a specific variable or config option.
```

### Writing Style

- Use numbered steps for every workflow. Define a strict chronological sequence.
- Write in third-person imperative: "Extract the flag key from..." not "You should extract..." or "I will extract..."
- Map decision branches explicitly: "Step 3: If the user is on ASP.NET Core, proceed to Step 4. Otherwise skip to Step 5."
- Use one term per concept throughout the file. Never alternate between "feature flag", "flag", and "toggle".
- Use domain-native terminology (e.g., "dependency injection container" for .NET, not just "setup").

### Reference Files

- Each reference file addresses one topic completely and is self-contained.
- Name descriptively: `references/openfeature-integration.md`, not `references/extra.md`.
- `SKILL.md` must state exactly when and why to open each reference file.

### Scripts

- Scripts in `scripts/` must be tiny single-purpose CLIs (Python, Bash, or Node).
- Scripts emit descriptive stdout/stderr so the agent can self-correct without user intervention.
- Do not bundle library or application code in scripts.

### What to Remove

- Instructions for tasks the agent can handle reliably without help.
- Prose explanations — convert to numbered steps or move to `references/`.
- Marketing text ("FeatBit is a powerful platform that...").
- Information already covered by another skill (e.g., auth basics from `featbit-rest-api` need not repeat in SDK skills).

### New Skill Checklist

When adding a new skill, complete all steps in order:

1. Create `skills/featbit-{topic}/SKILL.md` with valid frontmatter.
2. Add entry to `package.json` → `skills` array.
3. Update skill count in `README.md` ("What is This?" section).
4. Add skill entry to `README.md` ("Available Skills" section).
5. Update `CHANGELOG.md`.
6. Bump MINOR version in `package.json`, `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`, and `README.md` version badge.

### Pre-commit Validation

Verify before every skill commit:

- [ ] `name` in frontmatter exactly matches the directory name
- [ ] Description has at least one negative trigger
- [ ] `SKILL.md` is under 500 lines
- [ ] All files referenced in `SKILL.md` exist at the specified paths
- [ ] All paths use forward slashes and are relative
- [ ] No `README.md` or `CHANGELOG.md` inside the skill directory
- [ ] All instructions are third-person imperative
- [ ] Skill is listed in `package.json` `skills` array

## Versioning

Follow Semantic Versioning (`MAJOR.MINOR.PATCH`).

| Change | Bump |
|--------|------|
| Breaking changes, removed skills, incompatible manifest changes | MAJOR |
| New skill, new feature, non-breaking enhancement | MINOR |
| Bug fix, typo, doc update, minor improvement | PATCH |

Keep version in sync across all four files simultaneously:
- `package.json`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`
- `README.md` version badge

## Release

Release title format: `v{MAJOR}.{MINOR}.{PATCH} - {3–6 word description in Title Case}`

Examples: `v1.5.0 - AWS Deployment Support`, `v2.0.0 - Breaking Architecture Changes`

Release checklist:
- [ ] Version bumped in all four files
- [ ] `CHANGELOG.md` updated
- [ ] `git tag -a v{VERSION} -m "{title}"`
- [ ] `git push origin main && git push origin v{VERSION}`
- [ ] GitHub Release created with notes

## Commit Messages

Format: `<type>(<scope>): <subject>`

Types: `feat` (MINOR), `fix` (PATCH), `docs` (PATCH), `refactor` (PATCH), `chore` (no bump).

Scope is the skill name (e.g., `dotnet-sdk`, `deployment-docker`) or `release` for version bumps.

Examples:
```
feat(deployment-aws): add AWS ECS Fargate deployment skill
fix(dotnet-sdk): correct namespace in ASP.NET Core examples
chore(release): bump version to 1.5.0
```

## Language

Use English (US) for all skill content, commit messages, and documentation.
