# Conflict on Reddit: Network & Text

**Project B** for [02467 Computational Social Science](https://github.com/lalessan/comsocsci2026), DTU, Spring 2026.

We map the [SNAP Reddit Hyperlinks](https://snap.stanford.edu/data/soc-RedditHyperlinks.html) dataset (Kumar et al., WWW 2018) into a directed subreddit-to-subreddit network and ask: do the communities that fall out of pure graph topology line up with the language of the linking posts?

- **Website (public-facing):** https://mikel-yu.github.io/02467_CSS_Project_B/
- **Explainer notebook (GitHub):** https://github.com/Mikel-Yu/02467_CSS_Project_B/blob/main/Project_B_Explainer.ipynb

## Headline findings

- The hyperlink network is strongly heavy-tailed (γ<sub>in</sub> = 2.14, γ<sub>out</sub> = 2.24, R² > 0.98). The top 15 of 67,180 subreddits account for ~10% of all incoming links.
- Hubs (askreddit, iama, pics) and bridges (subredditdrama, bestof) are different sets: popularity ≠ structural brokerage.
- Louvain finds 801 communities at modularity Q = 0.549. The top 6 cleanly map to defaults+drama, gaming, politics, music/arts, science/STEM, and europe/history, with labels coming only from network topology plus subreddit names via TF-IDF.
- Hostile hyperlinks are rare (~9.5%) but linguistically distinct: VADER compound averages −0.32 on hostile posts vs +0.15 on friendly ones.
- Every top community has higher within-community polarity than across-community polarity. Network structure and post-text sentiment align.

## Reproducing the analysis

This project uses [`uv`](https://docs.astral.sh/uv/) for environment management and Python ≥ 3.10. NetworkX is pinned to 3.3 (netwulf compatibility).

```bash
# 1. clone and enter
git clone https://github.com/mikel-yu/02467_CSS_Project_B
cd 02467_CSS_Project_B

# 2. install
uv sync

# 3. fetch raw data (~700 MB into ./data)
mkdir -p data
wget https://snap.stanford.edu/data/soc-redditHyperlinks-title.tsv -O data/soc-redditHyperlinks-title.tsv
wget https://snap.stanford.edu/data/soc-redditHyperlinks-body.tsv  -O data/soc-redditHyperlinks-body.tsv

# 4. run the explainer (regenerates figs/ and metrics.json)
uv run jupyter nbconvert --to notebook --execute Project_B_Explainer.ipynb \
    --output Project_B_Explainer.ipynb --ExecutePreprocessor.timeout=900
```

Expect ~10 minutes runtime. The notebook writes figures to `figs/` and a machine-readable summary to `metrics.json` (consumed by `index.html`).

## Layout

```
02467_CSS_Project_B/
├── Project_B_Explainer.ipynb   ← the full explainer
├── index.html                   ← the public-facing website (GitHub Pages)
├── metrics.json                 ← machine-readable analysis outputs
├── figs/                        ← 5 PNGs produced by the notebook
├── data/                        ← gitignored, holds the SNAP TSVs (~700 MB)
└── pyproject.toml               ← uv project + pinned deps
```

## Authors & contribution statement

- **Poul Guo Skov** — Network analysis (degree distribution, ER baseline, components, centrality), community detection (Louvain) and assortativity, notebook structure and code, write-up of the network sections.
- **Mikel Taotao Yu** — Dataset selection and preprocessing, sentiment landscape, LIWC / VADER feature analysis, TF-IDF on subreddit names, website design and implementation, GitHub Pages deployment.
- *Jointly* — Research question, interpretation of results, discussion, references.

## Dataset & references

- **Kumar, S., Hamilton, W. L., Leskovec, J., Jurafsky, D. (2018).** *Community Interaction and Conflict on the Web.* WWW 2018. https://snap.stanford.edu/data/soc-RedditHyperlinks.html
- **Barabási, A.-L.** *Network Science.* http://networksciencebook.com/
- **Blondel et al. (2008).** *Fast unfolding of communities in large networks.* J. Stat. Mech.
- **Hutto, C. & Gilbert, E. (2014).** *VADER: A parsimonious rule-based model for sentiment analysis.* ICWSM.
