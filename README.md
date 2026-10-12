# Student Safety Network

School transportation safety research by Kurt Schallitz for COSC 645, Maryville University.

The project combines intersection collision prediction mapping, traffic simulation, and reinforcement learning to compare safety interventions around schools. The current MetaDrive implementations cover East Avenue / Estates Street and East Avenue / Hillcrest Avenue in Livermore, California.

## Current release: v0.31 · October 11, 2026

Download **[Student-Safety-Network-source.zip](Student-Safety-Network-source.zip)** for the updated Python modules, map/workspace interface, job API, source-only notebooks, dependency lists, and tests. The archive preserves source folders and has no local Git history. This public snapshot is maintained separately from the live website.

- Hillcrest now has a four-way, signal-controlled simulation setup and website controls to submit a comparison to the Mac worker, follow progress, and retrieve checked results.
- Bicycle near misses now contribute to the safety score alongside pedestrian near misses. Vehicle–vehicle events remain unscored diagnostics.
- Three learners—tabular, linear, and neural—choose among fixed mitigation candidates. Every candidate receives matched traffic within each comparison.
- CSV intake checks site identity, movement, time, count, mode, source, and measurement status. An upload prepares a study plan; it does not automatically construct or validate an arbitrary intersection.
- Speed humps and crossing guards remain planning options; they are not yet implemented in the scored simulations.

The [live application](https://school-collision-explorer-2026.novadura.chatgpt.site/) remains account-restricted. Publishing this source does not make the hosted application or its data public. New website simulations require the configured Mac worker to be online; saved results can be viewed afterward.

## Current results and unfinished work

### Estates: completed

All 96 episodes passed the cohort and movement checks. All three agents selected the consultant-style physical package plus a **25 mph operating-speed response** (C08). These are the results across four May evaluation periods, with 1,458 matched travelers per design:

| Design | Scored motor–pedestrian/bicycle contact pairs | Pedestrian near misses | Bicycle near misses | Total reward |
|---|---:|---:|---:|---:|
| Unchanged | 0 | 62 | 0 | −310 |
| Consultant-style layout | 0 | 42 | 0 | −210 |
| Agent choice: package plus 25 mph | 0 | 39 | 0 | −195 |

Bicycle near misses were measured and were zero in these periods. C08 also produced two vehicle–vehicle contact pairs; those remain diagnostic and do not affect the stated pedestrian/bicycle reward.

The reward is **−1,000 per distinct motor–pedestrian/bicycle contact pair and −5 per distinct non-contact near-miss pair**. A contact pair cannot also receive a near-miss penalty. March episodes supply fitting and policy selection; May demand is excluded from learner fitting. The May periods were examined during simulator debugging, so they are not an untouched external validation set. These results use one matched arrival realization per period; they do not establish a statistically certain treatment ranking or change the map’s annual collision estimates.

### Hillcrest: training blocked

The first batch attempted all **72 episodes**. **64 passed**, while **eight did not finish all traveler trips** before the declared simulation limit. The quality checks therefore blocked training and recommendation release. The affected periods are March 2 at 15:45 and May 18 at 14:30. The records remain in the local audit; incomplete episodes are not removed to create a favorable ranking. Replaying the same saved batch will reproduce this blocker until it is repaired.

The source includes Hillcrest's simulator and bicycle scoring. It does **not** claim a completed Hillcrest recommendation. The older pilot figures in previous public releases are superseded by this status summary.

## Source guide

Inside the ZIP:

- `metadrive_intersections/`: site-specific simulation, pedestrian/bicycle conflict detection, reward, quality checks, three learners, exports, and local job worker.
- `mitigation_panel.js`, `mitigation_jobs.js`, `mitigation_planner.js`, `traffic_csv.js`: map-to-mitigation workflow, run controls, planning and CSV validation.
- `hosted_map/server/`, `hosted_map/db/`, `hosted_map/drizzle/`: authenticated simulation queue and database schema. This is the application API source, not a complete hosting scaffold.
- `INTERSECTION_SIMULATION.md` and `MetaDrive_Intersection_RL.ipynb`: current workflow and source-only notebook. `East_Avenue_MetaDrive.ipynb` is the historical public notebook.
- Mapping/model modules and earlier `metadrive_school`, `sumo_*`, and `mitigation_*` experiments remain available for inspection.
- `PUBLIC_SOURCE_MANIFEST.json`: file sizes and SHA-256 hashes for the source snapshot.

## Data access and release restrictions

Third-party study data are **not included**: raw collision/victim records, transcribed counts and speed observations, traced geometry, reports/images, prepared input tables, and data-containing reproduction bundles remain outside this public edition. Redistribution permission has not been confirmed. Personal documents, credentials, worker configuration, model/data payloads, vendor code/assets, and virtual environments are also excluded.

Obtain inputs from their providers under the applicable terms:

- [UC Berkeley Transportation Injury Mapping System](https://tims.berkeley.edu/): collision data.
- [City of Livermore East Avenue report](https://www.livermoreca.gov/home/showpublisheddocument/12943/638887914440930000): traffic counts and corridor study information; see the City's [website use policy](https://www.livermoreca.gov/departments/city-manager/website-use-policy).
- California Department of Education: school information.
- OpenStreetMap and local public agencies: geographic inputs and their attribution/usage requirements.

The public edition cannot reproduce the studies end to end without separately acquiring and preparing the required inputs. Notebook outputs have been cleared; the new notebook expects a separate authorized reproduction bundle that is not this source ZIP. The hosted application's authentication and private worker secrets must be configured independently.

## Setup and checks

Extract the ZIP, create a Python environment, and install the appropriate requirements file. `requirements-metadrive.txt` pins the external MetaDrive revision; `requirements.txt` supports the earlier mapping work. Older SUMO experiments require a separate SUMO installation. Dependencies retain their upstream licenses and are not bundled here.

These checks use synthetic fixtures and do not require the withheld study datasets:

```sh
python -m unittest discover -s tests -p 'test_bicycle_conflicts.py'
python -m unittest discover -s tests -p 'test_metadrive_reward_scope.py'
node tests/test_traffic_csv.cjs
node tests/test_simulation_api.mjs
```

The API check requires Node 22.13 or newer with `node:sqlite`. Other tests and simulator runs may require the omitted study inputs. Consult `INTERSECTION_SIMULATION.md` for the local simulation workflow.

No license grant for third-party materials is asserted by this publication.
