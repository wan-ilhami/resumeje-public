# Architecture

High-level, public-safe view of how ResumeJe is built. No internal URLs, credentials, or infrastructure identifiers — just the shape of the system.

## Two services, one product

- **`frontend`** (private) — Next.js 16 App Router. Owns the UI, all server route handlers, the Prisma/PostgreSQL data layer, and authentication (Google or email sign-in). Every account's resumes, application-tracker entries, and saved data live here.
- **`backend`** (private) — Express 5 on Bun. A dedicated service for the heavier, stateless work: parsing uploaded resumes (PDF/DOCX/TXT, with OCR for scanned/image-only PDFs), AI-driven rewriting and cover-letter generation, and ATS keyword scoring.

The browser never talks to the backend directly. Every request flows:

```
Browser → frontend (Next.js server routes) → backend (parsing / AI / ATS)
```

The frontend is the trust boundary: it authenticates the user, then forwards only what the backend needs to do its job.

## The shared contract

Every surface — the builder, the ATS checker, the cover letter generator, exports, templates — reads and writes one canonical shape, `ResumeFormData` (defined in this repo's [README](./README.md)). A saved resume is a database record tied to the account; the resume being actively edited is held in application state and mirrored to browser local storage, so a page reload doesn't lose in-progress work. Because every feature reads the same shape, an edit made in the builder is immediately visible to the ATS checker, the cover letter generator, and every export path — there's no separate "sync" step.

## Accounts and privacy

Every resume is stored against a user account (Google or email sign-in) — there's no anonymous document floating unowned in the database. Users get full data export and one-click account deletion; the same contract that lets one edit flow everywhere also means deleting the account has one clear place to cut from.

## What this repo publishes

This public repository carries the product description, the `ResumeFormData` contract, this architecture overview, and a live version tracker kept in sync by CI on every release of the private frontend/backend repos. It does not carry application source, API routes, database schema, AI prompt templates, or anything that would let someone stand up a copy of the private services.
