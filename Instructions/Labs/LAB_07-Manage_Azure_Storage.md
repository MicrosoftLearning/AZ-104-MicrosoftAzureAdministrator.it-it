---
lab:
  title: 'Lab 07: Gestire l''archiviazione di Azure'
  module: Administer Azure Storage
---

# Lab 07 - Gestire Archiviazione di Azure

## Introduzione al lab

In questo lab si apprenderà come creare account di archiviazione per BLOB di Azure e file di Azure. Si apprenderà come configurare e proteggere i contenitori BLOB. Si apprenderà anche come usare Archiviazione Browser per configurare e proteggere le condivisioni file di Azure. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali.

## Tempo stimato: 50 minuti

## Scenario laboratorio

L'organizzazione sta attualmente archiviando i dati negli archivi dati locali. La maggior parte di questi file non è accessibile di frequente. Si vuole ridurre al minimo il costo dell'archiviazione inserendo i file a cui si accede raramente in livelli di archiviazione con prezzi inferiori. Verranno anche esplorati diversi meccanismi di protezione offerti da Archiviazione di Azure, tra cui l'accesso alla rete, l'autenticazione, l'autorizzazione e la replica. Infine, si vuole determinare in quale misura File di Azure è adatta per ospitare le condivisioni file locali.

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Creare un archivio](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205) BLOB. Creare un account di archiviazione, gestire l'archiviazione BLOB e monitorare le attività di archiviazione. 
  
+ [Gestire l'archiviazione di](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011) Azure. Creare un account di archiviazione ed esaminare la configurazione. Gestire i contenitori di archiviazione BLOB. Configurare la rete di archiviazione. 

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab07-architecture.png)

## Competenze mansione

+ Attività 1: Creare e configurare un account di archiviazione. 
+ Attività 2: Creare e configurare l'archiviazione BLOB sicura.
+ Attività 3: Creare e configurare l'archiviazione file di Azure sicura.

## Attività 1: Creare e configurare un account di archiviazione. 

In questa attività si creerà e si configurerà un account di archiviazione. L'account di archiviazione userà l'archiviazione con ridondanza geografica e non avrà accesso pubblico. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Storage accounts`e quindi fare clic su **+ Crea**.

1. Nella **scheda Informazioni di base** del **pannello Crea un account** di archiviazione specificare le impostazioni seguenti (lasciare altri valori predefiniti):

    | Impostazione | Valore |
    | --- | --- |
    | Subscription          | nome della sottoscrizione di Azure  |
    | Gruppo di risorse        | **az104-rg7** (create new) |
    | Nome account di archiviazione  | Qualsiasi nome univoco globale composto da 3-24 lettere e numeri |
    | Area                | **(Stati Uniti) Stati Uniti orientali**  |
    | Prestazioni           | **Standard** (si noti l'opzione Premium) |
    | Ridondanza            | **Archiviazione** con ridondanza geografica (si notino le altre opzioni)|
    | Rendere l'accesso in lettura ai dati in caso di disponibilità a livello di area | Selezionare la casella |

>**Lo sapevi?** È consigliabile usare il livello di prestazioni Standard per la maggior parte delle applicazioni. Usare il livello di prestazioni Premium per applicazioni aziendali o ad alte prestazioni. 

1. **Nella scheda Avanzate** usare le icone informative per altre informazioni sulle scelte. Accettare le impostazioni predefinite. 

1. Nella **scheda Rete** esaminare le opzioni disponibili, selezionare **Disabilita l'accesso pubblico e usare l'accesso privato.**

1. Esaminare la **scheda Protezione dati** . Si noti che 7 giorni è il criterio di conservazione predefinito per l'eliminazione temporanea. Si noti che è possibile abilitare il controllo delle versioni dei BLOB. Accettare i valori predefiniti.

1. Esaminare la **scheda Crittografia** . Si notino le opzioni di sicurezza aggiuntive. Accettare i valori predefiniti.

1. Selezionare **Rivedi**, attendere il completamento del processo di convalida e quindi fare clic su **Crea**.

1. Dopo aver distribuito l'account di archiviazione, selezionare **Vai alla risorsa**.

1. Esaminare il pannello **Panoramica** e le configurazioni aggiuntive che è possibile modificare. Si tratta di impostazioni globali per l'account di archiviazione. Si noti che l'account di archiviazione può essere usato per contenitori BLOB, condivisioni file, code e tabelle.

1. Dalla sezione **Sicurezza e rete** selezionare **Rete**. Si noti che l'accesso alla rete pubblica è disabilitato.

    + Modificare il **livello** di accesso pubblico su **Abilitato da reti virtuali e indirizzi** IP selezionati.
    + **Nella sezione Firewall** selezionare la casella **Aggiungi l'indirizzo IP del client.**
    + Assicurarsi di **salvare** le modifiche. 
  
1. **Nella sezione Gestione** dati visualizzare il **pannello** Ridondanza. Si notino le informazioni sulle posizioni del data center primario e secondario.

1. **Nella sezione Gestione** dati selezionare **Gestione** del ciclo di vita e quindi selezionare **Aggiungi una regola**.

    + **Assegnare alla regola `Movetocool`il nome** . Si notino le opzioni per limitare l'ambito della regola.
    
    + Nella **scheda BLOB di** base, *se* i BLOB basati sono stati modificati più di `30 days` prima *,* **passare all'archiviazione ad accesso sporadico**. Si notino le altre opzioni. 
    
    + Si noti che è possibile configurare altre condizioni. Selezionare **Aggiungi** al termine dell'esplorazione.

    ![Screenshot per passare alle condizioni delle regole ad accesso sporadico.](../media/az104-lab07-movetocool.png)

## Attività 2: Creare e configurare l'archiviazione BLOB sicura

In questa attività si creerà un contenitore BLOB e si caricherà un'immagine. I contenitori BLOB sono strutture simili a directory che archiviano dati non strutturati.

### Creare un contenitore BLOB e criteri di conservazione basati sul tempo

1. Continuare nel portale di Azure, usando l'account di archiviazione.

1. Nella sezione **Archiviazione dati** fare clic su **Contenitori**. 

1. Fare clic su **+ Contenitore** e **creare** un contenitore con le impostazioni seguenti:

    | Impostazione | valore |
    | --- | --- |
    | Nome | `data`  |
    | Livello di accesso pubblico | Si noti che il livello di accesso è impostato su privato |

    ![Screenshot della creazione di un contenitore.](../media/az104-lab07-create-container.png)

1. Nel contenitore scorrere fino ai puntini di sospensione (...) all'estrema destra, selezionare **Criteri** di accesso.

1. Nell'area **Archiviazione** BLOB non modificabile selezionare **Aggiungi criterio**.

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di criterio | **Conservazione basata sul tempo**  |
    | Impostare il periodo di conservazione per | `180` giorni |

1. Seleziona **Salva**.

### Gestire i caricamenti dei BLOB

1. Tornare alla pagina contenitori, selezionare il **contenitore di dati** e quindi fare clic su **Carica**.

1. Nel pannello **Carica BLOB** espandere la **sezione Avanzate** .

    >**Nota**: individuare un file da caricare. Può trattarsi di qualsiasi tipo di file, ma un file di piccole dimensioni è ottimale. È possibile scaricare un file di esempio dalla directory AllFiles. 

    | Impostazione | Valore |
    | --- | --- |
    | Cercare i file | aggiungere il file selezionato per il caricamento |
    | Selezionare **Avanzato** | |
    | Tipo di BLOB | **BLOB in blocchi** |
    | Dimensioni blocco | **4 MiB** |
    | Livello di accesso | **Frequente**  (si notino le altre opzioni) |
    | Carica nella cartella | `securitytest` |
    | Ambito di crittografia | Usa l'ambito del contenitore predefinito esistente |

1. Fare clic su **Carica**.

1. Verificare di avere una nuova cartella e che il file sia stato caricato. 

1. Selezionare il file di caricamento ed esaminare le opzioni tra cui Download, Elimina, Cambia livello** e **Acquisisci lease**. **********

1. Copiare l'URL del file **e incollarlo in una nuova **finestra inprivate** browsing.**

1. Verrà visualizzato il messaggio in formato XML **ResourceNotFound**o **PublicAccessNotPermitted**.

    > **Nota**: questo comportamento è previsto, perché il livello di accesso pubblico del contenitore creato è impostato su **Privato (nessun accesso anonimo)**.

### Configurare l'accesso limitato all'archiviazione BLOB

1. Selezionare il file caricato e quindi nella **scheda Genera firma di accesso condiviso** . È anche possibile usare i puntini di sospensione (...) all'estrema destra. Specificare le impostazioni seguenti (lasciare gli altri con i valori predefiniti):

    | Impostazione | Valore |
    | --- | --- |
    | Chiave di firma | **Chiave 1** |
    | Autorizzazioni | **Lettura** (si notino le altre opzioni) |
    | Data di inizio | La data di ieri |
    | Ora di avvio | ora corrente |
    | Data di scadenza | La data di domani |
    | Ora di scadenza | ora corrente |
    | Indirizzi IP consentiti | lasciare vuoto |

1. Fare clic su **Genera URL e token di firma di accesso condiviso**.

1. Copiare la **voce DELL'URL** della firma di accesso condiviso BLOB negli Appunti.

1. Aprire un'altra finestra del browser InPrivate e passare all'URL di firma di accesso condiviso BLOB copiato nel passaggio precedente.

    >**Nota**: dovrebbe essere possibile visualizzare il contenuto del file. 

## Attività 3: Creare e configurare un'archiviazione file di Azure

In questa attività verranno create e configurate condivisioni file di Azure. Si userà Archiviazione Browser per gestire la condivisione file. 

### Creare la condivisione file e caricare un file

1. Nel portale di Azure tornare all'account di archiviazione, nella **sezione Archiviazione** dati fare clic su **Condivisioni** file.

1. Fare clic su **+ Condivisione** file e nella **scheda Informazioni di base** assegnare alla condivisione file un nome, `share1`. 

1. Si notino le **opzioni di livello** . Mantenere ottimizzata la transazione predefinita****.
   
1. Passare alla scheda Backup** e verificare che **l'opzione **Abilita backup** non** sia **selezionata. Il backup viene disabilitato per semplificare la configurazione del lab.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**. Attendere la distribuzione della condivisione file.

    ![Screenshot della pagina crea condivisione file.](../media/az104-lab07-create-share.png)

### Esplorare Archiviazione Browser e caricare un file

1. Tornare all'account di archiviazione e selezionare **Archiviazione Browser**. Il browser Archiviazione di Azure è uno strumento del portale che consente di visualizzare rapidamente tutti i servizi di archiviazione nell'account.

1. Selezionare **Condivisioni** file e verificare che la **directory share1** sia presente.

1. Selezionare la **directory share1** e notare che è possibile **+ Aggiungi directory**. In questo modo è possibile creare una struttura di cartelle.

1. Selezionare **Carica**. Passare a un file di propria scelta e quindi fare clic su **Carica**.

    >**Nota**: è possibile visualizzare le condivisioni file e gestirle nel browser Archiviazione. Attualmente non sono previste restrizioni.

### Limitare l'accesso alla rete all'account di archiviazione

1. Nel portale cercare e selezionare **Reti virtuali**.

1. Seleziona **+ Crea**. Selezionare il gruppo di risorse. e assegnare alla rete virtuale un **nome**, `vnet1`.

1. Accettare le impostazioni predefinite per altri parametri, selezionare **Rivedi e crea** e quindi **Crea**.

1. Attendere la distribuzione della rete virtuale e quindi selezionare **Vai alla risorsa**.

1. **Nella sezione Impostazioni** selezionare il **pannello Subnet**.
    + Selezionare la **subnet predefinita** .
    + **Nella sezione Endpoint servizio** scegliere **Microsoft.Archiviazione** nell'elenco **a discesa Servizi**.
    + Non apportare altre modifiche.    
    + Assicurarsi di **salvare** le modifiche. 

1. Tornare all'account di archiviazione.

1. **Nella sezione Sicurezza e rete** selezionare il **pannello Rete**.

1. Selezionare **Aggiungi rete** virtuale esistente e selezionare **vnet1** e **subnet predefinita** , selezionare **Aggiungi**.

1. Nella sezione **Firewall** eliminare** l'indirizzo **IP del computer. Il traffico consentito deve provenire solo dalla rete virtuale. 

1. Assicurarsi di **salvare** le modifiche.

    >**Nota:** l'account di archiviazione dovrebbe ora essere accessibile solo dalla rete virtuale appena creata. 

1. Selezionare il **browser Archiviazione e **Aggiornare** la** pagina. Passare alla condivisione file o al contenuto DEL BLOB.  

    >**Nota:** si dovrebbe ricevere un messaggio *non autorizzato a eseguire questa operazione*. Non ci si connette dalla rete virtuale. L'applicazione di questa operazione può richiedere alcuni minuti.


![Screenshot dell'accesso non autorizzato.](../media/az104-lab07-notauthorized.png)

## Pulire le risorse

Se si usa **la propria sottoscrizione** , è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.

## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Un account di archiviazione di Azure contiene tutti gli oggetti dati Archiviazione di Azure: BLOB, file, code e tabelle. L'account di archiviazione fornisce uno spazio dei nomi univoco per i dati di Archiviazione di Azure, accessibile da qualsiasi parte del mondo tramite HTTP o HTTPS.
+ Archiviazione di Azure offre diversi modelli di ridondanza, tra cui archiviazione con ridondanza locale, archiviazione con ridondanza della zona e archiviazione con ridondanza geografica. 
+ L'archiviazione BLOB di Azure consente di archiviare grandi quantità di dati non strutturati nella piattaforma di archiviazione dati di Microsoft. BLOB è l'acronimo di oggetto binario di grandi dimensioni, che include oggetti quali immagini e file multimediali.
+ Il file di Azure Archiviazione fornisce l'archiviazione condivisa per i dati strutturati. I dati possono essere organizzati in cartelle.
+ L'archiviazione non modificabile ti offre la possibilità di archiviare i dati in uno stato WORM (Write Once, Read Many). I criteri di archiviazione non modificabili possono essere basati sul tempo o con blocco legale.

## Altre informazioni con la formazione autogestita

+ [Ottimizzare i costi con Archiviazione BLOB di Azure](https://learn.microsoft.com/training/modules/optimize-your-cost-azure-blob-storage/). Informazioni su come ottimizzare i costi con Archiviazione BLOB di Azure.
+ [Controllare l'accesso alle Archiviazione di Azure con firme](https://learn.microsoft.com/training/modules/control-access-to-azure-storage-with-sas/) di accesso condiviso. Concedere l'accesso ai dati archiviati negli account di archiviazione di Azure in modo sicuro tramite l'uso di firme di accesso condiviso.
