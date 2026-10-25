# BrainWhite: Week-1 Plan (7-day MVP demo)

This plan overrides the timeline and scope of [ENGINEERING.md](ENGINEERING.md) for the first week. Everything not listed as "in" here is **deferred, not cancelled**. Setup commands are in [SETUP.md](SETUP.md).

> Honest warning: ENGINEERING.md describes a product that takes months. This plan builds a thin end-to-end slice that works on one public repo. It is a demo, not a product anyone should rely on. Estimates are my guesses and nothing here has been tried.

## 1. What "done" means [Decided]

- Goal: an **internal demo for the team**, running locally.
- Input: a **public GitHub repo URL or a zip upload**. No private customer code anywhere.
- Capacity: all four people, full-time, 7 days.

**Done** = the demo script (section 5) runs end to end on one public repo, three times in a row, and the checks in section 6 pass.

## 2. Scope

### In

1. **Add a repo** (public URL or zip) and watch the job status.
2. **Ingestion:** fetch, filter (ignore rules), simple secret scan, parse symbols and imports, detect technologies from manifests, chunk, embed, store in PostgreSQL with pgvector.
3. **Ask a question** about the repo. Answer includes citations (file, lines, symbol). Code validates every citation before showing it.
4. **Developer profile:** a short form (technologies and level for each).
5. **Skill gap:** profile versus technologies the project uses, with a reason for each gap.
6. **Learning path:** 3-5 modules for that developer, each with real code examples (validated citations) and a mini task. Two different profiles must produce different paths.
7. **Assessment:** 4-5 multiple-choice questions per module, generated from the repo, scored deterministically. Saves skill evidence.
8. **Readiness view:** per skill evidence and one of the five categories (Not assessed ... Ready). Manager view is one table.
9. Thin supporting pieces: `organization_id` on every table, a scoped query wrapper, a jobs table and worker, an `ai_runs` log, a secret scan before any LLM call, one ingestion audit row per job.

### Out (deferred)

GitHub App and webhooks, incremental sync, real login and roles (use a seeded dev user and org), SSO, Row-Level Security policies (columns and wrapper now, policies only if time allows), tutor chat with state machine, LLM grading and double grading, architecture graph UI (a symbols list is enough), manager dashboards beyond one table, hosting, CD, staging, DPA, audit log screens, multi-language beyond the two below.

### Languages

Python parser first. TypeScript parser on Day 4 if Python is stable. Pick the demo repo accordingly. **If time runs out, ship Python only.**

## 3. Day-0 blockers (resolve before Day 1 starts)

These stop everyone if missing. The lead resolves them today.

| # | Blocker | Needed |
|---|---|---|
| 1 | LLM provider, model and API key | A working key with a small budget. ENGINEERING.md open decision 2. |
| 2 | Embeddings | Which embedding model (provider API or a local model). New open decision. |
| 3 | Demo repo | One small public repo (a few hundred files or less) with tests and a README. A second as backup. |
| 4 | Dev auth | Decided for this week: a seeded dev user and org, no login screen. |
| 5 | GitHub repo created and everyone invited | SETUP.md section 3 |
| 6 | Folder names agreed | `backend/`, `apps/web/` as in SETUP.md 6.4 |

## 4. Ownership and day-by-day

| Person | Week-1 focus |
|---|---|
| P1 Product/Frontend | The five screens and the demo script |
| P2 Backend/Platform | API, database, jobs, repo fetch |
| P3 AI/Code Intelligence | Parsing, retrieval, citations, generation |
| P4 Infra/QA/Security | Setup, CI, tests, secret scan, tenancy, eval set |

Lead: unblocks, owns the contracts, reviews pull requests, runs the daily 15-minute sync, and tracks the demo script.

### Day 0 (half day): setup

Follow SETUP.md: S-01 to S-05, S-07, S-08 skeleton, S-10. Lead resolves the blockers above.

### Day 1: contracts first, then parallel work

**Morning (2 hours, everyone): freeze contracts** as Pydantic schemas in `packages/api-contracts`:
`RepoJob`, `Symbol`, `Chunk`, `Citation`, `AskRequest/AskResponse`, `DeveloperProfile`, `SkillGap`, `LearningPath/Module`, `Question`, `SkillEvidence`, `ReadinessSummary`.
After this, everyone works against mocks and fake data.

| P1 | P2 | P3 | P4 |
|---|---|---|---|
| Screens as static mocks using fake data: Add repo, Ask, Profile and path, Assessment, Readiness | Tables and migrations (with `organization_id`), seed dev org and user, `jobs` table and worker loop, `POST /repos`, `GET /jobs/{id}` | Spike: tree-sitter symbols and imports on the demo repo (Python) | docker-compose, CI backend and frontend green, ignore-rules and zip safety checks (size, path traversal, symlinks) |

### Day 2

| P1 | P2 | P3 | P4 |
|---|---|---|---|
| Add repo screen wired to the real API, job progress polling | Repo fetch (git clone of a public URL, or unzip) inside the worker, writes files and the audit row | Symbol and import extraction stored in tables, technology detection from manifests | Secret scan (pattern-based) before parse and before any LLM call, canary-secret test |

### Day 3: first integration checkpoint

| P1 | P2 | P3 | P4 |
|---|---|---|---|
| Ask screen: question box, answer, clickable citations | `POST /ask`, scoped query wrapper, `ai_runs` logging | Chunking, embeddings, pgvector storage, hybrid retrieval (vector plus symbol and keyword), answer prompt, **citation validator** | Golden question set: 10 questions about the demo repo with the right files |

**Checkpoint (end of day):** add repo, then ask one question, get a validated citation. If this is not working, stop new work and fix it on Day 4 morning.

### Day 4

| P1 | P2 | P3 | P4 |
|---|---|---|---|
| Profile form and skill gap view | Profile and skill endpoints, skill-gap calculation with reasons | TypeScript parser (only if Python is stable). Learning-path generation prompt with validated citations. | Citation and retrieval evaluation run on the golden set, tenancy test harness (two orgs) |

### Day 5: feature freeze (end of day)

| P1 | P2 | P3 | P4 |
|---|---|---|---|
| Learning path screen with modules, code examples, mini tasks | Learning path endpoints, versioned paths, prerequisites check (no cycles) | Assessment generation (multiple choice), answer key stored, quality checks | Assessment scoring tests, readiness category tests |

No new features after Day 5.

### Day 6: assessment, readiness, hardening

| P1 | P2 | P3 | P4 |
|---|---|---|---|
| Assessment screen and readiness table | Scoring, `skill_evidence`, readiness endpoint | Fix quality of paths and questions, reduce wrong citations | Run the full demo script, fix flaky steps, RLS policies only if the rest is stable, log scan for secrets |

### Day 7: demo day

Morning: fix what the rehearsal broke. Run the demo script three times. Afternoon: demo and a 30-minute retro. Write down what was cut and why. Decide the next week from the deferred list.

## 5. Demo script

1. Open the app as the seeded dev org. Paste the public repo URL (or upload the zip).
2. Watch the job move through its stages to READY.
3. See detected technologies and a symbols list.
4. Ask two repo questions (one about architecture, one about a specific function). Open a citation and see the real lines.
5. Ask a question the repo cannot answer. The app says it has insufficient evidence.
6. Create **Developer A** (strong in the project's main framework) and **Developer B** (weak in it).
7. Show that their skill gaps and learning paths differ, with reasons.
8. Developer B takes one module assessment. Show the score and the evidence.
9. Open the readiness table and see B's categories change.
10. Show the job log and the `ai_runs` table: cost and tokens per run.

## 6. Done checks (the demo must meet these)

| Check | Target |
|---|---|
| Demo script | Runs end to end three times in a row |
| Citations shown to users | 100% pass validation (an invalid citation is never displayed) |
| Golden questions | At least 8 of 10 answered with correct file citations |
| Personalization | Two different profiles give different paths (a test asserts it) |
| Assessment | Answers come from a stored key, scored by code, not by the LLM |
| Secrets | Planted canary secret never appears in an LLM payload or a log |
| Safety | A zip with a path-traversal entry and a symlink is rejected |
| Tenancy | Every table has `organization_id`. A two-org test passes, or the missing RLS is listed as a known gap. |
| CI | Green on `develop` |

## 7. Working agreements for the week

- Daily 15-minute sync at the same time. Written update before it: yesterday, today, blockers.
- Pull requests small and reviewed within a few hours. A blocked review blocks the day.
- Mocks first: if another person's part is not ready, use the frozen contract and fake data.
- If something is behind by half a day, tell the lead at once. Cut scope, not quality of citations.
- Never use customer or private code. Public repos only.
- `main` and `develop` branches are kept as in SETUP.md, but there is no staging deploy this week. Merge to `develop` daily. Merge to `main` only on Day 7 for the demo build.

## 8. Risks

| Risk | Sign | Action |
|---|---|---|
| Day-0 blockers not cleared | People idle on Day 1 | Lead clears them first |
| Parsing takes longer than planned | No symbols by Day 2 end | Fall back to file-level chunks plus a simple regex symbol extractor |
| Retrieval answers cite wrong lines | Golden set below 8 of 10 | Tighten the citation validator, drop unverifiable claims, smaller chunks |
| LLM output is not valid JSON | Many retries | Schema-validated output with retry, simpler prompts, lower scope per call |
| Paths feel generic | Two profiles give similar paths | Make the skill-gap result the main input of the prompt, add a test |
| Cost surprise | `ai_runs` shows high spend | Cap tokens per job, cache by content hash, use the cheaper model for extraction |
| Integration happens too late | Checkpoint on Day 3 fails | Stop new work, fix on Day 4 morning |

## 9. After the demo

Look at what was cut, choose the next week from: GitHub App and incremental sync, real auth and RLS policies, tutor chat, LLM grading with a rubric, architecture explorer, hosting and CD, then talking to design partners (Phase 0 in ENGINEERING.md 15.3).
