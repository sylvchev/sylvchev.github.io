---
layout: post
title: From conda to uv
date: 2026-10-01 09:00:00
description: Cheat sheet for forks and dev versions
tags: python uv git
---

### Contributing to a library

- Fork and clone, with the original repo as `upstream`: `gh repo fork org/lib --clone`
- If already cloned, add fork as `origin`: `gh repo fork --remote`
- Environment, if the project has a `uv.lock`: `uv sync`
- Otherwise: `uv venv && uv pip install -e ".[dev]"`
- Push to your fork and open a draft PR: `git push -u origin my-branch && gh pr create --draft`
- Sync with upstream: `git fetch upstream && git rebase upstream/main`

### Using dev versions in an analysis

- New project: `uv init my-analysis`
- Local clone, editable (changes visible immediately): `uv add --editable ~/src/lib`
- Branch of a fork (commit pinned in `uv.lock`, could be shared with others): `uv add "lib @ git+https://github.com/me/lib" --branch my-branch`
- Get new commits from the branch: `uv lock --upgrade-package lib`
- Test against released versions, ignoring sources: `uv sync --no-sources`
- Back to a release: remove the line in `[tool.uv.sources]`, then `uv lock`

### Quick experiments

- Self-contained script ([PEP 723](https://peps.python.org/pep-0723/)): `uv init --script exp.py`
- Add deps, git branches included: `uv add --script exp.py numpy "lib @ git+https://github.com/me/lib" --branch my-branch`
- Run it, env created on the fly: `uv run exp.py`
- Named envs like `conda activate`: [uve](https://github.com/robert-mcdermott/uve)

### CUDA

PyTorch wheels bundle CUDA. Declare the PyTorch index, Linux only so the project still installs on a Mac (see the [uv guide](https://docs.astral.sh/uv/guides/integration/pytorch/)):

```toml
[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

[tool.uv.sources]
torch = [{ index = "pytorch-cu126", marker = "sys_platform == 'linux'" }]
```

### conda → uv

| conda                                 | uv                                             |
| ------------------------------------- | ---------------------------------------------- |
| `pip install -e ~/src/lib`            | `uv add --editable ~/src/lib`                  |
| `pip install git+https://…@branch`    | `uv add "lib @ git+https://…" --branch branch` |
