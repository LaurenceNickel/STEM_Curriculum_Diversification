# Anticipating Technological Change through STEM Curriculum Diversification: Labour Market Alignment in Higher Education

This repository contains the analysis workflow for the Hogescholen Content Project, focused on clustering Dutch university-of-applied-sciences study programmes and comparing those clusters with labour-market outcomes and predicted job classifications.

The main workflow is implemented in `Clustering_Study_Programmes.ipynb`. It ingests study programme descriptions, prepares multilingual text data, performs LDA-based topic modelling and clustering, links clustered programmes to HBO Monitor data, and generates figures for evaluating study programme and job classification patterns.

## Project Structure

```text
.
├── Clustering_Study_Programmes.ipynb       # Main end-to-end analysis notebook
├── data/                                   # External and intermediate input data
├── JSON_files/                             # Scraped/structured study programme text files and derived CSVs
├── LDA_Modeling_Preparation/               # Prepared token/noun datasets for LDA modelling
├── LDA_Modeling_Optimal_Num_Clusters/      # Cluster-number diagnostics
├── LDA_Modeling_Execution/                 # LDA/K-means output visualizations
└── Analyzing_Predicted_Job_Classifications/# Job-cluster evaluation outputs
```

## Analysis Overview

The notebook follows these main stages:

1. Load Python libraries and define project/data directories.
2. Convert institution-level JSON text files into a consolidated study programme dataset.
3. Download or load language and translation models.
4. Build a domain-specific English language corpus for STEM-related programme content.
5. Extract nouns, remove frequent words, tokenize descriptions, and construct bag-of-words corpora.
6. Estimate LDA topic models and apply K-means clustering to topic distributions.
7. Evaluate cluster representation by hogeschool and programme type.
8. Link clustered programmes to CROHO and HBO Monitor data.
9. Evaluate cluster differences in programme age, salary, job-search duration, and predicted job classifications.
10. Export diagnostic plots, heatmaps, entropy measures, match-rate figures, and CSV summaries.

## Data Inputs

The repository currently includes these data files:

- `data/CrohoActueel14-10-2024.xlsx`: CROHO programme recognition data from DUO.
- `data/codelijstenisco08.xlsx`: ISCO occupation classification data from CBS.
- `data/hogescholen_df.xlsx`: Hogeschool metadata.
- `data/study_programmes_hogescholen_df.xlsx`: Study programme metadata.
- `JSON_files/*.txt`: Institution-specific JSON-style study programme files.
- `JSON_files/*.csv`: Derived study programme text datasets.

The notebook also references HBO Monitor files that are intentionally omitted from this repository for privacy reasons. To run the HBO Monitor sections, these files must be supplied locally in `data/`:

- `Sis2024hbo incl extra vragen.sav`
- `Sis2024hbo incl extra vragen.csv`

If the CSV is missing but the `.sav` file is available locally, the notebook attempts to create the CSV using `pyreadstat`.

## Dependencies

The notebook was written for Python 3.12 and uses the following main packages:

- `argostranslate`
- `gensim`
- `joblib`
- `matplotlib`
- `networkx`
- `nltk`
- `numpy`
- `openpyxl`
- `pandas`
- `plotly`
- `pyreadstat`
- `scipy`
- `seaborn`
- `sentence-transformers`
- `scikit-learn`
- `spacy`
- `statsmodels`
- `statannotations`
- `torch`
- `pyspellchecker`

Example setup:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install argostranslate gensim joblib matplotlib networkx nltk numpy openpyxl pandas plotly pyreadstat scipy seaborn sentence-transformers scikit-learn spacy statsmodels statannotations torch pyspellchecker
```

The notebook can download required spaCy models when missing:

```bash
python -m spacy download nl_core_news_sm
python -m spacy download en_core_web_sm
```

Some sections also rely on external model resources, including Argos Translate models and fastText/Gensim word vectors. A first full run may therefore require an internet connection and substantial local storage.

## Running the Notebook

1. Open `Clustering_Study_Programmes.ipynb` in Jupyter, JupyterLab, VS Code, or PyCharm.
2. Ensure the expected input data files are present in `data/` and `JSON_files/`.
3. Review the state variables near the top of the notebook:

```python
state_variable_find_optimal_num_clusters = False
state_variable_storing_clustering_results = False
state_variable_loading_clustering_results = True
```

Set these depending on whether you want to recompute clustering diagnostics, store outputs, or load previously saved outputs.

4. Run the notebook cells sequentially.

## Outputs

The workflow writes or uses generated outputs in these folders:

- `LDA_Modeling_Preparation/`: prepared text variants after noun extraction and word filtering.
- `LDA_Modeling_Optimal_Num_Clusters/`: inertia, silhouette, and Davies-Bouldin diagnostic plots.
- `LDA_Modeling_Execution/`: cluster visualizations, t-SNE plots, programme-age plots, and outcome comparisons.
- `Analyzing_Predicted_Job_Classifications/`: confusion matrices, match/mismatch rates, entropy plots, significance charts, and CSV summaries.

## Notes

- The project is notebook-driven; there is currently no package entry point or automated CLI.
- Several outputs are already committed as generated image or CSV artifacts.
- Some data referenced by the notebook may be restricted or too large to store directly in the repository. Place those files in `data/` before running the dependent sections.
- The existing folder names are part of the notebook's path assumptions, so rename them only if you also update the notebook paths.

## Authors

- Laurence Nickel, M.Sc.  
  Current affiliation: Maastricht Centre for Systems Biology and Bioinformatics (MaCSBio), Maastricht University  
  This research was conducted while affiliated with: School of Business and Economics (SBE) - The Research Centre for Education and the Labour Market (ROA), Maastricht University  
  laurence.nickel@maastrichtuniversity.nl
- Dr. Barbara Belfi  
  School of Business and Economics (SBE) - The Research Centre for Education and the Labour Market (ROA), Maastricht University  
  b.belfi@maastrichtuniversity.nl
- Dr. Zsuzsa Bakk  
  Methodology & Statistics of the Institute of Psychology, Leiden University  
  z.bakk@fsw.leidenuniv.nl

## References

1. Argos Open Tech (2025). Argos Translate Package Index - translate-en_nl-1_8.argosmodel. Available: https://www.argosopentech.com/argospm/index/ (last accessed: 24 June 2025).
2. Argos Open Tech (2025). Argos Translate Package Index - translate-nl_en-1_8.argosmodel. Available: https://www.argosopentech.com/argospm/index/ (last accessed: 24 June 2025).
3. Centraal Bureau voor de Statistiek (2025). Beroepenclassificatie (ISCO en SBC) - codelijstenisco08.xlsx. Available: https://www.cbs.nl/nl-nl/onze-diensten/methoden/classificaties/onderwijs-en-beroepen/beroepenclassificatie--isco-en-sbc-- (last accessed: 9 July 2025).
4. Dienst Uitvoering Onderwijs (2024). HO Erkenningen CROHO - CrohoActueel14-10-2024.xlsx. Available: https://onderwijsdata.duo.nl/datasets/overzicht-erkenningen-ho/resources/28a4d89b-c223-4dbc-8deb-9d02a533f215 (last accessed: 17 October 2024).
5. fastText (2022). English word vectors - wiki-news-300d-1M.vec.zip. Available: https://fasttext.cc/docs/en/english-vectors.html (last accessed: 24 June 2025).
