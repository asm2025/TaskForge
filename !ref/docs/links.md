# Demo Reference Links

Curated links the agent (and you) can navigate to during the demo. Grouped by purpose. Where a link's stability is uncertain, that's noted.

> **Note on accuracy**: Claude Code plugins are a relatively new ecosystem and plugin names are often community-authored. The links below are the canonical entry points; if a specific plugin's repo URL has moved, search the Anthropic plugin marketplace or GitHub by plugin name. Do not assume a link is correct without opening it once.

---

## 1. Claude Code — Core

- [Claude Code overview](https://www.anthropic.com/claude-code) — product page
- [Claude Code docs](https://docs.claude.com/en/docs/claude-code/overview) — official documentation root
- [Plugins documentation](https://docs.claude.com/en/docs/claude-code/plugins) — how plugins, skills, and sub-agents work
- [Sub-agents documentation](https://docs.claude.com/en/docs/claude-code/sub-agents) — Orchestrator / Implementer model
- [Skills documentation](https://docs.claude.com/en/docs/claude-code/skills) — what `skill-creator` produces
- [`CLAUDE.md` reference](https://docs.claude.com/en/docs/claude-code/memory) — what `claude-md-management` manages

---

## 2. Core Plugins (the six you committed to)

Search-first links — open the marketplace and confirm the publisher before installing.

- [Claude Code plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugins#marketplace) — entry point
- `superpowers` — Orchestrator + sub-agent harness. Search the marketplace for "superpowers"; verify the publisher.
- `skill-creator` — typically maintained by Anthropic as a first-party skill authoring tool.
- `claude-md-management` — community plugin. Confirm the active repo before installing.
- `code-review` — multiple plugins share this name; pick the one whose sub-agent model matches your loop.
- `code-simplifier` — community plugin. Read its prompt before enabling; some variants are aggressive.
- `github` — first-party GitHub integration for branching, PRs, and review threads.

---

## 3. Conditional Plugins

- `frontend-design` — design-system / Tailwind-aware UI helper.
- `security-guidance` — useful only to produce `SECURITY.md`; avoid letting it expand scope.
- `ralph-loop` — iterative refinement loop; useful for flaky tests.
- [Playwright for Claude Code](https://github.com/microsoft/playwright-mcp) — official Microsoft Playwright MCP server, the most stable way to give agents browser control.
- `terraform` — **not used in this demo**, link omitted intentionally.
- `typescript-lsp` — TS language-server bridge for the agent.

---

## 4. Stack Documentation

### Backend (.NET)

- [.NET 10 docs](https://learn.microsoft.com/en-us/dotnet/) — verify version availability at demo time
- [ASP.NET Core minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis)
- [EF Core](https://learn.microsoft.com/en-us/ef/core/)
- [Npgsql provider](https://www.npgsql.org/efcore/)
- [FluentValidation](https://docs.fluentvalidation.net/)
- [Serilog](https://serilog.net/)
- [xUnit](https://xunit.net/) · [`WebApplicationFactory`](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests)

### Frontend (React + Vite)

- [React 19 docs](https://react.dev/)
- [Vite](https://vitejs.dev/)
- [TypeScript handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [TanStack Query](https://tanstack.com/query/latest)
- [Zustand](https://zustand-demo.pmnd.rs/)
- [Vitest](https://vitest.dev/) · [Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [Playwright](https://playwright.dev/)

### Database & Infra

- [PostgreSQL 16 docs](https://www.postgresql.org/docs/16/index.html)
- [Docker Desktop](https://docs.docker.com/desktop/)
- [Docker Compose reference](https://docs.docker.com/compose/compose-file/)

---

## 5. Conventions & Standards

- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [RFC 7807 — Problem Details for HTTP APIs](https://datatracker.ietf.org/doc/html/rfc7807)
- [Semantic Versioning](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)

---

## 6. Useful During the Demo

- [GitHub CLI](https://cli.github.com/) — if `github` plugin needs a fallback
- [HTTPie](https://httpie.io/) or [curl docs](https://curl.se/docs/) — for poking the API live
- [JSON Server's `db.json` format](https://github.com/typicode/json-server) — handy reference if anyone asks "why not just mock?"

---

## 7. Background Reading (optional, before the demo)

- [Anthropic — "Claude Code best practices"](https://www.anthropic.com/engineering/claude-code-best-practices) — confirm the URL at demo time; Anthropic engineering blog posts occasionally move.
- [Anthropic — Building agents documentation](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)

---

## Caveats

- **Empirical**: links 4 and 5 are stable, official documentation.
- **Likely-stable**: links 1 and 6.
- **Verify before relying on**: section 2 and 3 plugin links — the plugin ecosystem is young and repos move. Confirm the publisher in the marketplace before installing anything.