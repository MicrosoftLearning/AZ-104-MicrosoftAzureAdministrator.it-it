---
lab:
  title: 02a - Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo
  module: Administer Governance and Compliance
---

# <a name="lab-02a---manage-subscriptions-and-rbac"></a>Lab 02a - Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-requirements"></a>Requisiti del lab

This lab requires permissions to create Azure Active Directory (Azure AD) users, create custom Azure Role Based Access Control (RBAC) roles, and assign these roles to Azure AD users. Not all lab hosters may provide this capability. Ask your instructor for the availability of this lab.

## <a name="lab-scenario"></a>Scenario del lab

Per migliorare la gestione delle risorse di Azure in Contoso, è stato chiesto di implementare le funzionalità seguenti:

- Creazione di un gruppo di gestione che includa tutte le sottoscrizioni di Azure di Contoso

- granting permissions to submit support requests for all subscriptions in the management group to a designated Azure Active Directory user. That user's permissions should be limited only to: 

    - Creazione dei ticket della richiesta di supporto
    - Visualizzazione dei gruppi di risorse 

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Implementare i gruppi di gestione
+ Attività 2: Creare ruoli Controllo degli accessi in base al ruolo personalizzati 
+ Attività 3: Assegnare ruoli Controllo degli accessi in base al ruolo


## <a name="estimated-timing-30-minutes"></a>Tempo stimato: 30 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab02a.png)


## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-implement-management-groups"></a>Attività 1: Implementare i gruppi di gestione

In questa attività si creeranno e configureranno gruppi di gestione. 

1. Accedere al [**portale di Azure**](http://portal.azure.com).

1. Cercare e selezionare **Gruppi di gestione** per passare al pannello **Gruppi di gestione**.

1. Review the messages at the top of the <bpt id="p1">**</bpt>Management groups<ept id="p1">**</ept> blade. If you are seeing the message stating <bpt id="p1">**</bpt>You are registered as a directory admin but do not have the necessary permissions to access the root management group<ept id="p1">**</ept>, perfom the following sequence of steps:

    1. Nel portale di Azure cercare e selezionare **Azure Active Directory**.
    
    1.  Nel pannello che visualizza le proprietà del tenant di Azure Active Directory selezionare **Proprietà** nella sezione **Gestisci** del menu verticale a sinistra.
    
    1.  Nel pannello **Proprietà** del tenant di Azure Active Directory, nella sezione **Gestione degli accessi per le risorse di Azure**, selezionare **Sì** e quindi **Salva**.
    
    1.  Tornare nel pannello **Gruppi di gestione** e selezionare **Aggiorna**.

1. Nel pannello **Gruppi di gestione** fare clic su **+ Crea**.

    >**Nota**: se in precedenza non sono stati creati gruppi di gestione, selezionare **Inizia a usare i gruppi di gestione**

1. Creare un gruppo di gestione con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | ID gruppo di gestione | **az104-02-mg1** |
    | Nome visualizzato del gruppo di gestione | **az104-02-mg1** |

1. Nell'elenco dei gruppi di gestione fare clic sulla voce che rappresenta il gruppo di gestione appena creato.

1. Nel pannello **az104-02-mg1** fare clic su **Sottoscrizioni**. 

1. Nel pannello **az104-02-mg1 \| Sottoscrizioni** fare clic su **+Aggiungi**, quindi nell'elenco a discesa **Sottoscrizione** del pannello **Aggiungi sottoscrizione** selezionare la sottoscrizione in uso in questo lab e fare clic su **Salva**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: On the <bpt id="p2">**</bpt>az104-02-mg1 <ph id="ph1">\|</ph> Subscriptions<ept id="p2">**</ept> blade, copy the ID of your Azure subscription into Clipboard. You will need it in the next task.

#### <a name="task-2-create-custom-rbac-roles"></a>Attività 2: Creare ruoli Controllo degli accessi in base al ruolo personalizzati

In questa attività si creerà una definizione di un ruolo Controllo degli accessi in base al ruolo personalizzato.

1. Nel computer del lab aprire il file **\\Allfiles\\Labs\\02\\ az104-02a-customRoleDefinition.json** nel Blocco note ed esaminarne il contenuto:

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```
    > **Nota**: se non si è certi della posizione in cui i file vengono archiviati in locale nell'ambiente lab, rivolgersi all'insegnante.

1. Sostituire il segnaposto `SUBSCRIPTION_ID` nel file JSON con l'ID sottoscrizione copiato negli Appunti e salvare la modifica.

1. Nel portale di Azure aprire il riquadro **Cloud Shell** facendo clic sull'icona della barra degli strumenti accanto alla casella di testo della ricerca.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**. 

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**. 

1. Sulla barra degli strumenti del riquadro Cloud Shell fare clic sull'icona **Carica/Scarica file**, nel menu a discesa fare clic su **Carica** e caricare il file **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** nella home directory di Cloud Shell.

1. Nel riquadro Cloud Shell eseguire il comando seguente per creare la definizione del ruolo personalizzato:

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. Chiudere il riquadro Cloud Shell.

#### <a name="task-3-assign-rbac-roles"></a>Attività 3: Assegnare ruoli Controllo degli accessi in base al ruolo

In questa attività si creerà un utente di Azure Active Directory a cui si assegnerà il ruolo Controllo degli accessi in base al ruolo creato nell'attività precedente, quindi si verificherà che l'utente possa eseguire l'attività specificata nella definizione del ruolo.

1. Nel portale di Azure cercare e selezionare **Azure Active Directory**, nel pannello Azure Active Directory fare clic su **Utenti**,e quindi su **+ Nuovo utente**.

1. Creare un nuovo utente con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome utente | **az104-02-aaduser1**|
    | Nome | **az104-02-aaduser1**|
    | Consenti la creazione manuale della password | Enabled |
    | Password iniziale | **Specificare una password sicura** |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User name<ept id="p3">**</ept>. You will need it later in this lab.

1. Nel portale di Azure tornare nel gruppo di gestione **az104-02-mg1** e visualizzarne i **dettagli**.

1. Per questo lab sono necessarie le autorizzazioni per creare utenti di Azure Active Directory (Azure AD), creare ruoli Controllo degli accessi in base al ruolo di Azure personalizzati e assegnarli a utenti di Azure AD. 

    >**Nota**: se il ruolo personalizzato non è visibile, possono essere necessari fino a 10 minuti perché il ruolo personalizzato venga visualizzato dopo la creazione.

1. Non tutti i provider di servizi di hosting possono fornire questa funzionalità.

1. Chiedere al docente informazioni sulla disponibilità di questo lab.

    >**Nota**: invece di digitare il nome utente, è possibile incollare il contenuto degli Appunti.

1. Nella finestra del browser **InPrivate** cercare e selezionare **Gruppi di risorse** nel portale di Azure per verificare che l'utente az104-02-aaduser1 possa visualizzare tutti i gruppi di risorse.

1. Nella finestra del browser **InPrivate** cercare e selezionare **Tutte le risorse** nel portale di Azure per verificare che l'utente az104-02-aaduser1 non possa visualizzare nessuna risorsa.

1. Nella finestra del browser **InPrivate**, nel portale di Azure, cercare e selezionare **Guida e supporto** e quindi fare clic su **+ Crea una richiesta di supporto**. 

1. In the <bpt id="p1">**</bpt>InPrivate<ept id="p1">**</ept> browser window, on the <bpt id="p2">**</bpt>Problem Desription/Summary<ept id="p2">**</ept> tab of the <bpt id="p3">**</bpt>Help + support - New support request<ept id="p3">**</ept> blade, type <bpt id="p4">**</bpt>Service and subscription limits<ept id="p4">**</ept> in the Summary field and select the <bpt id="p5">**</bpt>Service and subscription limits (quotas)<ept id="p5">**</ept> issue type. Note that the subscription you are using in this lab is listed in the <bpt id="p1">**</bpt>Subscription<ept id="p1">**</ept> drop-down list.

    >**Nota**: la presenza della sottoscrizione in uso in questo lab nell'elenco a discesa **Sottoscrizione** indica che l'account in uso ha le autorizzazioni necessarie per creare la richiesta di supporto specifica della sottoscrizione.

    >**Nota**: se l'opzione **Limiti del servizio e della sottoscrizione (quote)** non è visualizzata, disconnettersi dal portale di Azure e accedere di nuovo.

1. Do not continue with creating the support request. Instead, sign out as the az104-02-aaduser1 user from the Azure portal and close the InPrivate browser window.

#### <a name="task-4-clean-up-resources"></a>Attività 4: Eseguire la pulizia delle risorse

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges, although, resources created in this lab do not incur extra cost.

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going.

1. Nel portale di Azure cercare e selezionare **Azure Active Directory**, quindi nel pannello Azure Active Directory fare clic su **Utenti**.

1. Nel pannello **Utenti - Tutti gli utenti** fare clic su **az104-02-aaduser1**.

1. Nel pannello **az104-02-aaduser1 - Profilo** copiare il valore dell'attributo **ID oggetto**.

1. Nel portale di Azure avviare una sessione di **PowerShell** all'interno di **Cloud Shell**.

1. Nel riquadro Cloud Shell eseguire il comando seguente per rimuovere l'assegnazione della definizione del ruolo personalizzato (sostituire il segnaposto `[object_ID]` con il valore dell'attributo **ID oggetto** dell'account utente di Azure Active Directory **az104-02-aaduser1** copiato in precedenza in questa attività):

   ```powershell
   
   $scope = (Get-AzRoleDefinition -Name 'Support Request Contributor (Custom)').AssignableScopes[0]

   Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. Nel riquadro Cloud Shell eseguire il comando seguente per rimuovere la definizione del ruolo personalizzato:

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. Nel portale di Azure tornare nel pannello **Utenti - Tutti gli utenti** di **Azure Active Directory** ed eliminare l'account utente **az104-02-aaduser1**.

1. Nel portale di Azure tornare nel pannello **Gruppi di gestione**. 

1. Nel pannello **Gruppi di gestione** selezionare l'icona con i **puntini di sospensione** accanto alla sottoscrizione nel gruppo di gestione **az104-02-mg1** e selezionare **Sposta** per spostare la sottoscrizione nel **gruppo di gestione radice tenant**.

   >**Nota**: è probabile che il gruppo di gestione di destinazione sia il **gruppo di gestione radice tenant**, a meno che non sia stata creata una gerarchia di gruppi di gestione personalizzata prima di eseguire questo lab.
   
1. Selezionare **Aggiorna** per verificare che la sottoscrizione sia stata spostata correttamente nel **gruppo di gestione radice tenant**.

1. Tornare nel pannello **Gruppi di gestione**, fare clic sull'icona con i **puntini di sospensione** a destra del gruppo di gestione **az104-02-mg1** e fare clic su **Elimina**.
  >Concessione di autorizzazioni per l'invio di richieste di supporto per tutte le sottoscrizioni incluse nel gruppo di gestione a un utente di Azure Active Directory designato.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

- Implementazione di gruppi di gestione
- Creazione di ruoli Controllo degli accessi in base al ruolo personalizzati 
- Assegnazione di ruoli Controllo degli accessi in base al ruolo
