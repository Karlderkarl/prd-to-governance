# SOUL.md Template

This is the structural blueprint for SOUL.md. Adapt sections to fit the project - not every project needs every section.

## Structure

```markdown
# Project Soul - {Project Name}

## Identity

{One paragraph: what is being built, for whom, and why. Include:}
{- Company/product name}
{- Domain / industry}
{- Legacy systems being replaced (if any)}
{- Primary goal of the project}

## Core Stack

| Layer | Technology |
|---|---|
| Frontend | {e.g., Next.js 16+ (App Router, SSG/ISR)} |
| CMS | {e.g., Payload CMS 3.x integrated into Next.js} |
| Database | {e.g., PostgreSQL 16} |
| Language | {e.g., TypeScript with strict mode} |
| Styling | {e.g., Tailwind CSS} |
| Hosting | {e.g., Vercel / AWS / Hetzner + Docker + Coolify} |
{Add or remove rows as needed - only include what's relevant}

## Architecture

{Deployment topology as text diagram, e.g.:}
`Internet -> Reverse Proxy -> App Server -> Database`

Principles:
- {Key architectural decisions, e.g., "Monolith, not microservices"}
- {Deployment model, e.g., "Single container deployment"}
- {Routing/rendering strategy, e.g., "SSG with ISR for dynamic content"}

## Content Model
{Only for CMS-based projects. Skip for pure APIs or CLI tools.}

Core collections:
- {e.g., `Pages`, `Posts`, `Users`, `Media`}

Core globals:
- {e.g., `Header`, `Footer`, `SiteSettings`}

## Design Tokens
{Only if the project has a UI. Skip for APIs, CLIs, libraries.}

- Primary color: {e.g., `#e6007a`}
- Primary font: {e.g., `Inter`}
- Approach: {e.g., mobile-first, responsive, clean corporate layout}

## Coding Standards

- {Language-specific rules, e.g., "TypeScript strict mode. No `any` without a clear reason."}
- {Export style, e.g., "Prefer named exports."}
- {File organization, e.g., "Keep block definitions and renderers colocated."}
- {Image handling, e.g., "Use `next/image` with proper `sizes`."}
- {API rules, e.g., "Use structured error codes for public APIs."}

## Security Principles

- No secrets in code, commits, or tracked files.
- {Encryption requirements, e.g., "User data encrypted at rest with AES-256-GCM."}
- {Auth requirements, e.g., "JWT with short expiry, refresh tokens in httpOnly cookies."}
- {Input validation, e.g., "All public POST routes need validation and rate limiting."}
- {Upload rules, e.g., "Sanitize filenames and restrict MIME types."}

## Compliance
{Only include sections that apply to the project.}

- {Privacy: e.g., "DSGVO/GDPR: consent-first analytics, privacy policy, self-hosted consent"}
- {Industry-specific: e.g., "HinSchG: anonymous whistleblower flow, encrypted storage"}
- {SEO: e.g., "sitemap, JSON-LD, hreflang for DE/EN"}
- {Accessibility: e.g., "semantic HTML, keyboard support, alt text, sufficient contrast"}

## Reference Documents

- {e.g., `prd.md` - Product Requirements Document}
- {e.g., `setup-guide.md` - Technical implementation reference}
```

## Guidelines

- Target ~60-90 lines. This is a distillation, not a copy of the PRD.
- Every item should be something an agent needs to know before writing any code.
- Use concrete values (`#e6007a`, `AES-256-GCM`, `PostgreSQL 16`), not vague statements like "use encryption".
- If something is truly non-negotiable, it belongs here. If it's a preference, it probably doesn't.
- The Content Model section lists collection/global *names* only - detailed field definitions belong in the PRD or setup guide.
