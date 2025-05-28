---
description: >-
  Ecco la procedura per preparare un branch all'unione con il branch da cui è
  stato forcato, questa procedura permette di mantenere la storia di git pulita
  e lineare se eseguita correttamente
---

# Unione

Lo step iniziale per iniziare l'unione dei branch è di spostarsi sul branch padre e allinearsi con la sua remote

```bash
git checkout BRANCH_PADRE 
git pull
```

Successivamente spostarsi nuovamente sul branch in cui si stava lavorando

```bash
git checkout -
```

A questo punto possiamo eseguire il primo allineamento con il rebase

```bash
git rebase BRANCH_MASTER
```

Il comando precedente allinea il nostro branch mettendo i commit che abbiamo nel nostro ramo in cima alla storia git e riportando gli eventuali commit che erano presenti nel padre subito sotto ai nostri in modo che le 2 storie siano allineate al 100%.

A questo punto possiamo agire in 2 modi diversi:

* Semplicemente fare il merge in modalità fast-forward.
* Sistemare la storia del nostro branch e poi eseguire il merge in modalità fast-forward, per seguire la procedura [premere qui](sistemazione-di-un-branch.md).&#x20;

La seconda opzione è più articolata ma ci permette di sistemare i commit che eventualmente abbiamo fatto solo per non perdere i dati o di sistemare quei commit che riportano descrizioni poco chiare.





