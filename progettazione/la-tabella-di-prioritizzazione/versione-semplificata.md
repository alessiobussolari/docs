# Versione semplificata

**Introduzione:**

Nella fase di progettazione, utilizziamo una tabella di prioritizzazione per determinare l'ordine in cui sviluppare le funzionalità. In questa versione semplificata, consideriamo solo due criteri:

1. **Importanza sul Business**
2. **Fattibilità** (lato sviluppo)

**Nota:** Per il criterio **Fattibilità**, un valore più alto indica una maggiore facilità di implementazione (quindi più fattibile), mentre un valore più basso indica una maggiore complessità.

***

**Metodo di Calcolo:**

* **Punteggio Medio per Funzionalità:** Impostato a **3**
* **Numero di Funzionalità:** **3**
*   **Punteggio Totale per Colonna:**

    Punteggio Medio \* Numero di Funzionalità = 3 \* 3 = **9**

Questo significa che, per ogni criterio, abbiamo un totale di **9 punti** da distribuire tra le funzionalità.

***

**Funzionalità da Valutare:**

1. **Funzionalità 1:** Aggiunta del campo 'Cardinalità' nella tab 'Flusso'.
2. **Funzionalità 2:** Implementazione dei vincoli sulla 'Cardinalità'.
3. **Funzionalità 3:** Integrazione con il sistema H2O.

***

**Assegnazione dei Punteggi:**

**1. Importanza sul Business**

Distribuiamo i **9 punti** totali in base all'importanza di ogni funzionalità per il business.

* **Funzionalità 1:** **4**
* **Funzionalità 2:** **3**
* **Funzionalità 3:** **2**

**Calcolo:**

* Somma dei punteggi: 4 + 3 + 2 = **9**

**2. Fattibilità**

Distribuiamo i **9 punti** totali in base alla facilità di implementazione di ogni funzionalità.

* **Funzionalità 1:** **5** (più facile da sviluppare)
* **Funzionalità 2:** **3**
* **Funzionalità 3:** **1** (più complessa da sviluppare)

**Calcolo:**

* Somma dei punteggi: 5 + 3 + 1 = **9**

***

**Tabella Riassuntiva:**

| Funzionalità                                         | Importanza sul Business | Fattibilità | Punteggio Totale |
| ---------------------------------------------------- | ----------------------- | ----------- | ---------------- |
| **Funzionalità 1:** Aggiunta del campo 'Cardinalità' | 4                       | 5           | 9                |
| **Funzionalità 2:** Implementazione dei vincoli      | 3                       | 3           | 6                |
| **Funzionalità 3:** Integrazione con H2O             | 2                       | 1           | 3                |

**Calcolo del Punteggio Totale per Funzionalità:**

* **Funzionalità 1:** 4 (Business) + 5 (Fattibilità) = **9**
* **Funzionalità 2:** 3 (Business) + 3 (Fattibilità) = **6**
* **Funzionalità 3:** 2 (Business) + 1 (Fattibilità) = **3**

***

**Ordine di Priorità:**

1. **Funzionalità 1** - Punteggio Totale: **9** (Priorità Alta)
2. **Funzionalità 2** - Punteggio Totale: **6** (Priorità Media)
3. **Funzionalità 3** - Punteggio Totale: **3** (Priorità Bassa)

***

**Spiegazione dei Calcoli:**

* **Punteggio Totale per Colonna:** Stabilito a **9** per mantenere coerenza nella valutazione.
  * **Importanza sul Business:** 4 + 3 + 2 = **9**
  * **Fattibilità:** 5 + 3 + 1 = **9**
* **Punteggio Totale per Funzionalità:** Somma dei punteggi nei due criteri per ogni funzionalità.

***

**Interpretazione dei Risultati:**

* **Funzionalità 1** ha la massima priorità perché è sia molto importante per il business che altamente fattibile.
* **Funzionalità 2** è moderatamente importante e ha una fattibilità media.
* **Funzionalità 3** è la meno prioritaria a causa della sua minore importanza e maggiore complessità.

***

**Conclusioni:**

Utilizzando questa tabella semplificata, possiamo indirizzare le risorse di sviluppo in modo efficace, concentrandoci sulle funzionalità che offrono il massimo valore al business e sono più facili da implementare.

***

**Note Finali:**

* Questo metodo assicura una distribuzione equa dei punti e aiuta nella decisione strategica delle priorità.
* I punteggi possono essere rivisti se cambiano le esigenze del business o se emergono nuove informazioni sulla fattibilità tecnica.

***

**Esempio di Come Utilizzare la Tabella:**

* **Inizio dello Sviluppo:**
  * Avviare immediatamente lo sviluppo della **Funzionalità 1**.
* **Pianificazione Successiva:**
  * Programmare la **Funzionalità 2** dopo il completamento della prima.
* **Valutazione a Lungo Termine:**
  * Considerare la **Funzionalità 3** in una fase successiva, magari dopo ulteriori analisi o quando le risorse lo permettono.

***

Speriamo che questa tabella semplificata e i calcoli associati facilitino la pianificazione e la gestione del progetto, garantendo che le funzionalità più critiche siano affrontate per prime.
