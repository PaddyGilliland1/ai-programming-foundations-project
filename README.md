# ai-programming-foundations

## Project description

A reusable pandas workflow in one Jupyter notebook, `data_workflow.ipynb`, that loads Birmingham
City Council's purchase card transactions for 2024, cleans them with four documented functions,
summarises spend with one analysis function and draws three charts. The question is where the
council's card spend goes, and how far the raw record can be trusted to answer that. One
merchant, by its name the council paying itself, is kept and flagged but set aside from the
primary analysis because nothing in the data says what those payments are for, and the notebook
reports the total in the published file beside the total analysed. The notebook records each
cleaning decision, including the rules I considered and rejected.

## Dataset

- Name: Purchase Card Transactions, Birmingham City Council, calendar year 2024 (Birmingham City
  Observatory)
- Source: <https://cityobservatory.birmingham.gov.uk/explore/dataset/purchase-card-transactions/information/>
- Licence: Open Government Licence v3.0,
  <https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/>, as stated on the
  dataset page
- File: `birmingham_purchase_cards_2024.csv`, in this repository beside the notebook. It is the
  dataset page's CSV export filtered to transactions dated in 2024 (42,560 rows, 10 columns), kept
  as downloaded apart from its file name.

Contains public sector information licensed under the Open Government Licence v3.0.

## How to run

You need Git and **Python 3.12 or newer**. The notebook was run with Python 3.12.3, and the
package versions pinned in `requirements.txt` (for example pandas 3.0.6 and numpy 2.5.3) will not
install on older versions of Python. Check yours with `python3 --version` (on Windows,
`py --version`).

1. Clone this repository and move into its folder:

   ```bash
   git clone https://github.com/PaddyGilliland1/ai-programming-foundations-project.git
   cd ai-programming-foundations-project
   ```

2. Create and activate a virtual environment:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

   On Windows, create and activate it in PowerShell with:

   ```powershell
   py -3.12 -m venv .venv
   .venv\Scripts\Activate.ps1
   ```

   If PowerShell refuses to run the activate script, allow it for this window only with
   `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`, then run the activate line again.
   On a Mac whose `python3` is older than 3.12, install Python 3.12 from python.org and use
   `python3.12 -m venv .venv`.

3. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Open the notebook, then restart the kernel and run all cells:

   ```bash
   jupyter notebook data_workflow.ipynb
   ```

   The notebook reads `birmingham_purchase_cards_2024.csv` from its own folder, so start Jupyter
   from the repository folder.

## How requirements.txt was created

The notebook was run top to bottom in a fresh virtual environment, and the file was then written
from that environment with:

```bash
pip freeze > requirements.txt
```

## Reflections

### Bias awareness

[TO WRITE in your own words.]

### ML workflow changes

[TO WRITE in your own words.]

### Neural network preparation

[TO WRITE in your own words.]

### Agentic automation potential

[TO WRITE in your own words.]
