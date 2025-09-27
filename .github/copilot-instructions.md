# Copilot Instructions for proyecto_final_L1_AD

## Project Overview
This project analyzes call data to predict call outcomes and generate reports/visualizations. The main data source is the `datos-llamadas/` folder, with processed data in `datos_unidos_procesados.csv`. Models and data processing are implemented in Jupyter notebooks.

## Key Components
- **Data Source:** Raw CSVs in `datos-llamadas/`.
- **Processing:** `proceso_datos.ipynb` cleans and encodes data, saving processed results and encoder mappings to `Encoders/mapeo_encoders.json`.
- **Modeling:**
  - `modelo_clasificador_llamadas.ipynb`: Classification (predict if calls are answered).
  - `modelo_prediccion_minutos.ipynb`: Regression (predict call duration in minutes).
- **Encoders:** Categorical columns are label-encoded, with mappings stored in `Encoders/mapeo_encoders.json`.

## Data Flow
1. Load and clean raw data from `datos-llamadas/`.
2. Encode categorical features and save mappings.
3. Select features for modeling (typically integer columns, with target variable `Estado_encoder`).
4. Split data into train/test sets (often with high test size for validation).
5. Train models (RandomForest, DecisionTree) and evaluate with accuracy, precision, recall, F1.
6. Save results and models as needed.

## Developer Workflow
- **Environment:** Use the `_env/` Python virtual environment. Install dependencies via pip in this environment.
- **Notebooks:** Main logic is in Jupyter notebooks. Run cells sequentially; outputs are not always saved to disk.
- **Data Updates:** If new data is added to `datos-llamadas/`, rerun `proceso_datos.ipynb` to regenerate processed files and encoders.
- **Model Updates:** Update or retrain models in the respective notebook. Save models with `joblib` if needed.

## Conventions & Patterns
- **Feature Selection:** Use `select_dtypes('int')` to select features for modeling.
- **Target Variable:** For classification, use `Estado_encoder`.
- **Encoder Mapping:** Always update `Encoders/mapeo_encoders.json` when re-encoding data.
- **Test Split:** Commonly use `train_test_split(X, y, test_size=0.9, random_state=42)` for validation.
- **Model Evaluation:** Print shapes and metrics after training.

## Integration Points
- **External Libraries:** scikit-learn, pandas, joblib, json.
- **Outputs:** Models and encoder mappings are saved to disk; reports/visualizations may be exported as CSV/Excel.

## Example: Data Preparation & Modeling
```python
# Load processed data
df = pd.read_csv('datos_unidos_procesados.csv')
# Drop unused columns
df = df.drop(columns=['Fecha'])
# Encode categorical columns and save mapping
# Select integer features for modeling
df_listo = df.select_dtypes('int')
X = df_listo.drop(columns=['Estado_encoder'])
y = df_listo['Estado_encoder']
# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.9, random_state=42)
```

## Reference Files
- `datos-llamadas/`: Raw data
- `datos_unidos_procesados.csv`: Processed data
- `Encoders/mapeo_encoders.json`: Encoder mappings
- `proceso_datos.ipynb`: Data processing
- `modelo_clasificador_llamadas.ipynb`: Classification modeling
- `modelo_prediccion_minutos.ipynb`: Regression modeling

---
**Feedback:** If any workflow, convention, or integration is unclear or missing, please specify so it can be improved.
