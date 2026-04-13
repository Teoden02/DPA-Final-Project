# ♟️ Ottimizzazione Convessa sui Big Data: SAGA vs L-BFGS

Questo repository contiene un'analisi matematica e computazionale sui tassi di convergenza dei risolutori per la Regressione Logistica. 
Gli scacchi vengono utilizzati come **campo di studio per la complessità spaziale**: estraendo centinaia di migliaia di posizioni da un dataset di partite, abbiamo costruito una matrice sparsa per testare il limite fisico e matematico degli algoritmi di ottimizzazione, dimostrando asintoticamente il **collasso dei metodi Batch (L-BFGS)** in favore dei **metodi Stocastici (SAGA)** nell'era dei Big Data.

---

## 🧠 L'Obiettivo Matematico

Il Machine Learning moderno si scontra spesso con il "muro" della complessità spaziale $\mathcal{O}(N \cdot d)$. L'obiettivo di questo progetto è:

1. Dimostrare che **L-BFGS (Quasi-Newton)**, pur richiedendo un numero di iterazioni bassissimo e costante, collassa sui Big Data a causa del calcolo del gradiente esatto.
2. Dimostrare che **SAGA (Stochastic Average Gradient)**, pur faticando su piccoli dataset, abbassa drasticamente il fabbisogno di *Epoche* all'aumentare di $N$, grazie alla ridondanza dell'informazione.
3. Calcolare l'esatto **Punto di Crossover** matematico estraendo gli autovalori reali dalla matrice di covarianza del dataset.

---

## 🏗️ Struttura del Progetto

Il progetto è suddiviso in fasi sequenziali all'interno di un Jupyter Notebook:

### Fase 1: Estrazione e Spazio Vettoriale

* **Parsing PGN:** Traduzione dinamica del formato scacchistico ufficiale tramite RegEx.
* **Feature Extraction:** Estrapolazione delle coordinate vettoriali (X, Y) dei pezzi con gestione dinamica delle promozioni dei pedoni.
* **Standardizzazione:** Scaling dei dati per garantire una funzione di costo (Log-Loss) ben condizionata, con $\mu = 0$ e $\sigma^2 = 1$.

### Fase 2: Benchmark e Validazione

* Addestramento parallelo di modelli SAGA e L-BFGS sul dataset ripulito.
* Dimostrazione empirica del raggiungimento del medesimo minimo globale della funzione convessa (concordanza delle Matrici di Confusione > 99.9%).

### Fase 3: Calcolo Approssimativo Costo Computazionale: SAGA vs L-BFGS

Per mettere a confronto i due metodi ci si affida alla differenza del tempo di calcolo, misurando quante operazioni matematiche (FLOPs) deve eseguire la macchina per raggiungere la convergenza.

#### 1. L-BFGS 
L-BFGS calcola il Gradiente Esatto. Per fare un singolo passo (un'iterazione), deve moltiplicare ogni singola riga per ogni singolo peso.
* **Costo di 1 Iterazione:** $\mathcal{O}(N \cdot d)$
* **Iterazioni necessarie ($I$):** L-BFGS è estremamente stabile. Sia che si abbiano mille o un miliardo di dati, impiegherà sempre all'incirca lo stesso numero di iterazioni per trovare il fondo della vallata convessa (empiricamente, $I \approx 35$).

Il costo totale è quindi lineare rispetto ai dati:
$$Costo_{L-BFGS} = I \times (N \times d)$$

#### 2. SAGA 
SAGA aggiorna i pesi in modo stocastico, riga per riga (Step). Un'**Epoca** si completa quando l'algoritmo ha letto tutte le $N$ righe esattamente una volta.
* **Costo di 1 Step (singola riga):** $\mathcal{O}(d)$
* **Costo di 1 Epoca ($N$ Step):** $N \times \mathcal{O}(d) = \mathcal{O}(N \cdot d)$

Il costo totale dipende da quante volte deve rileggere il dataset:
$$Costo_{SAGA} = E \times (N \times d)$$

#### 📐 La Stima delle Epoche (Formula di Defazio)
Come facciamo a sapere quante Epoche ($E$) impiegherà SAGA? Grazie alla dimostrazione di Defazio (2014), possiamo calcolare un'approssimazione esatta tramite queste variabili:
* **$\kappa$ (Kappa):** Il Numero di Condizionamento della matrice ($\kappa = \frac{L}{\mu}$). Rappresenta la complessità o la "pendenza" della vallata dell'errore.
* **$N$:** Numero di campioni nel dataset (le partite di scacchi).
* **$\epsilon$:** La tolleranza di errore desiderata.

La formula matematica per le Epoche necessarie a SAGA è:

$$E \approx \bigg( 1 + \frac{\kappa}{N} \bigg) \cdot \ln\bigg(\frac{1}{\epsilon}\bigg)$$

#### 📊 Il Crossover Algoritmico
Poiché il blocco di costo $(N \times d)$ è identico per entrambi gli algoritmi, il vincitore assoluto si determina confrontando semplicemente **$I$ (Iterazioni di L-BFGS)** con **$E$ (Epoche di SAGA)**. 

Mantenendo $I$ costante ($\approx 35$) e calcolando $E$ al variare di $N$, possiamo costruire un modello asintotico per individuare il punto di rottura ("Crossover") in cui SAGA diventa matematicamente superiore a L-BFGS.


## 📊 Risultati e Visualizzazioni

L'estrazione del condizionamento $\kappa$ ha permesso di tracciare il collasso asintotico di L-BFGS. 
Come si evince dal grafico sottostante, oltre una certa soglia critica di osservazioni (il *Crossover*), l'aggiornamento stocastico di SAGA annienta l'analisi computazionale di L-BFGS, risparmiando miliardi di operazioni.

---

## 🚀 Come avviare il progetto
Per il progetto utlizzare questo dataset: https://www.kaggle.com/datasets/arevel/chess-games 
inserendolo direttamente nella cartella DPA_FINAL_PROJECT 
controllare non ci siano altri file .csv 

Tutte le librerie richieste sono elencate nei requirement da installare nel virtual enviroment


