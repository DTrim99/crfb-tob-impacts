# Jupyter Book Build Instructions

This directory contains a Jupyter Book analyzing Social Security taxation reform options using PolicyEngine.

## Prerequisites

- Python 3.8 or higher
- pip package manager

## Installation

1. Install the required packages:
```bash
pip install -r requirements.txt
```

2. Install Jupyter Book:
```bash
pip install jupyter-book
```

## Building the Book

To build the Jupyter Book:

```bash
jupyter-book build jupyterbook/
```

This will create an HTML version of the book in the `jupyterbook/_build/html/` directory.

## Viewing the Book

After building, you can view the book by opening `jupyterbook/_build/html/index.html` in your web browser.

## Development

For development and live preview:

```bash
jupyter-book serve jupyterbook/
```

This will start a local server and automatically rebuild the book when files change.

## Structure

- `_config.yml`: Jupyter Book configuration
- `_toc.yml`: Table of contents structure
- `index.md`: Main introduction page
- `intro/`: Introduction chapter
- `policies/`: Policy descriptions chapter
- `methods/`: Methodology chapter
- `results/`: Results and analysis chapter
- `comparison/`: Comparison with other studies chapter
- `conclusion/`: Policy recommendations chapter

## Data

The analysis uses data from the `analyze_policy_impacts.ipynb` notebook in the parent directory. Results are saved to `policy_impacts.csv` and copied to the React dashboard.

## Notes

- The book is configured to force re-execution of notebooks
- All chapters are written in Markdown format
- Interactive elements can be added using Jupyter widgets
- The book includes navigation, search, and download functionality
