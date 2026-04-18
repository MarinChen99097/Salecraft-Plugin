# Dogfooding Findings — Plugin × Real LLMs

Living document. Each finding is what broke or surprised when a real LLM
(Gemini Web, Claude.ai with MCP, ChatGPT Plus, Claude Code) ran the
SaleCraft plugin against the production backend.

## Format

| # | Title | Severity | Status | Owner |
|---|-------|----------|--------|-------|
| Severity: 🔴 critical / 🟠 high / 🟡 medium / ⚪ low / informational
| Status: open / fixed / wont-fix / needs-clarification

---

## Findings

### #1 — Sprint Plan guard not strong enough 🟠 fixed (plugin)
LLMs default to "be helpful, write the strategy" when user asks for paid
execution, instead of triggering the AI Token flow first. **Fix**: added
the `🚨 FIRST-RESPONSE RULE` at the top of `CLAUDE.md` — when user uses a
"do" verb on a paid output, the AI's first reply must be ONLY (1) cost
sentence (2) AI Token 3-step prompt (3) optional ≤1-line scope question.
No Hero Section text allowed in this turn.

### #2 — Brand authorization check missing 🟡 needs-clarification
Originally raised by Claude.ai but without specifics. Possible meaning:
the AI should verify the user has the right to use a brand's name/assets
before generating LPs (e.g., shouldn't generate "Apple iPhone LP" for
random user). Needs follow-up dogfooding to confirm.

### #3 — `authenticate_with_token` MCP tool returns 404 instead of 401 🔴 fixed (backend env)
Root cause: `marketing-backend-staging` was missing the
`AI_TOKEN_ENABLED=true` env var, so `/auth/ai-token/exchange` returned
404 instead of 401 INVALID_AI_TOKEN. The `service-system-staging` MCP
proxy faithfully relayed the 404, so MCP-using LLMs saw `NOT_FOUND`
errors and got confused (one diagnosed it as "JWT secret mismatch" —
wrong direction). **Fix**: added env var to staging Cloud Run service +
forced traffic to latest revision. Verified B站 now returns proper 401.

### #4 — Staging vs production token isolation has misleading errors 🟡 wont-fix
Reported by Claude.ai but turned out to be wrong diagnosis: A站 and B站
share the same Cloud SQL DB and same JWT `SECRET_KEY` (both via
GCP Secret Manager). Tokens issued from either side validate on either
side identically. The only difference users see is the URL hash format
in Cloud Run URLs (`s6ykq3ylca-de` vs `876464738390` — both route to
the same service). No real environment-isolation issue exists.

### #5 — `list_pricing_plans` requires `user_token` 🟠 open (backend)
Pricing should be public information so AI agents can quote costs during
the pre-paid consultation phase. Currently AI must wait until the user
authenticates to even tell them what things cost. Backend fix: make
`/pricing/plans` (and similar read-only pricing endpoints) public.

### #6 — No lightweight `/auth/validate` endpoint 🟠 open (backend)
After exchanging a `sc_live_*` for an `access_token`, the AI has no
cheap way to confirm the token is still valid + see remaining scope/credits
without calling `/auth/me` (which loads full user profile). For polling
and pre-flight checks, a `/auth/validate` returning
`{valid: bool, scope: str, credits: int, expires_at: str}` would let the
AI verify before committing to a paid call.

### #7 — Rung 2/3 sandbox egress allowlist not handled 🟠 fixed (plugin)
Claude.ai's bash and Python sandboxes have a domain allowlist that
excludes `*.run.app`, blocking direct REST to the SaleCraft backend
even though the AI has the right tool. The original capability ladder
assumed all bash/python environments allow arbitrary HTTPS, which isn't
true in commercial chat products. **Fix**: added Rung 2.5 to `CLAUDE.md`
— LLMs first probe whether the sandbox can reach `*.run.app`; if not,
they tell the user explicitly which platforms work (Claude Code,
Cursor, Cline, Gemini Code Execution) instead of silently failing or
generating curl for the user to run.

### #8 — (folded into #3)

### #9 — `brand_name` stays `None` when `create_session` lacks `brand_id` 🟡 fixed (plugin doc)
`POST /sessions/` doesn't auto-create a brand from `brand_name`/
`product_name` payload — it only joins to an existing brand via
`brand_id`. If LLM skips `POST /brands/` and goes straight to
`create_session`, the session has `brand_name=None`, downstream agents
lose brand context, and resulting LP visuals come out generic.
**Fix**: added "Pre-flight gotchas" section to `skills/generate-landing/
SKILL.md` documenting the required order: `analyze_brand_url` → `POST
/brands/` → `create_session(brand_id=...)` → ...

### #10 — LP time estimate hallucinated as "1-3 minutes" 🟡 fixed (plugin)
Claude.ai told the user "Pipeline 會跑約 1-3 分鐘" when the plugin
actually documents `~30 min` for full LP generation in 4 places
(CLAUDE.md L515, L567, L577, L691). Likely confused with Quick Ad
(~5 min) or with the Strategist+Architect early phases (which are
fast). **Fix**: time guidance was already in plugin; no further
plugin change needed. This is an LLM hallucination problem the plugin
can't fully prevent — but the FIRST-RESPONSE rule's cost sentence
should now also include time estimate to anchor expectation.

### #11 — Carousel `num_images` parameter typo silently uses default 🔴 fixed (plugin doc + pending backend)
LLM (Claude.ai) sent `{"stripe_count": 10, ...}` to `generate_carousel`
expecting 10 slides. Backend's `GenerateCarouselRequest` Pydantic model
doesn't set `extra="forbid"`, so the unknown field was silently dropped
and `num_images` defaulted to 5. User got a 5-slide carousel for
800 pts instead of the 10-slide they expected (1,300 pts) — and the
LLM didn't notice until after the carousel completed.
**Fix (plugin)**: documented exact field name in `lib/rest-api-direct.md`
and `skills/generate-landing/SKILL.md` pre-flight gotchas.
**Fix (backend, pending)**: add `extra="forbid"` to all paid request
models so wrong field names return 422 immediately.

### #12 — `generate_session` defaults to 10 stripes when `stripe_count` omitted 🟠 fixed (plugin doc)
Same class of bug as #11: LLM created a session intending 8-page LP
(1,600 pts/TA quote it gave the user) but didn't pass `stripe_count`
to `generate_session`. Backend defaulted to 10 stripes (2,000 pts/TA),
overcharging by 400 pts/TA × 2 TAs = 800 pts in this run.
**Fix (plugin)**: pre-flight gotcha documented; FIRST-RESPONSE rule
now forces the AI to ask "8-page or 10-page?" before triggering and
to pass `stripe_count` explicitly.
**Fix (backend, optional)**: same `extra="forbid"` hardening +
consider making `stripe_count` required (no default) so callers
can't accidentally over-spend.

---

## How to add a new finding

When you (an LLM or human) hit a new edge case during dogfooding:

1. Append a new entry to this file with the next # in sequence.
2. Severity + status badges at the top.
3. Brief root-cause explanation.
4. If you fixed it: note where (plugin / backend / both) and what you
   changed. If pending: note who owns the fix and approximate effort.
5. Don't include AI Tokens, real test account emails, real user data,
   or stack traces with internal hostnames in this file.
