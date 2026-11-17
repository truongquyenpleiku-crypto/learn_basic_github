The zero-dollar data stack
---
A free modern data stack, end to end: GitHub activity of the data tools themselves goes from the GitHub API to a shared dashboard for $0 a month.
<img width="1560" height="452" alt="68747470733a2f2f6d656469612e7365636f6e64627261696e2e6c656c6f7576696e63782e636f6d2f323032362f31312f61616438646537373361343537323030336531353439383732306132356566632e676966" src="https://github.com/user-attachments/assets/c35f2609-63a1-4cdd-ae08-fe15b9a517de" />
|Layer|Tool|Where|
|:-|:-:|-:|
|Ingestion |dlt|ingest/|
|Storage|DuckDB locally, MotherDuck to share|data/|

The write-up: The zero-dollar data stack on lelouvincx.com.

Run it 
---
You need uv, anfra, and a GitHub token for the API rate limit.

```
make install
GITHUB_TOKEN=$(gh auth token) make ingest   # about 10 minutes for 12 months of data
make transform                              # dbt seed + build, with tests
make serve                                  # anfra serve: open http://127.0.0.1:7878/
```

Everything lands in data/github.duckdb. No account needed.

Tracked tools
---

dlt, DuckDB, dbt, SQLMesh, anfra, Cube, Evidence, Metabase and Superset.

Want another tool tracked? Add it to REPOS in ingest/github_pipeline.py and its layer to transform/seeds/tool_layers.csv, then submit a PR!

Metrics
---
Defined once in anfra/datasets/github_health.dataset.aml:
- Median first response (hours): how fast a maintainer replies to an issue from the community
- Issues answered by a maintainer: the share of community issues that got a maintainer reply
- Community PR share: the share of merged pull requests from outside the core team, bots excluded
- Median time to merge, releases, issues and pull requests opened, and stars

"Core team" means anyone who merged a pull request in the repo, or whom GitHub shows as an owner, member or collaborator. Merging catches engineers whose org membership is private, whom GitHub labels as contributors. Bots are GitHub Apps and *[bot], *-bot or *automation* logins.
