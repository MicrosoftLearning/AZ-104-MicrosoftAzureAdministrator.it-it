---
lab:
  title: 'Lab 02a: Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo'
  module: Administer Governance and Compliance
---

# Lab 02a - Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo

## Introduzione al lab

In questo lab vengono fornite informazioni sul controllo degli accessi in base al ruolo. Si apprenderà come usare autorizzazioni e ambiti per controllare le azioni che le identità possono e non possono eseguire. Si apprenderà anche come semplificare la gestione delle sottoscrizioni usando i gruppi di gestione. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali. 

## Tempo stimato: 30 minuti

## Scenario laboratorio

Per semplificare la gestione delle risorse di Azure nell'organizzazione, è stato richiesto di implementare le funzionalità seguenti:

- Creazione di un gruppo di gestione che include tutte le sottoscrizioni di Azure.

- Concessione delle autorizzazioni per inviare richieste di supporto per tutte le sottoscrizioni nel gruppo di gestione a un utente designato. Le autorizzazioni dell'utente devono essere limitate solo a: 

    - Creazione di ticket di richiesta di supporto
    - Visualizzazione dei gruppi di risorse

## Scenari di lab interattivi

Esistono alcune simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Gestire l'accesso con il controllo degli accessi in base al ruolo](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2014). Assegnare un ruolo predefinito a un utente e monitorare i log attività. 

+ [Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202). Implementare un gruppo di gestione e creare e assegnare un ruolo controllo degli accessi in base al ruolo personalizzato.

+ [Aprire una richiesta](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2022) di supporto. Esaminare le opzioni del piano di supporto, quindi creare e monitorare una richiesta di supporto, una fatturazione o tecnica.

## Diagramma dell'architettura

![Diagramma delle attività del lab.](../media/az104-lab02a-architecture.png)

## Attività

+ Attività 1: Implementare i gruppi di gestione.
+ Attività 2: Esaminare e assegnare un ruolo di Azure predefinito.
+ Attività 3: Creare un ruolo controllo degli accessi in base al ruolo personalizzato per il personale help desk.
+ Attività 4: Testare il ruolo personalizzato per assicurarsi che disponga delle autorizzazioni corrette
+ Attività 5: Monitorare le assegnazioni di ruolo con il log attività.

## Attività 1: Implementare i gruppi di gestione

In questa attività si creeranno e configureranno gruppi di gestione. I gruppi di gestione vengono usati per organizzare logicamente le sottoscrizioni. Le sottoscrizioni devono essere segmentate e consentire il controllo degli accessi in base al ruolo e Criteri di Azure essere assegnate e ereditate ad altri gruppi di gestione e sottoscrizioni. Ad esempio, se l'organizzazione ha un team di supporto dedicato per l'Europa, è possibile organizzare le sottoscrizioni europee in un gruppo di gestione per fornire al personale di supporto l'accesso a tali sottoscrizioni (senza fornire l'accesso individuale a tutte le sottoscrizioni). Nello scenario tutti gli utenti dell'Help Desk dovranno creare una richiesta di supporto in tutte le sottoscrizioni. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Management groups`.

1. Esaminare i messaggi nella parte superiore del pannello **Gruppi di gestione**. Se viene visualizzato il messaggio Che indica **che l'utente è registrato come amministratore della directory ma non dispone delle autorizzazioni necessarie per accedere al gruppo** di gestione radice, seguire questa sequenza di passaggi:

    + Nella portale di Azure cercare e selezionare **Microsoft Entra ID**.
    
    + Nel pannello che visualizza le proprietà del tenant, nel menu verticale a sinistra selezionare Proprietà nella **sezione Gestisci**.****
    
    + Nel pannello **Proprietà** del tenant, nella **sezione Gestione degli accessi per le risorse** di Azure selezionare **Sì** e quindi selezionare **Salva**.
    
    + Tornare al pannello **Gruppi** di gestione e selezionare **Aggiorna**.

1. Nel pannello **Gruppi di gestione** fare clic su **+ Crea**.

1. Creare un gruppo di gestione con le impostazioni seguenti. Al termine, selezionare **Invia** . 

    | Impostazione | Valore |
    | --- | --- |
    | ID gruppo di gestione | `az104-mg1` |
    | Nome visualizzato del gruppo di gestione | `az104-mg1` |

1. In questo scenario, tutte le sottoscrizioni verranno ora aggiunte al gruppo di gestione. Il controllo degli accessi in base al ruolo verrà quindi applicato al gruppo di gestione.

1. **Aggiornare** la pagina del gruppo di gestione fino a visualizzare il nuovo gruppo di gestione. 

   >**Nota:** si è notato il gruppo di gestione radice? Tutti i gruppi di gestione e le sottoscrizioni fanno parte del gruppo di gestione radice.

## Attività 2: Esaminare e assegnare un ruolo di Azure predefinito

In questa attività si esamineranno i ruoli predefiniti e si assegnerà il ruolo Collaboratore macchina virtuale all'account utente. Azure offre un numero elevato di [ruoli](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles) predefiniti.

1. Selezionare il **gruppo di gestione az104-mg1** .

1. Selezionare il **pannello Controllo di accesso (IAM)** e quindi la **scheda Ruoli** .

   >**Nota:** si notino le altre opzioni disponibili per **Controllare l'accesso**, **l'assegnazione** di ruolo e **le assegnazioni** Nega. 

1. Scorrere le definizioni di ruolo predefinite disponibili. **Visualizzare** un ruolo per ottenere informazioni dettagliate su **Autorizzazioni**, **JSON** e **Assegnazioni**. 

1. Selezionare **+ Aggiungi** dal menu a discesa, selezionare **Aggiungi assegnazione di** ruolo. 

1. Nel pannello **Aggiungi assegnazione di** ruolo cercare e selezionare Collaboratore **** macchina virtuale. Il ruolo collaboratore macchina virtuale consente di gestire le macchine virtuali, ma non di accedere al sistema operativo o di gestire la rete virtuale e l'account di archiviazione a cui sono connessi. Seleziona **Avanti**. 

1. Nella **scheda **Membri** selezionare Membri**.

1. Cercare e selezionare *l'account utente. Le informazioni sull'account utente vengono visualizzate nell'angolo superiore destro del portale. Fare clic su **Seleziona**. 

1. Fare clic su **Verifica e assegna** due volte per creare l'assegnazione di ruolo.

1. Tornare al gruppo di gestione. Seleziona **Controllo di accesso (IAM)**. Nella **scheda Assegnazioni** di ruolo verificare di avere il **ruolo Collaboratore** macchina virtuale. 

    >**Nota:** questa assegnazione potrebbe non concedere in realtà altri provilegi. Se si ha già il ruolo Proprietario, questo ruolo include tutti i privilegi associati al ruolo Collaboratore macchina virtuale.
    >
    >**Nota:** questa attività illustra come assegnare un ruolo predefinito.  Come procedura consigliata, assegnare sempre ruoli a gruppi non singoli. 


## Attività 3: Creare un ruolo controllo degli accessi in base al ruolo personalizzato per il personale help desk

In questa attività verrà creato un ruolo controllo degli accessi in base al ruolo personalizzato. I ruoli personalizzati sono una parte fondamentale dell'implementazione del principio dei privilegi minimi per un ambiente. I ruoli predefiniti potrebbero avere troppe autorizzazioni per l'organizzazione. In questa attività verrà creato un nuovo ruolo e verranno rimosse le autorizzazioni non necessarie.

1. Continuare a lavorare sul gruppo di gestione. Nel pannello **Controllo di accesso (IAM)** selezionare la **scheda Controlla accesso** .

1. **Nella casella Crea un ruolo** personalizzato selezionare **Aggiungi**.

1. Nella scheda Informazioni di base di **Crea un ruolo** personalizzato specificare il nome `Custom Support Request`. Nel campo Descrizione immettere `A custom contributor role for support requests.` 

1. Per **Autorizzazioni** di base selezionare **Clona un ruolo**. **Nel menu a discesa Role to clone (Ruolo da clonare**) selezionare Support Request Contributor (Collaboratore **** richiesta di supporto).

    ![Screenshot che clona un ruolo.](../media/az104-lab02a-clone-role.png)

1. Selezionare **Avanti** per passare alla **scheda Autorizzazioni** e quindi selezionare **+ Escludi autorizzazioni**.

1. Nel campo di ricerca del provider di risorse immettere `.Support` e selezionare **Microsoft.Support**.

1. Nell'elenco delle autorizzazioni posizionare una casella di controllo accanto a **Altro: Registra il provider** di risorse di supporto e quindi selezionare **Aggiungi**. Il ruolo deve essere aggiornato per includere questa autorizzazione come *NotAction*.

    >**Nota:** un provider di risorse di Azure è un set di operazioni REST che abilitano la funzionalità per un servizio di Azure specifico. Non vogliamo che l'Help Desk sia in grado di avere questa funzionalità, quindi viene rimossa dal ruolo clonato. 

1. Selezionare **+ Aggiungi ambiti** assegnabili. Selezionare il **gruppo di gestione az104-mg1** , quindi fare clic su **Avanti**.

1. Esaminare il codice JSON per Azioni**, *NotActions* e *AssignableScopes* personalizzati nel ruolo. 

1. Selezionare **Rivedi e crea** e quindi **Crea**.

    >**Nota:** a questo punto è stato creato un ruolo personalizzato. Il passaggio successivo consiste nell'assegnare il ruolo a un help desk. Prima di eseguire questa operazione, verrà testato un utente. 

## Attività 4: Assegnare e testare il ruolo controllo degli accessi in base al ruolo personalizzato.

In questa attività si aggiunge il ruolo personalizzato a un utente di test e si confermano le relative autorizzazioni. 

1. Nella portale di Azure cercare e selezionare **Microsoft Entra ID** e quindi selezionare il pannello **Utenti**.

    >**Nota**: questa attività richiede un account utente per il test. Per questo lab si userà helpdesk-user1****. Se necessario, è possibile **aggiungere** un nuovo utente. Se si sta creando un nuovo utente, richiedere che la password venga impostata al momento dell'accesso. 

1. Prima di continuare, assicurarsi di avere il nome** dell'entità **utente utente per l'account utente di test. Sarà necessario eseguire l'accesso al portale. È possibile copiare l'UPN negli Appunti. 

1. Nella portale di Azure tornare al **gruppo di gestione az104-mg1**.

1. Fare clic su **Controllo di accesso (IAM)**, fare clic su **+ Aggiungi** e quindi su **Aggiungi assegnazione di ruolo**. 

1. Nella **scheda Ruolo** cercare `Custom Support Request`. 

    >**Nota**: se il ruolo personalizzato non è visibile, possono essere necessari fino a 5 minuti prima che il ruolo personalizzato venga visualizzato dopo la creazione. **Aggiornare** la pagina. 

1. Selezionare il **Ruolo** e fare clic su **Avanti**. Nella **scheda Membri** fare clic su **+ Seleziona membri** e **selezionare** user-user-user-helpdesk account **utente1**.  

1. Selezionare **Rivedi e assegna** due volte.

    >**Nota:** a questo punto, si ha un account utente help desk con privilegi personalizzati per creare un ticket di supporto. Il passaggio successivo consiste nel testare l'account.
    
1. Aprire una **finestra del browser InPrivate** e passare al portale di Azure all'indirizzo `https://portal.azure.com`.

1. Specificare il nome del principio utente per helpdesk-user1. Quando viene richiesto di aggiornare la password, cambiare la password per l'utente.

1. Nella finestra del **browser InPrivate**, nella portale di Azure, cercare e selezionare Gruppi** di risorse per verificare che l'utente dell'Help Desk possa visualizzare **i gruppi di risorse.

1. Nella finestra del **browser InPrivate**, nella portale di Azure, cercare e selezionare **Tutte le risorse** per verificare che l'utente help desk non possa visualizzare singole risorse.

1. Nella finestra del browser **InPrivate**, nel portale di Azure, cercare e selezionare **Guida e supporto** e quindi fare clic su **+ Crea una richiesta di supporto**. 

    >**Nota**: molte organizzazioni scelgono di fornire a tutti gli amministratori cloud l'accesso ai casi di supporto aperti. In questo modo gli amministratori possono risolvere i casi di supporto più velocemente.

1. Per **Tipo di** problema selezionare **Limiti** di servizio e sottoscrizione. Si notino le altre opzioni.

1. nel campo Riepilogo e selezionare il **tipo di problema Limiti di servizio e sottoscrizione (quote).** Seleziona **Avanti**.

    >**Nota**: poiché il ruolo è stato assegnato al gruppo di gestione, tutte le sottoscrizioni devono essere disponibili per l'Help Desk. Se non viene visualizzata l'opzione **Limiti di servizio e sottoscrizione (quote),** disconnettersi dal portale di Azure e accedere di nuovo.

1. Dedicare alcuni minuti all'esplorazione della creazione di una **nuova richiesta** di supporto, ma non continuare con la creazione della richiesta di supporto. Disconnettersi invece come utente help desk dal portale di Azure e chiudere la finestra del browser InPrivate.

    >**Nota:** è stato verificato che un utente dell'Help Desk disponga delle autorizzazioni corrette.

## Attività 5: Monitorare le assegnazioni di ruolo con il log attività

In questa attività viene visualizzato il log attività per determinare se qualcuno ha creato un nuovo ruolo. 

1. Tornare al portale e nella **risorsa az104-mg1** selezionare **Log** attività.

2. Selezionare Aggiungi filtro **, selezionare **** Operazione** e quindi **Crea assegnazione di** ruolo.

    ![Screenshot della pagina Log attività con il filtro configurato.](../media/az104-lab02a-searchactivitylog.png)

3. Verificare che il log attività mostri le attività di creazione dei ruoli. 

## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ I gruppi di gestione vengono usati per organizzare logicamente le sottoscrizioni.
+ Azure ha molti ruoli predefiniti. È possibile assegnare questi ruoli per controllare l'accesso alle risorse.
+ È possibile creare nuovi ruoli o personalizzare i ruoli esistenti.
+ I ruoli vengono definiti in un file in formato JSON e includono *Actions*, *NotActions* e *AssignableScopes*.
+ È possibile usare il log attività per monitorare le assegnazioni di ruolo. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.


