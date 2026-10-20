# GRAF_113 — Graph Data Science Midterm (UTS)

Repository for the Mid-Term Examination (UTS) in **Graph Data Science** (Gasal 2026/2027), Undergraduate Data Science Program, Faculty of Mathematics and Natural Sciences, Universitas Negeri Surabaya.

This project implements graph database construction and analysis using **Neo4j** on the [RetailRocket e-commerce dataset](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset). It covers data temporal filtering, batch Cypher ingestion, graph data modeling, and user-item Vector Space Modeling (VSM) using sparse matrices.

## Profile

- **Name:** E. Adi Sforza S.R.
- **Student ID (NIM):** 24031554113
- **Class:** 24-A
- **Study Program:** S1 Sains Data (Data Science)
- **Institution:** Universitas Negeri Surabaya

## Repository Structure

```text
GRAF_113/
├── README.md                 # Root documentation
├── requirements.txt          # Python dependencies
└── UTS/                      # Midterm examination workspace
    ├── README_UTS.md         # Detailed documentation for the UTS directory
    ├── Pendahuluan.ipynb     # Main lab: filtering, Cypher constraints, batch import, VSM
    ├── Soal_UTS.IPYNB        # Conceptual checkpoint questions & initial Neo4j setup
    └── Data/                 # Preprocessed dataset splits
        ├── events_P1.csv     # Filtered interactions for Period 1 (Train fold)
        └── events_P2.csv     # Filtered interactions for Period 2 (Test fold)
