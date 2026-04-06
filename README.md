# Robotics SWE Interview Roadmap

A self-contained, single-file interactive study tracker for engineers preparing for senior **Robotics Software Engineer** interviews. No build step, no dependencies, no server — just one `index.html` file you host on GitHub Pages for free.

![Phases: 9 · Topics: 56 · Timeline: ~34 weeks](https://img.shields.io/badge/phases-9-blue) ![Topics: 56](https://img.shields.io/badge/topics-56-green) ![Timeline: ~34 weeks](https://img.shields.io/badge/timeline-~34%20weeks-orange)

---

## What it covers

| Phase | Topic | Type |
|-------|-------|------|
| 1 | C++ & mathematical foundations (Lie groups, probability, numerics) | Core |
| 2 | ROS 2 architecture & patterns | Core |
| 3 | Kinematics, dynamics & control (holonomic/nonholonomic, exact linearization) | Core |
| 4 | State estimation & sensor fusion (KF, EKF, UKF, particle filter) | Core |
| 5 | Localisation & SLAM (EKF-SLAM, graph SLAM, LiDAR/visual odometry, Nav2) | Core |
| 6 | Computer vision (projective geometry, filtering, morphology, epipolar, detection) | Core |
| 7 | Graphics, Vulkan API & simulation (Gazebo, PCL, Isaac Sim) | Supporting |
| 8 | ML for robotics (PyTorch, PointNet, RL, imitation learning, NeRF) | Supporting |
| 9 | Interview preparation (system design, coding, mock interviews) | Prep |

---

## Features

### Progress tracking
- **Topic-level checkboxes** — click any topic row to mark it done. State is saved to `localStorage` and persists across browser sessions.
- **Phase completion** — a "Mark phase complete" button appears at the bottom of each phase card. It glows green when all topics are checked, but you can click it at any time.
- **Auto-advance** — when you complete a phase, the featured section at the top automatically switches to your next phase, scrolls the roadmap to it, and opens it.
- **Overall progress bar** — a percentage bar and pip strip at the top shows your progress across all 9 phases.
- **Reset** — the ↺ button in the header clears all progress with a confirmation prompt.

### Featured "current phase" panel
The section at the top always shows the first phase you have not yet completed, with a full **week-by-week study plan** including:
- Day-by-day topic breakdowns
- Graded coding exercises (easy / medium / hard)
- Interview questions specific to that week
- Curated resources for that week (clickable links with type tags: `free`, `book`, `video`, `docs`, `paper`)

### Two types of Claude prompts
Every week tab and every phase card has a **"Copy Claude prompt"** button. Click it and paste into a new Claude conversation.

1. **Phase prompt** (in the roadmap cards below) — a broad conversational prompt covering all topics in the phase. Good for an exploratory session or when starting a new phase.
2. **Weekly prompt** (in each week tab of the featured panel) — a structured, detailed prompt that includes the full day plan, all coding exercises, interview questions, and the week's resource list. Claude will teach the days in order, pause to ask interview questions, and walk through exercises interactively.

### Resources
Each phase card has a full resource list. Each week tab additionally shows resources specific to that week's content — textbook chapters, papers, YouTube playlists, docs pages, and free tools. All are clickable links that open in a new tab.

### Filtering & navigation
- Filter the roadmap by **All / Core / Supporting / Interview prep** using the pill buttons.
- Click any coloured pip in the strip to jump to that phase.
- Light/dark theme toggle in the header.

---

## Deploying on GitHub Pages (5 minutes, no git required)

1. **Create a repository** — go to [github.com/new](https://github.com/new), name it `robotics-roadmap` (or anything you like), set it to **Public**, and click **Create repository**.

2. **Upload the file** — on the empty repo page, click **uploading an existing file**. Drag `index.html` into the drop zone. Click **Commit changes**.

3. **Enable Pages** — go to **Settings → Pages**. Set source to **Deploy from a branch**, branch to `main`, folder to `/ (root)`. Click **Save**.

4. **Visit your site** — after ~60 seconds, your site is live at:
   ```
   https://<your-username>.github.io/robotics-roadmap/
   ```

**To update later:** open your repo → click `index.html` → click the pencil ✏ icon → select all (Ctrl+A / Cmd+A) → paste the new file → click **Commit changes**. Pages redeploys in ~30 seconds.

> **Note on progress:** progress is saved in your browser's `localStorage`. It persists across visits on the same device but does **not** sync between devices (e.g. your laptop and phone will have separate progress).

---

## Customising for yourself

The roadmap was built for a specific engineering background. To make the Claude prompts useful for **you**, you need to update two short strings in the JavaScript. Open `index.html` in any text editor and search for the following.

---

### The background paragraph (⚠ most important change)

Both prompt generators share a background string that Claude uses to calibrate its explanations — skipping things you already know and leaning into areas where you have gaps. **If you leave this as-is, Claude will assume you have a Vulkan renderer, a Polimi thesis, and a mechanical engineering degree.**

Search for this text (appears twice — once in `buildPrompt`, once in `buildWeekPrompt`):

```
My background: Strong C++ (advanced templates, RAII, built a Vulkan 3D renderer from scratch),
computer vision (OpenCV, 3D reconstruction pipelines, projective geometry — took Polimi IACV
course), ROS/SLAM (Polimi Robotics + Control of Mobile Robots courses), Master's thesis on
stochastic hybrid digital twins (L*SHA learning framework, UPPAAL SMC formal verification).
Bachelor's in Mechanical Engineering + CS from Polimi.
```

Replace it with your own background. Some examples:

```
// Software engineer background, new to robotics:
My background: 3 years of backend software engineering (Python, Go, some C++).
Comfortable with algorithms and data structures. New to robotics — no prior ROS or
SLAM experience. Strong in probability and linear algebra from university.

// Mechanical engineer background:
My background: 5 years of mechanical engineering with CAD and FEA experience.
Comfortable with dynamics and control theory. Moderate Python skills. New to
software-heavy robotics and ROS.

// CS student with some robotics:
My background: CS degree student. Comfortable with C++ and Python. Took one
robotics course covering basic kinematics and ROS 1. No professional experience.
Solid in algorithms and data structures.
```

The more specific you are about what you know and what you don't, the better Claude will calibrate. Mention specific courses, projects, or skills — not just job titles.

---

### The roadmap content itself

The roadmap content lives in two JavaScript data structures:

#### `PHASES` array
Each object defines one phase card in the roadmap:

```js
{
  id: 1,
  level: 'core',          // 'core' | 'supporting' | 'prep'
  color: '#888780',       // hex colour for the pip and dot
  title: 'C++ & mathematical foundations',
  duration: '2–3 wks',
  desc: 'Short description shown in the collapsed card',
  topics: [
    { n: 'Topic name', d: 'Short description', f: false }
    //                                          ^ true = mark as "familiar"
  ],
  interview: ['Question 1', 'Question 2', ...],
  resources: [
    { name: 'Display name', url: 'https://...', tag: 'free' }
    //                                           ^ 'free' | 'book' | 'video' | 'docs' | 'paper'
  ]
}
```

#### `PHASE_WEEKS` object
Keyed by phase `id`, each value is an array of week objects for the featured panel:

```js
PHASE_WEEKS[1] = [
  {
    tab: 'Week 1 — Modern C++',         // tab button label
    title: 'Week 1: Modern C++ (17/20)', // heading inside the panel
    goal: 'What this week achieves...',  // shown below the heading
    days: [
      { label: 'Days 1–2', topic: 'Topic name', detail: 'What to study' },
      ...
    ],
    exercises: [
      { diff: 'easy', name: 'Exercise name', desc: 'What to build' },
      ...
    ],
    interview: ['Question 1', 'Question 2', ...]
  },
  ...
]
```

#### `WR` object (week resources)
Keyed as `'phaseId-weekIndex'` (e.g. `'1-0'` for Phase 1 Week 1):

```js
WR['1-0'] = [
  { name: 'Display name', url: 'https://...', tag: 'free' },
  ...
]
```

---

## Adding a new phase

1. Add an entry to the `PHASES` array with a new `id` (e.g. `10`).
2. Add a matching entry to `PHASE_WEEKS` keyed by that `id`.
3. Add week resources to `WR` keyed as `'10-0'`, `'10-1'`, etc.
4. Update the total topic count in the stats card (`/ 56` in the HTML) if you add topics.

---

## File structure

Everything is in a single `index.html` file — no external dependencies, no build step.

```
index.html
├── <style>          CSS variables, layout, component styles
├── <body>           HTML structure (header, main, footer, toast)
└── <script>
    ├── PHASE_WEEKS  Week-by-week study plan data (per phase)
    ├── WR           Week-specific resource links
    ├── PHASES       Phase card data (topics, questions, resources)
    ├── Progress     localStorage read/write helpers
    ├── Prompt       buildPrompt() and buildWeekPrompt() generators
    └── Render       renderFeatured(), renderPhases(), etc.
```

---

## Covered syllabi

The content was designed to cover the following Politecnico di Milano courses alongside the broader robotics SWE interview landscape:

- **089013 — Robotics** (Matteucci) — SLAM, kinematics, sensors, ROS
- **052368 — Control of Mobile Robots** (Bascetta) — holonomic/nonholonomic models, exact linearization, flatness, trajectory tracking, odometric localisation
- **099993 — Image Analysis and Computer Vision** (Caglioti) — projective geometry, image filtering, morphology, epipolar geometry, RANSAC, Hough Transform

---

## License

MIT — do whatever you like with it. If you improve the content (more phases, better exercises, additional courses), a pull request is welcome.
