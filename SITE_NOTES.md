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

- [ ] Review the 9 news drafts in `_news/` (uncommitted): `mellotphd`, `yamamotophd`, `neurips2024`, `verbockhavenphd`, `icml2025`, `eegchallenge`, `aristimunhaphd`, `opensource2026`, `phdautumn2026`. They were written from CV data; check the facts, and set the real date in `eegchallenge.md` (marked TODO).
- [ ] Commit the CV repo change: `*_en` fields in `data/supervision.yml` and a note in `design.md` (uncommitted in `~/admin/CV/CVadmin/CV_2026`).
- [ ] Run `make` in the CV repo, then `uv run bin/sync_cv.py` again. The PDF is identical, but the script warns because `supervision.yml` is newer than the PDF.
- [ ] Bluesky: set `bluesky_url` in `_data/socials.yml` (handle not provided yet).
- [ ] Email in `_data/socials.yml` is still `sylvain.a.chevallier@inria.fr`; confirm or switch to the Paris-Saclay address.
- [ ] `extra.bib`: `hoxha_eeg_2023` (bioRxiv) looks like the preprint of `hoxha2026modality` (Neuropsychologia); remove it if so. Decide whether `delgado_riemann-based_2020` (TNSRE journal article) belongs in the CV bib.
- [ ] CI cleanup: upstream workflows `unit-tests`, `prettier-comment-on-pr`, `visual-regression`, `codeql`, `copilot-setup-steps`, `update-tocs` and `render-cv` run on this repo and are useless or failing here. Proposed: delete them, keep `deploy`, `prettier`, `upgrade-check`, `update-citations`.
- [ ] Delete the `site-v1` branch once `main` is settled.

Ideas and future changes:

- [ ] Update the "Short version" of `_pages/cv.md` from the CV's header (positions, team role: the January 2024 news says co-leader of AO/TAU, the page says head of AO).
- [ ] Add `hal_id: sylvain-chevallier` to `_data/socials.yml` (HAL icon, supported by jekyll-socials).
- [ ] Use `sync_cv.py` to also flag new CV publications that could become news items.
- [ ] Publication style: now `apa`; the old site used `frontiers-in-bioscience`.
- [ ] Refresh the project pages (last updated around 2020): MANOLO, DeMythif.AI, neural network growth.
- [ ] Add Sylvain's own items here.

## Done

- 2026-09: migrated to the al-folio v1 starter; `main` created and deployed through GitHub Actions to `gh-pages`.
- 2026-09: content formatted with Prettier; `{% highlight %}` replaced by fenced code blocks.
- 2026-09: `bin/sync_cv.py` added; bibliography (122 entries), PhD and intern lists, and CV PDF synced from the LaTeX CV. X link removed; tweet link in `_news/pyriemann0-3.md` replaced by the release notes.
