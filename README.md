# Student Safety Network

School transportation safety research by Kurt Schallitz for COSC 645, Maryville University.

The project combines intersection collision prediction mapping, traffic simulation, and reinforcement learning to compare safety interventions. The initial study area is East Avenue and Estates Street near East Avenue Middle School in Livermore, California.

## Project source

Download **[Student-Safety-Network-source.zip](Student-Safety-Network-source.zip)** and extract it to view the Python modules, HTML/JavaScript interface, notebook, dependency lists, and tests. This is a curated source snapshot dated October 8, 2026. The archive preserves module folders and contains no local Git history.

The source includes collision mapping, the MetaDrive pilot, earlier SUMO experiments, and tabular, linear, and neural mitigation learners. Notebook outputs have been cleared.

## MetaDrive pilot findings

| Scenario | Simulated contact pairs |
|---|---:|
| Unchanged baseline | 58 |
| 25 mph | 35 |
| Shorter crossings plus 25 mph | 27 |

These totals cover the same 12 traffic windows with matched travelers. All three learner families selected shorter crossings plus 25 mph. Individual windows varied. Three episodes failed geometry checks and require repair. These are simulated contacts, not observed crashes; changing simulator, driving behavior, and contact detection together does not establish that MetaDrive is more accurate than SUMO. Delay is reported separately from the safety score.

## Data access and release restrictions

Third-party study data are **not included**. This includes raw collision/victim records, transcribed traffic and speed observations, traced geometry, source reports and images, prepared input datasets, and the earlier data-containing runnable bundle. Redistribution permission has not been confirmed for these materials. Personal documents, credentials, vendor code/assets, and virtual environments are also excluded.

Obtain inputs directly from their providers under the applicable terms:

- [UC Berkeley Transportation Injury Mapping System](https://tims.berkeley.edu/): collision data.
- [City of Livermore East Avenue report](https://www.livermoreca.gov/home/showpublisheddocument/12943/638887914440930000): traffic counts and corridor study information. See the City's [website use policy](https://www.livermoreca.gov/departments/city-manager/website-use-policy).
- California Department of Education: school location information.
- OpenStreetMap and local public agencies: geographic inputs, subject to their individual terms and attribution requirements.

The source expects prepared inputs at the paths specified in each module. This public edition cannot reproduce the study end to end without acquiring and preparing those inputs. The original study data remain in the local project.

## Setup

After extracting the archive, create a Python virtual environment and install the appropriate requirements file. `requirements-metadrive.txt` pins the external MetaDrive dependency; `requirements.txt` supports the earlier mapping and simulation work. SUMO is installed separately for older studies. Dependencies retain their upstream licenses and are not bundled here.

Run the learner checks from the extracted source directory:

```sh
python -m unittest discover -s tests -p 'test_metadrive_learning.py'
```

No license grant for third-party materials is asserted by this publication.
