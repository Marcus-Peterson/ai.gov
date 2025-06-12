# AGENTS.md – AI.gov Monorepo Agents Specification

## 📦 Overview

* **Project**: `gsa‑tts‑archived/ai.gov` – a full-stack monorepo for the upcoming AI.gov platform .
* **Purpose**: Provides a public site and internal console to enable federal agencies to connect to LLMs (OpenAI, Google, Anthropic, AWS Bedrock, Meta LLaMA) via a unified API, with usage tracking and analytics .
* **Tech Stack**:

  * Monorepo with Turborepo & `pnpm` workspaces
  * Node.js + TypeScript, Astro, Svelte, CSS/SCSS
  * Test suite: Playwright

---

## 🗂️ Folder Structure & Agents

```
/apps/
  ├── site/        ← Public-facing site agent
  └── console/     ← Internal dashboard agent

/packages/         ← Shared libraries & components
docs/adr/          ← Decision-record agent
.github/, .husky/  ← CI & commit hooks
.vscode/           ← Editor settings
```

### 🧠 Agents Defined:

* **site-agent** (`apps/site/`):

  * Responsible for UI/UX of the public site
  * Likely includes AI chatbot interface, site navigation, marketing copy

* **console-agent** (`apps/console/`):

  * Dashboard for administrators
  * Tracks API usage, user activity, model preferences

* **shared-agent** (within `packages/`):

  * Builds components, utilities, and cross-app logic (auth, API clients)

* **infra-agent** (Turborepo config):

  * Manages builds: `turbo build`, `dev`, per-app targets
  * Enforces consistency across dev environment and pipelines

* **ci-lint-agent** (`.github`, `.husky`):

  * Handles CI, lint, formatting, commit validation using Husky & GitHub Actions

---

## ✅ Development Agents

These scripts streamline developer workflows:

* `pnpm install`, `pnpm build`, `pnpm dev`
* `pnpm dev:site` → launches `apps/site` locally
* `pnpm dev:console` → launches `apps/console` locally
* `pnpm lint`, `pnpm check-types`, `pnpm test` (via Playwright)

---

## 🤖 Agent Roles & Capabilities

| Agent           | Location         | Capabilities                                   |
| --------------- | ---------------- | ---------------------------------------------- |
| `site-agent`    | `apps/site`      | Serves AI landing page, chatbot UI             |
| `console-agent` | `apps/console`   | Provides analytics UI: user, model, API usage  |
| `shared-agent`  | `packages/*`     | Reusable code: API client, schemas, auth       |
| `infra-agent`   | repo root config | Build orchestration via Turborepo + pnpm       |
| `ci-lint-agent` | `.github/.husky` | Enforces code hygiene, formatting, type safety |

---

## 🛠️ Workflow: Build & Development

1. **Build orchestration**: Root `pnpm build` compiles shared packages and both apps.
2. **Local development**: `pnpm dev` spawns both site and console for local testing.
3. **Testing**: `pnpm test` runs Playwright suite across agent frontends.
4. **CI enforcement**: GitHub Actions and Husky validate styling, linting, types, commit messages, and builds.

---

## 🔍 Security Considerations

* **Public staging**: Codebase was publicly exposed before being archived
* Agents to audit for secrets usage (e.g., `.env`) in `console-agent` and `shared-agent`
* Shared API client may include endpoints for external LLM services; must be secured and validated
* CI pipelines may store keys/secrets – check `.github` workflows and `.env.example`
* Shared schemas may carry sensitive data types—need audit of data flow definitions

---

## 🗄️ Next Steps

* **Clone repo** for deeper code introspection
* **Generate file maps** in each app (API routes, page definitions, components)
* **Audit dependencies** in `package.json` and `pnpm-lock.yaml` for CVEs
* **Inspect shared-agent** for authentication logic, API fetching & key exposures
* **Review console-agent** for role-based access control and usage logging
* **Add security-check-agent**: script to search for strings like `process.env.*`, `.env`, `.secret`, `TODO(security)`

---

## 🧩 Summary

This AGENTS spec outlines a modular, cleanly structured system:

* **Public UI** via `site-agent`
* **Admin analytics** via `console-agent`
* **Shared logic** for API, models, auth
* **Robust infra and CI** via monorepo tooling
