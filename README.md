# README.md

# Assessing the Macroeconomic Impacts of Disasters: an Updated Multi-Regional Impact Assessment (MRIA) model

<!-- PROJECT SHIELDS -->
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)
[![Type Checking: mypy](https://img.shields.io/badge/type_checking-mypy-blue)](http://mypy-lang.org/)
[![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=flat&logo=numpy&logoColor=white)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-%23025596?style=flat&logo=scipy&logoColor=white)](https://scipy.org/)
[![Gurobi](https://img.shields.io/badge/Gurobi-8A2BE2.svg?style=flat&logo=gurobi&logoColor=white)](https://www.gurobi.com/)
[![Jupyter](https://img.shields.io/badge/Jupyter-%23F37626.svg?style=flat&logo=Jupyter&logoColor=white)](https://jupyter.org/)
[![arXiv](https://img.shields.io/badge/arXiv-2508.00510-b31b1b.svg)](https://arxiv.org/abs/2508.00510)
[![Research](https://img.shields.io/badge/Research-Disaster%20Economics-green)](https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters)
[![Discipline](https://img.shields.io/badge/Discipline-Computational%20Economics-blue)](https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters)
[![Methodology](https://img.shields.io/badge/Methodology-Input--Output%20Optimization-orange)](https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters)
[![Year](https://img.shields.io/badge/Year-2025-purple)](https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters)

**Repository:** `https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters`

**Owner:** 2025 Craig Chirinda (Open Source Projects)

This repository contains an **independent**, professional-grade Python implementation of the research methodology from the 2025 paper entitled **"Assessing the Macroeconomic Impacts of Disasters: an Updated Multi-Regional Impact Assessment (MRIA) model"** by:

*   Surender Raj Vanniya Perumal
*   Mark Thissen
*   Marleen de Ruiter
*   Elco E. Koks

The project provides a complete, end-to-end computational framework for evaluating the regional and macroeconomic consequences of disasters. It implements the updated MRIA model, a supply-constrained, multi-regional input-output model solved via linear programming. The goal is to provide a transparent, robust, and computationally efficient toolkit for researchers and policymakers to replicate, validate, and apply the MRIA framework to assess economic resilience and inform disaster risk mitigation strategies.

## Table of Contents

- [Introduction](#introduction)
- [Theoretical Background](#theoretical-background)
- [Features](#features)
- [Methodology Implemented](#methodology-implemented)
- [Core Components (Notebook Structure)](#core-components-notebook-structure)
- [Key Callable: run_full_experiment](#key-callable-run_full_experiment)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Input Data Structure](#input-data-structure)
- [Usage](#usage)
- [Output Structure](#output-structure)
- [Project Structure](#project-structure)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)
- [Citation](#citation)
- [Acknowledgments](#acknowledgments)

## Introduction

This project provides a Python implementation of the methodologies presented in the 2025 paper "Assessing the Macroeconomic Impacts of Disasters: an Updated Multi-Regional Impact Assessment (MRIA) model." The core of this repository is the iPython Notebook `MRIA_model_macro_impact_disasters_draft.ipynb`, which contains a comprehensive suite of functions to replicate the paper's findings, from initial data validation to the final execution of a full suite of robustness checks.

Traditional input-output models often struggle to capture the supply-side shocks and logistical bottlenecks characteristic of disasters. This project implements the updated MRIA framework, which addresses these shortcomings by incorporating production capacity constraints, inter-regional substitution possibilities, and explicit logistical frictions.

This codebase enables users to:
-   Rigorously validate and structure a complete set of multi-regional supply-use data.
-   Transform tabular economic data into the precise numerical tensors required for optimization.
-   Calibrate the model's key behavioral parameter (`alpha`) to ensure its baseline fidelity.
-   Execute the core three-step optimization algorithm to simulate the post-disaster economic equilibrium.
-   Conduct a full suite of analyses presented in the paper:
    -   **Sensitivity Analysis:** Explore the trade-offs between production and trade flexibility.
    -   **Criticality Analysis:** Identify systemically important economic sectors based on their irreplaceability.
    -   **Incremental Disruption Analysis:** Trace the non-linear failure pathways of the economy under escalating stress.
-   Execute a full suite of robustness checks to validate the stability of the model's conclusions.

## Theoretical Background

The implemented methods are grounded in input-output economics and linear programming, providing a quantitative framework for simulating economic shocks.

**1. Supply-Constrained, Multi-Regional Framework:**
The model is built on a multi-regional Supply-and-Use Table (SUT) framework. Unlike traditional demand-driven models, the MRIA model's primary shock is a reduction in production capacity ($\delta_{r,s} < 1$) in a disaster-affected region. Unaffected regions can compensate by increasing their own production, but this is limited by their own capacity and by logistical constraints on trade.

**2. Three-Step Optimization Algorithm:**
The post-disaster equilibrium is found by solving a sequence of three linear programs, which reflects a clear hierarchy of economic priorities:

-   **Step 1: Minimize Rationing.** The primary goal is to satisfy as much final demand as possible, minimizing the direct welfare loss to consumers.
    $$ \min z_1 = \sum_{r,p} v_{r,p} $$
-   **Step 2: Minimize Economic Effort.** Given the unavoidable level of rationing found in Step 1, the model finds the most efficient (least-cost) way to organize production and trade to meet the remaining demand. The objective function includes a calibrated penalty ($\alpha$) for using new or expanded trade routes.
    $$ \min z_2 = \sum_{r,s} x_{r,s} + \alpha \sum_{r',r,p} t_{r',r,p} $$
-   **Step 3: Quantify Production Equivalent of Rationing.** An analytical step to calculate the hypothetical production ($x'$) that would have been needed to satisfy the rationed demand, allowing for a comprehensive impact assessment.
    $$ \min z_3 = \sum_{r,s} x'_{r,s} $$

**3. Comprehensive Impact Assessment:**
The total economic impact ($c$) is calculated as the sum of three distinct components, providing a holistic view of the disaster's costs:
$$ c = \sum_{r,p} (s^i_{r,p} - (s_{r,p} - v_{r,p})) + \sum_{r,p} k_{r,p} + \sum_{r,s} x'_{r,s} $$
where the terms represent (1) the loss in efficiently supplied products, (2) the value of inefficient overproduction, and (3) the production equivalent of the welfare loss from rationing.

## Features

The provided iPython Notebook (`MRIA_model_macro_impact_disasters_draft.ipynb`) implements the full research pipeline, including:

-   **Data Validation Pipeline:** A robust, modular system for validating the structure, content, and economic consistency of all input data.
-   **Rigorous Preprocessing:** A deterministic pipeline for transforming tabular data into the precise `numpy` tensors required by the optimization engine.
-   **Correct and Remediated Core Engine:** An accurate and professional-grade implementation of the three-step optimization algorithm using the `gurobipy` library, including a robust calibration routine.
-   **Automated Analysis Orchestrators:** High-level functions that automate the execution of the Sensitivity, Criticality, and Incremental Disruption analyses with a single call.
-   **Comprehensive Robustness Suite:** A full suite of advanced robustness checks to analyze the framework's sensitivity to parameters, data uncertainty (via Monte Carlo), and methodological choices.
-   **Full Research Lifecycle:** The codebase covers the entire research process from data ingestion to final, validated results, providing a complete and transparent replication package.

## Methodology Implemented

The core analytical steps directly implement the methodology from the paper:

1.  **Input Data Validation (Task 1):** The pipeline ingests four `pandas` DataFrames and a configuration dictionary, and rigorously validates their integrity.
2.  **Data Preprocessing (Task 2):** It transforms the validated data into a structured set of `numpy` arrays and index mappings.
3.  **Model Calibration (Task 3):** It systematically calibrates the `alpha` trade cost parameter to ensure the model replicates the baseline economy.
4.  **Core MRIA Algorithm (Task 4):** It implements the central three-step optimization algorithm for a single disaster scenario.
5.  **Sensitivity Analysis (Task 5):** It executes the core algorithm across a grid of 18 parameter settings to analyze resilience trade-offs.
6.  **Criticality Analysis (Task 6):** It executes hundreds of simulations to stress-test each economic sector individually and calculate its systemic importance.
7.  **Incremental Disruption Analysis (Task 7):** It executes dozens of simulations to trace the economy's non-linear response to escalating shocks.
8.  **Orchestration & Robustness (Tasks 8-9):** Master functions orchestrate the main pipeline and the optional, full suite of robustness checks.

## Core Components (Notebook Structure)

The `MRIA_model_macro_impact_disasters_draft.ipynb` notebook is structured as a logical pipeline with modular orchestrator functions for each of the 9 major tasks.

## Key Callable: run_full_experiment

The central function in this project is `run_full_experiment`. It orchestrates the entire analytical workflow, providing a single entry point for running the main analyses and the advanced robustness checks, all controlled by a single configuration dictionary.

```python
def run_full_experiment(
    initial_production: pd.DataFrame,
    initial_trade: pd.DataFrame,
    supply_coefficients: pd.DataFrame,
    use_coefficients: pd.DataFrame,
    config: Dict[str, Any]
) -> Dict[str, Any]:
    """
    Executes the complete MRIA research study, including primary and robustness analyses.
    """
    # ... (implementation is in the notebook)
```

## Prerequisites

-   Python 3.8+
-   A Gurobi license. Gurobi offers free academic licenses. The `gurobipy` package must be installed and the license configured.
-   Core dependencies: `pandas`, `numpy`, `scipy`, `gurobipy`, `tqdm`.

## Installation

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters.git
    cd MRIA_model_macro_impact_disasters
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Python dependencies:**
    ```sh
    pip install pandas numpy scipy "gurobipy>=9.5" tqdm
    ```

4.  **Install and configure Gurobi:**
    Follow the instructions on the [Gurobi website](https://www.gurobi.com/documentation/) to install the Gurobi Optimizer and activate your license.

## Input Data Structure

The pipeline requires four `pandas` DataFrames with specific structures, which are rigorously validated by the first task.
1.  `initial_production`: `MultiIndex('region', 'sector')`, column `'production_value'`.
2.  `initial_trade`: `MultiIndex('origin_region', 'dest_region', 'product')`, column `'trade_value'`.
3.  `supply_coefficients`: `MultiIndex('region', 'sector', 'product')`, column `'coefficient'`.
4.  `use_coefficients`: `MultiIndex('dest_region', 'dest_sector', 'product', 'origin_region')`, column `'coefficient'`.

The entire experiment is controlled by a single, comprehensive Python dictionary, `config`. A fully specified example is provided in the notebook.

## Usage

The `MRIA_model_macro_impact_disasters_draft.ipynb` notebook provides a complete, step-by-step guide. The core workflow is:

1.  **Prepare Inputs:** Load your four data `DataFrame`s and define your `config` dictionary. A complete template is provided.
2.  **Execute Pipeline:** Call the master orchestrator function.

    ```python
    # This single call runs all analyses enabled in the config dictionary.
    full_results = run_full_experiment(
        initial_production,
        initial_trade,
        supply_coefficients,
        use_coefficients,
        full_config
    )
    ```
3.  **Inspect Outputs:** Programmatically access any result from the returned dictionary. For example, to view the criticality analysis results:
    ```python
    crit_results = full_results['main_analysis_results']['criticality_analysis_results']
    print(crit_results.head())
    ```

## Output Structure

The `run_full_experiment` function returns a single, comprehensive dictionary with two top-level keys:
-   `main_analysis_results`: A dictionary containing the results of the primary analyses (Sensitivity, Criticality, Incremental Disruption) that were enabled in the config.
-   `robustness_analysis_results`: A dictionary containing the results of the advanced robustness checks that were enabled in the config.

## Project Structure

```
MRIA_model_macro_impact_disasters/
│
├── MRIA_model_macro_impact_disasters_draft.ipynb  # Main implementation notebook   
├── requirements.txt                                 # Python package dependencies
├── LICENSE                                          # MIT license file
└── README.md                                        # This documentation file
```

## Customization

The pipeline is highly customizable via the master `config` dictionary. Users can easily enable/disable any analysis and modify all relevant parameters, including:
-   The `disruption_scenario` for the sensitivity analysis.
-   The `parameter_grid` of resilience factors to test.
-   The `disruption_magnitude` for the criticality analysis.
-   The target sectors and `disruption_levels` for the incremental analysis.
-   All parameters for the suite of robustness checks.

## Contributing

Contributions are welcome. Please fork the repository, create a feature branch, and submit a pull request with a clear description of your changes. Adherence to PEP 8, type hinting, and comprehensive docstrings is required.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Citation

If you use this code or the methodology in your research, please cite the original paper:

```bibtex
@article{perumal2025assessing,
  title={Assessing the Macroeconomic Impacts of Disasters: an Updated Multi-Regional Impact Assessment (MRIA) model},
  author={Perumal, Surender Raj Vanniya and Thissen, Mark and de Ruiter, Marleen and Koks, Elco E.},
  journal={arXiv preprint arXiv:2508.00510},
  year={2025}
}
```

For the implementation itself, you may cite this repository:
```
Chirinda, C. (2025). A Python Implementation of "Assessing the Macroeconomic Impacts of Disasters: an Updated Multi-Regional Impact Assessment (MRIA) model". 
GitHub repository: https://github.com/chirindaopensource/MRIA_model_macro_impact_disasters
```

## Acknowledgments

-   Credit to Surender Raj Vanniya Perumal, Mark Thissen, Marleen de Ruiter, and Elco E. Koks for their insightful and clearly articulated research.
-   Thanks to the developers of the scientific Python ecosystem (`numpy`, `pandas`, `scipy`) and the Gurobi team for their powerful optimization tools.

---

*This README was generated based on the structure and content of `MRIA_model_macro_impact_disasters_draft.ipynb` and follows best practices for research software documentation.*
