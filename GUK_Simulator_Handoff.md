# GuK Prüfungssimulator — Handoff Document
**For: Claude Desktop + Claude Code + Cowork**
**Project owner:** Andrew
**Date:** June 2026

---

## What We Are Building

A web-based exam simulator for nursing students (GuK Diplomprüfung) hosted free on GitHub Pages. Students open the app via a shared link, draw a random Fall Beispiel, fill in a DURST schema and a POP Pflegeplanung, submit, and receive AI-generated feedback. A Q&A chat is available after submission.

No login. No user accounts. Anyone with the link can use it.

---

## Tech Stack

| Layer | Choice | Reason |
|---|---|---|
| Frontend | Single HTML/JS/CSS file | No build step, works on GitHub Pages |
| Hosting | GitHub Pages (free) | Zero cost, public URL |
| AI | Anthropic API (Claude Sonnet) | Evaluation + Q&A |
| API key | Owner's key baked into source | Repo must be **private** |
| Knowledge bank | Markdown files in repo | Simple, editable, readable by the app |
| Fall Beispiele | Markdown files in repo | Pre-written, static |

> ⚠️ The GitHub repo must be set to **private** because the API key is embedded in the code. GitHub Pages works with private repos on free accounts.

---

## Repo Structure

```
guk-simulator/
│
├── index.html              ← The entire app (one file)
│
├── data/
│   ├── krankheitsbilder/
│   │   ├── herzinsuffizienz.md
│   │   ├── myokardinfarkt.md
│   │   ├── copd.md
│   │   └── ... (22 total, one per Krankheitsbild)
│   │
│   └── fall-beispiele/
│       ├── zerebraler-insult_fall1.md
│       ├── zerebraler-insult_fall2.md
│       ├── zerebraler-insult_fall3.md
│       └── ... (3 per Krankheitsbild = 75 files total)
│
└── README.md
```

---

## Krankheitsbilder (25)

These are the conditions the exam can draw from. Each needs a corresponding `.md` file in `data/krankheitsbilder/` and **3 Fall Beispiele** in `data/fall-beispiele/`.

1. Zerebraler Insult (mit Neurologie bzw. Komplikationen)
2. Multiple Sklerose
3. PCP (Hüft-TEP)
4. Morbus Parkinson
5. Schädelhirntrauma
6. Querschnittlähmung
7. COPD
8. Pneumonie
9. Rechtsherzinsuffizienz
10. Linksherzinsuffizienz
11. Myokardinfarkt
12. PAVK
13. Diabetes mellitus Typ I und Typ II
14. Schizophrenie
15. Delir
16. Depression
17. Alkoholsucht
18. Chronisch entzündliche Darmerkrankungen (Morbus Crohn oder Kolitis)
19. Leukämie
20. Grundprinzipien der Onkologie
21. Niereninsuffizienz
22. Nierensteine
23. Darmkrebs
24. Prostata-Karzinom
25. Mamma-Karzinom

---

## Knowledge Bank — Markdown Format

Each Krankheitsbild file follows the DURST structure so the AI can cross-reference student answers against it directly.

```markdown
# Herzinsuffizienz

## D — Definition
...

## U — Ursachen & Pathophysiologie
...

## R — Risiken & Komplikationen
...

## S — Symptome & Diagnostik
...

## T — Therapie & Pflege
- Pharmakologie: Furosemid (Lasix) — Schleifendiuretikum — NW: Elektrolytstörungen
- Pharmakologie: Bisoprolol (Concor) — Betablocker — NW: Bradykardie
- Pflegemaßnahmen: Flüssigkeitsbilanzierung, Lagerung, ...
```

---

## Fall Beispiele — Markdown Format

```markdown
# Fall: Herzinsuffizienz — Fall 1

**Patient:** Herr Karl Brunner, 74 Jahre
**Aufnahmegrund:** ...
**Anamnese:** ...
**Vitalzeichen:** RR 95/60 mmHg, HF 98/min, SpO₂ 91%
**Medikation:** Furosemid 40mg, Bisoprolol 2,5mg, Ramipril 5mg
**Befund:** ...
```

Andrew will create all Fall Beispiele (3 per Krankheitsbild, 75 total) in a separate Claude chat session and save them as Markdown files.

---

## App Flow

### 1. Landing Screen
- List of all 22 Krankheitsbilder (display only)
- Single "Prüfung starten" button
- No login, no API key input

### 2. Exam Screen
- Randomly selected Fall Beispiel displayed as plain text (no highlights, no tags)
- 15-minute countdown timer — turns red when over, does NOT stop anything
- **DURST Schema** — 5 free-text fields:
  - D — Definition (Was ist die Erkrankung? Klassifikation, Subtypen)
  - U — Ursachen & Pathophysiologie (Ätiologie, Risikofaktoren, Mechanismus)
  - R — Risiken & Komplikationen (Komplikationen, Krankheitsprogression)
  - S — Symptome & Diagnostik (Klinische Zeichen, Labor, Bildgebung)
  - T — Therapie & Pflege (Pharmakologie mit Handelsname + NW, pflegerische Maßnahmen)
- **POP Pflegeplanung** — blank interactive sheet, 30 rows, columns:
  - Nr. (free text input)
  - Pflegediagnose (dropdown per row: PD / Ä / S / R / RF + free text)
  - Pflegeziele (free text)
  - Zeitgrenze (free text)
  - Pflegeinterventionen (free text)
- "Abgeben & Bewerten" button

### 3. Results Screen
- Overall score + DURST score + Pflegeplanung score
- Colour-coded feedback per DURST section (green / amber / red)
- Colour-coded feedback on Pflegeplanung
- Q&A chat — student can ask follow-up questions (AI answers based on knowledge bank)
- "Als PDF speichern" — prints Fall Beispiel + filled answers + feedback to PDF via window.print()

---

## AI Evaluation Logic (to be developed in build session)

The app sends to the Anthropic API:
1. The full Krankheitsbild knowledge file (as system context)
2. The student's DURST answers
3. The student's Pflegeplanung entries
4. A grading prompt instructing Claude to evaluate against POP criteria and DURST expectations

Claude returns structured feedback (JSON) which the app renders as the results screen.

**Grading criteria details and prompt engineering are to be worked out in a dedicated session during the build phase.** Andrew will provide the POP book content and any additional grading rubrics at that point.

---

## Estimated API Cost

| Usage | Est. cost |
|---|---|
| 200 exam evaluations | ~$4 |
| 200 × 5 follow-up Q&A | ~$5 |
| **Total for full class use** | **~$9–15** |

---

## Visual Reference

A fully working HTML mockup of the app (`guk_mockup.html`) has been built and approved. Claude Desktop should use this as the visual and structural reference for `index.html`. The mockup includes:
- All three screens (landing, exam, results)
- Working timer
- Full 30-row Pflegeplan table
- Mock feedback and Q&A

---

## GitHub Pages Deployment Steps

1. Create a new **private** GitHub repo named `guk-simulator`
2. Push all files (`index.html` + `data/` folder)
3. Go to repo Settings → Pages → set source to `main` branch, root folder
4. GitHub provides a URL: `https://yourusername.github.io/guk-simulator/`
5. Share that URL with classmates

---

## First Session in Claude Desktop — What to Ask For

Start a new chat in Claude Desktop and say:

> "I want to build a web app exam simulator for nursing students. I have a mockup HTML file and a handoff document with all the specs. Please read both files and then build the real `index.html` starting with the app shell, data loading from the `data/` folder, and the exam flow. We will handle the AI evaluation logic in a later step."

Attach:
- `GUK_Simulator_Handoff.md` (this file)
- `guk_mockup.html` (the approved mockup)

---

## Open Items (for build session)

- [ ] Grading prompt and rubric for DURST evaluation
- [ ] Grading criteria for POP Pflegeplanung (Andrew to provide POP book excerpts)
- [ ] Exact mechanism for loading Markdown files client-side (fetch from repo)
- [ ] Whether to show which Krankheitsbild was drawn before or after the exam starts
- [ ] Any session history / result saving beyond PDF export

