# ai-programming-foundations-project

## Project description

I built a reusable pandas workflow in one Jupyter notebook, `data_workflow.ipynb`, that loads
Birmingham City Council's purchase card transactions for 2024, cleans them with four documented
functions, screens them with a fifth and records each cleaning decision, including the rules I
considered and rejected. One analysis function summarises spend, another lists Amazon Prime charges
for review, and one helper draws three charts. The question is where the council's card spend goes,
and how far the raw record can be trusted to answer that. One merchant, by its name the council
paying itself, is kept and flagged but set aside from the primary analysis because nothing in the
data says what those payments are for, and the notebook reports the total in the published file
beside the total analysed.

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

Poor cleaning here would not just be untidy. It would move money between parts of the council and change the story the data tells. Four places matter most in this dataset:

- **Merging names.** Merchant names are cut at 25 characters and carry order numbers, so one supplier appears under many names. A loose rule, such as joining names that share the same start, would join different suppliers ("argos ltd" and "argosy toys") and inflate some totals while hiding others. So a supplier's names are joined only where each starts with the supplier's own name as a whole word, which keeps "argosy toys" apart from Argos, and the channel, such as stores, fuel or online, is kept in its own column. I applied only rules I could check against real names in the file, and I kept the raw name beside the cleaned one.
- **Merging directorates.** The council's own page warns that a directorate may appear under different names after internal changes, and the 2024 data shows it. Joining two names on a guess would move spend between services and could suggest one department overspent. I merged only one name, "Adult Social Care (eip)", which I assumed is part of Adult Social Care because the two run side by side from April, and left the uncertain renames apart.
- **Dropping rows.** Removing refunds or foreign-currency purchases would look like tidying, but removing refunds would overstate spend, and removing foreign purchases would understate it and hide who buys abroad. I kept refunds as negatives and used the amount billed in pounds.
- **Hiding what cannot be explained.** One merchant, which by its name is the council paying itself, takes 18.06% of the published net spend, and nothing in the file says what for. Quietly deleting it, or quietly keeping it, would both mislead. I kept the rows, flagged them, set them aside from the primary analysis and reported both totals, so anyone can see the effect.

The data also has a coverage bias. Purchase cards are one payment method of council spending, and nearly a fifth of the spend in this file appears to be the council paying itself. Any conclusion about Birmingham's spending as a whole would be misleading if drawn from this file alone.

### ML workflow changes

The same cleaning functions would sit at the front of a machine learning pipeline, run in the same order on the training data and on any new data. I would change three things. First, split the data by date before fitting anything, so the model is tested on months it has not seen. Second, fix the cleaning rules on the training months only, so nothing from the test months leaks into them. Third, add checks that fail loudly when a new file brings an unknown currency, a new directorate name or a column in a different format. A natural first model would predict a transaction's spending category from its merchant and amount, which is the coding job a finance team does by hand. The file carries no category, so the finance team would first need to label a sample to train on.

### Neural network preparation

A neural network needs numbers on a similar scale and a consistent set of inputs. Amounts would need scaling, and possibly a log transform, because hundreds of payments are over a thousand pounds while a typical transaction is tens of pounds. Directorate and currency would become one-hot or embedded categories. Merchant names, with thousands of values even after cleaning, would suit an embedding or a text encoding of the name itself. Dates would become features such as month and day of week, because the data shows strong monthly patterns such as the August dip in school transactions. I would also need to decide what to do with refunds, which a model could learn to treat as noise.

### Agentic automation potential

The strongest case is the monthly publication itself. An agent could fetch each new month's file, run the same cleaning functions, flag what the rules cannot handle (a new directorate name, an unfamiliar currency, a merchant that fits no rule) and draft a short summary for a finance officer to check. The functions in this notebook are already written as separate steps, so each could become a tool the agent calls. The limit is judgement: deciding whether two directorate names are the same service, or whether a payment to the council itself should count as spend, needs a person. So the agent would prepare and flag, and a person would approve.
