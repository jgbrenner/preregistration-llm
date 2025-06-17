# Simulated Selves: Stress-Testing Large Language Models with Big-Five Personality Inventories

This repository contains all materials for the preregistered study *Simulated Selves: Stress-Testing Large Language Models with Big-Five Personality Inventories*, including the preregistration document, code for data generation and analysis, persona prompts, and anonymized data.

## Repository Link

- **GitHub:** [https://github.com/jgbrenner/preregistration-llm](https://github.com/jgbrenner/preregistration-llm)

## OSF Preregistration

The official preregistration will be archived on OSF under the Open Science Framework Preregistration Template. The OSF registration ID will be added here upon submission.

## Directory Structure

```
├── preregistration/       # OSF preregistration source and PDF
├── code/                  # Python and R scripts for data generation and analysis
│   ├── generate_data/     # Scripts to run LLM simulations
│   ├── analysis_pipeline/ # Bayesian factor, mixed models, network psychometrics
│   └── utils/             # Helper functions
├── personas/              # Persona prompt files (Standard, Extreme, Paradox, Baseline)
├── data/                  # Raw and processed data (anonymized)
├── results/               # Figures, tables, and summary outputs
├── docs/                  # Supplementary materials and references
├── notebooks/             # Google Colab notebooks demonstrating proof-of-concept
├── environment.yml        # Conda environment specification
├── requirements.txt       # Python dependencies (pip)
├── LICENSE                # MIT License
└── README.md              # Project overview (this file)
```





## Prerequisites

- Python ≥ 3.10  
- R ≥ 4.2  
- Git

### Python Packages

Install via:

pip install -r requirements.txt


### R Packages

Install via the provided setup script:

Rscript code/analysis_pipeline/setup_packages.R


## Usage

1. **Clone the Repository**

git clone https://github.com/jgbrenner/preregistration-llm.git
cd preregistration-llm


2. **Generate Simulation Data**

All LLM runs use a fixed temperature (0.0) for deterministic responses:

python code/generate_data/run_all.py --models gpt-4o claude-3-opus mistral-medium gemini-1.5-flash cohere-commander-r --personas all


3. **Preprocess and Analyze**
```
Rscript code/analysis_pipeline/01_preprocess.R
Rscript code/analysis_pipeline/02_structural_analysis.R
Rscript code/analysis_pipeline/03_entropy_models.R
Rscript code/analysis_pipeline/04_network_psychometrics.R
```

4. **View Results**

Results (figures, tables) are saved in the `results/` directory. Key outputs include:

- Factor congruence indices  
- Entropy-linked mixed model summaries  
- Community detection comparisons  

## Contributing

Contributions are welcome! Please submit issues or pull requests for:

- Bug reports or fixes  
- Additional analyses or extensions  
- Enhancements to documentation  

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contact

Primary Author: JB (BSc Psychology, Computational Psychometrics track)  
Email: [brennja8557_aeh@students.vizja.pl](brennja8557_aeh@students.vizja.pl)
