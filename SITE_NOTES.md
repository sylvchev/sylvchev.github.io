# Site notes: state, workflow and to-do

Working notes for Sylvain's personal site (https://sylvchev.github.io). They record decisions already made and the work still pending, so an agent can resume without replaying past sessions. Start by reading `AGENTS.md` (upstream al-folio rules), then this file.

This file is excluded from the Jekyll build (`exclude:` in `_config.yml`). Keep it up to date: move finished items to "Done" with the date.

## Repository layout

| Branch / tag                            | Role                                                                                      |
| --------------------------------------- | ----------------------------------------------------------------------------------------- |
| `main`                                  | Jekyll sources, default branch. Pushing it runs the `Deploy site` workflow.               |
| `gh-pages`                              | Built site, written by the workflow. GitHub Pages serves this branch. Never edit by hand. |
| `site-v1`                               | Branch used for the v1 migration, merged into `main`. Can be deleted.                     |
| `master`                                | Old built site (pre-v1), kept as an archive.                                              |
| `legacy-source`, tag `legacy-site-2025` | Pre-v1 Jekyll sources and built site, for reference.                                      |

Remotes: `origin` is `sylvchev/sylvchev.github.io`; `upstream` is `alshedivat/al-folio`, with push disabled on purpose. Never open a PR against al-folio.

The repo is a fork of al-folio, migrated in September 2026 to the v1 starter. Theme code lives in `al_*` gems; this repo holds only content and config.

## How to work on the site

- **Local preview:** `docker compose up`, then http://localhost:8080. Ruby and Node are not installed on the Mac; everything runs in Docker or through `uv`.
- **Test build:** `docker run --rm -v "$PWD":/srv/jekyll -w /srv/jekyll amirpourmand/al-folio:latest bash -lc "bundle exec jekyll build --destination /tmp/_site"`
- **Formatting (CI runs Prettier):** `docker run --rm -v "$PWD":/app -w /app node:20 sh -c "npm ci --ignore-scripts >/dev/null; npx prettier --write <files>"`. Run it on tracked files only, never on `.kilo/`. Use fenced code blocks, not `{% highlight %}`: Prettier strips the indentation inside `highlight` tags.
- **Pulling upstream updates:** `git fetch upstream && git merge upstream/main` on a branch, then build, check, and merge into `main`.

## Sync with the LaTeX CV

The CV lives in `~/admin/CV/CVadmin/CV_2026/` (its own git repo; see its `design.md`). It is the source of truth for publications, PhD students and interns.

```sh
cd ~/admin/CV/CVadmin/CV_2026 && make      # rebuild the CV PDF first
cd ~/recherche/sylvchev.github.io && uv run bin/sync_cv.py
```

`bin/sync_cv.py` only reads the CV repo. It writes:

- `cv/cv.pdf`: copy of the compiled CV.
- `_bibliography/papers.bib`: the CV's `data/publications.bib`, followed by `_bibliography/extra.bib`. **Generated: never edit it.** Website-only entries (theses, abstracts, preprints) go in `extra.bib`. The `\fullcite` keys of the CV's "Sélection de publications" become `selected = {true}` and show on the about page.
- `_data/supervision.yml`: English version of the CV's `data/supervision.yml`, rendered by `_pages/cv.md`. Translations come from the `*_en` fields of the CV YAML; the script warns when a French title has no `title_en`.

Rules:

- A new student or intern is added in the CV repo, with its `*_en` fields, then synced.
- `gen_supervision.py` in the CV repo ignores the `*_en` fields. After editing them, check that `generated/*.tex` is unchanged.
- News items and pronouns: never infer someone's gender from their name. Write "defended a PhD thesis", not "her/his PhD".

## Decisions already made

- Publications are grouped by year (not by the CV's categories).
- The CV page lists all PhD students; M2 and other interns sit in collapsible sections.
- The teaching pages stay plain `layout: page` cards, not the v1 `course` layout. Old URLs (`/teaching/…`, `/projects/…`, `/news/…`) are preserved.
- X/Twitter links removed (September 2026); Sylvain is on Bluesky now.
- Google Analytics removed: the old `UA-` ID stopped working when Google retired Universal Analytics.

## Pending

Ongoing (September 2026):

- [ ] Review the 14 news drafts in `_news/` (uncommitted): `cortico2026keynote`, `practicalmeeg2025`, `eegemgchallenge2026`, `mellotprize`, `eegchallenge2025results`, `mellotphd`, `yamamotophd`, `neurips2024`, `verbockhavenphd`, `icml2025`, `eegchallenge`, `aristimunhaphd`, `opensource2026`, `phdautumn2026`. They were written from CV data; check the facts.
- [ ] Commit the CV repo change: `*_en` fields in `data/supervision.yml` and a note in `design.md` (uncommitted in `~/admin/CV/CVadmin/CV_2026`).
- [ ] Run `make` in the CV repo, then `uv run bin/sync_cv.py` again. The PDF is identical, but the script warns because `supervision.yml` is newer than the PDF.
- [x] Bluesky: `bluesky_url` set to `sylvchev.bsky.social` (confirmed by Sylvain).
- [x] Email: keep `sylvain.a.chevallier@inria.fr` in `_data/socials.yml` (confirmed by Sylvain).
- [ ] `extra.bib`: `hoxha_eeg_2023` (bioRxiv) looks like the preprint of `hoxha2026modality` (Neuropsychologia); remove it if so. Decide whether `delgado_riemann-based_2020` (TNSRE journal article) belongs in the CV bib.
- [ ] CI cleanup: upstream workflows `unit-tests`, `prettier-comment-on-pr`, `visual-regression`, `codeql`, `copilot-setup-steps`, `update-tocs` and `render-cv` run on this repo and are useless or failing here. Proposed: delete them, keep `deploy`, `prettier`, `upgrade-check`, `update-citations`.
- [ ] Delete the `site-v1` branch once `main` is settled.

Sylvain's tasks (requested 2026-09-24). Work on a branch, build with Docker, run Prettier, and show the result before pushing to `main`:

1. [ ] **Update the news.**
   - Start from the 9 drafts above.
   - Then add items for publications since 2024 that have no news yet: list the 2024+ entries of `papers.bib` and compare them with `_news/`. Candidates: `velut2026tackling` (IJCNN), `lopes2026learning` (Scientific Reports), `velut2026neurophysiological` (Imaging Neuroscience), `surrel2025geometry` (TMLR), `douka2025growth` (ESANN), `tangermann2025learning` (JNE).
   - Style: 1 or 2 sentences, `layout: post`, `inline: true`, a link to HAL or the DOI.
   - Check every date and fact with Sylvain or a source. No gendered pronouns.
   - Idea: make `sync_cv.py` print new CV publications that have no news yet.
2. [ ] **Badges and thumbnails on the publications page.**
   - al-folio v1 already supports:
     - `abbr` (a venue badge; its colour and link come from `_data/venues.yml`)
     - `preview` (an image in `assets/img/publication_preview/`)
     - altmetric, Dimensions and Google Scholar badges (`enable_publication_badges` is on; they need `doi`, `altmetric` or `google_scholar_id` fields)
     - `code`, `pdf`, `slides` buttons
   - `papers.bib` is generated, so do not edit it. Instead, add a hand-edited file (for example `_data/publication_extras.yml`, keyed by bib key) that `sync_cv.py` merges into the bib.
   - Derive `abbr` automatically from `booktitle`/`journal`, for example NeurIPS, ICML, JNE, TMLR.
   - Thumbnails: ask Sylvain for figures, or propose one figure per selected or recent paper taken from its HAL/arXiv PDF (check the licence). Start with the 5 selected papers and the 2024+ ones.
3. [ ] **Fill the project pages from Sylvain's papers and projects.**
   - The current `_projects/` pages date from about 2020.
   - Propose a new set of themes, each with a short text, a figure and its publications (`related_publications: true` plus `{% cite key %}`, or a filtered `{% bibliography %}`). Possible themes:
     - Riemannian BCI and pyRiemann
     - neural network growth (MANOLO; Verbockhaven, Douka, Rudkiewicz)
     - deep learning for EEG and Braindecode (Aristimunha)
     - domain adaptation and brain health (Mellot, de Surrel)
     - benchmarks and MOABB
     - data challenges and Codabench
     - the DeMythif.AI COFUND program
   - Consider project categories (`enable_project_categories`), for example "current" and "past".
   - Ask Sylvain which old projects to keep, archive or delete before rewriting.
4. [ ] **Update the CV page.**
   - Refresh the "Short version" of `_pages/cv.md` from the header of the CV `.tex`: positions, team role (the January 2024 news says co-leader of AO/TAU; the page says head of AO), responsibilities, funded projects, teaching.
   - The supervision part is already generated: keep it.
   - If more sections are worth generating (projects, responsibilities), read them from the CV repo through `sync_cv.py`, not by copying text.
5. [ ] **List more repositories** in `_data/repositories.yml`.
   - Now listed: moabb, pyRiemann, codabench, mdla.
   - Candidates: the `soft` entries of the CV bib (for example Braindecode), repos Sylvain contributes to (pymanopt, geomstats; see the about page), and Sylvain's own repos (`gh repo list sylvchev`).
   - Propose a list and let Sylvain choose.

Smaller ideas:

- [ ] Add `hal_id: sylvain-chevallier` to `_data/socials.yml` (HAL icon, supported by jekyll-socials).
- [ ] Publication style: now `apa`; the old site used `frontiers-in-bioscience`.

## Done

- 2026-09: migrated to the al-folio v1 starter; `main` created and deployed through GitHub Actions to `gh-pages`.
- 2026-09: content formatted with Prettier; `{% highlight %}` replaced by fenced code blocks.
- 2026-09: `bin/sync_cv.py` added; bibliography (122 entries), PhD and intern lists, and CV PDF synced from the LaTeX CV. X link removed; tweet link in `_news/pyriemann0-3.md` replaced by the release notes.
