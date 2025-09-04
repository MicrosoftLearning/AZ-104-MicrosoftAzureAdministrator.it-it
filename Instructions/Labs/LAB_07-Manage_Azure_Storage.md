---
lab:
  title: 'Lab 07: Gestire Archiviazione di Azure'
  module: Administer Azure Storage
---

# Lab 07 - Gestire Archiviazione di Azure

## Introduzione al lab

In questo lab si apprenderà a creare account di archiviazione per BLOB di Azure e file di Azure. Si apprenderà a configurare e proteggere i contenitori BLOB. Si apprenderà anche a usare il browser archiviazione per configurare e proteggere le condivisioni file di Azure. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti orientali**.

## Tempo stimato: 50 minuti

## Scenario laboratorio

L'organizzazione al momento archivia i dati negli archivi dati locali. Alla maggior parte di questi file non si accede di frequente. È possibile ridurre al minimo i costi di archiviazione inserendo i file a cui si accede raramente in livelli di archiviazione a prezzi più bassi. Verranno anche esplorati diversi meccanismi di protezione offerti da Archiviazione di Azure, tra cui l'accesso alla rete, l'autenticazione, l'autorizzazione e la replica. Infine, è possibile determinare in quale misura File di Azure è adatto per ospitare le condivisioni file locali.

## Simulazioni interattive del lab

>**Nota**: le simulazioni lab fornite in precedenza sono state ritirate.

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab07-architecture.png)

## Competenze mansione

+ Attività 1: Creare e configurare un account di archiviazione. 
+ Attività 2: Creare e configurare l’archiviazione BLOB sicura.
+ Attività 3: Creare e configurare l'archiviazione file di Azure sicura.

## Attività 1: Creare e configurare un account di archiviazione. 

In questa attività verrà creato e configurato un account di archiviazione. L'account di archiviazione userà l'archiviazione con ridondanza geografica e non avrà accesso pubblico. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Storage accounts`, quindi cliccare su **+ Crea**.

1. Nella scheda **Informazioni di base** del pannello **Crea un account di archiviazione** specificare le impostazioni seguenti, mantenendo i valori predefiniti per le altre:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription          | Il nome della propria sottoscrizione di Azure  |
    | Gruppo di risorse        | **az104-rg7** (crea nuovo) |
    | Nome account di archiviazione  | Qualsiasi nome univoco globale composto da 3-24 lettere e numeri |
    | Paese                | **(Stati Uniti) Stati Uniti orientali**  |
    | Prestazioni           | **Standard** (si noti l'opzione Premium) |
    | Ridondanza            | **Archiviazione con ridondanza geografica** (si notino le altre opzioni)|
    | Selezionare l'accesso in lettura ai dati in caso di disponibilità a livello di area | Selezionare la casella |

    >**Suggerimenti utili** È consigliabile usare il livello di prestazioni Standard per la maggior parte delle applicazioni. Usare il livello di prestazioni Premium per applicazioni aziendali o ad alte prestazioni. 

1. Nella scheda **Avanzate** usare le icone informative per altre indicazioni sulle opzioni. Accettare i valori predefiniti. 

1. Nella sezione Accesso alla rete** pubblica della ****scheda Rete** selezionare **Disabilita**. Questo limiterà l'accesso in ingresso consentendo l'accesso in uscita. 

1. Esaminare la scheda **Protezione dati**. Si noti che 7 giorni è il criterio di conservazione predefinito per l'eliminazione temporanea. Si noti che è possibile abilitare il controllo delle versioni per i BLOB. Accettare i valori predefiniti.

1. Esaminare la scheda **Crittografia**. Si notino le opzioni di sicurezza aggiuntive. Accettare i valori predefiniti.

1. Selezionare **Rivedi e crea**, attendere il completamento del processo di convalida e quindi fare clic su **Crea**.

1. Dopo aver distribuito l'account di archiviazione, selezionare **Vai alla risorsa**.

1. Esaminare il pannello **Panoramica** e le configurazioni aggiuntive che è possibile modificare. Si tratta di impostazioni globali per l'account di archiviazione. Si noti che l'account di archiviazione può essere usato per contenitori BLOB, condivisioni file, code e tabelle.

1. Nel pannello **Sicurezza e rete** selezionare **Rete**. Si noti che **l'accesso** alla rete pubblica è disabilitato.

    + Selezionare **Gestisci** e modificare l'impostazione **Accesso alla rete** pubblica su **Abilitato**. 
    + Modificare l'ambito **di** accesso alla rete pubblica in **Abilita dalle reti** selezionate.
    + Nella sezione Indirizzi** IPv4 selezionare **Aggiungi l'indirizzo **** IPv4 del client.
    + Salva le modifiche.
  
1. Nel pannello **Gestione** dati selezionare **Ridondanza**. Si notino le informazioni sulle posizioni del data center primario e secondario.

1. Nel pannello **Gestione** dati selezionare **Gestione** del ciclo di vita e quindi selezionare **Aggiungi una regola**.

    + **Assegnare un nome** alla regola `Movetocool`. Si notino le opzioni per limitare l'ambito della regola. Fare clic su **Avanti**. 
    
    + Nella **pagina Aggiungi regola** , *se* i BLOB di base sono stati modificati più di `30` giorni fa *, passare* **all'archiviazione ad accesso sporadico**. Si notino le altre opzioni. 
    
    + Si noti che è possibile configurare altre condizioni. Selezionare **Aggiungi** al termine dell'esplorazione.

    ![Screenshot per passare alle condizioni delle regole ad accesso sporadico.](../media/az104-lab07-movetocool.png)

## Attività 2: Creare e configurare l’archiviazione BLOB sicura

In questa attività verrà creato un contenitore BLOB è verrà caricata un’immagine. I contenitori BLOB sono strutture simili alle directory, che archiviano dati non strutturati.

### Creare un contenitore BLOB e criteri di conservazione basati sul tempo

1. Passare al portale di Azure, usando l'account di archiviazione.

1. Nel pannello **Archiviazione dati** selezionare **Contenitori**. 

1. Fare clic su **+ Aggiungi contenitore** e **creare** un contenitore con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | `data`  |
    | Livello di accesso pubblico | Si noti che il livello di accesso è impostato su privato |

    ![Screenshot della creazione di un contenitore.](../media/az104-lab07-create-container.png)

1. Nel contenitore scorrere fino ai puntini di sospensione (...) all'estrema destra, selezionare **Criteri di accesso**.

1. Nell'area **Archiviazione BLOB non modificabile** selezionare **Aggiungi criterio**.

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di criterio | **Conservazione basata sulla durata**  |
    | Impostare periodo di conservazione per | `180` giorni |

1. Seleziona **Salva**.

### Gestire i caricamenti dei BLOB

1. Tornare alla pagina contenitori, selezionare il contenitore **dati**, quindi fare clic su **Carica**.

1. Nel pannello **Carica BLOB** espandere la sezione **Avanzate**.

    >**Nota**: Individuare un file da caricare. Può trattarsi di qualsiasi tipo di file, ma un file di piccole dimensioni è ottimale. È possibile scaricare un file di esempio dalla directory AllFiles. 

    | Impostazione | Valore |
    | --- | --- |
    | Cercare file | aggiungere il file selezionato per il caricamento |
    | Selezionare **Avanzato** | |
    | Tipo di BLOB | **BLOB in blocchi** |
    | Dimensioni blocco | **4 MiB** |
    | Livello di accesso | **Accesso frequente**  (si notino le altre opzioni) |
    | Carica nella cartella | `securitytest` |
    | Ambito di crittografia | Usa l'ambito del contenitore predefinito esistente |

1. Fare clic su **Carica**.

1. Verificare di avere una nuova cartella e che il file sia stato caricato. 

1. Selezionare il file di caricamento ed esaminare le opzioni con i puntini di sospensione (...) tra cui Download, Elimina, Cambia livello** e **Acquisisci lease**. **********

1. Copiare l'URL del file **(impostazioni --> pannello Proprietà) e incollarlo in una nuova **finestra di esplorazione inprivate**.**

1. Verrà visualizzato il messaggio in formato XML **ResourceNotFound**o **PublicAccessNotPermitted**.

    > **Nota**: questo comportamento è previsto, perché il livello di accesso pubblico del contenitore creato è impostato su **Privato (nessun accesso anonimo)**.

### Configurare l'accesso limitato all'archiviazione BLOB

1. Tornare al file caricato e selezionare i puntini di sospensione (...) all'estrema destra, quindi selezionare **Genera firma di accesso condiviso** e specificare le impostazioni seguenti (lasciare altri i valori predefiniti):

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

1. Copiare la voce **URL della firma di accesso condiviso BLOB** negli Appunti.

1. Aprire un'altra finestra del browser InPrivate e passare all'URL della firma di accesso condiviso BLOB copiato nel passaggio precedente.

    >**Nota**: Dovrebbe essere possibile visualizzare il contenuto del file. 

## Attività 3: Creare e configurare un'archiviazione file di Azure

In questa attività verranno create e configurate le condivisioni di File di Azure. Si userà il browser archiviazione per gestire la condivisione file. 

### Creare la condivisione file e caricare un file

1. Nel portale di Azure tornare all'account di archiviazione, nel pannello **Archiviazione** dati fare clic su **Condivisioni** file.

1. Fare clic su **+ Condivisione file** e nella scheda **Informazioni di base** assegnare alla condivisione file un nome, `share1`. 

1. Si notino le opzioni del **livello di accesso**. Mantenere l’opzione predefinita **Transazione ottimizzata**.
   
1. Passare alla scheda **Backup** e verificare che l'opzione **Abilita backup** **non** sia selezionata. Il backup viene disabilitato per semplificare la configurazione del lab.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**. Attendere la distribuzione della condivisione file.

    ![Screenshot della pagina di creazione della condivisione file.](../media/az104-lab07-create-share.png)

### Esplorare Il browser archiviazione e caricare un file

1. Tornare all'account di archiviazione e selezionare **Browser archiviazione**. Il browser archiviazione di Azure è uno strumento del portale che consente di visualizzare rapidamente tutti i servizi di archiviazione dell'account.

1. Selezionare **Condivisioni file** e verificare che la directory **condivisione1** sia presente.

1. Selezionare la directory **condivisione1** e notare che è possibile scegliere **+ Aggiungi directory**. Questo consente di creare una struttura di cartelle.

1. Selezionare **Carica**. Scegliere a un file a piacere, quindi cliccare su **Carica**.

    >**Nota**: È possibile visualizzare le condivisioni file e gestirle nel browser archiviazione. Attualmente non sono previste restrizioni.

### Limitare l'accesso alla rete all'account di archiviazione

1. Nel portale cercare e selezionare `Virtual networks`.

1. Seleziona **+ Crea**. Selezionare il gruppo di risorse. e assegnare alla rete virtuale un **nome**, `vnet1`.

1. Accettare le impostazioni predefinite per altri parametri, selezionare **Rivedi e crea**, quindi **Crea**.

1. Attendere che venga distribuita la rete virtuale e quindi selezionare **Vai alla risorsa**.

1. Nella sezione **Impostazioni** selezionare il pannello **Endpoint di servizio**.
    + Selezionare **Aggiungi**. 
    + Nell'elenco a discesa **Servizi** selezionare **Microsoft.Storage**.
    + Nell'elenco a discesa **Subnet** selezionare la subnet **Predefinita**.
    + Fare clic su **Aggiungi** per salvare le modifiche.  

1. Tornare all'account di archiviazione.

1. Nel pannello **Sicurezza e rete** selezionare **Rete**.

1. In **Accesso alla rete** pubblica selezionare **Gestisci**. 

1. Selezionare **Aggiungi una rete** virtuale e quindi **Aggiungi rete** esistente.

1. Selezionare vnet1** e **subnet predefinita**, selezionare **Aggiungi**.**

1. Nella sezione **Indirizzi** IPv4 eliminare** l'indirizzo **IP del computer. Il traffico consentito deve provenire solo dalla rete virtuale. 

1. Assicurarsi di **salvare** le modifiche.

    >**Nota:** L'account di archiviazione ora dovrebbe essere accessibile solo dalla rete virtuale appena creata. 

1. Selezionare il **browser archiviazione** e **aggiornare** la pagina. Passare alla condivisione file o al contenuto del BLOB.  

    >**Nota:** Dovrebbe essere visualizzato un messaggio *non autorizzato a eseguire questa operazione*. Non ci si connette dalla rete virtuale. L'applicazione di questa operazione può richiedere alcuni minuti. È comunque possibile visualizzare la condivisione file, ma non i file o i BLOB nell'account di archiviazione. 


![Screenshot dell'accesso non autorizzato.](../media/az104-lab07-notauthorized.png)

## Pulire le risorse

Se si usa la **sottoscrizione personale**, dedicare qualche minuto all’eliminazione delle risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse lab. 

+ Nel portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse**, **Immettere il nome del gruppo di risorse**, quindi fare clic su **Elimina**.
+ Tramite Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usando l’interfaccia della riga di comando, `az group delete --name resourceGroupName`.

## Estendere l'apprendimento con Copilot
Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a *copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.

+ Fornire uno script di Azure PowerShell per creare un account di archiviazione con un contenitore BLOB. 
+ Fornire una checklist da seguire per assicurarsi che l'account di archiviazione di Azure sia sicuro.
+ Creare una tabella per confrontare i modelli di ridondanza di Archiviazione di Azure.

## Altre informazioni con la formazione autogestita

+ [Creare un account di archiviazione di Azure](https://learn.microsoft.com/training/modules/create-azure-storage-account/). Creare un account di Archiviazione di Azure con le opzioni più adatte alle esigenze aziendali.
+ [Gestire il ciclo di vita](https://learn.microsoft.com/training/modules/manage-azure-blob-storage-lifecycle) dell'archiviazione BLOB di Azure. Informazioni su come gestire la disponibilità dei dati durante il ciclo di vita dell'archiviazione BLOB di Azure.

## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab. 

+ Un account di archiviazione di Azure contiene tutti gli oggetti dati di Archiviazione di Azure: BLOB, file, code e tabelle. L'account di archiviazione fornisce uno spazio dei nomi univoco per i dati di Archiviazione di Azure, accessibile da qualsiasi parte del mondo tramite HTTP o HTTPS.
+ Archiviazione di Azure offre diversi modelli di ridondanza, tra cui archiviazione con ridondanza locale, archiviazione con ridondanza della zona e archiviazione con ridondanza geografica. 
+ L'archiviazione BLOB di Azure consente di archiviare grandi quantità di dati non strutturati nella piattaforma di archiviazione dati di Microsoft. BLOB è l'acronimo di oggetto binario di grandi dimensioni, che include oggetti quali immagini e file multimediali.
+ Archiviazione file di Azure fornisce l'archiviazione condivisa per i dati strutturati. I dati possono essere organizzati in cartelle.
+ L'archiviazione non modificabile ti offre la possibilità di archiviare i dati in uno stato WORM (Write Once, Read Many). I criteri di archiviazione non modificabile possono essere basati sul tempo o con blocco legale.
