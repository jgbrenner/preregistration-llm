# Breaking the Big Five
### Red-Teaming Large Language Models with Paradoxical Persona Injection

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
[![OSF Preregistration](https://img.shields.io/badge/OSF-Preregistration-blue)](https://osf.io/...)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1234567.svg)](https://doi.org/10.5281/zenodo.1234567)



This repository contains the full source code, persona prompts, data, and analysis pipeline for the preregistered study, **"Breaking the Big Five."** We red-team five frontier LLMs with a semantic stress-test we dub the **Persona Gauntlet**—a series of biographical prompts escalating from "ordinary human" to paradox.

Our goal: to use the 44-item Big Five Inventory (BFI-44)  to discover exactly when, and how, the models' simulated psychological structures collapse under pressure.

This repo contains everything you need to replicate, verify, or build upon our findings.

## Project Links

*   **Preregistration (OSF):** The finalized preregistration document can be found [at this DOI](https://osf.io/...).
*   **Live Repository (GitHub):** [https://github.com/jgbrenner/preregistration-llm](https://github.com/jgbrenner/preregistration-llm)
*   **Archival DOI (Zenodo):** A permanent, citable snapshot of the repository will be minted on Zenodo upon project completion.

## Preregistration Document

This repository contains the full preregistration document in two formats inside the `/preregistration` directory.

*   📄 **[preregistration.md](./preregistration/preregistration.md):** The primary, version-controlled source file written in Markdown. Use this to track the complete history of changes.
*   📄 **[preregistration.pdf](./preregistration/preregistration.pdf):** A PDF version generated from the Markdown file. Use this for easy downloading and as the static, archival copy that matches the OSF submission.

## The Persona Gauntlet

Our core manipulation involves six levels of escalating semantic and psychological strain:

1.  **Void:** No biography. The model's raw, unprompted baseline.
2.  **Standard:** Realistic, on-distribution human lives based on IPIP descriptors.
3.  **Extreme Domain:** One Big Five trait is cranked to 11 (e.g., maximum possible Agreeableness).
4.  **Internal Facet Paradox:** A single domain’s two facets are set in opposition (e.g., high Assertiveness, low Activity).
5.  **Cross-Domain Paradox:** Personas embodying mutually exclusive traits across different domains.
6.  **Impossible/Ontological:** Logically absurd or non-human entities.

## Directory Structure

```
├── preregistration/ # OSF preregistration source (Markdown and PDF)
├── code/ # All Python and R scripts
│ ├── 01_generate_data/ # Scripts to query LLM APIs and run the Gauntlet
│ ├── 02_analysis/ # Bayesian CFA, IRT, regression, and network models
│ └── utils/ # Helper functions and configuration
├── personas/ # The .txt files for every persona prompt, organized by Gauntlet level
├── data/ # Raw (JSONL) and processed (CSV) data
├── notebooks/ # jupyter notebooks
├── results/ # Generated figures, tables, and model summaries
├── environment.yml # Conda environment for a fully reproducible setup
├── requirements.txt # Python dependencies for pip
├── LICENSE # MIT License
└── README.md # You are here.
```

## Replication Guide

### Step 0: Setup

You will need Python (≥ 3.10), R (≥ 4.2), and Git. We strongly recommend using a mamba virtual environment.

```
## 1. Clone the repository

git clone https://github.com/jgbrenner/preregistration-llm.git
cd preregistration-llm

# 2. Create and activate the Conda environment
# (This installs Python, R, and all dependencies from environment.yml)
mamba env create -f environment.yml
conda activate llm-red-team

# 3. Set up your API keys
# (Follow instructions in code/01_generate_data/config.py.template)

###Step 1: Run the Gauntlet
This script queries the LLM APIs for all 30,000 runs. It will take a significant amount of time and API credits.


# Run the full experiment on all preregistered models
python code/01_generate_data/run_gauntlet.py --models gpt-4o gemini-2.5-pro claude-4-sonnet llama-3.1-405b deepseek-chat-v3


Step 2: Run the Analysis Pipeline
These R scripts reproduce every statistical test and figure from the paper.


# Execute the full pipeline in order
Rscript code/02_analysis/01_preprocess.R
Rscript code/02_analysis/02_cfa_and_breakdown.R
Rscript code/02_analysis/03_irt_and_entropy_models.R
Rscript code/02_analysis/04_network_analysis.R

Step 3: Check the Outputs
All generated figures, tables, and statistical summaries are saved to the results/ directory.
```
Contributing
We welcome contributions! Please feel free to submit an issue or pull request for:

Bug fixes and dependency updates
Replication on new or different LLMs
Alternative analysis approaches
Documentation improvements
License
This project is licensed under the MIT License. See the LICENSE file for full details.

Contact
Primary Author: JB (MSc Psychology, Computational Psychometrics)
Email:  brennja8557_aeh@students.vizja.pl
