# Liver-Fibrosis-Hybrid-Model

An implementation of a hybrid model for liver fibrosis classification that combines ultrasound images with clinical data.

## Key resources

- Original Paper: https://arxiv.org/abs/2504.19755
- Ultrasound image dataset: https://www.kaggle.com/datasets/vibhingupta028/liver-histopathology-fibrosis-ultrasound-images/data
- Clinical data: https://archive.ics.uci.edu/dataset/878/primary+biliary+cirrhosis

## How to run

1. Prerequisites

   - Python 3.8+ (recommended). If you plan to train models on GPU, install CUDA-enabled PyTorch that matches your CUDA version.
   - Git and Jupyter (or JupyterLab) to run the notebooks.
   - Sufficient disk space for datasets and model artifacts.

2. Obtain the data

   Download the two datasets referenced in the Key resources section (ultrasound images and clinical data). Place the downloaded data in an accessible directory and note the paths — the preprocessing notebooks expect to find or be pointed to these files.

3. Repository setup

   Clone the repository and create an isolated Python environment:

   ```bash
   git clone https://github.com/shadabtanjeed/Liver-Fibrosis-Hybrid-Model.git
   cd Liver-Fibrosis-Hybrid-Model
   python -m venv .venv
   source .venv/bin/activate
   # Install dependencies referenced in the notebooks. There is no project-wide requirements.txt, so inspect the notebooks
   # and install required packages (example below):
   pip install xyz abc ...
   ```

   Note: If you maintain a requirements file locally, you can run `pip install -r requirements.txt`.

4. Data preprocessing

   All preprocessing is implemented as Jupyter notebooks in the `Preprocessing/` folder:

   - `Preprocessing/ultrasound_preprocessing.ipynb` — image loading, resizing, augmentation, and saving image datasets.
   - `Preprocessing/clinical_data_preprocessing_knn.ipynb` and `Preprocessing/clinical_data_preprocessing_simplified.ipynb` — clinical table cleaning and imputation (KNN or simple mean imputation). Both notebooks achieve similar results; choose one based on your preference.

5. Create train/test splits

   Run `Training/create_test_data.ipynb`. This notebook produces a `Hybrid_Test_Dataset/` folder (used for final evaluation) and prepares training/validation splits for model development.

6. Training

   Model training notebooks are under `Training/`:

   - `Training/Ultrasound/` — notebooks for ultrasound CNN training (e.g., Densenet201, Resnet50, AlexNet).
   - `Training/Clinical_Data/` — notebooks for clinical models (XGBoost, CatBoost, TabNet).

   Open the desired training notebook, set any dataset/model hyperparameters, and run the cells. Trained artifacts are saved in the `Models/` directory.

7. Evaluation and ensemble

   Use `us_clinical_ensemble.ipynb` to run the final evaluation pipeline. This notebook combines the ultrasound model (Densenet201) and the clinical model (XGBoost) for ensemble predictions on the test set in `Hybrid_Test_Dataset/` and reports metrics.

8. Explainability

   Gradient-weighted Class Activation Mapping (Grad-CAM) utilities are available in the `XAI/` folder. See `XAI/gradcam_us_densenet201.ipynb` for examples that generate visual explanations for ultrasound model predictions.
