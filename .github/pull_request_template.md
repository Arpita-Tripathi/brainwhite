## What changed
<!-- 1-3 lines. What is the feature or fix? -->

## Why
<!-- Link the issue: Closes #123 -->

## How I tested it
<!-- Commands you ran, output, or screenshots. Only write "tested" if you really ran it. -->
- [ ] Unit tests added or updated
- [ ] CI checks pass locally (lint, types, tests)

## Checklist
- [ ] Type hints and schema validation on new inputs and outputs
- [ ] New table? It has an `organization_id` column, an RLS policy and a migration
- [ ] No secrets and no raw customer or private source code in code, logs, fixtures or prompts
- [ ] I did not import another module's internals (public interface only)
- [ ] Changed a contract in `packages/api-contracts`? Owners approved and the team was told
- [ ] Docs or ADR updated (if behavior or a decision changed)

## Security impact
- [ ] No security impact
- [ ] There is an impact: <!-- auth, tenant isolation, LLM payload, file handling, network, retention. Explain. -->

## AI-assisted?
- [ ] Yes, I read the whole diff myself and I understand it
- [ ] No

## Risks and rollout notes
<!-- Feature flag? Migration order? How to roll back? -->
