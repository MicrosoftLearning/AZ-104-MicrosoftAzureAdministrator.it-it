---
lab:
  title: 'Lab 10: Implementare la protezione dei dati'
  module: Administer Data Protection
---

# Lab 10 - Implementare la protezione dati

## Introduzione al lab    

In questo lab vengono fornite informazioni sul backup e il ripristino delle macchine virtuali di Azure. Si apprenderà come creare un insieme di credenziali del servizio di ripristino e un criterio di backup per le macchine virtuali di Azure. Informazioni sul ripristino di emergenza con Azure Site Recovery per le macchine virtuali. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare le aree, ma i passaggi vengono scritti usando Stati Uniti orientali e Stati Uniti occidentali.

## Tempo stimato: 40 minuti

## Scenario laboratorio

L'organizzazione sta valutando Servizi di ripristino di Azure per il backup e il ripristino di file ospitati in macchine virtuali di Azure. Vogliono identificare i metodi di protezione dei dati archiviati nell'insieme di credenziali di Servizi di ripristino da perdite accidentali o dannose di dati.

## Simulazione interattiva del lab

È disponibile una simulazione di lab interattiva che può risultare utile per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ **[Eseguire il backup di macchine virtuali e file](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)** locali. Creare un insieme di credenziali dei servizi di ripristino e implementare un backup di macchine virtuali di Azure. Implementare il backup di file e cartelle locali usando l'agente di Servizi di ripristino di Microsoft Azure. I backup locali non rientrano nell'ambito di questo lab, ma potrebbero essere utili per visualizzare questi passaggi. 

## Attività

+ Attività 1: Effettuare il provisioning dell'ambiente lab
+ Attività 2: Creare un insieme di credenziali di Servizi di ripristino
+ Attività 3: Implementare il backup a livello di macchina virtuale di Azure
+ Attività 4: Monitorare Backup di Azure
+ Attività 5: Implementare Azure Site Recovery per le macchine virtuali

## Tempo stimato: 40 minuti

## Diagramma dell'architettura

![Diagramma delle attività di architettura.](../media/az104-lab10-architecture-diagram.png)

## Attività 1: Effettuare il provisioning dell'ambiente lab

In questa attività si userà un modello per distribuire una macchina virtuale. La macchina virtuale verrà usata per testare diversi scenari di backup.

1. Se necessario, scaricare i **\\file lab Allfiles\\\\10\\az104-10-vms-edge-template.json** e **\\Allfiles\\Labs\\10\\az104-10-vms-edge-parameters.json** nel computer.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Nella portale di Azure cercare e selezionare `Deploy a custom template`.

1. Nella pagina di distribuzione personalizzata selezionare **Compila un modello personalizzato nell'editor**.

1. Nella pagina modifica modello selezionare **Carica file**.

1. Individuare e selezionare il **file Allfiles\\Labs\\10\\az104-10-vms-edge-template.json** e selezionare **Apri**.\\

1. Seleziona **Salva**.

1. Nella pagina di distribuzione personalizzata selezionare **Modifica parametri**.

1. Nella pagina modifica parametri selezionare **Carica file**. Individuare e selezionare il **file Allfiles\\Labs\\10\\az104-10-vms-edge-parameters.json** e selezionare **Apri**.\\

1. Seleziona **Salva**.

1. Usare le informazioni seguenti per completare i campi di distribuzione personalizzati, lasciando tutti gli altri campi con i valori predefiniti:

    | Impostazione       | Valore         | 
    | ---           | ---           |
    | Subscription  | la propria sottoscrizione di Azure |
    | Gruppo di risorse| `az104-rg10` (Se necessario, selezionare **Crea nuovo**)
    | Area geografica        | **Stati Uniti orientali**   |
    | Username      | `Student`   |
    | Password      | Specificare una password complessa |

1. Selezionare **Rivedi e crea** e quindi **Crea**.

## Attività 2: Creare un insieme di credenziali di Servizi di ripristino

In questa attività verrà creato un insieme di credenziali di Servizi di ripristino. Un insieme di credenziali di Servizi di ripristino fornisce i servizi di backup per le macchine virtuali di Azure.

1. Nella portale di Azure cercare e selezionare `Recovery Services vaults` e nel pannello **Insiemi** di credenziali di Servizi di ripristino fare clic su **+ Crea**.

1. Nel pannello **Crea Insieme di credenziali di Servizi di ripristino** specificare le impostazioni seguenti:

    | Impostazioni | Valore |
    | --- | --- |
    | Subscription | nome della sottoscrizione di Azure |
    | Gruppo di risorse | **az104-rg10vault** (se necessario, selezionare Crea nuovo) |
    | Nome dell'insieme di credenziali | `az104-vault1` |
    | Area | **Stati Uniti** occidentali (o un'area non usata nell'attività precedente per distribuire le macchine virtuali) |

    >**Nota**: assicurarsi di specificare un'area diversa in cui sono state distribuite le macchine virtuali nell'attività precedente.

    ![Screenshot dell'insieme di credenziali di Servizi di ripristino.](../media/az104-lab10-create-rsv.png)

1. Fare clic su **Rivedi e crea**, assicurarsi che la convalida abbia avuto esito positivo e fare clic su **Crea**.

    >**Nota**: attendere il completamento della distribuzione. La distribuzione dovrebbe richiedere meno di 1 minuto.

1. Al termine della distribuzione, fare clic su **Vai alla risorsa**.

1. Nel pannello **az104-vault1** Insieme di credenziali di Servizi di ripristino fare clic su **Proprietà** nella **sezione Impostazioni**.

1. Nel pannello **az104-vault1 - Proprietà** fare clic sul **collegamento Aggiorna** in **Etichetta configurazione** backup.

1. Nel pannello **Configurazione** backup esaminare le opzioni disponibili per **Archiviazione tipo di** replica. Lasciare selezionata l'impostazione predefinita **Con ridondanza geografica** e chiudere il pannello.

    >**Nota**: questa impostazione può essere configurata solo se non sono presenti elementi di backup esistenti.

1. Tornare al **pannello az104-vault1 - Proprietà**, fare clic sul **collegamento Aggiorna** in **Sicurezza Impostazioni** etichetta.

1. Nel pannello **Impostazioni di sicurezza** si noti che l'opzione **Eliminazione temporanea (per carichi di lavoro in esecuzione in Azure)** è **abilitata**.

1. Chiudere il **pannello Sicurezza Impostazioni** e fare clic su **Panoramica** nel **pannello az104-vault1** Insieme di credenziali di Servizi di ripristino.

## Attività 3: Implementare il backup a livello di macchina virtuale di Azure

In questa attività verrà implementato il backup a livello di macchina virtuale di Azure. Come parte di un backup di vm, sarà necessario definire i criteri di backup e conservazione applicabili al backup. A diverse macchine virtuali possono essere assegnati criteri di backup e conservazione diversi.

   >**Nota**: prima di avviare questa attività, assicurarsi che la distribuzione avviata nella prima attività di questo lab sia stata completata correttamente.

1. Nel pannello **az104-vault1** Insieme di credenziali di Servizi di ripristino fare clic su **Panoramica** e quindi su **+ Backup**.

1. Nel pannello **Obiettivo del backup** specificare le impostazioni seguenti:

    | Impostazioni | Valore |
    | --- | --- |
    | Dove viene eseguito il carico di lavoro? | **Azure** (si notino le altre opzioni) |
    | Di quali elementi si vuole eseguire il backup? | **Macchina** virtuale (si notino le altre opzioni) |

1. Nel pannello **Obiettivo del backup** fare clic su **Backup**.

1. In **Criteri di backup** esaminare le impostazioni per **DefaultPolicy** e selezionare **Crea nuovi criteri**.

1. Definire un nuovo criterio di backup con le impostazioni seguenti e non modificare i valori predefiniti per gli altri criteri:

    | Impostazione | Valore |
    | ---- | ---- |
    | Nome del criterio | `az104-policy` |
    | Frequenza | **Giornaliero** |
    | Ora | **12:00** |
    | Fuso orario | Nome del fuso orario locale |
    | Conservare gli snapshot del ripristino istantaneo per | **2** giorni |

    ![Screenshot della pagina dei criteri di backup.](../media/az104-lab10-backup-policy.png)

1. Fare clic su **OK** per creare il criterio e quindi nella sezione **Macchine virtuali** selezionare **Aggiungi**.

1. Nel pannello **Seleziona macchine virtuali** selezionare **az-104-10-vm0**, fare clic su **OK**, quindi nel pannello **Backup** visualizzato di nuovo fare clic su **Abilita backup**.

    >**Nota**: attendere il completamento dell'abilitazione del backup. L'operazione dovrebbe richiedere circa 2 minuti.

1. Tornare al **pannello az104-vault1** Insieme di credenziali di Servizi di ripristino, nella **sezione Elementi protetti fare clic su **Elementi**** di backup e quindi fare clic sulla voce Macchina **** virtuale di Azure.

1. Nel pannello **Elementi di backup (macchina virtuale di Azure)** selezionare il collegamento **Visualizza dettagli** per **az104-10-vm0** ed esaminare i valori delle voci **Controllo preliminare di backup** e **Stato ultimo backup**.

1. Nel pannello dell'elemento di backup **az104-10-vm0** fare clic su **Esegui backup ora**, accettare il valore predefinito nell'elenco a discesa **Conserva backup fino a** e fare clic su **OK**.

    >**Nota**: non attendere il completamento del backup, ma procedere con l'attività successiva.

## Attività 4: Monitorare Backup di Azure

In questa attività si distribuirà un account di archiviazione di Azure. Si configurerà quindi l'insieme di credenziali per inviare i log e le metriche all'account di archiviazione. Questo repository può quindi essere usato con Log Analytics o con un'altra soluzione di monitoraggio di terze parti.

1. Nella portale di Azure cercare e selezionare `Storage accounts`.

1. Nella pagina Archiviazione account selezionare **Crea**.

1. Usare le informazioni seguenti per definire l'account di archiviazione e quindi selezionare **Rivedi**.

    | Impostazioni | Valore |
    | --- | --- | 
    | Subscription          | *Sottoscrizione in uso*    |
    | Gruppo di risorse        | **az104-rg10**        |
    | Nome account di archiviazione  | Specificare un nome univoco globale, ad esempio *backupdiag1042024123*    |
    | Area                | **Stati Uniti** orientali (o un'area nelle vicinanze)    |

1. Nella scheda Rivedi selezionare **Crea**.

    >**Nota**: attendere il completamento della distribuzione. Ci vorranno circa un minuto.

1. Passare all'insieme di credenziali di **Servizi di ripristino az104-vault1** .

1. Nella **pagina az104-vault1** selezionare **Diagnostica Impostazioni**.

1. Nella pagina Diagnostica Impostazioni selezionare **Aggiungi impostazione** di diagnostica.

1. Nella pagina Impostazioni diagnotiche denominare l'impostazione `Logs and Metrics to storage`.

1. Posizionare un segno di spunta accanto ai registri e alle catagorie delle metriche seguenti:

    - **Backup di Azure dati di report**
    - **Dati del processo di addon Backup di Azure**
    - **Addon Backup di Azure i dati degli avvisi**
    - **Processi di Azure Site Recovery**
    - **Eventi di Azure Site Recovery**
    - **Integrità**

1. In Dettagli destinazione posizionare un segno di spunta accanto a Archivia **a un account** di archiviazione.

1. Nel campo a discesa Archiviazione account selezionare l'account di archiviazione distribuito in precedenza in questa attività.

1. Seleziona **Salva**.

1. Tornare alla pagina della **risorsa az104-vault1** . 

1. In **az104-vault1** selezionare **Processi di backup**.

1. Individuare l'operazione di backup per la **macchina virtuale az104-10-vm0** e selezionare **Dettagli**

    >**Nota**: a seconda della risoluzione dello schermo, potrebbe essere necessario scorrere verso destra nella tabella dei processi di backup.

1. Esaminare i dettagli del processo di backup attivato nella macchina virtuale. Quali sono le due sottoattività del processo di backup?


## Attività 5: Implementare Azure Site Recovery

In questa attività, 

1. Cercare e selezionare l'insieme di credenziali di Servizi di ripristino, **az104-vault1**.

1. Nel pannello **Panoramica selezionare **+ Abilita Site Recovery****.

1. Esaminare le opzioni e quindi selezionare nella sezione **Azure Macchine virtuali** Abilitare la **replica**.

1. Nella **scheda Origine** configurare le impostazioni.

    | Impostazione | Valore |
    | ---- | ---- |
    | Area| **Stati Uniti** orientali (leggere la notifica sulla replica nella stessa area) |
    | Gruppo di risorse | **az104-rg10** |
    | Modello di distribuzione di macchine virtuali | **Resource Manager** |
    | Ripristino di emergenza tra zone di disponibilità | **No** |

1. Selezionare **Avanti e nella **scheda Macchine** virtuali selezionare **az104-10-vm0****.

1. Selezionare **Avanti** e passare alla **scheda Impostazioni** replica. Si noti il percorso di destinazione e le informazioni sulla rete di failover. Queste risorse verranno create automaticamente. Prendere le impostazioni predefinite e selezionare **Avanti**.

1. Nella **scheda Gestisci** esaminare i parametri.

    | Impostazione | Valore |
    | ---- | ---- |
    | Criteri di replica | **Criteri di conservazione** di 24 ore (che possono essere modificati da 0 a 15 giorni) |
    | Aggiorna impostazioni | **Consentire la gestione di AsR** |

1. Selezionare **Avanti e quindi **Abilitare la replica****.

    >**Nota**: l'abilitazione della replica sarà di circa 15 minuti. Controllare i messaggi di notifica in alto a destra nel portale.

1. Al termine della replica, cercare e individuare l'insieme di credenziali di Servizi di ripristino az104-vault1****.

1. **Nella sezione Elementi** protetti selezionare **Elementi** replicati. Verificare che la macchina virtuale sia visualizzata come integra per l'integrità della replica. Si noti che lo stato mostrerà la sincronizzazione (a partire dallo stato 0%) e in definitiva mostra **Protected** dopo il completamento della sincronizzazione iniziale. 

    **Lo sapevi?** È consigliabile [testare il failover di una macchina virtuale](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm) protetta.


## Esaminare i punti principali del lab

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Backup di Azure servizio offre soluzioni semplici, sicure e convenienti per eseguire il backup e il ripristino dei dati.

+ Backup di Azure può proteggere le risorse locali e cloud, incluse le macchine virtuali e le condivisioni file.

+ Backup di Azure criteri configurano la frequenza dei backup e il periodo di conservazione per i punti di ripristino. 

+ Azure Site Recovery è una soluzione di ripristino di emergenza che fornisce protezione per le macchine virtuali e le applicazioni.

+ Azure Site Recovery replica i carichi di lavoro in un sito secondario e, in caso di interruzione o emergenza, è possibile eseguire il failover nel sito secondario e riprendere le operazioni con tempi di inattività minimi.

+ Un insieme di credenziali di Servizi di ripristino archivia i dati di backup e riduce al minimo il sovraccarico di gestione.


## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.

+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.


