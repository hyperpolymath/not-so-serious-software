<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 Jonathan D.A. Jewell (hyperpolymath) <6759885+hyperpolymath@users.noreply.github.com>
-->

# Contributing — humor-ecosystem

## Audience

Developers working **on** `humor-ecosystem`. For consumers (people
calling or depending on it) see

<!-- markdownlint-disable MD033 -->

<a href="../docs/usage.adoc" class="adoc">usage</a>.

<!-- markdownlint-enable MD033 -->

## Local-dev setup

Prerequisites — the minimum versions and where to get them:

- [Git](https://git-scm.com/downloads) 2.39 or newer — install with
  your operating system's package manager (for example,
  `brew install git`) or the official installer.

- [just](https://just.systems/man/en/packages.html) 1.21 or newer —
  install with `brew install just` on macOS/Linux or
  `winget install --id Casey.Just --exact` on Windows.

- [Node.js](https://nodejs.org/en/download) 24 or newer (including npm
  11 or newer) — install with the official installer or
  `brew install node@24` on macOS/Linux. Node supplies `npx`, which runs
  the pinned formatting and linting tools below.

- GPG signing key configured (estate policy — all commits must be
  signed). See
  [standards/docs/secure-coding-training.md](https://github.com/hyperpolymath/standards/blob/main/docs/secure-coding-training.md).

One-shot setup:

```bash
git clone git@github.com:hyperpolymath/humor-ecosystem.git
cd humor-ecosystem
just setup       # installs deps, sets up hooks
just test        # runs the full test suite
```

## Running tests

- **Unit**: `just` `test-unit` — fast, no I/O.

- **Integration**: `just` `test-int` — uses real services (database,
  HTTP, etc.). Estate policy: prefer real over mocked (see
  `feedback_integration_tests_real_db` in maintainer’s memory).

- **Property**: `just` `test-prop` — randomised, slower; budget
  documented in `docs/proof-debt.md` if applicable.

- **Full**: `just` `test` — runs all of the above.

## Code style

We enforce style via CI (governance-reusable.yml from
hyperpolymath/standards). Locally:

```bash
npx --yes prettier@3.6.2 --write .github/CONTRIBUTING.md
npx --yes markdownlint-cli2@0.18.1 .github/CONTRIBUTING.md
```

The generic `just fmt` and `just lint` recipes are not configured for
this documentation repository; do not use their status-only output as
evidence that formatting or linting passed.

- All commits must be **GPG-signed** (CI enforces; see
  [standards](https://github.com/hyperpolymath/standards)).

- All source files must carry an **SPDX-License-Identifier** header (CI
  enforces).

- Conventional commits — `feat`, `fix`, `chore`, `refactor`, `docs`,
  `test`, `ci`, `revert` (CHANGELOG is auto-generated from these via
  [`changelog-reusable.yml`](https://github.com/hyperpolymath/standards/blob/main/.github/workflows/changelog-reusable.yml)).

## Branching & PR workflow

1. Branch off `main` as `claude/<topic>` (for AI agents) or
   `<initials>/<topic>` (for humans).

2. Make focused, narrow commits — one logical change per commit.

3. Open a PR against `main`.

4. **Enable auto-merge immediately** on every PR you open (`gh` `pr`
   `merge` `<num>` `--auto` `--squash`) — estate standing policy (see
   standards#196 audit and policies).

5. CI must be green. The PR auto-merges when checks pass + reviews
   land.

## Adding a new dependency

1. State the **why** in the PR body — what does this dependency unlock?

2. Check provenance (maintained, audited, no malicious history).

3. Pin to a SHA, not a tag.

4. Update `docs/architecture.adoc#Dependencies`.

## Adding an ADR

When you make a non-obvious design decision, write it down:

1. Copy `docs/decisions/0001-template.adoc` →
   `docs/decisions/0002-<slug>.adoc`.

2. Immediately replace the copied title with
   `= ADR-0002 — <title>`, set `:revdate:` to the current date, and set
   `:status:` to `PROPOSED`.

3. Fill in: Context, Decision, Consequences, Alternatives.

4. Link the ADR from the README or relevant code as a comment.

## Reporting issues

- Bugs in `humor-ecosystem`: file at
  `hyperpolymath/humor-ecosystem/issues`.

- Estate-wide concerns (policy, conventions, CI): file at
  `hyperpolymath/standards/issues`.
