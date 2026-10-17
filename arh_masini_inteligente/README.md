# Kantorovich Linear Programming Study

A Python project for exploring Leonid Kantorovich's machine-allocation model and comparing linear-programming solutions with reference results. It also includes transportation and portfolio examples, benchmark data, result plots, and a Romanian teaching notebook.

## What the code solves

`main.py` uses PuLP with CBC for linear programs and SciPy's SLSQP optimizer for the portfolio example:

- **Machine allocation:** maximize the number of complete sets produced. Each machine's available time is allocated across parts, and production of every part is balanced to the same output value `Z`.
- **Resource-constrained allocation:** the same model with an optional resource-consumption limit, for example energy or labor.
- **Transportation:** minimize shipping cost while respecting supply limits and meeting destination demands.
- **Portfolio:** minimize portfolio variance subject to weights summing to 1, long-only bounds, and a minimum target return.

For the allocation model, `h[i, j]` is the time assigned to machine `i` for part `j`. The core constraints are:

```text
maximize Z
sum_j h[i, j] = machine_capacity[i]
sum_i productivity[i, j] * h[i, j] = Z  for each part j
h[i, j] >= 0
```

The model uses continuous variables, so calculated output and allocations may be fractional.

## Requirements

- Python 3
- PuLP and a CBC executable available to PuLP
- NumPy and SciPy
- For plots: Matplotlib, pandas, and seaborn
- For interactive notebook use: Jupyter (optional)

Install the Python packages from the project directory:

```bash
python -m pip install pulp numpy scipy matplotlib pandas seaborn
```

If PuLP cannot find CBC on your system, install CBC separately and make it available to PuLP.

## Run the benchmark report

Run from the project directory because the scripts load JSON files by relative path:

```bash
python main.py
```

The script prints calculated values, reference values when available, solve time, and a `PASS`, `FAIL`, or `N/A` validation status. Validation uses a tolerance of `0.5` for calculated values above `10`, and `0.05` otherwise.

## Generate plots

```bash
python plot_results.py
```

This reruns the benchmark calculations and saves `grafic_validare.png`, `grafic_performanta.png`, and `grafic_heatmap_kantorovich.png` in the project directory. The heatmap uses a hard-coded allocation example in `plot_results.py`.

## Teaching notebook

Open `Lectia_Kantorovich_Advanced.ipynb` in Jupyter to read the Romanian lesson on the historical context, the allocation model, and resource constraints. To regenerate the notebook from its Python generator, run:

```bash
python genereaza_lectie.py
```

This rewrites `Lectia_Kantorovich_Advanced.ipynb` in the current directory.

## Data and project files

| File | Purpose |
| --- | --- |
| `kantorovich_input_data.json` | Historical problem definitions and metadata. |
| `kantorovich_output_data.json` | Reference solutions and benchmarks from the source material. |
| `generated_input_data.json` | Extended, generated problem set and metadata. |
| `generated_output_data.json` | Baseline values used to validate generated allocation and transportation cases. |
| `main.py` | Solver, benchmark runner, validation, and console report. |
| `plot_results.py` | Benchmark plots. |
| `genereaza_lectie.py` | Generator for the teaching notebook. |
| `arh_masini_calc (1).pdf`, `kantorovich_mathematical_methods-3.pdf`, `tayyar.text` | Supporting source and presentation materials; not read by the scripts. |

## Current benchmark coverage

Although the JSON data contains other problem families, `main.py` currently runs only:

- generated Problem A allocation variants;
- generated Problem B resource-constrained allocation variants;
- generated transportation problems;
- generated portfolio optimization cases;
- the original Kantorovich Problem A.

The cutting-stock, diet, scheduling, and other historical examples are present as data but are not processed by `main.py`. The reference output file has no portfolio baseline entries, so portfolio runs are reported as `N/A` for validation. Historical reference outputs other than the original Problem A are also not currently wired into the validation lookup.
