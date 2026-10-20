# UTS — Graph Data Science

This directory contains the notebooks and data files for the Graph Data Science midterm. The work uses the RetailRocket event dataset to build a visitor–item graph in Neo4j and explore user–item interactions.

## Directory structure

```text
UTS/
├── README.md
├── Pendahuluan.ipynb
├── Soal_UTS.IPYNB
└── Data/
    ├── events_P1.csv
    └── events_P2.csv
```

## Files

- **`Pendahuluan.ipynb`** — Data preparation and exploration, Neo4j constraints and graph import, and user–item Vector Space Modelling (VSM).
- **`Soal_UTS.IPYNB`** — Introductory Neo4j and Cypher examples, checkpoint answers, and a RetailRocket import example.
- **`Data/events_P1.csv`** and **`Data/events_P2.csv`** — Event records split into two time periods for analysis.

## Dataset periods

The notebook divides the events into two consecutive 10-day periods using UTC dates:

| Period | Role | Start date | End date |
|---|---|---|---|
| P1 | Training | 2015-06-12 | 2015-06-21 |
| P2 | Test | 2015-06-22 | 2015-07-01 |

Recorded dataset counts:

| Period | Events | Visitors | Items | Views | Add-to-cart | Transactions |
|---|---:|---:|---:|---:|---:|---:|
| P1 | 188,267 | 99,659 | 59,808 | 181,674 | 5,002 | 1,591 |
| P2 | 206,884 | 110,012 | 64,577 | 200,049 | 5,053 | 1,782 |

The periods share 4,777 visitors and 31,454 items.

## Setup

From the repository root, install the project dependencies and notebook tools:

```bash
pip install -r requirements.txt pandas tqdm jupyter
```

Start a Neo4j instance, then open the notebooks:

```bash
jupyter notebook
```

The notebooks include example connection settings:

```python
URI = "neo4j://localhost:7687"
AUTH = ("neo4j", "password")
DATABASE = "neo4j"
```

Update these values for your Neo4j instance. Do not commit real passwords or credentials.

## Data availability and paths

The period-filtered CSV files are included in `Data/`. The original RetailRocket `events.csv` file is **not** included. Cells that perform raw-data preprocessing refer to local paths such as `../UTS/Local_data/events.csv` or `../UTS/Data/events.csv`; obtain the source dataset and update the relevant path before running those cells.

For graph import and analysis, use the included files:

```text
UTS/Data/events_P1.csv
UTS/Data/events_P2.csv
```

## Notebook workflow

### 1. Introduction and graph basics

`Soal_UTS.IPYNB` demonstrates connecting to Neo4j with the Python driver and creating `Person` nodes connected by directed `KNOWS` relationships. It also contains checkpoint answers about Neo4j databases, Cypher, and graph types.

### 2. Prepare and import event data

`Pendahuluan.ipynb` filters events into P1 and P2, creates uniqueness constraints, and imports data in batches using the Neo4j Python driver and Cypher.

The RetailRocket graph uses `Visitor` and `Item` nodes, with interactions represented by relationships:

- `(:Visitor)-[:VIEWED]->(:Item)`
- `(:Visitor)-[:ADDED_TO_CART]->(:Item)`
- `(:Visitor)-[:BOUGHT]->(:Item)`

Purchase imports can also create `Transaction` nodes and connect them with `MADE` and `CONTAINS` relationships.

**Database caution:** The notebook has a `RESET_DB` option. When enabled, it runs `MATCH (n) DETACH DELETE n` and deletes all nodes and relationships in the selected database. Check the setting before running import cells. The import code also checks for existing relationships for a period to avoid reimporting that period.

### 3. Build and inspect user–item VSM

The notebook assigns interaction weights and aggregates them into a sparse user-by-item matrix:

| Event | Weight |
|---|---:|
| `VIEWED` | 1 |
| `ADDED_TO_CART` | 2 |
| `BOUGHT` | 3 |

The resulting matrices are stored in SciPy CSR format. Recorded dimensions and non-zero counts:

| Period | Matrix shape (users × items) | Non-zero entries |
|---|---:|---:|
| P1 | 99,659 × 59,808 | 146,519 |
| P2 | 110,012 × 64,577 | 161,406 |

The notebook reports that more than 99.99% of matrix entries are zero, and includes summary statistics and visualizations of sampled user–item connections and user interaction counts.
