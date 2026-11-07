# National Parks Biodiversity: Observations by Conservation Status

An exploratory data analysis of species observations recorded across four U.S. national parks. The project cleans and merges two raw datasets into a single analysis-ready table, then examines how recorded observation counts differ across conservation-status groups.

The work is framed from the perspective of a biodiversity analyst supporting the National Park Service's mission to protect at-risk species while maintaining biodiversity across parks.

## Key Findings

- **Observation counts fall as risk rises.** In every species category, Endangered species have the lowest average observation counts (about 145-160). Threatened species average about 280, Species of Concern about 510-655, and species with no recorded status about 570-690.
- **The pattern is consistent across categories.** It holds in every category where several statuses are present, though not every category has every status (for example, Reptiles and Nonvascular Plants have only Species of Concern and Not listed).
- **Case studies: gray wolf (*Canis lupus*) and rainbow trout (*Oncorhynchus mykiss*).** Their counts (715 and 813) are far above the Endangered and Threatened groups and in line with, or above, the Not listed groups. The trout is a mild outlier among Not listed fish.
- **These are descriptive results.** Observation counts reflect survey effort and detectability, not population size, and they cannot explain why a species has a given status.

## Datasets

| File | Rows | Columns | Description |
|---|---|---|---|
| `observations.csv` | 23,296 | 3 | Observation counts per species per park (`scientific_name`, `park_name`, `observations`) |
| `species_info.csv` | 5,824 | 4 | Species metadata (`category`, `scientific_name`, `common_names`, `conservation_status`) |

**Final cleaned table** (`species_observation_processed.csv`): 5,541 rows, one per species.

| Column | Description |
|---|---|
| `scientific_name` | Species identifier (unique per row) |
| `observations` | Total observations summed across the four parks |
| `category` | Species group (7 categories) |
| `conservation_status` | Endangered, Threatened, Species of Concern, In Recovery, or missing (about 97% missing) |

## Project Structure

```
.
├── data/
│   ├── raw/
│   │   ├── observations.csv
│   │   └── species_info.csv
│   └── cleaned/
│       └── species_observation_processed.csv
├── notebooks/
│   ├── 01-data-cleaning-and-preparation.ipynb
│   └── 02-exploratory-data-analysis.ipynb
└── README.md
```

## Notebooks

### 1. Data Cleaning and Preparation
Inspects both raw files, checks data quality, resolves duplicate and conflicting records, and exports one row per species.

- Confirmed both files cover the same 5,541 scientific names, and that every species has records in all four parks.
- Summed observations across parks to get one total per species (range 131-1,785, median 573).
- Dropped `common_names`: 246 common names are shared by more than one scientific name, so the column does not identify a single species.
- Removed exact duplicates, taking the species table from 5,824 to 5,543 rows.
- Resolved two species that appeared twice with conflicting statuses (*Canis lupus* and *Oncorhynchus mykiss*) by setting their statuses to missing and keeping one row each.
- Merged the tables with a left join on `scientific_name` and exported the result.

### 2. Exploratory Data Analysis
Explores observation counts by conservation status.

- Fills missing statuses with a "Not listed" label so those species appear as their own group.
- Compares average observation counts across all categories with a grouped bar chart.
- Uses box plots to compare distributions within mammals and fish, each with a reference line for one species of interest.
- Summarizes findings, limitations, and next steps.

## Limitations

- **Most statuses are missing.** About 97% of species have no recorded conservation status. "Not listed" is a label added in this analysis, and it does not confirm that a species is not at risk.
- **The case-study species are labelled Not listed because of a cleaning decision.** Their conflicting status records were cleared in Notebook 1, so the case studies check whether their counts resemble the Not listed group. They do not validate an independent label.
- **Counts are not population sizes.** They depend on survey effort, detectability, and park coverage.
- **Some groups are very small.** For example, mammals have only 5 Endangered and 2 Threatened species, so their averages and box plots are unstable.
- **One count per species.** After aggregation, the data has no per-park or time dimension.

## Possible Next Steps

- Check the wolf and trout labels, and the Not listed group more broadly, against an external conservation source.
- Add group sizes (`n`) to the charts or a summary table.
- Normalize counts by park or survey effort, if that data becomes available.
- Test whether the differences between status groups are statistically meaningful, for example with a Kruskal-Wallis test.

## Tools

Python, pandas, Matplotlib, Seaborn, Jupyter Notebook.

## How to Run

1. Clone the repository.
2. Install the dependencies:
   ```bash
   pip install pandas matplotlib seaborn jupyter
   ```
3. Place `observations.csv` and `species_info.csv` in `data/raw/`.
4. Run the notebooks in order, from the `notebooks/` folder: `01-data-cleaning-and-preparation.ipynb`, then `02-exploratory-data-analysis.ipynb`.

## Data Source
The CSV dataset was obtained from Codecademy as part of its learning materials. The dataset is used here for educational and portfolio purposes.
<!-- Add the source and license of the original datasets here. -->
