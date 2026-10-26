# Evaluating 2-Opt Local Search Intensity in a Hybrid Genetic Algorithm for the Traveling Salesman Problem

This repository contains the implementation, experimental outputs, statistical analyses, and figures for a study evaluating how the **intensity of 2-opt local search** affects the performance of a hybrid Genetic Algorithm (GA) for the Traveling Salesman Problem (TSP).

## Research Objective

The study investigates whether increasing the proportion of the GA population receiving 2-opt local refinement produces sufficient improvements in solution quality and convergence to justify the additional computational cost.

Four configurations were evaluated:

| Configuration | 2-Opt Intensity | Tours Refined |
|---|---:|---:|
| Standard GA | 0% | 0 |
| Low intensity | 10% | 10 |
| Moderate intensity | 30% | 30 |
| High intensity | 50% | 50 |

The population size was fixed at 100.

## Research Questions

**RQ1.** How does the intensity of 2-opt local search affect solution quality across TSP benchmark instances of varying sizes?

**RQ2.** How does 2-opt application intensity affect convergence behavior?

**RQ3.** What solution-quality and computational-time tradeoff results from different 2-opt intensities as problem size increases?

## Benchmark Instances

Six symmetric Euclidean TSP instances from TSPLIB were used.

| Instance | Cities | Known Optimum |
|---|---:|---:|
| eil51 | 51 | 426 |
| eil76 | 76 | 538 |
| kroA100 | 100 | 21,282 |
| ch150 | 150 | 6,528 |
| kroA200 | 200 | 29,368 |
| pr264 | 264 | 49,135 |

All instances use the TSPLIB `EUC_2D` edge-weight specification.

## Genetic Algorithm Configuration

The underlying GA was held constant across all experimental configurations.

| Parameter | Setting |
|---|---|
| Population size | 100 |
| Initialization | Random permutations |
| Selection | Tournament selection |
| Tournament size | 3 |
| Crossover | Order Crossover (OX) |
| Crossover probability | 0.80 |
| Mutation | Inversion mutation |
| Mutation probability | 0.20 |
| Elitism | Best 1 tour |
| Generations | 500 |
| 2-opt strategy | Best improvement |
| 2-opt frequency | Every 10 generations |

Best-improvement 2-opt was continued until no improving exchange remained for each selected candidate.

## Experimental Design

Each of the four configurations was evaluated on all six benchmark instances using **30 independent runs**.

Seeds **42-71** were used.

The complete experiment therefore consisted of:

`6 instances x 4 configurations x 30 runs = 720 runs`

Matched GA seeds were used across the four configurations. A separate random-number stream was used for 2-opt candidate selection:

`2-opt seed = 10000 + GA seed`

This prevented differences in 2-opt intensity from altering the random sequence used by the underlying GA.

At each 2-opt activation, a common randomized ordering of population indices was used for corresponding hybrid runs. The 10% configuration used the first 10 candidates, the 30% configuration used the first 30, and the 50% configuration used the first 50. This created nested local-search recipient sets.

## Recorded Measures

For each run, the experiment recorded:

- final best tour length;
- percentage optimality gap;
- execution time;
- generation at which the final best solution was first reached;
- first generation within 5% of the known optimum;
- first generation within 1% of the known optimum;
- whether the known optimum was reached;
- number of 2-opt applications;
- number of accepted 2-opt moves; and
- best-so-far solution at every generation.

The experiment generated **720 run-level records** and **360,000 generation-level convergence records**.

## Statistical Analysis

The analysis included:

- descriptive statistics;
- Friedman tests across the four configurations;
- Kendall's W effect size;
- paired Wilcoxon signed-rank post-hoc tests;
- Holm correction for multiple pairwise comparisons;
- convergence-threshold attainment analysis; and
- quality-runtime tradeoff analysis.

Statistical significance was evaluated at **alpha = 0.05**.

## Repository Structure

```text
GA-2Opt-TSP-Intensity/
|-- notebooks/
|   `-- GA_2Opt_TSP_Experiment.ipynb
|-- results/
|   |-- individual_runs/
|   |-- convergence_histories/
|   |-- statistical_tests/
|   |-- master_experiment_results.csv
|   |-- descriptive_summary.csv
|   |-- convergence_threshold_summary.csv
|   |-- matched_quality_improvement_vs_GA.csv
|   |-- publication_results_table.csv
|   `-- runtime_cost_summary.csv
|-- figures/
|   |-- figure1_mean_optimality_gap.png
|   |-- figure2_mean_runtime.png
|   |-- figure3_combined_convergence.png
|   |-- figure4_quality_runtime_tradeoff.png
|   `-- supplementary_convergence/
|-- requirements.txt
|-- LICENSE
`-- README.md
```

## Main Experimental Outputs

`master_experiment_results.csv` contains the complete run-level dataset for all 720 experimental runs.

`convergence_histories/` contains the generation-by-generation best-so-far histories for all 720 runs.

`individual_runs/` contains the saved output for each individual experimental run.

`statistical_tests/` contains the Friedman and Holm-adjusted Wilcoxon test results.

`figures/` contains the figures generated from the final experimental analysis.

## Reproducing the Analysis

Install the required Python packages:

```bash
pip install -r requirements.txt
```

The complete experimental and analysis workflow is provided in:

`notebooks/GA_2Opt_TSP_Experiment.ipynb`

The notebook documents the implementation, experiment execution, aggregation, statistical analysis, and figure generation.

## Software

The experiment was implemented in Python and executed using Google Colab.

Primary Python dependencies include NumPy, pandas, SciPy, and Matplotlib.

## Data Source

Benchmark instances were obtained from the **TSPLIB** benchmark library.

The repository does not redistribute the original TSPLIB benchmark files. Instance names and benchmark specifications are documented so that the original instances can be obtained from TSPLIB for reproduction.

## License

This repository is released under the MIT License.

## Author

**Oluwatoyin Kode**  
Department of Computer Science  
Bowie State University
