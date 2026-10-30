# AI-securevibe

**A rule file that makes AI coding agents build fast, small, and secure, by default.**

AI writes code quickly. It also writes more code than needed, invents packages that don't exist, hides errors, and skips security. This repo gives your AI agent one set of rules so it handles that on its own, without you asking every time.

Drop `AGENTS.md` into your project. Your agent reads it and follows it.

## What it does

The agent builds on its own and runs security checks in the background after every change. It only stops to ask you before risky actions (deleting data, touching production, changing secrets or CI).

| Area | What the agent does |
|---|---|
| **Keep code small** | Searches for existing code first. Deletes dead code and duplicates. No extra abstractions. Smallest change that works. |
| **Secure by default** | Treats all input as hostile. Checks auth on the server for every request. Fails closed. Never puts secrets in code or logs. |
| **Safe dependencies** | Checks that a package really exists before adding it (AI invents package names, and attackers register them). Pins versions. |
| **Verify its own work** | Runs linter, tests, secret scan, static analysis, and dependency scan. Writes security tests. Tries to break what it built. Fixes what it finds. |
| **AI/agent safety** | Treats outside text as data, not orders. Least privilege. Guards against prompt injection and over-powered agents. |
| **Honest reporting** | Short report after each task. Labels every claim as *Verified*, *Tested*, *Not tested*, or *Assumed*. Never says "this is secure". |
| **Stop list** | Always asks first before deleting data, force-pushing, deploying, touching production, or editing its own rules. |

## Quick start

1. Copy `AGENTS.md` into the root of your project.
2. Copy `CLAUDE.md` too if you use Claude Code. It just points to `AGENTS.md`, so you keep one source of truth.
3. For tools that don't read `AGENTS.md` automatically, point their rules file at it.
4. **Enforce it in CI** (see below). This step matters most.

## Files

- `AGENTS.md`: the rules. Written for the AI, not for humans.
- `CLAUDE.md`: one line that tells Claude Code to follow `AGENTS.md`.

## Important: rules are not a guarantee

A rule file is a request. A model can ignore it, forget it in a long session, or follow it badly. Treat it as raising the baseline, not as a safety net.

Make it real with checks that don't depend on the AI:

- Secret scanning (for example gitleaks)
- Static analysis (for example Semgrep)
- Dependency scanning (for example OSV, `npm audit`, `pip audit`)
- Branch protection, so nothing merges unless the checks are green

Also: the agent that wrote the code rarely finds its own holes. Have a separate agent, a fresh session, or a human review anything touching auth, payments, data access, or infrastructure.

## Why these rules

The rules draw on public research and standards:

- **OWASP Top 10 for LLM Applications 2026**: prompt injection stays #1, and "excessive agency" jumped to #3 as agents get more tools and power.
- **Package hallucination research** (Spracklen et al., USENIX Security 2025): about 19.7% of packages suggested across 576,000 code samples from 16 models did not exist.
- **GitClear code-quality analysis**: AI-assisted code shows more duplication, less reuse, and more swallowed errors.
- **NIST SSDF, CISA, and CIS/SAFECode Secure by Design**: AI-generated code should get the same testing and review as human-written code.

## Customize it

Edit `AGENTS.md` to fit your stack: approved libraries, your auth solution, language and framework conventions. Keep the stop list. If the same mistake keeps showing up, add a rule for it.

## Language

The rules are in English because tools and security terms are English. One line at the top of `AGENTS.md` makes the agent reply to you in Norwegian while writing code, comments, and commits in English. Change or remove it for your own use.

## Limits

- Not a replacement for a real security review or penetration test.
- Not tested against every model or tool. Results vary.
- Active attack testing should only be done on systems you own.

## License

Add a license before others reuse this (MIT is a common choice).
