# Earlier analysis notebooks

These notebooks record the first data preparation and exploratory analysis. The root-level `Graduate_Cancellation_Presentation.ipynb` is now the main runnable analysis and reads the raw training CSV directly.

`00_data_preparation.ipynb` writes ignored prepared CSVs to `data/processed/`. Run it before `01_data_audit_and_eda.ipynb` if you want to reproduce the original descriptive tables and `reports/figures/`. The EDA notebook includes market-segment, special-request, and price views that are not all repeated in the presentation.

Their original descriptions refer to a modeling notebook that has since been replaced by the root-level presentation notebook.
