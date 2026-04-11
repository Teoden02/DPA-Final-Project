# ♟️ Ottimizzazione Convessa sui Big Data: SAGA vs L-BFGS


Questo repository contiene un'analisi matematica e computazionale sui tassi di convergenza dei risolutori per la Regressione Logistica. 
Gli scacchi vengono utilizzati come **generatore di complessità spaziale**: estraendo centinaia di migliaia di posizioni da un dataset di partite, abbiamo costruito una matrice densa per testare il limite fisico e matematico degli algoritmi di ottimizzazione, dimostrando asintoticamente il **collasso dei metodi Batch (L-BFGS)** in favore dei **metodi Stocastici (SAGA)** nell'era dei Big Data.

---

## 🧠 L'Obiettivo Matematico

Il Machine Learning moderno si scontra spesso con il "muro" della complessità spaziale $\mathcal{O}(N \cdot d)$. L'obiettivo di questo progetto è:

1. Dimostrare che **L-BFGS (Quasi-Newton)**, pur richiedendo un numero di iterazioni bassissimo e costante, collassa sui Big Data a causa del calcolo del gradiente esatto (approccio Batch).
2. Dimostrare che **SAGA (Stochastic Average Gradient)**, pur faticando su piccoli dataset, abbassa drasticamente il fabbisogno di *Epoche* all'aumentare di $N$, grazie alla ridondanza dell'informazione.
3. Calcolare l'esatto **Punto di Crossover** matematico estraendo gli autovalori reali dalla matrice di covarianza del dataset.

---

## 🏗️ Struttura del Progetto

Il progetto è suddiviso in fasi sequenziali all'interno di un Jupyter Notebook:

### Fase 1-3: Estrazione e Spazio Vettoriale

* **Parsing PGN:** Traduzione dinamica del formato scacchistico ufficiale tramite RegEx.
* **Feature Extraction:** Estrapolazione delle coordinate vettoriali (X, Y) dei pezzi al 40° ply (metà partita), con gestione dinamica delle promozioni dei pedoni.
* **Standardizzazione:** Scaling dei dati per garantire una funzione di costo (Log-Loss) ben condizionata, con $\mu = 0$ e $\sigma^2 = 1$.

### Fase 4-6: Benchmark e Validazione

* Addestramento parallelo di modelli SAGA e L-BFGS su scaglioni di dati crescenti (10k, 50k, 250k+).
* Dimostrazione empirica del raggiungimento del medesimo minimo globale della funzione convessa (concordanza delle Matrici di Confusione > 99.9%).

### Fase 7-8: Calcolo Geometrico del Crossover 

Invece di affidarci a stime di *worst-case scenario*, l'algoritmo calcola la vera geometria della vallata dell'errore:

* Calcolo del tensore di Covarianza $X^T X$.
* Estrazione di $\lambda_{min}$ e $\lambda_{max}$ tramite algebra lineare pura (`numpy.linalg.eigvalsh`).
* Calcolo esatto del **Numero di Condizionamento (L2)** $\kappa$.
* Proiezione asintotica (fino a $10^9$ osservazioni) tramite l'equazione di convergenza limite di Defazio (2014) per SAGA.

---

## 📊 Risultati e Visualizzazioni

L'estrazione del condizionamento $\kappa$ ha permesso di tracciare il collasso asintotico di L-BFGS. 
Come si evince dal grafico sottostante, oltre una certa soglia critica di osservazioni (il *Crossover*), l'aggiornamento stocastico di SAGA annienta l'overhead computazionale di L-BFGS, risparmiando miliardi di operazioni FLOPs.

---

## 🚀 Come avviare il progetto
Per il progetto utlizzare questo dataset: https://www.kaggle.com/datasets/arevel/chess-games 
inserendolo direttamente nella cartella DPA_FINAL_PROJECT 
controllare non ci siano altri file .csv
<<<<<<< HEAD
Tutte le librerie richieste sono elencate nei requirement da installare nel virtual enviroment
=======
>>>>>>> 616262ed020cec8c7c780a0e4543403c02ecccc4

