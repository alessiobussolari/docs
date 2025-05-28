---
description: >-
  Con questa procedura possiamo andare a sistemare i commit e decidere se
  riportare titti i messaggi di commit o solo quelli più significativi per la
  storia.
---

# Sistemazione di un branch

Per prima cosa dobbiamo cercare il commit subito dopo il fork del nostro ramo da quello padre e una volta individuato copiare l'hash del commit e usarlo in questo comando

```bash
git rebase -i CODICE_DEL_COMMIT
```

Con questo comando iniziamo la procedura di rebase formata solitamente da 2 step, che sono:

1. Selezione dei commit da scartare o tenere, in questa fase propongo di fare il pick (P) solo del primo commit e lo squash (S) di tutti i commit sottostanti in modo da ricavare un commit unico da tutti i commit precedenti con dentro tutte le modifiche e procedere.
2. Successivamente ci verrà chiesto di formare il nuovo testo del commit con titolo e descrizione. in questo punto verranno riportati i testi di tutti i commit precedenti e possiamo eliminare quelli superflui e inutili o riscriverlo completamente e procedere.

Al termine troveremo un commit unico con dentro tutte le modifiche che abbiamo effettuato, se naturalmente abbiamo tenuto tutti i commit e nel primo passaggio non abbiamo fatto il discard di qualche commit.

<mark style="color:yellow;">N.B. durante il rebase potrebbe essere necessario eseguire la risoluzione di eventuali conflitti. la procedura da seguiire e quella solita per la risoluzione di conflitti su file che si ha anche nel merge.</mark>
