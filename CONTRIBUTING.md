# Contributing

This repository follows [PLAN-distributed.md](PLAN-distributed.md). Keep contributions within the assigned team's ownership. The plan is manager-owned; coordinate a change rather than editing it to fit an implementation.

## Clean-room requirements

- Independently collect every institutional source from its original approved public URL. Record the retrieval and verification dates.
- Do not copy any prompts, code, summaries, datasets, PDFs, HTML, exports, cached responses, or fixtures from an existing NCF bot or parent/current-bot path.
- Do not use files outside `ncfbot/` as factual inputs. `PUBLICBOT-PLANS/PLAN-1.md` through `PLAN-7.md`, if present, are design proposals only. Their links and factual claims require fresh independent verification.
- No private accounts, records, tokens, saved authenticated sessions, MyNCF, Canvas, or intranet integrations. Only the public course-search workflow may use its own transient anonymous session cookie.
- Use original summaries with citations. Keep raw public downloads in Agent 5's gitignored cache; do not stage them before cache exclusions exist. Never commit credentials, cookies, local environments, or private responses.
- Build small synthetic public-like test fixtures from scratch. Default tests must not require network access or an API key.

## Ownership and branches

| Team | Branch | Owned files and directories |
|---|---|---|
| Agent 1 | `agent-1/bootstrap-governance` | `AGENTS.md`, `README.md`, `CONTRIBUTING.md`, `docs/architecture.md`, `docs/public-source-policy.md`, `docs/integration-contracts.md`, `resources/README.md` |
| Agent 2 | `agent-2/student-domain` | `skills/students.md`, `resources/students/**`, `resources/shared/academic-calendar.md` and sidecar, `evaluations/questions/students.jsonl` |
| Agent 3 | `agent-3/faculty-domain` | `skills/faculty.md`, `resources/faculty/**`, `resources/shared/office-routing.md`, `resources/shared/sensitive-referrals.md` and sidecars, `evaluations/questions/faculty.jsonl` |
| Agent 4 | `agent-4/outside-domain` | `skills/outside.md`, `resources/outside/**`, `resources/shared/glossary.md` and sidecar, `evaluations/questions/outside.jsonl` |
| Agent 5 | `agent-5/source-pipeline` | `.gitignore`, `docs/source-pipeline.md`, `resources/shared/source-policy.md` and sidecar, `resources/inventory/**`, `resources/generated/**`, `schemas/source-record.schema.json`, source tools and tests below |
| Agent 6 | `agent-6/course-data` | `docs/course-data.md`, `resources/courses/**`, `schemas/course-section.schema.json`, course tools and tests below, `evaluations/questions/courses.jsonl` |
| Agent 7 | `agent-7/integration-evaluation` | `pyproject.toml`, `ncfbot/**`, `schemas/evaluation-case.schema.json`, `evaluations/questions/cross-cutting.jsonl`, integration tests below, `docs/integration-report.md` |

Matching `.source.json` sidecars belong to the resource's owner.

- Agent 5 tools: `tools/survey_sources.py`, `fetch_sources.py`, `convert_sources.py`, `validate_sources.py`, and `check_freshness.py` in `tools/`. Tests: `tests/test_source_pipeline.py` and `tests/fixtures/source-pipeline/**`.
- Agent 6 tools: `tools/discover_public_terms.py`, `fetch_public_courses.py`, `fetch_course_details.py`, `build_course_history.py`, `query_courses.py`, and `poll_live_sections.py` in `tools/`. Tests: `tests/test_courses.py` and `tests/fixtures/courses/**`.
- Agent 7 tests: `tests/test_router.py`, `tests/test_retrieval.py`, `tests/test_repository_contract.py`, and `tests/test_evaluation_data.py`.
- `PLAN-distributed.md` belongs to the manager. Agent 7 may coordinate narrowly scoped README command corrections with Agent 1, not rewrite the master rules or domain facts without their owners' review.

Do not create placeholders in someone else's area. Missing dependencies must be documented, not filled with fabricated facts or substitute tools. Propose cross-owned fixes in a small coordinated PR with the relevant owner's approval and public evidence for factual changes.

## Merge order

1. Merge Agent 1's reviewed bootstrap contracts first.
2. Agents 2–6 branch from that merged commit and work in parallel on disjoint files.
3. Agent 7 can develop after bootstrap merges, but must rebase and submit the final integration PR after Agents 2–6 merge.
4. Agent 7 runs the combined suite and reports gaps; do not weaken tests or rewrite another owner's facts to hide failures.

Use the assigned branch, keep commits topic-focused, and inspect the diff before staging. Do not force-push shared history or publish changes without the project contributor's authorization.

## Authoring contracts

Follow [docs/integration-contracts.md](docs/integration-contracts.md), [docs/public-source-policy.md](docs/public-source-policy.md), and [resources/README.md](resources/README.md).

- Factual resource Markdown needs a title, scope, `Verified through: YYYY-MM-DD`, applicability, stable versus volatile content, conflicts/gaps, and a final `Sources` section.
- Every factual resource has a neighboring `<name>.source.json` with a unique stable ID, source URLs, retrieval timestamps, hashes, public-access verification, status, volatility, and review date.
- Resource prose and sidecar source URLs must agree. Never fabricate a hash or timestamp to satisfy a schema.
- Three role skills share the required section structure and contain behavior, not duplicated dates, costs, contacts, or policy text.
- Reference a shared resource by its stable ID/path rather than maintaining another copy. Agree on cross-team IDs when their owning resource first lands.
- Evaluation JSONL uses behavior-oriented expectations and stable IDs, not exact generated answers or sensitive real records.
- Contract examples contain placeholders and are not verified institutional resources. Do not copy them into the corpus unchanged.

## Verification before a PR

Run these commands when the corresponding implementation exists; record missing dependencies as not run rather than passed:

```sh
git diff --check
python tools/validate_sources.py --all
python tools/check_freshness.py --offline
python -m ncfbot doctor
python -m ncfbot evaluate
```

Run the owned offline unit tests using the test runner documented by the implementation owner/Agent 7. Network smoke checks are optional, separate, and dated. Include all commands, results, and known failures in the PR.

For Agent 1 specifically:

- Check the seven-file allowlist and confirm `PLAN-distributed.md` is unchanged.
- Check Markdown links to existing documentation. Treat future exact plan-owned paths as declared dependencies, not as files to create.
- Manually trace at least twelve prompts: three per audience, two role-independent, and one ambiguous. Record expected paths and limitations in `docs/architecture.md`.
- Review privacy, source-instruction injection, freshness, emergency, and academic-integrity branches.
- Ask another student to review only `docs/integration-contracts.md` before merge. A self-review or an automated check does not replace that student review.

## PR description template

Copy the following sections into the PR description and fill them honestly:

```markdown
## Scope and ownership
- Team/branch:
- Changed owned files:
- Coordinated cross-owner changes, if any:

## Independently consulted public sources
- Original public URLs:
- Retrieval commands or method:
- Retrieval/verification dates:
- For governance-only work: identify the public build plan; state that no NCF factual corpus was collected.

## Validation
- Commands and results (including git diff --check):
- Manual checks:
- Tests not run and why:
- Network checks, separately dated:

## Known gaps and dependencies
- Missing public information, conflicts, stale sources, or unmerged files:
- Required owner/student review:

## Clean-room attestation
- All source material used for this contribution was independently consulted at its original public location.
- No material was copied from the existing bot, a parent/current-bot directory, private systems, or out-of-repository factual files.
- No secrets, cookies, private records, raw private responses, or local caches are included.
```

For Agent 1, the independently consulted design input is the public repository's [PLAN-distributed.md](https://github.com/mhulden/ncfbot/blob/main/PLAN-distributed.md); this contribution must not claim it scraped or verified NCF institutional facts.
