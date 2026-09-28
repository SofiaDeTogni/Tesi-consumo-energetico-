
#  Card del Modello: Previsione del Consumo Energetico Residuo

###  Panoramica del Task

* **Obiettivo:** Prevedere il consumo energetico futuro della categoria *Appliances* (elettrodomestici).


* **Orizzonte temporale:** 1 ora nel futuro ($t+1$).


* **Dataset di origine:** *Energy Data Complete* (misurazioni originariamente a cadenza decennale/10 minuti).



---

###  Modifiche Chiave rispetto alle Baseline Iniziali

* **Frequenza Temporale:** Aggregazione dei dati da 10 minuti a **1 ora** (tramite media) per eliminare il forte rumore stocastico causato dall'accensione casuale di elettrodomestici.


* **Trasformazione del Target:** Applicazione della funzione logaritmica $\log(1 + x)$ per comprimere la scala dei picchi estremi ed evitare che il modello subisca un forte bias a discapito della precisione generale.


* **Feature Engineering:** Abbandono di librerie complesse e non idonee (come *Catch22*) in favore di feature basate su *domain knowledge*:


* **Variabili Cicliche:**  Seni e coseni per ora del giorno e giorno della settimana (per catturare la continuità temporale, es. vicinanza tra le 23:00 e le 00:00).


* **Lag Temporali:** Consumi storici passati (1h, 2h, 3h, 6h, 12h, 24h, 168h/1 settimana).


* **Statistiche Mobili (Rolling):** Medie e deviazioni standard mobili a 3 ore e 24 ore per descrivere il livello di attività recente della casa.




* **Benchmark:** Introduzione di un confronto sistematico con una **Baseline Naïve (Persistenza)** $y_{t+1} = y_t$.



---

### Specifiche dell'Algoritmo (`XGBRegressor`)

* **Libreria:** `xgboost`

* **Iperparametri principali:**
* `n_estimators = 300`

* `learning_rate = 0.03`

* `max_depth = 5`

* `subsample = 0.8`, `colsample_bytree = 0.8`

* `objective = "reg:squarederror"`




---

###  Risultati e Valutazione (Test Set)

* **Split dei dati:** Cronologico (70% Train, 15% Validation, 15% Test).


* **Metriche di Performance:**
* **MAE (Mean Absolute Error):** 40.59 W *(vs Naïve MAE: 31.71 W)*

* **RMSE:** 56.38 W


* **MedAE:** 29.54 W


* **$R^2$ Score:** 0.4054 *(vs Naïve $R^2$: 0.3121)*


>  **Nota Critica:** Sebbene l'indice $R^2$ di XGBoost sia superiore alla baseline (0.41 vs 0.31), la regola di persistenza semplice batte il modello in termini di errore medio assoluto (MAE). Questo accade perché il modello avanzato coglie meglio l'andamento generale della curva ma tende a sottostimare i picchi improvvisi e isolati guidati da eventi casuali.
> 
>