# Third-party skills

Copies of outside skills that I've picked for climate-health data science and day-to-day coding. Each folder keeps its upstream MIT licence. To update one, copy it again from upstream and record the new commit below.

| Folder | Upstream | Commit | Skills |
|---|---|---|---|
| `k-dense-scientific/` | [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) | `92ace75` | 22 (stats, spatial, time series, literature) |
| `superpowers/` | [obra/superpowers](https://github.com/obra/superpowers) | `bb92a77` | 15 |
| `agent-skills/` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | `1be8e34` | 25 |
| `taste-skill/` | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) | `d237dd3` | 13 |
| `scienceclaw/` | [beita6969/scienceclaw](https://github.com/beita6969/scienceclaw) | HEAD as of 2026-10-12 | 2 (`epidemiology`, `copernicus-climate`) |

## K-Dense picks

- **Stats:** statsmodels, pymc, statistical-analysis, statistical-power, experimental-design, uncertainty-and-units
- **Machine learning:** scikit-learn, scikit-survival, shap
- **Data:** exploratory-data-analysis, polars, dask, zarr-python
- **Spatial:** geopandas, geomaster
- **Time series:** aeon, timesfm-forecasting
- **Literature:** literature-review, paper-lookup, citation-management, pyzotero, scientific-critical-thinking

## Notes from vetting

- No obfuscated code, `eval` or remote install scripts in the copied scripts.
- `literature-review/scripts/generate_schematic*.py` sends prompts to OpenRouter, and only does so if `OPENROUTER_API_KEY` is set.
- `citation-management/scripts/search_google_scholar.py` scrapes Google Scholar. Prefer `paper-lookup`, which uses OpenAlex, Crossref and PubMed.
- The skills don't install their Python dependencies. Each SKILL.md lists what it needs.
- `scienceclaw/copernicus-climate` uses the older `"format": "netcdf"` request key. Newer CDS requests use `data_format` / `download_format`.
- `scienceclaw/epidemiology` is a general checklist. It doesn't cover DLNM, case-crossover or time-stratified designs, so keep `he2at-heat-analysis` as the authority for heat–health work.
- `scienceclaw/environmental-science` was reviewed and left out: it's generic and US-centric.
- The claudskills.com listings (climate-modeling, epidemiology-guide, confounder-adjustment) couldn't be traced to a source repo, so they aren't included.

## Name clashes

`test-driven-development` exists in both `superpowers/` and `agent-skills/`. Link only one into `~/.claude/skills`. The script below keeps the superpowers version.

## Installing on your machine (Claude Code)

```bash
cd ~/path/to/awesome-claude-skills && git pull
mkdir -p ~/.claude/skills
for d in third-party/*/*/; do
  name=$(basename "$d")
  [ -f "$d/SKILL.md" ] || continue
  [ -e ~/.claude/skills/$name ] && { echo "skip $name (exists)"; continue; }
  ln -s "$PWD/$d" ~/.claude/skills/$name
done
```

The links point into this repo, so a `git pull` updates them.

**superpowers** and **agent-skills** are built as plugins. Their session-start hooks, agents and commands are not copied here. For the full behaviour, install them as plugins instead:

```
/plugin install superpowers@claude-plugins-official
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

If you do that, delete those two folders' links from `~/.claude/skills` so the skills don't load twice.

For claude.ai or Cowork, zip a skill folder and upload it under Settings → Capabilities → Skills.
