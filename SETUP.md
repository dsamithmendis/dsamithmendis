# Setup Guide — Animated GitHub Profile

Modern, animated developer profile with a **split-dashboard layout**.

---

## 1. File layout

```
.
├── README.md                              ← your new profile (paste into the profile repo)
└── .github/
    └── workflows/
        ├── snake.yml                      ← auto-generates the contribution snake
        └── 3d-contrib.yml                 ← auto-generates the 3D contribution graph
```

---

## 2. Profile architecture (the new flow)

The old build was a flat vertical stack. It now reads as a **dashboard**, left-to-right:

| # | Zone | Layout pattern |
|:--|:--|:--|
| 1 | **Hero** | Full-bleed animated banner + typing tagline + badges |
| 2 | **The Dashboard** | **Two-column split** — Identity & stack (36%) on the left, Live Pulse stats (62%) on the right, arranged as a **condensed 2×2** |
| 3 | **Contributions** | Full-width band — 3D graph + snake (repo-generated) |
| 4 | **Selected Work** | **Two-up card grid**, grouped by track — Production · AI · Juicee · PHP · Portfolios |
| 5 | **Footer** | Full-bleed animated wave + contact CTA |

### How the split is built
GitHub strips CSS, so the side-by-side layout uses an HTML `<table>`:

- Row 0 → two `<td colspan="2">` headers (`Identity` / `Live Pulse`)
- The Identity cell uses **`rowspan="2"`** so it spans both stat rows
- Rows 1–2 hold the four stat cards as a 2×2
- A 2%-wide spacer column creates the gutter

> Rows 1–2 have 4 and 4 cells (counting the rowspan), so the columns stay aligned. If you add stats, keep the cell counts balanced.

---

## 3. Install (5 minutes)

1. **Use the special profile repo.** GitHub only renders a README on your profile page if the repository is named exactly like your username: `dsamithmendis/dsamithmendis`.
2. **Drop in `README.md`** — replace the existing one.
3. **Add the workflows** under `.github/workflows/` — the folder structure must match exactly.
4. **Enable Actions.** Repo → **Settings → Actions → General → Workflow permissions** → **Read and write permissions** → Save. *(Both workflows need this to push images back.)*
5. **Run each workflow once manually.** **Actions** tab → **🐍 Generate Contribution Snake** → **Run workflow**; repeat for **🌈 Update 3D Contribution Graph**.
6. **Done.** Both images appear and refresh automatically:
   - Snake → every 12 hours
   - 3D graph → daily

> Until step 5 completes, the snake and 3D graph show as broken — expected. They are generated inside *your* repo, not by an external service.

---

## 4. What makes it "alive"

| Element | Type | Refresh |
|:--|:--|:--|
| Waving gradient hero + footer | Animated gradient, twinkling | On page load |
| Rotating tagline | Animated typing SVG | Continuous |
| Stats / Top languages | Live GitHub data | Auto |
| Streak | Live GitHub data | Auto |
| Productive time | Live data (set to UTC+5:30) | Auto |
| 3D contribution graph | **Your Action** | Daily |
| Contribution snake | **Your Action** | Every 12 h |
| Profile view counter | Live counter | Real time |

Every image URL in `README.md` was verified to return **HTTP 200** before delivery (except the two repo-generated assets, which 404 until step 5).

---

## 5. Palettes used

| Token | Hex | Used for |
|:--|:--|:--|
| Cyan | `22D3EE` | Primary accent, typing text, titles |
| Indigo | `6366F1` | Secondary accent, icons |
| Violet | `A855F7` | AI-platform badges |
| Surface | `0D1117` | Card background (GitHub dark) |

To recolour, find-and-replace `22D3EE` and `6366F1` in `README.md`.

---

## 6. Editing tips

- **Add a project:** copy any `<td width="50%" align="center">…</td>` card block and change the name / stack / link.
- **Adjust column split:** change `width="36%"` (identity) and `width="31%"` (each stat card); keep the four widths summing to ~100% (`36 + 2 + 31 + 31`).
- **Swap a stat card:** every card is `<img width="100%" src="…">` — drop in any stat-service URL.
- **Don't put Markdown inside the HTML cells.** GitHub disables Markdown parsing inside block HTML, so project cards use raw `<b>`, `<sub>`, `<br />` instead.

---

## 7. Optional extras (not included)

### a) Contribution activity graph
`github-readme-activity-graph.vercel.app` returned **HTTP 402** (Vercel free-tier quota blown on the shared public instance) at build time — server-wide, not your account. Either wait for it to recover and paste:
```html
<img width="98%" src="https://github-readme-activity-graph.vercel.app/graph?username=dsamithmendis&theme=tokyo-night&area=true&hide_border=true&radius=12" alt="Activity graph" />
```
…or self-host: fork [`Ashutosh00710/github-readme-activity-graph`](https://github.com/Ashutosh00710/github-readme-activity-graph) and deploy to your own Vercel.

### b) GitHub trophies
`github-profile-trophy.vercel.app` was also **402** server-wide. Same two options (repo: [`ryo-ma/github-profile-trophy`](https://github.com/ryo-ma/github-profile-trophy)).

### c) Self-hosted metrics card
Add `.github/workflows/metrics.yml` using [`lowlighter/metrics`](https://github.com/lowlighter/metrics) for a fully customised animated stats card generated in your repo. Ask me and I'll wire it up.

### d) More motion
Spotify "now playing" card · WakaTime coding-time card · auto-pulled blog/RSS feed · daily-rotating banner text.

---

## 8. Local preview

GitHub strips some HTML/CSS in READMEs and disables Markdown inside block HTML, so always confirm the final look on the rendered GitHub page rather than relying on a local previewer.
