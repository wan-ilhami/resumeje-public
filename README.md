# ResumeJe

**AI-powered resume builder, ATS checker, and cover letter generator.**

[resumeje.com](https://resumeje.com)

---

## What is ResumeJe?

ResumeJe is an AI-powered toolset that helps job seekers build, optimize, and manage professional resumes.

**Core features:**

- **Resume Builder** — structured live-editing builder with cloud autosave, a completion meter, and multiple export formats (PDF, DOCX)
- **Upload & Enhance** — bring an existing PDF, DOCX, or TXT resume (or import a LinkedIn profile) and let AI parse and rewrite it section by section; scanned, image-only PDFs are handled with OCR
- **ATS Checker** — score a resume against a job description to surface keyword gaps before an applicant tracking system does
- **Cover Letter Generator** — tailored cover letters generated from your resume and the target role
- **Templates** — professional resume templates, previewable before you commit
- **Job Application Tracker** — companies, roles, statuses, and notes in one pipeline
- **Career Tools** — skills analysis, application packs, and interview preparation
- **Account & Privacy** — Google or email sign-in, full data export, and one-click account deletion

Building, analyzing, and generating require a free account, because every resume is stored against your user.

---

## This Repository

This is the **public** repository for ResumeJe — a product overview, architecture summary, and the shared data contract. The `frontend` and `backend` source code lives in a single private parent repository (`resumeje`), as plain folders, not separate repos.

**What's here**
- ❌ No API keys, tokens, credentials, internal URLs, or user data.
- ✅ Only the product overview, the canonical `ResumeFormData` contract, and the architecture summary.

**Not here**
- Frontend or backend source code
- Endpoint documentation, server configuration, or infrastructure detail
- Environment variables, secrets, or connection strings
- Database schema or AI prompt templates

---

## The Data Contract

Every ResumeJe surface — builder, ATS checker, cover letter generator, exports, templates — reads and writes one canonical shape:

```ts
interface ResumeFormData {
  profileImage?: string | null;
  fullName: string;
  jobTitle: string;
  email: string;
  phone: string;
  state: string;
  linkedin?: string;
  portfolio?: string;
  professionalSummary: string;
  education: EducationItem[];
  experience: ExperienceItem[];
  skills: { category: string; skills: string }[];
  certifications: string[];
  projects: ProjectItem[];
  languages: string[];
  strengths?: string[];
  keyAchievements?: string[];
  interests?: string[];
}
```

Saved resumes live in the account's database record; the resume currently being edited is held in application state and mirrored to the browser's local storage so a reload doesn't lose work. Because every surface reads the same shape, an edit in the builder flows straight into the cover letter generator, the ATS checker, the templates, and every export.

---

## Try it

[resumeje.com](https://resumeje.com) — free account required to build, analyze, or generate.

## Architecture

Two private services in one repo — a Next.js frontend (UI, accounts, data) and an Express/Bun backend (parsing, OCR, AI, ATS scoring) — with the browser only ever talking to the frontend. See [`ARCHITECTURE.md`](./ARCHITECTURE.md) for the full breakdown.

---

## License

MIT
