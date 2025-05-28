# Generale

Prima di iniziare a lavorare sui progetti con git, è necessario configurare o globalmente o in ogni progetto il repository nel modo seguente

```bash
git config user.name "Nome Cognome"
git config user.email "nome.cognome@pandev.it"
```

```bash
git config merge.ff only
git config pull.rebase true
git config fetch.prune true
```

In pandev usiamo i merge commit solo quando strettamente necessario e preferiamo i rebase nella fase di feature branching. Con queste configurazioni di git si evitano merge commit per errore, disabilitandolo nel pull e nel merge

Il fetch prune è utile per tenere pulite le referenze remote con il branch locale
