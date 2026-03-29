# Simulazione del Prezzo di Dogecoin tramite Agent-Based Model (ABM)

Questo repository contiene il codice sorgente sviluppato per la tesi di laurea, il cui obiettivo è modellare e simulare le dinamiche di prezzo della criptovaluta Dogecoin. 
L'infrastruttura utilizza un modello basato su agenti (Agent-Based Model - ABM) su una rete complessa per relazionare le dinamiche comportamentali degli investitori (altamente influenzati dalle informazioni sui social media, in particolare i tweet di influencer come Elon Musk) con i movimenti anomali e volatili dei mercati crittografici. Il modello include anche l'implementazione di forze di mercato come il *profit taking* (mean reversion) e shock esogeni/endogeni.

## 🛠️ Requisiti di Sistema

Per eseguire i notebook è necessaria un'installazione di Python 3.8+ e le seguenti librerie principali:
- `pandas`
- `numpy`
- `matplotlib`
- `networkx`
- `scikit-learn` (per la standardizzazione dei segnali)

Puoi installare le dipendenze eseguendo:
```bash
pip install pandas numpy matplotlib networkx scikit-learn jupyter
```

## 📥 Acquisizione dei Dati (Kaggle)

Per riprodurre la simulazione, è necessario scaricare i dataset originali (prezzi storici e tweet) prima di eseguire i codici.

1. Registrati o accedi a [Kaggle](https://www.kaggle.com/).
2. Scarica il dataset da questo link: https://www.kaggle.com/datasets/tderonde/doge-tweets-1121-43021
3. Estrai i file `.csv` scaricati e posizionali all'interno di una cartella denominata `data/` (o modifica i percorsi di importazione all'interno dei notebook per puntare alla tua directory di download).

## 🚀 Guida all'Esecuzione (Pipeline del Modello)

Il progetto è strutturato in una pipeline sequenziale divisa in quattro moduli (Jupyter Notebooks). Devono essere eseguiti rigorosamente in quest'ordine, poiché l'output di ogni step costituisce l'input del successivo.

### Step 1: `(1) pulizia prezzi.ipynb`
Questo notebook si occupa del pre-processing dei dati finanziari grezzi.
- Effettua il *resampling* temporale della serie storica a intervalli discreti ($\Delta t = 10$ minuti).
- Gestisce i dati mancanti (assenza di transazioni in un dato intervallo) applicando la tecnica del *Forward Fill* (Zero-Order Hold).

### Step 2: `(2) pulizia tweet e creazione n(t).ipynb`
Costruisce il segnale matematico di input che influenzerà gli agenti.
- Pulisce i dati di testo estratti dal social network.
- Genera la funzione di *forcing* $n(t)$, composta da due elementi: la *Baseline Activity* (il volume organico e costante della community) e la *Impulsive Component* (l'hype generato da eventi catalizzatori, modellato con un decadimento esponenziale nel tempo).

### Step 3: `(3) dinamica_opinione_agenti (con mu 0.5).ipynb`
Il cuore della simulazione comportamentale.
- Inizializza una rete Small-World di $N=1000$ agenti eterogenei.
- Calcola l'evoluzione temporale delle opinioni $x_i(t)$ degli agenti (tra -1 e 1) basandosi sull'inerzia cognitiva, sull'influenza sociale dei vicini, sulla sensibilità alle news esterne ($n(t)$) e su una componente di rumore stocastico.

### Step 4: `(4) sim prezzi con shock e freno.ipynb`
Converte la dinamica psicologica della rete in metriche finanziarie.
- Trasforma le opinioni aggregate in domanda netta di mercato.
- Applica un freno dinamico proporzionale allo *spread* rispetto a una media mobile logaritmica (per simulare le prese di profitto).
- Introduce variabili di *Shock* per modellare crolli esogeni strutturali (es. blackout dell'hashrate) ed endogeni comportamentali (dinamiche di *Sell the News*).
- Restituisce il grafico finale che sovrappone il prezzo simulato a quello reale storico.

## 📊 Configurazione dei Parametri
I parametri del modello (es. inerzia $\mu$, coefficiente di influenza sociale $C$, magnitudo degli shock, ecc.) sono stati calibrati per aderire agli eventi storici documentati. Eventuali modifiche a variabili come `MU`, `ALPHA` o `K_BRAKE` all'interno dei notebook altereranno radicalmente la topologia e la reattività del sistema, come dettagliato nell'Analisi di Sensibilità della tesi.

## 📝 Autore
* **Carmine Rauso**
```
