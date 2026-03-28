# FeatBit Agent Skills — Skill Forge Review Report

**Reviewer**: Jack (via skill-forge methodology)
**Date**: 2026-03-28
**Target**: https://github.com/featbit/featbit-skills (v2.1.2)
**Fork**: https://github.com/whiletrue0x/featbit-skills

---

## Executive Summary

This report evaluates the **featbit-skills** collection — 17 agent skills for the FeatBit feature flags platform — against the [skill-forge](https://github.com/motiful/skill-forge) quality standard.

**Verdict: PASS with warnings** — The collection is structurally sound, secure, and well-engineered. No critical issues found. Three systemic patterns and seven per-skill warnings were identified. The most actionable finding is terminology inconsistency ("feature flag" vs "flag") across ~15 of 17 skills, which violates the project's own AGENTS.md rule.

### Changes Since Previous Review (2026-03-26)

| Change | Detail |
|--------|--------|
| Review depth | Previous: summary-level. Current: per-file validation with parallel agents |
| New findings | 3 systemic patterns + 7 per-skill warnings (vs 2 warnings in previous review) |
| Terminology assessment corrected | Previous report claimed "feature flag used consistently" — this review found ~15/17 skills alternate between "feature flag" and "flag" |

---

## Scope

| Dimension | Value |
|-----------|-------|
| Skills reviewed | 17 |
| Reference files reviewed | 15 |
| Total SKILL.md lines | 2,397 |
| Total reference lines | 4,541 |
| Publishing model | Collection (single repo) |
| License | MIT |

### Skills Inventory

| Category | Skill | Lines | References |
|----------|-------|-------|------------|
| Platform | featbit-getting-started | 276 | 0 |
| Platform | featbit-documentation | 107 | 1 (204 lines) |
| Platform | featbit-opentelemetry | 99 | 0 |
| API | featbit-rest-api | 220 | 5 (994 lines) |
| API | featbit-evaluation-insights-api | 221 | 2 (592 lines) |
| Deployment | featbit-deployment-docker | 294 | 5 (2,555 lines) |
| Deployment | featbit-deployment-kubernetes | 89 | 0 |
| Deployment | featbit-deployment-aws | 39 | 1 (123 lines) |
| SDK Router | featbit-sdks | 42 | 0 |
| Server SDK | featbit-sdks-dotnet | 116 | 1 (73 lines) |
| Server SDK | featbit-sdks-node | 165 | 0 |
| Server SDK | featbit-sdks-python | 118 | 0 |
| Server SDK | featbit-sdks-java | 119 | 0 |
| Server SDK | featbit-sdks-go | 98 | 0 |
| Client SDK | featbit-sdks-javascript | 116 | 0 |
| Client SDK | featbit-sdks-react | 147 | 0 |
| Client SDK | featbit-sdks-react-native | 131 | 0 |

---

## 1. Security Audit

| Check | Result | Notes |
|-------|--------|-------|
| Leaked secrets (`sk-`, `ghp_`, `AKIA`, `xox[bpas]-`) | PASS | No real credentials found |
| Credential files (.env, .pem, .key) | PASS | None tracked in git |
| Private key patterns (`-----BEGIN.*PRIVATE KEY-----`) | PASS | Only found as scan-pattern text in prior REVIEW-REPORT.md |
| Password patterns in code | PASS | All are template placeholders (`please_change_me`, `your_password`, `${DB_PASSWORD}`) |
| .gitignore coverage | PASS | Covers `.env*`, `node_modules/`, `.DS_Store`, IDE configs, OS files, `.claude/`, `.agents/`, `202*.txt` |
| Hardcoded personal paths | PASS | No `~/`, `/Users/`, `/home/` paths found |

**No security issues found.**

---

## 2. Structure Validation

### 2.1 Frontmatter (all 17 skills)

| Check | Result |
|-------|--------|
| `name` present, kebab-case, matches directory | 17/17 PASS |
| `name` no reserved words ("anthropic", "claude") | 17/17 PASS |
| `name` max 64 chars, no start/end hyphen, no `--` | 17/17 PASS |
| `description` present, single-line, < 1024 chars | 17/17 PASS |
| `description` has negative trigger ("Do not use for...") | 17/17 PASS |
| `description` in third person | 17/17 PASS |
| `description` no YAML multi-line syntax (`>-`, `\|`) | 17/17 PASS |
| `description` no unquoted `: ` that breaks strict parsers | 17/17 PASS |
| `license` field present (MIT) | 17/17 PASS |
| `metadata.author` present | 17/17 PASS |
| `metadata.version` present | 17/17 PASS |
| `metadata.category` present | 17/17 PASS |
| No non-standard top-level frontmatter fields | 17/17 PASS |

### 2.2 File Structure

| Check | Result |
|-------|--------|
| All SKILL.md under 500 lines | PASS (max: 294, featbit-deployment-docker) |
| All referenced local files exist | PASS (15/15 references resolve) |
| No orphan reference files | PASS |
| Directory naming convention (`featbit-{topic}`) | PASS (17/17) |
| No README/CHANGELOG inside skill directories | PASS |
| No junk/unnecessary files in skill directories | PASS |
| No nested reference directories | PASS |
| References one level deep only | PASS (see WARNING-1 for cross-refs) |
| All paths use forward slashes | PASS |
| No runtime write directories (data/, cache/) | PASS |
| No meta-skill contamination | PASS |

### 2.3 Collection-Level Structure

| Check | Result |
|-------|--------|
| package.json `skills` array matches filesystem | PASS (17 entries, all match) |
| README skill catalog matches filesystem | PASS (17 entries) |
| CHANGELOG.md exists | PASS |
| .editorconfig present | PASS |
| .skillsignore present | PASS |
| AGENTS.md authoring guide present | PASS (230 lines) |
| Version sync (package.json / README badge) | PASS (both 2.1.2) |

---

## 3. Per-Skill Detailed Validation

Each skill was validated by an independent agent against the full skill-forge validation table (20 checks). Below is the per-file breakdown.

### 3.1 featbit-getting-started

**Path**: `skills/featbit-getting-started/SKILL.md` (276 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields, name matches directory |
| Description | PASS | 345 chars, has positive/negative triggers, third person |
| Body size | PASS | 276 lines |
| References | N/A | None |
| Three-layer format | WARN | No `## Execution Procedure`. Uses Overview → Knowledge Areas → Best Practices layout |
| Description coverage | PASS | Covers "getting started", "first feature flag", "Dino Game demo" |
| Terminology | **WARN** | Alternates "feature flag" / "flag" / "Feature Toggle" (line 171 heading). Also "variation" vs "variant" |
| Voice | **WARN** | Line 17: "You'll learn to create feature flags…" — not imperative |
| Entry complexity | WARN | ~30 lines of navigational content (Next Steps, Related Topics) add low value |

### 3.2 featbit-documentation

**Path**: `skills/featbit-documentation/SKILL.md` (107 lines, 1 reference)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields, name matches directory |
| Description | PASS | 298 chars, clear fallback role |
| Body size | PASS | 107 lines |
| References | PASS | `references/complete-documentation-index.md` (204 lines) exists |
| Three-layer format | WARN | No EP. Body uses "When to Use" / "Output Rules" sections |
| Invocation reliability | WARN | No structured execution path — agent may answer directly instead of returning URLs |
| Graceful skip | PASS | "Do not use when another FeatBit skill already provides a complete answer" |
| Terminology | PASS | Consistent use of "documentation", "URLs" |

**Reference: complete-documentation-index.md** (204 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing YAML frontmatter (name/description) |
| TOC | PASS | Has table of contents |
| Self-contained | PASS | Pure data index, no cross-references |

### 3.3 featbit-rest-api

**Path**: `skills/featbit-rest-api/SKILL.md` (220 lines, 5 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | ~345 chars, concrete triggers ("FeatBit API", "REST API", "create project API") |
| Body size | PASS | 220 lines |
| References | PASS | All 5 resolve, no orphans |
| Three-layer format | WARN | No EP. Navigation brain with quick examples + reference links |
| Terminology | **WARN** | "feature flag" and "flag" used interchangeably |

**Reference: authentication.md** (90 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | N/A | Under 100 lines |
| Self-contained | PASS | |

**Reference: common-patterns.md** (160 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |

**Reference: environments-api.md** (129 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |

**Reference: feature-flags-api.md** (242 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |

**Reference: projects-api.md** (373 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |
| Line count | **WARN** | 373 lines exceeds 300-line recommended split threshold |

### 3.4 featbit-evaluation-insights-api

**Path**: `skills/featbit-evaluation-insights-api/SKILL.md` (221 lines, 2 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 694 chars, specific triggers (Kotlin, Android, iOS, Swift, Unity, embedded) |
| Body size | PASS | 221 lines |
| References | PASS | Both resolve |
| Three-layer format | WARN | Uses `## Core Workflow` instead of `## Execution Procedure` — functionally equivalent |
| Description coverage | PASS | Comprehensive trigger phrases |
| Terminology | PASS | Consistent: "evaluation server", "environment secret key", "insights", "variation" |

**Reference: flag-evaluation-api.md** (286 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present (line 7) |
| Self-contained | PASS | |

**Reference: track-insights-api.md** (306 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present (line 8) |
| Self-contained | PASS | |

### 3.5 featbit-opentelemetry

**Path**: `skills/featbit-opentelemetry/SKILL.md` (99 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 352 chars, specific triggers ("monitoring FeatBit", "OTEL backends") |
| Body size | PASS | 99 lines |
| Three-layer format | WARN | No EP. Implicit workflows in Quick Start and Common Use Cases |
| Terminology | **WARN** | Service names appear in two forms ("Evaluation-Server" vs `featbit-els`) without explicit mapping |

### 3.6 featbit-deployment-docker

**Path**: `skills/featbit-deployment-docker/SKILL.md` (294 lines, 5 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 511 chars, covers three deployment tiers |
| Body size | PASS | 294 lines |
| References | PASS | All 5 resolve, no orphans |
| Three-layer format | WARN | No EP |
| Terminology | **WARN** | "tier" and "deployment" used interchangeably as qualifiers |

**Reference: standalone-configuration.md** (236 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |

**Reference: standard-configuration.md** (429 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |

**Reference: professional-configuration.md** (217 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | **WARN** | Cross-references sibling files `standalone-configuration.md` (line 68) and `environment-variables.md` (lines 69, 122). See WARNING-1 |

**Reference: environment-variables.md** (754 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |

**Reference: troubleshooting.md** (919 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present |
| Self-contained | PASS | |
| Date content | **WARN** | Line ~897: hardcoded date `2024-02-01` in `docker compose logs --since` example |

### 3.7 featbit-deployment-kubernetes

**Path**: `skills/featbit-deployment-kubernetes/SKILL.md` (89 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | Covers Kubernetes, Helm, K8s, AKS, EKS, GKE |
| Body size | PASS | 89 lines |
| Three-layer format | WARN | No EP, no numbered workflow steps |
| Content depth | **WARN** | Mostly external URLs (chart repo, examples, migration scripts). Minimal inline procedural knowledge — if URLs break, skill provides little value |

### 3.8 featbit-deployment-aws

**Path**: `skills/featbit-deployment-aws/SKILL.md` (39 lines, 1 reference)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | Covers ECS Fargate, EKS, Terraform |
| Body size | PASS | 39 lines |
| References | PASS | `references/ecs-high-availability.md` exists |
| Three-layer format | WARN | No EP |
| Invocation | **WARN** | Reference linked as bare path without just-in-time loading instruction ("Read when user asks about…") |
| Terminology | WARN | "task count" vs "desired_count", "concurrent instances" vs "tasks" |

**Reference: ecs-high-availability.md** (123 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | PASS | Present (lines 7-14) |
| Self-contained | PASS | |

### 3.9 featbit-sdks (router)

**Path**: `skills/featbit-sdks/SKILL.md` (42 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 309 chars, clear routing scope |
| Body size | PASS | 42 lines |
| Three-layer format | WARN | "Routing Workflow" section uses numbered list, not EP pseudocode |
| Routing mechanism | **WARN** | Says "Activate exactly one language-specific SDK skill" but doesn't specify activation mechanism |
| Terminology | PASS | Consistent: "SDK", "language-specific skill", "runtime" |

### 3.10 featbit-sdks-dotnet

**Path**: `skills/featbit-sdks-dotnet/SKILL.md` (116 lines, 1 reference)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | Specific triggers (".NET SDK", "C# feature flags") |
| Body size | PASS | 116 lines |
| References | PASS | `references/openfeature-integration.md` exists |
| Three-layer format | WARN | No EP, "Setup Workflow" checklist instead |
| Terminology | **WARN** | Alternates "feature flag" (6 uses) and "flag" (4+ uses) |
| Progressive disclosure | PASS | "Read Next Only When Needed" gates reference loading |

**Reference: openfeature-integration.md** (73 lines)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | WARN | Missing |
| TOC | N/A | Under 100 lines |
| Self-contained | PASS | |

### 3.11 featbit-sdks-node

**Path**: `skills/featbit-sdks-node/SKILL.md` (165 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 287 chars, clear server-side Node.js scope |
| Body size | PASS | 165 lines |
| Three-layer format | WARN | No EP |
| Terminology | **WARN** | Alternates "feature flag" (7 uses), "flag" (6+ uses). Also "fallback value" vs "default values" |

### 3.12 featbit-sdks-python

**Path**: `skills/featbit-sdks-python/SKILL.md` (118 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | Specific triggers (Python SDK, Flask, Django, FastAPI) |
| Body size | PASS | 118 lines |
| Three-layer format | WARN | No EP, uses checklist workflow |
| Terminology | **WARN** | "feature flag" in headings, "flag" in body text |

### 3.13 featbit-sdks-java

**Path**: `skills/featbit-sdks-java/SKILL.md` (119 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 244 chars, triggers on "Java SDK", `.java`/`.gradle` files |
| Body size | PASS | 119 lines |
| Three-layer format | WARN | No EP |
| Terminology | **WARN** | "feature flag" (line 29) vs "flag" (lines 69, 80, 96) |

### 3.14 featbit-sdks-go

**Path**: `skills/featbit-sdks-go/SKILL.md` (98 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | Triggers on "Go SDK", "Golang", `.go` files |
| Body size | PASS | 98 lines |
| Three-layer format | WARN | No EP |
| Terminology | **WARN** | "feature flag" (3 uses) vs "flag" (6+ uses) |

### 3.15 featbit-sdks-javascript

**Path**: `skills/featbit-sdks-javascript/SKILL.md` (116 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 285 chars, clear browser-only scope |
| Body size | PASS | 116 lines |
| Three-layer format | WARN | No EP |
| External read pattern | **WARN** | Lines 113-116: "Read the official README section for…" points to GitHub URLs. Agent cannot `Read` a GitHub URL — needs `WebFetch` |
| Terminology | **WARN** | "feature flag" (lines 57, 67, 69) vs "flag" (lines 72, 75, 81, 113) |

### 3.16 featbit-sdks-react

**Path**: `skills/featbit-sdks-react/SKILL.md` (147 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 321 chars, specific hooks and provider triggers |
| Body size | PASS | 147 lines |
| Three-layer format | WARN | No EP |
| Graceful skip body | WARN | No explicit exit instruction for out-of-scope questions in the body (description handles it) |
| Terminology | **WARN** | "feature flags" (line 15) vs "flags" (lines 80+) |

### 3.17 featbit-sdks-react-native

**Path**: `skills/featbit-sdks-react-native/SKILL.md` (131 lines, 0 references)

| Check | Result | Detail |
|-------|--------|--------|
| Frontmatter | PASS | Standard fields |
| Description | PASS | 329 chars, mobile-specific triggers ("buildConfig", "polyfills") |
| Body size | PASS | 131 lines |
| Three-layer format | WARN | No EP |
| Terminology | **WARN** | "feature flags" (line 15) vs "flags" (lines 77, 84, 87, 104) |
| Graceful skip | PASS | Explicitly routes React web to `featbit-sdks-react` |

---

## 4. Reference File Summary

| File | Skill | Lines | Frontmatter | TOC | Self-contained | Issues |
|------|-------|-------|-------------|-----|----------------|--------|
| authentication.md | rest-api | 90 | Missing | N/A | Yes | — |
| openfeature-integration.md | sdks-dotnet | 73 | Missing | N/A | Yes | — |
| ecs-high-availability.md | deployment-aws | 123 | Missing | Yes | Yes | — |
| common-patterns.md | rest-api | 160 | Missing | Yes | Yes | — |
| complete-documentation-index.md | documentation | 204 | Missing | Yes | Yes | — |
| professional-configuration.md | deployment-docker | 217 | Missing | Yes | **No** | Cross-refs siblings (WARNING-1) |
| standalone-configuration.md | deployment-docker | 236 | Missing | Yes | Yes | — |
| feature-flags-api.md | rest-api | 242 | Missing | Yes | Yes | — |
| flag-evaluation-api.md | evaluation-insights-api | 286 | Missing | Yes | Yes | — |
| track-insights-api.md | evaluation-insights-api | 306 | Missing | Yes | Yes | — |
| projects-api.md | rest-api | 373 | Missing | Yes | Yes | Exceeds 300-line threshold |
| standard-configuration.md | deployment-docker | 429 | Missing | Yes | Yes | — |
| environment-variables.md | deployment-docker | 754 | Missing | Yes | Yes | — |
| troubleshooting.md | deployment-docker | 919 | Missing | Yes | Yes | Stale date in example |

All 15 reference files > 100 lines have a Table of Contents.
All 15 reference files are missing YAML frontmatter — this is a systemic pattern (PATTERN-2), not a per-file oversight.

---

## 5. AGENTS.md Assessment

**Path**: `AGENTS.md` (230 lines)
**Verdict**: PASS — high quality project standard

| Section | Assessment |
|---------|-----------|
| Project structure & naming | Clear `featbit-{topic}` convention, enforced |
| Frontmatter spec | Comprehensive, with good/bad examples |
| Content rules | 500-line limit, just-in-time loading, degrees of freedom taxonomy |
| Writing style | Imperative voice, third-person descriptions, one-term-per-concept |
| Reference file rules | One level deep, TOC > 100 lines, self-contained |
| Workflow templates | Checklist-based pattern (diverges from skill-forge EP pseudocode) |
| Anti-patterns | Practical list covering common mistakes |
| Pre-commit checklist | 13-item validation list |
| Versioning & release | SemVer, CHANGELOG, commit message format |

**Gap**: AGENTS.md does not require `## Execution Procedure` pseudocode or YAML frontmatter for reference files. This is the root cause of PATTERN-1 and PATTERN-2. However, the project's own conventions (checklist workflows, flat reference files) are coherent and appropriate for this use case.

---

## 6. Findings

### Systemic Patterns

#### PATTERN-1: No Execution Procedure pseudocode (17/17 skills)

**Severity**: Info (project convention)

All skills use AGENTS.md's "Setup Workflow" checklist pattern instead of skill-forge's `## Execution Procedure` pseudocode block. This is a deliberate project-level design choice documented in AGENTS.md. The checklist pattern is appropriate for SDK quick-starts and deployment guides.

**Recommendation**: No action. The AGENTS.md convention is coherent and works for this use case.

#### PATTERN-2: Reference files lack YAML frontmatter (15/15 references)

**Severity**: Info (project convention)

No reference file has a `---` frontmatter block with `name`/`description`. AGENTS.md doesn't require this. All reference files are still well-structured with TOCs and clear scope.

**Recommendation**: No action. Adding frontmatter would improve machine-readability but provides marginal benefit for this collection.

#### PATTERN-3: "feature flag" / "flag" terminology drift (~15/17 skills)

**Severity**: Warning

AGENTS.md explicitly states: *"Use one term per concept throughout the file. Never alternate between 'feature flag', 'flag', and 'toggle'."* Despite this rule, nearly every skill alternates between "feature flag" in explanatory headings and "flag" in code-adjacent text. `featbit-getting-started` additionally introduces "Feature Toggle" as a third term.

**Affected skills**: All except `featbit-evaluation-insights-api` and `featbit-documentation`.

**Recommendation**: Pick one term ("feature flag" for formal text, or "flag" as the universal short form) and normalize across all 15 affected skills. The most practical approach: use "feature flag" on first mention per skill, then "flag" throughout — and document this convention in AGENTS.md.

### Per-Skill Warnings

#### WARNING-1: Nested cross-references in professional-configuration.md

**Severity**: Warning
**File**: `skills/featbit-deployment-docker/references/professional-configuration.md`, lines 67-69 and 122

Cross-references sibling files (`standalone-configuration.md`, `environment-variables.md`). Violates AGENTS.md rule: *"all reference files link directly from SKILL.md."* The parent SKILL.md already links both files, making these redundant.

**Recommendation**: Replace cross-references with inline guidance or a note that the parent skill routes to those files.

#### WARNING-2: Hardcoded date in troubleshooting example

**Severity**: Warning
**File**: `skills/featbit-deployment-docker/references/troubleshooting.md`, line ~897

Contains `docker compose logs --since 2024-02-01T10:00:00 api-server` with a hardcoded date that reads as stale (2024).

**Recommendation**: Replace with a relative time example: `docker compose logs --since 1h api-server`.

#### WARNING-3: Link-collection pattern in deployment-kubernetes

**Severity**: Warning
**File**: `skills/featbit-deployment-kubernetes/SKILL.md`

Almost entirely external URLs (chart repo, examples, migration scripts). No inline procedural knowledge. If an agent cannot fetch URLs, the skill provides near-zero actionable content.

**Recommendation**: Add a minimal inline quick-start procedure (add repo → install chart → verify) so the skill has standalone value.

#### WARNING-4: projects-api.md exceeds 300-line threshold

**Severity**: Warning
**File**: `skills/featbit-rest-api/references/projects-api.md` (373 lines)

Exceeds the 300-line recommended split threshold. Has a TOC, so navigation is adequate.

**Recommendation**: Consider splitting by resource (projects CRUD vs project-level settings). Low priority — the file is coherent as-is.

#### WARNING-5: "Feature Toggle" heading in getting-started

**Severity**: Warning
**File**: `skills/featbit-getting-started/SKILL.md`, line 171

Introduces "Feature Toggle" as a heading alongside "feature flag" and "flag" — three terms for the same concept.

**Recommendation**: Rename to "Feature Flag Pattern" or "Simple Feature Flag" to align with project terminology.

#### WARNING-6: External GitHub "Read" pattern in sdks-javascript

**Severity**: Warning
**File**: `skills/featbit-sdks-javascript/SKILL.md`, lines 113-116

Uses "Read the official README section for…" pointing to GitHub URLs. An agent cannot `Read` a GitHub URL — it would need `WebFetch` or similar.

**Recommendation**: Change wording to "See the official README section at…" or "Refer the user to…" to avoid implying the agent should use the Read tool.

#### WARNING-7: Missing just-in-time loading instruction in deployment-aws

**Severity**: Warning
**File**: `skills/featbit-deployment-aws/SKILL.md`, line 32

Reference linked as bare path without explicit loading condition (e.g., "Read `references/ecs-high-availability.md` when the user asks about scaling or multi-AZ placement").

**Recommendation**: Add a conditional loading instruction.

---

## 7. Collection-Level Notes

| Item | Status | Note |
|------|--------|------|
| 13 conversation dump files in working dir | OK | Gitignored by `202*.txt` pattern |
| `REVIEW-REPORT.md` / `REVIEW-REPORT.pdf` tracked | Info | These are review artifacts, not skill content. Consider moving to `.github/` or untracking |
| Collection size (17 skills) | Info | Exceeds 15-skill context flooding threshold but mitigated by SDK router, selective install, and specific triggers |

---

## 8. Comparison with Best Practices

This collection demonstrates several noteworthy patterns:

1. **AGENTS.md as authoring guide** — Clear, enforced standards for skill creation with a 13-item pre-commit checklist. This is the right way to govern quality in a multi-skill collection.

2. **SDK Router pattern** — `featbit-sdks` acts as a language-detection dispatcher, preventing users from needing to know exact skill names. Reduces misrouting.

3. **Slim SKILL.md + GitHub pointer model** — SDK skills contain only FeatBit-specific vocabulary and link to official GitHub READMEs for advanced topics. Minimizes token waste.

4. **Consistent negative triggers** — Every skill explicitly states what it should NOT be used for. Prevents cross-activation.

5. **Progressive disclosure via references** — Large configuration details in reference files loaded just-in-time. Peak context load ~1,200 lines per skill.

6. **Naming convention** — All skills use `featbit-{topic}` prefix, eliminating collision risk.

---

## 9. Conclusion

### Summary of Findings

| ID | Severity | Category | Description |
|----|----------|----------|-------------|
| PATTERN-1 | Info | Systemic | No EP pseudocode — project uses checklist convention (17/17) |
| PATTERN-2 | Info | Systemic | Reference files lack YAML frontmatter (15/15) |
| PATTERN-3 | Warning | Systemic | Terminology drift: "feature flag" / "flag" (~15/17) |
| WARNING-1 | Warning | Per-skill | Nested cross-references in professional-configuration.md |
| WARNING-2 | Warning | Per-skill | Hardcoded 2024 date in troubleshooting.md example |
| WARNING-3 | Warning | Per-skill | deployment-kubernetes is a link collection with no inline procedure |
| WARNING-4 | Warning | Per-skill | projects-api.md at 373 lines exceeds 300-line threshold |
| WARNING-5 | Warning | Per-skill | "Feature Toggle" heading in getting-started introduces 3rd term |
| WARNING-6 | Warning | Per-skill | "Read" wording for GitHub URLs in sdks-javascript |
| WARNING-7 | Warning | Per-skill | Missing just-in-time loading instruction in deployment-aws |

### Verdict

**PASS with warnings** — No critical issues. The collection is structurally sound, secure, and well-engineered. The most impactful fix would be PATTERN-3 (terminology normalization), which is a batch find-and-replace operation guided by the project's own AGENTS.md rule.

---

*Report generated by [skill-forge](https://github.com/motiful/skill-forge) methodology*
*Reviewer: Jack (whiletrue0x)*
*Date: 2026-03-28*
