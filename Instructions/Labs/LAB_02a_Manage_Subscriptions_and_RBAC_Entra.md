---
lab:
  title: 'Lab 02a: Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo'
  module: Administer Governance and Compliance
---

# Lab 02a - Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo

## Introduzione al lab

In questo lab vengono fornite informazioni sul controllo degli accessi in base al ruolo. Si apprenderà come usare autorizzazioni e ambiti per controllare le azioni che le identità possono e non possono eseguire. Si apprenderà anche come semplificare la gestione delle sottoscrizioni usando i gruppi di gestione. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti orientali**. 

## Tempo stimato: 30 minuti

## Scenario laboratorio

Per semplificare la gestione delle risorse di Azure nell'organizzazione, è stato richiesto di implementare le funzionalità seguenti:

- Creazione di un gruppo di gestione che include tutte le sottoscrizioni di Azure.

- Concessione delle autorizzazioni per inviare richieste di supporto per tutte le sottoscrizioni nel gruppo di gestione. Le autorizzazioni devono essere limitate solo a: 

    - Creare e gestire macchine virtuali
    - Creare ticket di richiesta di supporto (non includere l'aggiunta di provider di Azure)


## Simulazioni interattive del lab

>**Nota**: le simulazioni lab fornite in precedenza sono state ritirate.

## Diagramma dell'architettura

![Diagramma delle attività del lab.](../media/az104-lab02a-architecture.png)

## Competenze mansione

+ Attività 1: Implementare i gruppi di gestione.
+ Attività 2. Esaminare e assegnare un ruolo di Azure predefinito.
+ Attività 3. Creare un ruolo Controllo degli accessi in base al ruolo personalizzato.
+ Attività 4: Monitorare le assegnazioni di ruolo con il log attività.

## Attività 1: Implementare i gruppi di gestione

In questa attività si creeranno e configureranno gruppi di gestione. I gruppi di gestione vengono usati per organizzare e segmentare in modo logico le sottoscrizioni. Consentono di assegnare e ereditare il controllo degli accessi in base al ruolo e le Criteri di Azure ad altri gruppi di gestione e sottoscrizioni. Ad esempio, se l'organizzazione ha un team di supporto dedicato per l'Europa, è possibile organizzare le sottoscrizioni europee in un gruppo di gestione per fornire al personale di supporto l'accesso a tali sottoscrizioni (senza fornire l'accesso individuale a tutte le sottoscrizioni). Nello scenario tutti gli utenti dell'Help Desk dovranno creare una richiesta di supporto in tutte le sottoscrizioni. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Microsoft Entra ID`.

1. Nel pannello **Gestisci** selezionare **Proprietà**.

1. Esaminare l'area **Gestione degli accessi per le risorse di Azure**. Assicurarsi di poter gestire l'accesso a tutte le sottoscrizioni e i gruppi di gestione di Azure nel tenant.
   
1. Cercare e selezionare `Management groups`.

1. Nel pannello **Gruppi di gestione** fare clic su **+ Crea**.

1. Creare un gruppo di gestione con le impostazioni seguenti. Al termine, selezionare **Invia**. 

    | Impostazione | Valore |
    | --- | --- |
    | ID gruppo di gestione | `az104-mg1` (deve essere univoco nella directory) |
    | Nome visualizzato del gruppo di gestione | `az104-mg1` |

1. **Aggiornare** la pagina del gruppo di gestione per assicurarsi che venga visualizzato il nuovo gruppo di gestione. L'operazione potrebbe richiedere alcuni minuti. 

   >**Nota:** Si è notato il gruppo di gestione radice? Il gruppo di gestione radice è integrato nella gerarchia in modo che tutti i gruppi di gestione e le sottoscrizioni vi possano confluire. Il gruppo di gestione radice permette l'applicazione di criteri globali e assegnazioni di ruolo di Azure a livello di directory. Dopo aver creato un gruppo di gestione, aggiungere tutte le sottoscrizioni che devono essere incluse nel gruppo. 

## Attività 2: Esaminare e assegnare un ruolo predefinito di Azure

In questa attività si esamineranno i ruoli predefiniti e si assegnerà il ruolo Collaboratore macchina virtuale a un membro dell'Help Desk. Azure offre un numero elevato di [ruoli predefiniti](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles). 

1. Selezionare il gruppo di gestione **az104-mg1**.

1. Selezionare il pannello **Controllo di accesso (IAM)** e quindi la scheda **Ruoli**.

1. Scorrere le definizioni di ruolo predefinite disponibili. **Visualizzare** un ruolo per ottenere informazioni dettagliate su **Autorizzazioni**, **JSON** e **Assegnazioni**. Spesso si useranno *proprietario*, *collaboratore* e *lettore*. 

1. Selezionare **+ Aggiungi** dal menu a discesa, selezionare **Aggiungi assegnazione di ruolo**. 

1. Nel pannello **Aggiungi assegnazione di ruolo**, cercare e selezionare **Collaboratore macchina virtuale**. il ruolo Collaboratore Macchina virtuale consente di gestire le macchine virtuali, ma non di accedere al relativo sistema operativo né di gestire la rete virtuale e l'account di archiviazione a cui sono connesse. Questo è un buon ruolo per l'Help Desk. Selezionare **Avanti**.

    >**Suggerimenti utili** Azure ha originariamente fornito solo il modello di distribuzione **classico**. Questa operazione è stata sostituita dal modello di distribuzione **Azure Resource Manager**. Come procedura consigliata, non usare le risorse classiche. 

1. Nella scheda **Membri**, **Seleziona membri**.

    >**Nota:** Il passaggio successivo assegna il ruolo al gruppo **helpdesk**. Se non si ha un gruppo help desk, è necessario prendersi un minuto per crearlo.

1. Cercare e selezionare il `helpdesk` gruppo. Fare clic su **Seleziona**. 

1. Fare clic su **Verifica e assegna** due volte per creare l'assegnazione di ruolo.

1. Continuare nel pannello **Controllo di accesso (IAM)**. Nella scheda **Assegnazioni di ruolo** verificare che il gruppo**helpdesk ** abbia il ruolo di **collaboratore macchina virtuale**. 

    >**Nota:** Come procedura consigliata, assegnare sempre ruoli a gruppi non singoli. 

    >**Suggerimenti utili** Questa assegnazione potrebbe non concedere privilegi aggiuntivi. Se si ha già il ruolo Proprietario, tale ruolo include tutte le autorizzazioni associate al ruolo Collaboratore macchina virtuale.
    
## Attività 3: Creare un ruolo controllo degli accessi in base al ruolo personalizzato

In questa attività verrà creato un ruolo controllo degli accessi in base al ruolo personalizzato. I ruoli personalizzati sono una parte fondamentale dell'implementazione del principio dei privilegi minimi per un ambiente. I ruoli predefiniti potrebbero avere troppe autorizzazioni per lo scenario. Verrà anche creato un nuovo ruolo e verranno rimosse le autorizzazioni non necessarie. Si prevede di gestire le autorizzazioni sovrapposte?

1. Continuare a lavorare sul gruppo di gestione. Passare al pannello **Controllo di accesso (IAM).**

1. Selezionare **+ Aggiungi** dal menu a discesa, selezionare **Aggiungi ruolo** personalizzato.

1. Nella scheda Informazioni di base completare la configurazione.

    | Impostazione | Valore |
    | --- | --- |
    | Nome del ruolo personalizzato | `Custom Support Request` |
    | Descrizione | `A custom contributor role for support requests.` |

1. Per **Autorizzazioni di base** selezionare **Clona un ruolo**. Nel menu a discesa **Ruolo da clonare** selezionare **Collaboratore richiesta di supporto**.

    ![Schermata di clonazione di un ruolo.](../media/az104-lab02a-clone-role.png)

1. Selezionare **Avanti** per passare alla scheda **Autorizzazioni** e quindi selezionare **+ Escludi autorizzazioni**.

1. Nel campo di ricerca del provider di risorse immettere `.Support` e selezionare **Microsoft.Support**.

1. Nell'elenco delle autorizzazioni posizionare una casella di controllo accanto a **Altro: Registra il provider di risorse di supporto** e quindi selezionare **Aggiungi**. Il ruolo deve essere aggiornato per includere questa autorizzazione come *NotAction*.

    >**Nota:** Un provider di risorse di Azure è un set di operazioni REST che consentono la funzionalità per un servizio di Azure specifico. Non vogliamo che l'Help Desk sia in grado di avere questa funzionalità, quindi viene rimossa dal ruolo clonato. 

1. Nella scheda **Ambiti assegnabili** verificare che il gruppo di gestione sia elencato e quindi fare clic su **Avanti**.

1. Esaminare il codice JSON per *Actions*, *NotActions* e *AssignableScopes* personalizzati nel ruolo. 

1. Selezionare **Rivedi e crea** e quindi **Crea**.

    >**Nota:** A questo punto, è stato creato un ruolo personalizzato e assegnato al gruppo di gestione.  

## Attività 4: Monitorare le assegnazioni di ruolo con il log attività

In questa attività viene visualizzato il log attività per determinare se qualcuno ha creato un nuovo ruolo. 

1. Nel portale individuare la risorsa **az104-mg1** e selezionare **Log attività**. Il log attività offre informazioni dettagliate relative agli eventi a livello di sottoscrizione. 

1. Esaminare le attività per le assegnazioni di ruolo. Il log attività può essere filtrato per operazioni specifiche. 

    ![Screenshot della pagina Log attività con il filtro configurato.](../media/az104-lab02a-searchactivitylog.png)

## Pulire le risorse

Se si usa la **sottoscrizione personale**, dedicare qualche minuto all’eliminazione delle risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse lab. 

+ Nel portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse**, **Immettere il nome del gruppo di risorse**, quindi fare clic su **Elimina**.
+ Tramite Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usando l’interfaccia della riga di comando, `az group delete --name resourceGroupName`.
  
## Estendere l'apprendimento con Copilot

Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a *copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.
+ Creare due tabelle che evidenziano comandi importanti di PowerShell e dell'interfaccia della riga di comando per ottenere informazioni sulle sottoscrizioni dell'organizzazione in Azure e spiegare ogni comando nella colonna "Spiegazione". 
+ Qual è il formato del file JSON del controllo degli accessi in base al ruolo di Azure?
+ Quali sono i passaggi di base per la creazione di un ruolo controllo degli accessi in base al ruolo di Azure personalizzato?
+ Qual è la differenza tra i ruoli controllo degli accessi in base al ruolo di Azure e i ruoli Microsoft Entra ID?

## Altre informazioni con la formazione autogestita

+ [Proteggere le risorse di Azure con il controllo degli accessi in base al ruolo di Azure](https://learn.microsoft.com/training/modules/secure-azure-resources-with-rbac/). Uso del controllo degli accessi in base al ruolo di Azure per gestire l'accesso alle risorse in Azure.
+ [Creare ruoli personalizzati per le risorse di Azure con il controllo degli accessi in base al ruolo (RBAC)](https://learn.microsoft.com/training/modules/create-custom-azure-roles-with-rbac/). Informazioni sulla struttura delle definizioni di ruolo per il controllo di accesso. Identificare le proprietà dei ruoli da usare per definire le autorizzazioni del ruolo personalizzato. Creare un ruolo personalizzato di Azure e assegnarlo a un utente.

## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab. 

+ I gruppi di gestione vengono usati per organizzare logicamente le sottoscrizioni.
+ Il gruppo di gestione radice predefinito include tutti i gruppi di gestione e le sottoscrizioni.
+ Azure ha molti ruoli predefiniti. È possibile assegnare questi ruoli per controllare l'accesso alle risorse.
+ È possibile creare nuovi ruoli o personalizzare i ruoli esistenti.
+ I ruoli vengono definiti in un file in formato JSON e includono *Actions*, *NotActions* e *AssignableScopes*.
+ È possibile usare il log attività per monitorare le assegnazioni di ruolo.




