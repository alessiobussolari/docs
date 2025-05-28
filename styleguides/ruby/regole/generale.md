# Generale

* Fate in modo che tutte le linee dei vostri metodi operino allo stesso livello di astrazione. (Principio del singolo livello di astrazione).&#x20;
* Codificare in modo funzionale.
* Evitare la mutazione (effetti collaterali) quando è possibile.
* Evitare la programmazione difensiva, una programmazione eccessivamente difensiva può salvaguardare da errori che non si verificheranno mai, con conseguenti costi di esecuzione e manutenzione..
* Evitare di mutare gli argomenti.
* Evitare il monkeypatching.
* Evitare metodi lunghi.
* Evitare lunghi elenchi di parametri.
* Evitare la metaprogrammazione inutile.
* Preferire <mark style="color:red;">`public_send`</mark> a <mark style="color:red;">`send`</mark> per non aggirare la visibilità <mark style="color:red;">`private/protected`</mark>.
* Scrivere codice sicuro con <mark style="color:red;">`ruby -w`</mark>.
* Evitare più di tre livelli di annidamento dei blocchi.
