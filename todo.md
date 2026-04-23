# TODO — Launch first searches

Checklist to go from "fork duplicated" to "first job scores in the tracker."
Target roles: Marketing Manager, Solutions Consultant. Primary market: AI companies. Secondary: B2B SaaS.

---

## 0. Pre-work (once, after you duplicate the repo)

- [ ] Duplicate this fork to a **private** repo so your CV and tracker never hit a public mirror.
- [ ] Clone it locally: `git clone <your-private-repo>`
- [ ] Install Node deps if Playwright is needed: `npm install` (or `bun install`)
- [ ] Run `node doctor.mjs` to confirm the environment is healthy.

---

## 1. Seed your user-layer files (required before any evaluation)

The system reads these on every run. If any is missing, Claude will force you back into onboarding.

- [ ] **CV** → create `cv.md` at repo root.
  - Paste your existing CV, LinkedIn URL, or bullet points. Claude converts whatever you give it.
  - Should have: Summary, Experience, Projects/Campaigns, Education, Skills.
- [ ] **Profile** → copy example, then fill in:
  - `cp config/profile.example.yml config/profile.yml`
  - Replace `candidate` block: name, email, location, LinkedIn, portfolio URL.
  - Replace `target_roles` with your real targets (examples below).
  - Replace `compensation` with your real OTE / base + bonus range and floor.
  - Replace `location` with your country, city, timezone, visa status.
- [ ] **Profile modes file** → already present as `modes/_profile.md` (marketing/SC archetypes + scoring overrides). Skim it; tweak phrasing to match how you want Claude to frame you.
- [ ] **Portals** → already present as `portals.yml` (61 AI + SaaS companies, marketing/SC keywords). Skim the company list; disable any you don't want via `enabled: false`.
- [ ] **Tracker** → gets created on first evaluation. No action needed.

### `config/profile.yml` — suggested target_roles block

```yaml
target_roles:
  primary:
    - "Product Marketing Manager"
    - "Solutions Consultant"
    - "Solutions Engineer"
  archetypes:
    - name: "AI Product Marketing Manager"
      level: "Senior/Lead"
      fit: "primary"
    - name: "Pre-Sales Solutions Consultant / Solutions Engineer"
      level: "Senior"
      fit: "primary"
    - name: "AI Solutions Consultant / Forward Deployed"
      level: "Senior"
      fit: "secondary"
    - name: "Growth / Demand Gen Marketing Manager"
      level: "Senior"
      fit: "adjacent"
```

---

## 2. Optional but high-ROI before the first search

Each of these makes the first evaluations materially better. Skip if short on time — come back after 3-5 evaluations.

- [ ] **Proof points** → create `article-digest.md` with your 5–10 strongest moves:
  - Format per item: `Name | URL | Hero metric | 2-sentence context`.
  - For marketing: launches, campaigns, pipeline generated, positioning wins, ARR influenced.
  - For SC: deals influenced, win rate lift, complex integrations shipped, demo/POC assets.
- [ ] **Compensation benchmark** → fill `compensation.target_range` and `minimum` with a real OTE figure. If unsure, Levels.fyi / Pave / RepVue have ranges for your geography.
- [ ] **Portfolio or demo reel** → if you have a recorded demo, case study site, or newsletter, add it to `candidate.portfolio_url` in profile.yml. Claude surfaces it in the right applications.

---

## 3. First search (one command)

- [ ] In Claude Code, run: `/career-ops scan`
  - Scanner hits Greenhouse / Ashby / Lever APIs + web queries defined in `portals.yml`.
  - Zero LLM cost for discovery — only evaluation costs tokens.
  - Output lands in `data/pipeline.md` as an inbox of URLs.
- [ ] Review the inbox. Delete any that are obviously wrong.
- [ ] Run `/career-ops pipeline` to process the inbox one by one, OR `/career-ops batch` for parallel processing.
- [ ] For each offer Claude evaluates, a report lands in `reports/` and the tracker updates in `data/applications.md`.

## 4. First evaluation (paste-and-score)

If a specific JD caught your eye outside the scan:

- [ ] Paste the job URL or JD text into Claude. The auto-pipeline detects archetype, scores A–G, writes the report, registers in the tracker, and offers a PDF.
- [ ] Review the report at `reports/001-{company}-{date}.md`.
- [ ] Only generate the tailored CV if score ≥ 4.0: `/career-ops pdf`.
- [ ] **NEVER let Claude submit on your behalf.** Review the draft answers, then apply manually.

## 5. Feedback loop (every 3–5 evaluations)

- [ ] If Claude scored something too high or too low, tell it why. It updates `modes/_profile.md` or `config/profile.yml` so future scores calibrate.
- [ ] After ~10 evaluations, run `/career-ops patterns` to see rejection patterns and tighten targeting.
- [ ] After first applications sent, run `/career-ops followup` to track cadence.

---

## 6. Automate (optional, when comfortable)

- [ ] Schedule a recurring scan so fresh jobs land without you asking: `/loop 3d /career-ops-scan` (or a cron hitting `node scan.mjs`).
- [ ] Enable branch protection + the shipped CI on your private fork (already wired in `.github/`).

---

## Guardrails to keep in mind

- Offers below score 4.0 → Claude will explicitly recommend **not** applying. Trust it unless you have a specific reason.
- Always quote **OTE** (base + variable) when negotiating SC roles. `modes/_profile.md` has scripts ready to paste.
- Travel expectation is a hard constraint for most field SC roles (20–40%). Surface it in the evaluation, don't skip it.
- System-layer files (`modes/_shared.md`, `CLAUDE.md`, the `*.mjs` scripts) get auto-updated from upstream. Your personalization goes in `modes/_profile.md` + `config/profile.yml` — edit those, not `_shared.md`.
