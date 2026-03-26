# FeatBit Agent Skills — Skill Forge Review Report

**Reviewer**: Jack (via skill-forge methodology)
**Date**: 2026-03-26
**Target**: https://github.com/featbit/featbit-skills (v2.1.2)
**Fork**: https://github.com/whiletrue0x/featbit-skills

---

## Executive Summary

This report evaluates the **featbit-skills** collection — 17 agent skills for the FeatBit feature flags platform — against the [skill-forge](https://github.com/motiful/skill-forge) quality standard.

**Verdict: PASS** — This is a well-engineered skill collection that demonstrates strong authoring discipline. All 17 skills pass security, structure, and quality validation. Two minor warnings were identified; neither blocks publishing.

---

## Scope

| Dimension | Value |
|-----------|-------|
| Skills reviewed | 17 |
| Reference files reviewed | 15 |
| Total SKILL.md lines | 2,395 |
| Total reference lines | 4,541 |
| Publishing model | Collection (single repo) |
| License | MIT |

---

## 1. Security Audit

| Check | Result | Notes |
|-------|--------|-------|
| Leaked secrets (API keys, tokens) | PASS | No real credentials found |
| Credential files (.env, .pem, .key) | PASS | None tracked in git |
| Password patterns in code | PASS | All are template placeholders (`please_change_me`, `your_password`, `${DB_PASSWORD}`) |
| .gitignore coverage | PASS | Covers .env*, node_modules/, .DS_Store, IDE configs, OS files |
| Hardcoded personal paths | PASS | No `~/`, `/Users/`, `/home/` paths found |

**No security issues found.** All password strings in deployment reference files are documentation examples, not real credentials.

---

## 2. Structure Validation

### 2.1 Frontmatter (all 17 skills)

| Check | Result |
|-------|--------|
| `name` present, kebab-case, matches directory | 17/17 PASS |
| `name` no reserved words ("anthropic", "claude") | 17/17 PASS |
| `description` present, single-line, < 1024 chars | 17/17 PASS |
| `description` has negative trigger ("Do not use for...") | 17/17 PASS |
| `description` in third person | 17/17 PASS |
| `license` field present (MIT) | 17/17 PASS |
| `metadata` present (author, version, category) | 17/17 PASS |
| No non-standard top-level fields | 17/17 PASS |
| No YAML multi-line syntax (`>-`, `|`) in description | 17/17 PASS |

### 2.2 File Structure

| Check | Result |
|-------|--------|
| All SKILL.md under 500 lines | PASS (max: 293, featbit-deployment-docker) |
| All referenced files exist | PASS (13/13 local references resolve) |
| Directory naming convention (`featbit-{topic}`) | PASS |
| No README/CHANGELOG inside skill directories | PASS |
| No junk/unnecessary files | PASS |
| No nested reference directories | PASS |
| No scripts/ directories | N/A (pure knowledge skills) |
| No assets/ directories | N/A |

### 2.3 Collection-Level Structure

| Check | Result |
|-------|--------|
| package.json skills array matches filesystem | PASS (17 entries, all match) |
| README skill catalog matches filesystem | PASS |
| CHANGELOG exists and follows Keep a Changelog | PASS |
| .editorconfig present | PASS |
| .skillsignore present | PASS |
| AGENTS.md authoring guide present | PASS |
| Version sync (package.json / README badge) | PASS (both 2.1.2) |

---

## 3. Quality Assessment

### 3.1 Skill Content Quality

| Dimension | Assessment |
|-----------|-----------|
| **Description coverage** | All descriptions accurately reflect body content. Trigger phrases match actual skill scope |
| **Description clarity** | All descriptions are standalone comprehensible to a stranger unfamiliar with FeatBit |
| **Scope boundaries** | Clear negative triggers prevent false activation between SDK skills (e.g., Node skill excludes Python, Java, etc.) |
| **SDK Router pattern** | `featbit-sdks` correctly routes to language-specific skills based on detection signals |
| **Progressive disclosure** | Well-implemented — SKILL.md provides quick start, references loaded just-in-time |
| **Code examples** | Minimal, FeatBit-specific code; general language patterns correctly omitted |
| **Terminology consistency** | "feature flag" used consistently; no alternation with "toggle" or "switch" |
| **Writing style** | Imperative voice for workflow steps; third person for descriptions |

### 3.2 Reference File Quality

| File | Lines | TOC | Self-contained | Issues |
|------|-------|-----|----------------|--------|
| authentication.md | 90 | N/A (<100) | Yes | None |
| openfeature-integration.md | 73 | N/A (<100) | Yes | None |
| ecs-high-availability.md | 123 | Yes | Yes | None |
| complete-documentation-index.md | 204 | Yes | Yes | None |
| flag-evaluation-api.md | 286 | Yes | Yes | None |
| track-insights-api.md | 306 | Yes | Yes | None |
| common-patterns.md | 160 | Yes | Yes | None |
| environments-api.md | 129 | Yes | Yes | None |
| feature-flags-api.md | 242 | Yes | Yes | None |
| projects-api.md | 373 | Yes | Yes | None |
| standalone-configuration.md | 236 | Yes | Yes | None |
| standard-configuration.md | 429 | Yes | Yes | None |
| **professional-configuration.md** | **217** | **Yes** | **No** | **Nested refs** |
| environment-variables.md | 754 | Yes | Yes | Large file |
| troubleshooting.md | 919 | Yes | Yes | Large file |

### 3.3 AGENTS.md Compliance

The project defines its own authoring standard in `AGENTS.md`. All 17 skills comply with the documented rules:

- Skill directory naming: `featbit-{topic}` — all conform
- Frontmatter schema matches specification
- Description includes positive and negative triggers
- SKILL.md stays under 500-line limit
- References one level deep from SKILL.md
- No README/CHANGELOG inside skill directories
- Workflows use numbered steps with imperative voice
- Just-in-time reference loading pattern followed

---

## 4. Findings

### WARNING-1: Nested references in professional-configuration.md

**Severity**: Warning
**File**: `skills/featbit-deployment-docker/references/professional-configuration.md`, lines 68-69

The file references two sibling files:
```markdown
- [standalone-configuration.md](./standalone-configuration.md) — Contains comprehensive service configuration examples
- [environment-variables.md](./environment-variables.md) — Complete environment variable reference
```

This violates the AGENTS.md rule: *"all reference files link directly from SKILL.md. Never create `advanced.md` that points to `details.md`."*

The parent SKILL.md already links both files directly, making these cross-references redundant.

**Recommendation**: Remove lines 67-69 from `professional-configuration.md` and replace with inline content or a brief summary. The reader reaches this file via SKILL.md, which already provides the reference routing.

### WARNING-2: Collection size at threshold

**Severity**: Warning (informational)
**Context**: 17 skills exceeds the 15-skill context flooding threshold

Per skill-forge composition guidelines, collections with 15+ skills risk consuming excessive context tokens from description loading alone (~1,700 tokens for 17 descriptions).

**Mitigating factors** (already present):
- README recommends `--skill` selective installation
- SDK router (`featbit-sdks`) reduces need to install all SDK skills
- Skills have specific trigger phrases — they won't all activate simultaneously
- This is a single-product collection where per-language SDK skills are inherently necessary

**Recommendation**: No action needed. The existing mitigations are appropriate.

### INFO-1: Large reference files

**Severity**: Info
**Files**:
- `troubleshooting.md` — 919 lines
- `environment-variables.md` — 754 lines

Both files have proper tables of contents and are loaded just-in-time. Their size is justified by comprehensiveness (50+ environment variables, extensive troubleshooting scenarios). No splitting recommended — these benefit from being single, searchable references.

---

## 5. Publishing Readiness

| Check | Result |
|-------|--------|
| README exists with install instructions | PASS |
| LICENSE exists (MIT) | PASS |
| .gitignore exists and covers standard patterns | PASS |
| package.json has homepage, repository, bugs URLs | PASS |
| package.json keywords for discoverability | PASS (18 keywords) |
| Install command documented | PASS (`npx skills add featbit/featbit-skills`) |
| Selective install documented | PASS (`--skill` flag shown) |
| No hardcoded personal paths | PASS |
| Version badge in README matches package.json | PASS (2.1.2) |

---

## 6. Comparison with Best Practices

This collection demonstrates several noteworthy practices:

1. **AGENTS.md as authoring guide** — Clear, enforced standards for skill creation. This is exactly how a multi-skill collection should govern quality.

2. **SDK Router pattern** — The `featbit-sdks` skill acts as a language-detection dispatcher, preventing users from needing to know exact skill names. This reduces misrouting.

3. **Slim SKILL.md + GitHub pointer model** — SDK skills contain only FeatBit-specific vocabulary and link to official GitHub READMEs for advanced topics. This minimizes token waste while preserving actionable guidance.

4. **Consistent negative triggers** — Every skill explicitly states what it should NOT be used for. This prevents cross-activation in multi-skill environments.

5. **Progressive disclosure via references** — Large configuration details are in reference files loaded just-in-time, keeping SKILL.md focused on quick start and routing.

---

## 7. Conclusion

**featbit-skills** is a production-quality skill collection that exceeds the bar set by most published skill repos. The authoring discipline (AGENTS.md), consistent structure, and thoughtful context management (selective install, SDK router, just-in-time references) reflect mature skill engineering.

The two warnings identified are minor:
- The nested reference cross-link is a cosmetic violation — it doesn't break functionality
- The collection size is at threshold but properly mitigated

**No changes are required for publishing readiness.** The optional fix for WARNING-1 (nested references) would bring the project to full compliance with its own AGENTS.md standard.

---

*Report generated by [skill-forge](https://github.com/motiful/skill-forge) methodology*
*Reviewer: Jack (whiletrue0x)*
