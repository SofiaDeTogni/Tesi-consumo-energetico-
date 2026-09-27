# Metodi di apprendimento automatico per l'analisi dei dati nella gestione intelligente dell'energia.

Tesi di laurea basata sul dataset [`energydata_complete`](https://archive.ics.uci.edu/dataset/374/appliances+energy+prediction) — 19.735 misurazioni a intervalli di 10 minuti (11 gennaio – 27 maggio 2016) del consumo energetico degli elettrodomestici di un'abitazione, insieme a condizioni climatiche interne ed esterne.

## Struttura del progetto

Il lavoro è organizzato in 2 fasi, corrispondenti ai notebook presenti nella cartella.

### 1. Studio del dataset e ricerca del modello di regressione più performante
| Notebook | Contenuto |
|---|---|
| `0-studio-del-dataset` | Analisi esplorativa (EDA), correlazioni, primi modelli di regressione (lineare, Gradient Boosting, Random Forest). |
| `1-studio-del-modello-random-forest` | Random Forest con diverse configurazioni di feature selection. |
| `2-studio-con-lasso` | Regressione Lasso: confronto di diversi valori di `alpha`, uso di Lasso come strumento di selezione delle variabili. |
| `3-analisi-con-lasso-pca-e-xgboost` | Random Forest e XGBoost sui dataset selezionati con Lasso; confronto finale fra tutte le configurazioni. |
| `4-studio-con-xgboost` | XGBoost con diverso numero di alberi; miglior risultato della fase di regressione (R² = 0,609). Include anche diagnosi di overfitting, feature importance e confronto con PCA. |

### 2. Previsione della serie temporale
| Notebook | Contenuto | Esito |
|---|---|---|
| `5-prima-previsione-con-xgboost` | XGBoost multi-orizzonte (1h–1 settimana) con lag, medie mobili, variabili cicliche, su dati a 10 minuti. | R² negativo per quasi tutti gli orizzonti |
| `6-previsione-con-catch24-e-xgboost` | Feature engineering con la libreria `catch22` (descrittori automatici di serie temporali) + XGBoost. | R² negativo (−0,54) — non funziona |
| `7-previsione-con-baseline-e-xgboost` | Dati aggregati su base oraria, target trasformato con logaritmo, confronto fra XGBoost e baseline naïve (persistenza). | **La baseline vince sul MAE** (31,71 vs 40,59 Wh) |

## Risultato principale

Nessun modello addestrato per la previsione batte, in termini di errore medio assoluto, la semplice regola "il consumo fra un'ora sarà uguale a quello attuale". La complessità aggiuntiva (catch22, PCA, multi-orizzonte) non produce un vantaggio reale rispetto a una baseline esplicita.

## Requisiti

```
pandas
numpy
scikit-learn
xgboost
matplotlib
pycatch22
```

## Come eseguire i notebook

I notebook sono stati sviluppati su Kaggle e fanno riferimento al percorso `/kaggle/input/...`. Per eseguirli in locale, scaricare il dataset `energydata_complete.csv` e aggiornare il percorso di caricamento nella prima cella di ciascun notebook.
