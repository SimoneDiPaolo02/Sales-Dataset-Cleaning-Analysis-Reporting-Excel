# Analisi e Pulizia di un Dataset di Vendite Retail

Progetto di data cleaning, analisi esplorativa e reportistica su un dataset sintetico di vendite di una catena di negozi, con errori e anomalie inseriti volutamente per simulare un contesto realistico.

## Obiettivo

Partire da un dataset grezzo con problemi tipici di dati reali (duplicati, valori mancanti, outlier, incoerenze di formattazione) e produrre un dataset pulito, verificato e pronto per l'analisi, corredato da insight di business e visualizzazioni.

## Struttura del file

| Foglio | Contenuto |
|---|---|
| `Raw_Data` | Dataset originale, non modificato — 7.996 transazioni |
| `NoteProcedurali` | Log delle decisioni prese durante la pulizia e la relativa motivazione |
| `Clean_Data` | Dataset pulito e pronto per l'analisi — 7.834 transazioni |
| `Operazioni_sospette` | Transazioni isolate per margine unitario negativo (75 righe), tenute separate invece che eliminate |
| `Analisi_Pivot` | Tabelle pivot per categoria, prodotto, metodo di pagamento e canale di vendita |
| `Insights` | Sintesi testuale dei risultati principali |
| `Charts` | Visualizzazioni a supporto degli insight |

## Il dataset

- **Periodo**: 1 gennaio 2024 – 31 dicembre 2025
- **10 negozi** in altrettante città italiane
- **5 categorie merceologiche**: Abbigliamento, Bellezza, Casa, Elettronica, Sport
- **25 prodotti** distinti
- Campi: ID vendita, data, negozio/città/regione, categoria/prodotto, quantità, costo e prezzo unitario, totale vendita, metodo di pagamento, canale di vendita, ID cliente

## Processo di pulizia

**1. Duplicati**
Verifica su ID vendita e su riga completa: nessun duplicato residuo nel dataset pulito.

**2. Valori mancanti**
Le righe con dati mancanti nei campi strutturali (data, negozio, categoria, prodotto, importi) sono state rimosse o corrette alla fonte. Per i campi categorici meno critici (metodo di pagamento, canale di vendita, ID cliente) i valori mancanti sono stati marcati esplicitamente come `"No Data"` invece di essere eliminati o imputati, per non perdere la riga né introdurre un valore inventato:
- Metodo di pagamento mancante: 313 transazioni
- Canale di vendita mancante: 196 transazioni
- ID cliente mancante: 470 transazioni

**3. Outlier e valori anomali**
Quantità, costo e prezzo unitario negativi o nulli sono stati individuati e corretti. Le 75 transazioni con margine unitario negativo (prezzo di vendita inferiore al costo) non sono state eliminate ma spostate nel foglio `Operazioni_sospette` per una verifica separata, per non alterare le metriche aggregate del dataset principale né perdere l'informazione.

**4. Standardizzazione**
Uniformati i nomi di negozi, categorie e metodi di pagamento (es. varianti come "Carta" ricondotte a "Carta Di Credito") per evitare che lo stesso valore venga contato come categorie distinte nelle aggregazioni.

**5. Colonne calcolate**
Aggiunte colonne derivate (prezzo-costo, costo totale, margine) tramite formule su tabella strutturata, per mantenere automaticamente coerenti i totali con i dati sorgente.

Il dataset pulito passa così da 7.996 a 7.834 transazioni (162 righe rimosse per duplicati, dati non recuperabili o errori non correggibili in modo affidabile).

## Insight principali

- **Elettronica** è la categoria col margine complessivo più alto (~131.000 €), seguita da Abbigliamento (~119.000 €) e Casa (~104.000 €); **Bellezza** è la categoria più debole (~64.000 €).
- Lo **Smartwatch** è il singolo prodotto più redditizio (~55.600 € di margine), davanti a Giacca, Tappeto e Profumo.
- Il canale **Negozio Fisico** resta leggermente davanti all'Online (3.080 vs 3.028 transazioni), con l'App Mobile in crescita ma ancora minoritaria (1.530).
- **Contanti** e **Paypal** sono i metodi di pagamento più usati, seguiti da carta di credito, buono regalo e bancomat.

## Strumenti utilizzati

Excel — tabelle strutturate, formule con riferimenti strutturati, tabelle pivot, grafici.
