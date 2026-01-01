# Chitosan-Cobalt Nanoparticle Antimicrobial Activity Prediction

This project utilizes machine learning to predict the antimicrobial potential of Chitosan-Cobalt (Co-CS) nanoparticles. It involves a rigorous data cleaning pipeline, physics-informed feature engineering, and a composite scoring system based on physicochemical properties (Particle Size, Zeta Potential, and PDI).

## 📌 Project Overview

The goal of this project is to model the relationship between the synthesis parameters of nanoparticles and their resulting antimicrobial efficacy. Instead of relying solely on experimental inhibition zones which can vary, this project calculates a robust **Antimicrobial Score** derived from literature-validated physical properties:
* **Particle Size (40% weight):** Smaller particles (optimal 20-50nm) penetrate bacterial walls better.
* **Zeta Potential (35% weight):** Higher magnitude indicates stability; positive charge enhances electrostatic interaction with bacteria.
* **Polydispersity Index (PDI) (25% weight):** Lower PDI indicates uniform particle size distribution.

## 📂 Repository Structure

### 1. Data Files
* `data_cc_v3.csv`: Raw dataset containing synthesis parameters and physicochemical properties.
* `data_cleaned_v1.csv`: Cleaned dataset after outlier removal, imputation, and categorical encoding.
* `data_with_antimicrobial_score.csv`: Final dataset including the calculated physics-based `Antimicrobial_Score`.
* `train_augmented.csv`: Training dataset (likely augmented to handle data scarcity).
* `test_original.csv`: Hold-out test set for model validation.

### 2. Code & Models
* `Untitled36.ipynb`: Main Jupyter Notebook containing the end-to-end workflow:
    * Data Loading & rigorous cleaning (RegEx parsing, unit handling).
    * Feature Engineering (Solvent polarity, Co:CS ratios, Energy proxies).
    * **Antimicrobial Score Calculation**: Physics-based logic to generate the target variable.
    * Validation checks (Correlations between Size/Zeta/PDI and the calculated Score).
* `best_model_corrected.joblib`: The final trained machine learning model saved for inference.
* `feature_scaler.joblib`: The scaler object (e.g., RobustScaler) used to normalize features.
* `feature_columns.pkl`: Pickle file storing the exact list of feature names used during training.

### 3. Documentation & Visuals
* `Data-Extraction-SOP.md`: Standard Operating Procedure used for extracting data from literature papers.
* `antimicrobial_score_validation.png`: Diagnostic plots showing distributions and correlations of the calculated score.
* `target_distribution_check.png`: Visualization of the target variable distribution.
* `augmentation_validation.png`: Plots validating the data augmentation strategy.
* `final_model_validation.png`: Performance metrics of the final trained model.

## ⚙️ Methodology

### Step 1: Data Cleaning
The raw data undergoes strict cleaning:
* **OCR Correction:** Fixes common extraction errors (e.g., 'O' -> '0').
* **Solvent Polarity:** Maps solvent names (e.g., "Acetic Acid", "Water") to numerical polarity indices.
* **Imputation:** Uses KNN Imputer for missing numeric values.
* **Categorical Encoding:** Label encoding for columns like `Synthesis_Method` and `Cobalt_Source`.

### Step 2: Feature Engineering
New physics-based features are derived to help the model learn interactions:
* **Co_to_CS_ratio:** Molar ratio of Cobalt to Chitosan.
* **CS_charge_index:** Proxy for charge density based on Deacetylation Degree (DD) and pH.
* **Energy_proxy:** Product of Temperature and Time.

### Step 3: Target Calculation (The Antimicrobial Score)
A composite score (0 to 1) is calculated for each sample:
$$\text{Score} = 0.4 \cdot f(\text{Size}) + 0.35 \cdot f(\text{Zeta}) + 0.25 \cdot f(\text{PDI})$$
*This approach ensures the target label is physically meaningful and consistent.*

## 🚀 Usage

1.  **Environment Setup:**
    Ensure you have Python installed with the following libraries:
    ```bash
    pip install pandas numpy scikit-learn matplotlib seaborn joblib
    ```

2.  **Running the Analysis:**
    Open `Untitled36.ipynb` in Jupyter Notebook or Google Colab to reproduce the data processing and score calculation steps.

3.  **Using the Model:**
    You can load the saved model to make predictions on new synthesis parameters:
    ```python
    import joblib
    import pandas as pd

    # Load model and scaler
    model = joblib.load('best_model_corrected.joblib')
    scaler = joblib.load('feature_scaler.joblib')
    features = joblib.load('feature_columns.pkl')

    # Example: Create a dataframe with your new synthesis parameters
    # new_data = pd.DataFrame(...) 
    
    # Preprocess and Predict
    # X_scaled = scaler.transform(new_data[features])
    # prediction = model.predict(X_scaled)
    ```

## 📊 Results
* **Score Validation:** The calculated antimicrobial scores show a strong negative correlation with particle size (smaller size -> higher score), validating the physics-based approach.
* See `final_model_validation.png` for specific accuracy/R² metrics of the predictive model.

## 🤝 Contributing
Feel free to open issues or pull requests if you find bugs or want to improve the feature engineering pipeline.
