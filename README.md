Project Title: Discovery of mRNA Therapeutic Targets in Melanoma Using Single-Cell Transcriptomics and Machine Learning

-------------------------------------------------------------------------------
Project Description
-------------------------------------------------------------------------------
This project develops and deploys a machine learning pipeline to classify single
melanoma cells as tumor or non-tumor using single-cell RNA sequencing (scRNA-seq)
data. The model integrates a denoising autoencoder (DAE) for dimensionality reduction
and a Random Forest classifier for prediction.

Outputs include:
- Classification metrics (Accuracy, F1, AUC-ROC)
- SHAP feature importance for gene-level interpretation
- Gene Ontology (GO) and KEGG pathway enrichment of prioritized genes
- External validation with UniProt, IEDB, and DrugBank
- A Flask-based web interface for interactive prediction and visualization

This research contributes to identifying novel gene targets for mRNA therapeutic
development in melanoma.

-------------------------------------------------------------------------------
Datasets Used
-------------------------------------------------------------------------------
1. GSE72056 (Tirosh et al., 2016) – ~4,500 melanoma single cells with tumor/immune
   annotations.
   Source: GEO database
   Format: log-normalized TPM expression matrix + metadata

2. GSE115978 (Jerby-Arnon et al., 2018) – ~7,000 melanoma single cells with tumor and
   immune populations profiled by SMART-Seq2.
   Source: GEO database
   Format:
     - cell.annotations.csv: metadata (cell type, treatment, QC stats)
     - counts.csv: raw expression counts (RSEM)
     - tpm.csv: log-transformed TPM values (log2[(TPM/10)+1])
   Note: This dataset is also log-transformed for expression analyses, ensuring
   compatibility with GSE72056.

3. Supporting resources:
   - UniProt: protein/gene annotation and subcellular localization
   - IEDB: immune epitope relevance
   - DrugBank: druggability and target information

Preprocessed files included in submission:
- adata_combined.h5ad – merged AnnData object (combined log-TPM matrices from
  GSE72056 and GSE115978, harmonized with cell annotations and QC filtering)
- hvg_gene_list.txt – highly variable genes used for modeling
- dae_encoder_model.pkl – trained autoencoder
- clf_combined_dae.pkl – trained Random Forest classifier
- latent_background.pkl – background latent embedding for SHAP
- uniprot_gene_annotations.csv – UniProt validation table
- GO_BP_enrichment_top20.csv, KEGG_enrichment_top20.csv – enrichment results

-------------------------------------------------------------------------------
Dependencies
-------------------------------------------------------------------------------
Install the following Python libraries before running the notebook or Flask app:

Python 3.10+
numpy
pandas
scikit-learn
tensorflow / keras
scanpy
matplotlib
seaborn
shap
joblib
flask
umap-learn

-------------------------------------------------------------------------------
Running the Project
-------------------------------------------------------------------------------
1. Jupyter Notebook
   Open Chapman_Project_final.ipynb in Jupyter Lab or VS Code.
   - Run all cells sequentially for data processing, model training, and evaluation.
   - Outputs include PCA/UMAP plots, classification metrics, SHAP plots, and enrichment
     results.

2. Flask Web Application
   Run from the project root directory:
   > python app.py

   Navigate to http://127.0.0.1:5000 in a browser.

   Upload a CSV file with two columns:
   Gene, Expression

   The app predicts Tumor vs Non-Tumor, displays top latent SHAP features, and provides
   a downloadable SHAP summary.

   Additional features in Flask deployment:
   - PCA Visualization: uploaded sample is projected into the trained latent space
     alongside reference tumor/non-tumor embeddings, plotted for comparison.
   - Top Gene Prediction Table: shows the real genes most strongly correlated with the
     top latent features driving classification, aiding biological interpretation.


-------------------------------------------------------------------------------
Example File Generation
-------------------------------------------------------------------------------
To simplify testing, the notebook `expression_template_code.ipynb` is provided.
Running this notebook will generate a small CSV file with the correct structure for
Flask app prediction. The file will contain:

Gene, Sample1
GAPDH, 10.5
ACTB, 8.2
TP53, 6.7
...

This ensures that mentors/instructors can easily test the model by uploading the
example file without preparing data manually.

-------------------------------------------------------------------------------
Note
-------------------------------------------------------------------------------
All required files are included so the user can reproduce results locally.
The file `adata_combined.h5ad` contains merged log-TPM matrices from both GSE72056
and GSE115978 datasets, harmonized with cell annotations and quality filters applied.

