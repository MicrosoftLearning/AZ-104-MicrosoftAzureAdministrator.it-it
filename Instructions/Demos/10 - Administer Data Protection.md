---
demo:
  title: 'Dimostrazione 10: Amministrare la protezione dei dati'
  module: Administer Data Protection
---

# 10 - Amministrare la protezione dati

## Backup di condivisioni file di Azure

In questa dimostrazione verrà illustrato il backup di una condivisione file nel portale di Azure.

> **Nota:** Questa dimostrazione richiede un account di archiviazione con una condivisione file. 

**Riferimento**: [Eseguire il backup delle condivisioni file di Azure nel portale di Azure](https://docs.microsoft.com/azure/backup/backup-afs)

**Creare un insieme di credenziali di Servizi di ripristino**

1. Usare il portale di Azure.

1. Cercare un insieme **di credenziali di Servizi di ripristino**.

1. Creare un **insieme di credenziali di Servizi di ripristino**. Esaminare il requisito che l'insieme di credenziali si trova nella stessa area della condivisione file. 

1. Attendere la creazione dell'insieme di credenziali. 

**Configurare il backup dei file di Azure**

1. Passare al **Centro backup** e creare una nuova istanza **di Backup** .

1. Esaminare e discutere le scelte nell'elenco a discesa **Tipo origine dati** . Selezionare **File di Azure (archiviazione di Azure).** 

1. Selezionare **l'insieme di credenziali**.

1. **Continuare** a configurare il backup. Selezionare l'account di archiviazione e la condivisione file specifici che si desidera eseguire il backup.  

1. Nei **dettagli dei criteri** fare clic su **Modifica questo criterio**. Discutere lo scopo dei criteri di backup. Esaminare **l'intervallo** di pianificazione e conservazione del **backup**.  

1. **Abilitare il backup** per salvare le modifiche. 

1. Quando si ha tempo, vedere come **ripristinare** **un'istanza di Backup**. Inoltre, come monitorare i **processi di backup**. 

## Backup di Azure Macchine virtuali

In questa dimostrazione verrà pianificato un backup giornaliero di una macchina virtuale in un insieme di credenziali di Servizi di ripristino.

> **Nota:** Questa dimostrazione richiede una macchina virtuale e un insieme di credenziali dei servizi di ripristino.

**Informazioni di riferimento**: [Esercitazione - Eseguire il backup di più macchine virtuali di Azure](https://docs.microsoft.com/azure/backup/tutorial-backup-vm-at-scale)

1. Usare il portale di Azure.

1. Passare al **Centro backup** e creare una nuova istanza **di Backup** .

1. Selezionare **Macchine virtuali di Azure** come **tipo origine dati** e selezionare l'insieme di credenziali.

1. Esaminare **DefaultPolicy**. Il criterio predefinito esegue il backup della macchina virtuale una volta al giorno. I backup giornalieri vengono conservati per 30 giorni. Gli snapshot di ripristino istantaneo vengono conservati per due giorni.

1. Usare **Abilita backup** per salvare la configurazione.

1. Come si ha tempo, vedere come **eseguire il backup ora**. Inoltre, come esaminare i **processi di backup**.  

