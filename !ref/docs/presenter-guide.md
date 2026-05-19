# TaskForge Demo — Presenter Cue Card

> **Audience:** Engineering manager + team
> **Total runtime:** 35 min hard cap (5 min buffer in a 40 min slot)
> **Mode:** Live execution, with pre-recorded screenshots as backup for every step
> **You read from this. The audience does not see it.**

---

## How to use this card

- Each step has: ⏱ time, 🎙 what to say, ⌨ what to type/click, ✅ what to verify before moving on.
- If a ✅ fails, jump to the matching **Fallback** entry at the bottom. Do not improvise.
- If you slip more than 2 min behind on any Act, cut to the **Trim** items marked `[CUT IF LATE]`.
- Read the literal prompts in `⌨` — do not paraphrase. The Orchestrator behaves better with the planned wording.

---

## T-30 min — Pre-flight (do BEFORE audience joins)

Run this list top-to-bottom. Tick every box. Do not start the demo with any box unchecked.

- [ ] **Hosts file**: `type C:\Windows\System32\drivers\etc\hosts | findstr taskforge` returns `127.0.0.1 taskforge.local`. If missing, open Notepad **as Administrator** and add the line.
- [ ] **DB folder exists**: `dir D:\Work\db\PostgreSQL-TaskForge` returns OK (create if missing).
- [ ] **Clean DB state**: `docker compose down -v` then verify the bind-mount folder is empty (or just deleted + recreated).
- [ ] **.NET SDK**: `dotnet --list-sdks` shows a `10.x` line.
- [ ] **Node + pnpm**: `node -v` ≥ 22 and `pnpm -v` works.
- [ ] **`dotnet-ef`**: `dotnet ef --version` works (if not: `dotnet tool install --global dotnet-ef`).
- [ ] **Docker**: `docker info` returns server info, Docker Desktop tray icon green.
- [ ] **GitHub CLI**: `gh auth status` shows logged in to the demo account.
- [ ] **Cert**: `dir docker\certs\taskforge.local.pfx` exists. If not, run `cert.bat setup` now (NOT during the demo).
- [ ] **Browser cleared**: open Chrome/Edge, navigate to `https://taskforge.local:44385` once and dismiss any cert warning. (You don't want to do this on camera.)
- [ ] **Windows arranged**:
  - Window 1 (left, ~60%): Claude Code session, project open at `D:\Work\AI\TaskForge`.
  - Window 2 (right top): terminal at repo root.
  - Window 3 (right bottom): browser, tabs open to GitHub repo + `https://taskforge.local:44385`.
- [ ] **Notifications muted**: close Slack, Teams, email, Discord. Phone on silent.
- [ ] **Screen-share rehearsal**: confirm font size ≥ 14pt in terminal and Claude Code.
- [ ] **Backup folder open** in Explorer: `!ref/docs/screenshots/` — every Act has a fallback PNG here.
- [ ] **This card open** on a second monitor or printed.

> If any box is red and you can't fix it in 2 minutes, **start with the pre-recorded video** and narrate over it. Do not start a broken live demo.

---

## Opening — 60 seconds

🎙 *"I'm going to show you the Claude Code multi-agent loop on a real, small project. The point isn't the app — it's the loop: planner, implementer, reviewer, simplifier, all coordinating. You'll see two features built end-to-end, two PRs opened, and one reusable skill extracted on the fly. Total time: about half an hour."*

🎙 *"What you will not see: a polished UI, real auth, production deployment. Those are out of scope on purpose — they'd hide the loop behind production noise."*

🎙 *"Questions: hold them to the end so we stay on time. If something visibly breaks, I have screenshots for every step — we keep moving."*

---

## Act 1 — Setup (5 min) ⏱ T+1 → T+6

### 1.1 — Show the empty repo · 30s

⌨ Terminal: `ls`
🎙 *"Empty repo. README placeholder. `.claude/` already has `CLAUDE.md` with conventions seeded — IDs are GUIDs, timestamps are UTC, errors are RFC 7807. That's the baseline `claude-md-management` maintains."*
✅ You see `.claude/`, `CLAUDE.md`, `README.md`, and the `!ref/` planning folder. Nothing else.

### 1.2 — Install a plugin live · 90s

🎙 *"All twelve plugins are pre-installed except one — `frontend-design`. I'll install it live so you see the mechanics."*

⌨ In Claude Code: open the plugin browser, search `frontend-design`, click Install.
✅ Plugin appears in the skills list. Confirm with `/help` or the skills panel.

🎙 *"Now `frontend-design` is available as a skill. We'll use it later for the task detail page."*

### 1.3 — Show CLAUDE.md · 30s

⌨ Open `CLAUDE.md` in Claude Code.
🎙 *"Conventions live here. Notice the section on entity timestamps and the rule that Notes bodies store raw markdown. `claude-md-management` will update this file as we go."*
✅ File is visible, top section shows TaskForge-specific conventions.

### 1.4 — Boot Postgres · 90s

⌨ Terminal: `docker compose up -d db`
⌨ Then: `docker compose ps`
✅ `taskforge-db` shows `(healthy)`. The bind-mount folder `D:\Work\db\PostgreSQL-TaskForge` now contains `PG_VERSION`, `base/`, `pg_wal/`, etc.

🎙 *"Postgres 18, external volume on a fixed path on disk. No surprises if I rebuild containers later — the data survives."*

### 1.5 — Quick orientation · 30s

🎙 *"Plan lives at `!ref/plans/01. initial plan.md`. The Orchestrator reads it. I won't open it — you'll see its decisions in the chat."*

---

## Act 2 — `Tasks` slice via the Orchestrator loop (12 min) ⏱ T+6 → T+18

### 2.1 — Kick off the slice · 30s

⌨ In Claude Code, paste verbatim:

```
Start the Tasks slice per !ref/plans/01. initial plan.md Phase 3. Drive the full
Orchestrator loop: plan → implement → test → code-review → code-simplifier on
TasksController.cs → claude-md-management revise → commit-push-pr.
```

🎙 *"Watch the Orchestrator decompose this into ≤5 tasks and dispatch the first one."*

### 2.2 — Narrate decomposition · 60s

🎙 *"There: it produced a task list. Each item names the file it'll create. Now the Implementer takes the first one — scaffolding the API project."*

✅ You see a numbered task list in the chat. First task starts executing.

### 2.3 — Implementer phase · 4–5 min

Implementer scaffolds:
- `TaskForge.Api.csproj` + Program.cs
- `Models/TaskItem.cs`, `Data/AppDbContext.cs`
- `Controllers/TasksController.cs` (5 endpoints)
- `Validators/TaskItemValidator.cs`
- Migration `0001_init`
- `TaskForge.Api.Tests` with `WebApplicationFactory` + `Testcontainers.PostgreSql`

🎙 (while it runs) *"Notice the Implementer is not asking me anything — `CLAUDE.md` tells it the conventions, and that's enough."*

✅ Implementer reports green build and green `dotnet test`.

> **[CUT IF LATE]**: if Implementer is still working at T+12, skip 2.3's narration and jump straight to 2.4 the moment tests are green.

### 2.4 — Code review · 2 min

⌨ `/code-review`
🎙 *"This runs against the working diff. Findings are scored by severity — only `high`/`error` block the loop."*
✅ Reviewer returns ≥1 finding. Implementer addresses it. Loop re-runs `dotnet test` and is green again.

> **Fallback**: if reviewer finds nothing, open `TasksController.cs` and point to the seeded issue. Don't pretend the reviewer caught it.

### 2.5 — Simplify · 90s

⌨ `/code-simplifier api/TaskForge.Api/Controllers/TasksController.cs`
🎙 *"Watch the LOC drop in the diff. The Orchestrator decides whether to accept — it's not automatic."*
✅ Diff is shown. Accept it. Re-run tests — still green.

### 2.6 — Sync CLAUDE.md · 30s

⌨ `/claude-md-management revise`
🎙 *"It noticed we introduced FluentValidation and the WebApplicationFactory pattern. New convention rows added."*
✅ `CLAUDE.md` diff shows additions.

### 2.7 — Open PR · 60s

⌨ `/commit-push-pr`
✅ Browser auto-opens (or click the URL) to a live PR on GitHub. CI is running.

🎙 *"That's one slice through the loop. Now the interesting part — making the next slice cheaper."*

---

## Act 3 — Skill extraction (3 min) ⏱ T+18 → T+21

### 3.1 — Author the skill · 90s

⌨ `/skill-creator new add-crud-resource`
🎙 *"This reads what the Implementer did for Tasks and writes a reusable skill: scaffold an entity + controller + validator + integration tests, following our conventions."*
✅ File appears at `.claude/skills/add-crud-resource/SKILL.md`.

### 3.2 — Show the skill content · 90s

⌨ Open `.claude/skills/add-crud-resource/SKILL.md` in Claude Code.
🎙 *"It encodes the pattern, not the specific names. Next slice will use it."*
✅ File is visible. Point at one section (e.g. "Steps") and one frontmatter line (`description`).

---

## Act 4 — `Notes` slice with markdown (10 min) ⏱ T+21 → T+31

### 4.1 — Kick off, referencing the skill · 30s

⌨ Paste verbatim:

```
Start the Notes slice per !ref/plans/01. initial plan.md Phase 5. Invoke the
add-crud-resource skill. Note.body is markdown — wire react-markdown +
remark-gfm + rehype-sanitize on the frontend. The /tasks/:id page is the
target.
```

### 4.2 — Watch the skill being used · 4 min

🎙 *"Notice the Implementer pattern-matches to the skill. The scaffolding lines up — but the schema is Notes-specific."*
✅ Implementer produces `NotesController.cs`, `Note` entity, `0002_add_notes` migration, integration tests. Tests go green.

> **[CUT IF LATE]**: if past T+27 and tests aren't green, skip 4.3 and jump to 4.4.

### 4.3 — Frontend markdown · 2 min

🎙 *"On the frontend we add `react-markdown`. Watch a note render with bold, lists, and a code block."*
⌨ Browser: open `https://taskforge.local:44385`, click into a task, type a markdown note like:

```
**bold**, a list:
- one
- two

```ts
const x = 1;
```
```

✅ Renders with formatting.

### 4.4 — Playwright smoke · 60s

⌨ Terminal: `pnpm --filter e2e test` (or wherever the smoke lives — check before the demo)
✅ Playwright spec runs and passes. Show the line count: one test, green.

### 4.5 — Review + PR · 2 min

⌨ `/code-review`
✅ Finding raised. If reviewer is quiet, point to the seeded issue in `NotesController.cs` (e.g. missing 404 on PUT).
⌨ `/commit-push-pr`
✅ Second PR open on GitHub.

---

## Act 5 — Wrap (3 min) ⏱ T+31 → T+34

### 5.1 — README plugin table · 60s

⌨ Open `README.md`, scroll to "Plugins used and why".
🎙 *"Six plugins, each with a real job. `superpowers` ran the loop. `skill-creator` made the skill you saw. `claude-md-management` kept conventions honest. `code-review` blocked exit on real findings. `code-simplifier` cut LOC on the controller. `github` opened both PRs."*

### 5.2 — Other six in one line each · 45s **[CUT IF LATE]**

🎙 *"The other six: `frontend-design` we installed live; `playwright` ran the E2E; `security-guidance` would own `SECURITY.md` if enabled; `ralph-loop` for stubborn iterative tasks; `typescript-lsp` for type-aware refactors; `terraform` deliberately skipped — no cloud target, including it would be theatre."*

### 5.3 — Retro · 75s

🎙 *"One-page retro. What paid off: the loop. The Implementer doesn't ask me trivia because `CLAUDE.md` answers it. What was friction: tuning reviewer severity took two passes. What I'd cut next time: `code-simplifier` saved LOC but the reviewer would've flagged the same lines — pick one."*

🎙 *"Two PRs, one reusable skill, full transcript linked in `DEMO_SCRIPT.md`. Questions?"*

---

## Fallback table — what to do when a thing breaks

| Symptom | Action |
|---|---|
| `docker compose up -d db` fails | `docker compose down -v`, retry. If still failing: fall back to `dotnet run` against a named docker volume (compose file has both configured). |
| `cert.bat` cert not trusted, browser blocks frontend | Click "Advanced → Proceed" once on camera. Don't try to fix the trust store live. |
| Orchestrator hangs >60s on any step | Type *"skip this step and continue"*. Show the matching screenshot from `!ref/docs/screenshots/`. |
| `/code-review` finds nothing | Open the file and point to the seeded issue. Be honest: *"the reviewer didn't catch this one — let me show you what a real finding looks like."* |
| `dotnet test` red | Read the failure aloud, let the Implementer self-heal. If two cycles don't fix it, switch to screenshots. |
| `pnpm test` red | Same: one self-heal cycle, then screenshots. |
| Plugin command not found | Confirm spelling, then try the alternate form (e.g. `/code-review` vs `/review`). If still missing: skip the act, narrate from the screenshot. |
| `gh` not authenticated | `gh auth login` — but only if you have 30s. Otherwise show the prepared PR URL from a previous rehearsal. |
| `taskforge.local` won't resolve | Confirm hosts file again. If broken, fall back to `https://localhost:44385` for the frontend and `https://localhost:44380` for API — works because the cert SAN includes `localhost`. |
| Running >2 min late at any boundary | Cut the marked `[CUT IF LATE]` sections in the next Act. Do not cut Acts 2.7, 4.5, or 5.1 — those are the receipts. |

---

## Hard rules

1. **Don't apologize for the demo.** If something breaks, narrate the recovery as the point. The audience came to see how this works under load.
2. **Don't open the plan file on camera.** It's long and looks like cheating. Quote it instead.
3. **Don't read this card aloud.** Glance at it. The phrasing is yours.
4. **Don't accept a `code-review` waive without saying why on camera.** The severity policy is part of the story.
5. **If you finish early, stop.** Don't fill with rambling — open Q&A.

---

## After the demo

- [ ] Save the Claude Code transcript (Session → Export).
- [ ] Commit and push any uncommitted demo-day fixes.
- [ ] Write the 1-page retro **today** (memory decays fast).
- [ ] Note in the retro: which fallback rows fired, which never did. That tells you what to cut from the next demo.
