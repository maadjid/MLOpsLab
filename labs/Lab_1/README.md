# MLOps Lab 1 — End-to-End Pipeline + API

Notebook complet (EDA, pipeline scikit-learn, GridSearchCV, évaluation) + micro-API Flask pour l’inférence.

## 1) Prérequis
- Python 3.10+ (testé avec 3.13)
- pip
- Windows PowerShell (ou WSL / macOS / Linux)

## 2) Installation (Windows PowerShell)
Depuis la **racine du projet** :
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r labs\Lab_1\requirements.txt
```

## 3) Entraînement via Notebook
1. Lancer JupyterLab :
   ```powershell
   jupyter lab
   ```
2. Ouvrir `labs/Lab_1/notebook.ipynb`.
3. Sélectionner le kernel du venv.
4. Exécuter toutes les cellules jusqu’à la sauvegarde du modèle :  
   **`labs/Lab_1/best_cancer_model_pipeline.joblib`**.

> Le pipeline ajoute la feature `Combined_radius_texture` en interne.  
> À l’inférence, fournissez **les 30 features d’origine** du dataset Breast Cancer; le pipeline crée la feature combinée.

## 4) Lancer l’API d’inférence (port 5001)
Depuis `labs\Lab_1` (venv actif) :
```powershell
python .\inference_pipeline.py
```
*Alternative sans activer le venv :*
```powershell
..\..\.venv\Scripts\python.exe .\inference_pipeline.py
```
Endpoints :
- `GET /` → message d’accueil  
- `POST /predict` → JSON :
```json
{ "features": [30 valeurs numériques dans l'ordre des features] }
```

## 5) Tester l’API
### Option A — PowerShell
```powershell
$body = @{
  features = @(17.99, 10.38, 122.8, 1001.0, 0.1184, 0.2776, 0.3001, 0.1471, 0.2419, 0.07871,
               1.095, 0.9053, 8.589, 153.4, 0.006399, 0.04904, 0.05373, 0.01587, 0.03003,
               0.006193, 25.38, 17.33, 184.6, 2019.0, 0.1622, 0.6656, 0.7119, 0.2654, 0.4601,
               0.1189)
}
Invoke-RestMethod -Method Post -Uri http://127.0.0.1:5001/predict -ContentType 'application/json' -Body ($body | ConvertTo-Json -Depth 5)
```

### Option B — Script Python
```powershell
..\..\.venv\Scripts\pip.exe install requests
..\..\.venv\Scripts\python.exe .\test.py
```

## 6) Variante d’API (port 5002)
`inference_pipeline_df.py` accepte un JSON avec **noms de features** (plus explicite) :
```powershell
..\..\.venv\Scripts\python.exe .\inference_pipeline_df.py
```
Exemple (raccourci) :
```json
{ "mean radius": 17.99, "mean texture": 10.38, "...": 0.1189 }
```

## 7) Notes
- Les fichiers `*.joblib` et dossiers de cache sont ignorés par Git (voir `.gitignore`).
- Arrêter l’API : `Ctrl + C`.
