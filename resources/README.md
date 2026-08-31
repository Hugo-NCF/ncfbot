# Resource Authoring and Use

This file describes the repository's resource contract; it is not a factual NCF resource and does not need a provenance sidecar. No domain knowledge base is included in Agent 1's bootstrap contribution.

## Organization and ownership

| Location | Owner | Contents |
|---|---|---|
| `students/` | Agent 2 | Academic/student rules, procedures, and support summaries |
| `faculty/` | Agent 3 | Public faculty operations, advising workflows, and coverage gaps |
| `outside/` | Agent 4 | Admissions, programs, costs, campus life, and visitor orientation |
| `shared/academic-calendar.md` | Agent 2 | Canonical term-labeled academic dates and conflicts |
| `shared/office-routing.md`, `shared/sensitive-referrals.md` | Agent 3 | Canonical routine and sensitive referrals |
| `shared/glossary.md` | Agent 4 | Verified terminology and applicability |
| `shared/source-policy.md` | Agent 5 | Bot-facing source-use guidance |
| `inventory/` | Agent 5 | Metadata-only discovery and documented approval decisions |
| `generated/` | Agent 5 | Rebuilt combined source manifest; not manually edited facts |
| `courses/` | Agent 6 | Public-term catalog, section archive, current snapshots, scans, and history |

Paths above are repository-relative beneath `resources/` and are planned dependencies until their owners merge. Do not create placeholder files or sidecars on another team's behalf.

## Naming and provenance

- Use descriptive lowercase kebab-case filenames: `topic-name.md` with `topic-name.source.json` beside it.
- Choose a stable, globally unique resource ID; use a domain prefix such as `students-`, `faculty-`, `outside-`, or `shared-` to reduce collisions. Keep IDs stable across ordinary content updates.
- The sidecar `resource_file` is relative to the repository root, for example `resources/students/topic-name.md`, not relative to the sidecar.
- Use `students`, `faculty`, and/or `outside` in `audiences`; a shared resource can list all three. `shared` is a location, not an audience or skill.
- Topics are descriptive lowercase kebab-case tags. Reuse an existing relevant tag when possible.
- Record real canonical public URLs, body hashes, retrieval timestamps, effective periods, public-access verification, resource status, volatility, and review date. See [integration contracts](../docs/integration-contracts.md) for exact fields and types.

## Resource Markdown template

The following is a structural example, not verified evidence. Replace placeholders only after independent collection and review.

```markdown
# Resource Title

Scope: Audience and questions covered; exclusions.
Verified through: YYYY-MM-DD
Applies to: Program level, catalog/cohort year, term, or effective period where relevant.

## Stable explanation
Original, source-supported explanation.

## Time-sensitive information
Explicit term/year and verification context; never claim a snapshot is live.

## Next steps and boundaries
Verified public route, shared office-resource reference, and authenticated/individual-case limits.

## Conflicts and gaps
Conflicting claims, source authority, missing public details, and who must confirm.

## Sources
- [Descriptive original source title](https://example.org/replace-with-verified-public-source)
```

`Sources` must be the final section, and its URLs must agree with the sidecar's canonical URLs. Headings should be independently retrievable: keep applicable audience, term/year, and necessary warnings close to facts rather than only in distant introductory text. Do not duplicate long policies, page boilerplate, or a whole downloaded document.

## Sharing across roles

Skills link to a resource by repository path/stable ID and load its relevant heading plus provenance. Faculty advising uses Agent 2's academic rules, and all roles use the shared calendar, glossary, offices, and sensitive referrals. Do not maintain several copies of the same date, phone number, or policy.

When a shared dependency has not merged, record its intended owner/path and the gap. Do not invent its ID or facts. After merge, update only your owned topic map/evaluation expectations to the owner's published stable ID. A factual correction to another owner's resource needs that owner's review and the independently verified public source.

Governance lives in `AGENTS.md` and `docs/`; skills contain behavior. Institutional facts belong here with provenance. Inventory candidates and conversion output are not approved answer evidence merely because they exist locally.

## Review schedule and updates

- Choose `daily`, `term`, `annual`, or `stable` as described in [public-source policy](../docs/public-source-policy.md).
- Set an explicit `review_after` date and review earlier if the source changes or applicability is uncertain.
- Identify historical and superseded resources; never silently present them as current rules. Applicability to an earlier cohort requires explicit evidence.
- Record conflicts in both resource prose and sidecar `notes`; create a behavior-oriented evaluation case.
- When an approved source changes, review and update the authored summary and sidecar together. Do not merely move the review date forward to suppress a warning.
- Raw downloads remain in a gitignored cache. Keep original public provenance even when a source is later removed.

## Validation workflow

Agent 5 will implement `python tools/validate_sources.py --all` and `python tools/check_freshness.py --offline`. They are reserved interfaces at bootstrap, not commands that have already passed. Network rechecks are explicit and separate.

Until tools arrive, manually verify the Markdown/sidecar pairing, required keys, ID uniqueness, root-relative path, actual public URLs, source-footers agreement, genuine hashes/timestamps, applicability, approval, and review dates. After they merge, run the validator and record its result in your PR.

The generated manifest is rebuilt from sidecars. Do not hand-edit it or a central list to conceal invalid metadata. Agent 7's retrieval/health checks must distinguish factual resources from this documentation file, inventory reports, generated metadata, and course artifacts.

## Course artifacts

`courses/public-terms.json` records the public terms actually discovered. `courses/historical-sections.jsonl` is the canonical offline listing archive with one record per section and explicit coverage/completeness metadata in companion artifacts. Agent 6 documents the current snapshot and other generated outputs in `docs/course-data.md`.

Use course tools to scan, filter, inspect sections, and compare observed history. Fetch details lazily for shortlisted historical sections. Current seats, waitlists, or open/closed status require a successful fresh public lookup and enrollment timestamp. Course-code/title similarities do not establish equivalency. Course JSON/JSONL follows its own schema; factual Markdown summaries in that area still need normal source footers and sidecars.
