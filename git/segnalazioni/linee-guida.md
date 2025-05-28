# Linee guida

* L'oggetto della mail di segnalazione o il titolo della segnalazione dev'essere cosi composto: "LIVELLO\_CRITICITÀ - TIPOLOGIA\_INTERVENTO - DESCRIZIONE\_CON\_7\_PAROLE" - (Es: 4 - BUG - Ricerca pagina prodotti non funzionante)
* Inserire sempre il "nome cognome" di chi fa la segnalazione.
* Inserire l'eventuale reporter, inteso come la persona da avvisare al momento della risoluzione, in mancanza di questo dato **NON** verrà eseguita nessuna azione di notifica della risoluzione dell'eventuale segnalazione.
* Eventuali screenshot devono essere forniti per **INTERO.** La definizione di intero in questo specifico caso è "schermata del desktop o mobile presa nella sua interezza dove sia presente barra degli indirizzi ed eventuale segnalazione dell'errore."
* Il link della pagina che è andato in errore per permettere al team di supporto di recarsi nella pagina avente la problematica il più agilmente possibile, se non viene fornito i tempi di risoluzione potrebbero aumentare o causare l'errata risoluzione del ticket.
* Le tipologie di intervento che possono essere richieste sono:
  * **BUG**: segnalazione di un bug con richiesta di risoluzione.
  * **CHECK**: anomalia/bug non verificato pervenuta da una segnalazione utente. questa tipologia di segnalazione NON tiene conto del livello di criticità ed è SEMPRE valorizzata come criticità lieve o trascurabile, anche se segnalata in modo diverso.&#x20;
  * **RESTORE:** un utente ha cancellato per sbaglio alcuni dati e si richiede se possibile recuperarli. come la precedente, salvo in rari casi, questo genere di segnalazioni viene valorizzata come una criticità lieve o trascurabile, anche se segnalata in modo diverso.&#x20;
* Criticità dell'errore espressa con una scala numerica. <mark style="color:blue;">Il buon senso ci insegna anche che non tutto può essere definito</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**DEFCON\_1**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">in quanto se</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**tutto**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">fosse catastrofico significherebbe che tutto sarebbe lieve e trascurabile!</mark> Ogni elemento della scala è spiegato di seguito:&#x20;
  1. <mark style="color:red;">Errore/Bug catastrofico</mark> - (Es: infrastruttura offline)
  2. <mark style="color:orange;">Errore/Bug critico</mark> - (Es: Impossibilità di effettuare il login)
  3. <mark style="color:yellow;">Errore/Bug di grave entità</mark> - (Es: Cambio password non funzionante)
  4. <mark style="color:green;">Errore/Bug di media entità</mark> - (Es: Form non funzionante o errore email)
  5. <mark style="color:blue;">Errore/Bug lieve o trascurabile</mark> - (Es: Non viene visualizzata una icona o una foto o il contenuto testuale è errato o formattato in modo errato)
* Nelle segnalazioni devono essere sempre presenti 4 elementi:
  * **CHI**: chi ha riscontrato l'errore. Es: Il cliente Pinko Pallino dell'azienda ACME S.R.L.
  * **COME**: Le condizioni nel momento in cui si è verificato il problema. Es: su windows 7 con chrome nella versione 106 dopo aver riempito il form della pagina home con questi parametri, elenco di parametri, e aver premuto invio.
  * **QUANDO**: L'elemento temporale in cui l'azione è svolta. Es: alle ore 18:30.
  * **ASPETTATIVA**: L'effetto dell'azione precedente che non si è verificato. Es: si aspettava di ricevere una email oltre al riepilogo delle informazioni inserite nel form ma la mail non è mai stata ricevuta.
* Nel caso di segnalazioni su piattaforme SaaS inserire nella voce **cliente/tenant** il riferimento dentro alla SaaS del cliente su cui verificare l'attività.
* **OGNI SEGNALAZIONE** produrrà una mail di lavorazione con la fattibilità o le eventuali tempistiche di lavorazione.
