---
lab:
  title: 'Lab 02a: Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo (test di rebranding)'
  module: Administer Governance and Compliance
---

# Lab 02a - Gestire le sottoscrizioni e il controllo degli accessi in base al ruolo
# Manuale del lab per studenti

## Requisiti del lab

Questo lab richiede autorizzazioni per creare utenti, creare ruoli personalizzati basati sul ruolo di Azure Controllo di accesso (RBAC) e assegnare questi ruoli agli utenti. Non tutti i provider di servizi di hosting possono fornire questa funzionalità. Chiedere al docente informazioni sulla disponibilità di questo lab.

## Scenario del lab

Per migliorare la gestione delle risorse di Azure in Contoso, è stato chiesto di implementare le funzionalità seguenti:

- Creazione di un gruppo di gestione che includa tutte le sottoscrizioni di Azure di Contoso

- concessione delle autorizzazioni per inviare richieste di supporto per tutte le sottoscrizioni nel gruppo di gestione a un utente designato. Le autorizzazioni dell'utente devono essere limitate solo a: 

    - Creazione dei ticket della richiesta di supporto
    - Visualizzazione dei gruppi di risorse

                **Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi.

## Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Implementare i gruppi di gestione
+ Attività 2: Creare ruoli Controllo degli accessi in base al ruolo personalizzati 
+ Attività 3: Assegnare ruoli Controllo degli accessi in base al ruolo


## Tempo stimato: 30 minuti

## Diagramma dell'architettura

![image](../media/lab02aentra.png)


### Istruzioni

## Esercizio 1

## Attività 1: Implementare i gruppi di gestione

In questa attività si creeranno e configureranno gruppi di gestione. 

1. Accedere al [**portale di Azure**](http://portal.azure.com).

1. Cercare e selezionare **Gruppi di gestione** per passare al pannello **Gruppi di gestione**.

1. Esaminare i messaggi nella parte superiore del pannello **Gruppi di gestione**. Se viene visualizzato il messaggio **Si è registrati come amministratore della directory ma non sono disponibili le autorizzazioni necessarie per accedere al gruppo di gestione radice**, eseguire questa sequenza di passaggi:

    1. Nella portale di Azure cercare e selezionare **Microsoft Entra ID**.
    
    1.  Nel pannello che visualizza le proprietà del tenant, nel menu verticale sul lato sinistro selezionare **Proprietà**.****
    
    1.  Nel pannello **Proprietà** del tenant selezionare **Sì** nella sezione **Gestione di accesso per le risorse di Azure** e quindi selezionare **Salva**.
    
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

    >**Nota**: nel pannello **az104-02-mg1 \| Sottoscrizioni** copiare l'ID della sottoscrizione di Azure negli Appunti. Sarà necessario nell'attività successiva.

## Attività 2: Creare ruoli Controllo degli accessi in base al ruolo personalizzati

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

## Attività 3: Assegnare ruoli Controllo degli accessi in base al ruolo

In questa attività si creerà un utente, si assegna il ruolo controllo degli accessi in base al ruolo creato nell'attività precedente a tale utente e si verificherà che l'utente possa eseguire l'attività specificata nella definizione del ruolo RBAC.

1. Nella portale di Azure cercare e selezionare **Microsoft Entra ID**, fare clic su **Utenti** e quindi fare clic su **+ Nuovo utente**.

1. Creare un nuovo utente con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome utente | **az104-02-aaduser1**|
    | Nome | **az104-02-aaduser1**|
    | Consenti la creazione manuale della password | Enabled |
    | Password iniziale | **Specificare una password sicura** |

    >**Nota**: **copiare negli Appunti** il valore completo di **Nome utente**. Sarà necessario più avanti in questa attività.

1. Nel portale di Azure tornare nel gruppo di gestione **az104-02-mg1** e visualizzarne i **dettagli**.

1. Fare clic su **Controllo di accesso (IAM)** , fare clic su **+ Aggiungi** e quindi su **Aggiungi assegnazione di ruolo**. Nella scheda **Ruolo** cercare **Collaboratore richiesta di supporto (personalizzato)** . 

    >**Nota**: se il ruolo personalizzato non è visibile, possono essere necessari fino a 10 minuti perché il ruolo personalizzato venga visualizzato dopo la creazione.

1. Selezionare il **Ruolo** e fare clic su **Avanti**. Nella scheda **Membri** fare clic su **+ Selezionare i membri** e **selezionare** l'account utente az104-***********************.**********.onmicrosoft.com. Fare clic su **Avanti** e quindi su **Rivedi e assegna**.

1. Aprire una finestra **InPrivate** del browser e accedere al [portale di Azure](https://portal.azure.com) con l'account utente appena creato. Quando viene richiesto di aggiornare la password, cambiare la password per l'utente.

    >**Nota**: invece di digitare il nome utente, è possibile incollare il contenuto degli Appunti.

1. Nella finestra del browser **InPrivate** cercare e selezionare **Gruppi di risorse** nel portale di Azure per verificare che l'utente az104-02-aaduser1 possa visualizzare tutti i gruppi di risorse.

1. Nella finestra del browser **InPrivate** cercare e selezionare **Tutte le risorse** nel portale di Azure per verificare che l'utente az104-02-aaduser1 non possa visualizzare nessuna risorsa.

1. Nella finestra del browser **InPrivate**, nel portale di Azure, cercare e selezionare **Guida e supporto** e quindi fare clic su **+ Crea una richiesta di supporto**. 

1. Nella finestra del browser **InPrivate**, nella scheda **Descrizione del problema/Riepilogo** del pannello **Guida e supporto - Nuova richiesta di supporto**, digitare **Limiti del servizio e della sottoscrizione** nel campo Riepilogo e selezionare il tipo di problema **Limiti del servizio e della sottoscrizione (quote)** . Si noti che la sottoscrizione in uso in questo lab è elencata nell'elenco a discesa **Sottoscrizione**.

    >**Nota**: la presenza della sottoscrizione in uso in questo lab nell'elenco a discesa **Sottoscrizione** indica che l'account in uso ha le autorizzazioni necessarie per creare la richiesta di supporto specifica della sottoscrizione.

    >**Nota**: se l'opzione **Limiti del servizio e della sottoscrizione (quote)** non è visualizzata, disconnettersi dal portale di Azure e accedere di nuovo.

1. Non continuare con la creazione della richiesta di supporto. Al contrario, disconnettersi come utente az104-02-aaduser1 dal portale di Azure e chiudere la finestra InPrivate del browser.

## Attività 4: Eseguire la pulizia delle risorse

   >**Nota**: ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate garantisce che non verranno effettuati addebiti imprevisti, anche se le risorse create in questo lab non comportano costi aggiuntivi.

   >**Nota**: non è necessario preoccuparsi se le risorse del lab non possono essere rimosse immediatamente. A volte le risorse hanno dipendenze e l'eliminazione può richiedere più tempo. Si tratta di un'attività comune dell'amministratore per monitorare l'utilizzo delle risorse, quindi è sufficiente esaminare periodicamente le risorse nel portale per verificare il funzionamento della pulizia.

1. Nella portale di Azure cercare e selezionare **Microsoft Entra ID**, fare clic su **Utenti**.

1. Nel pannello **Utenti - Tutti gli utenti** fare clic su **az104-02-aaduser1**.

1. Nel pannello **az104-02-aaduser1 - Profilo** copiare il valore dell'attributo **ID oggetto**.

1. Nel portale di Azure avviare una sessione di **PowerShell** all'interno di **Cloud Shell**.

1. Nel riquadro Cloud Shell eseguire il comando seguente per rimuovere l'assegnazione della definizione di ruolo personalizzata (sostituire il `[object_ID]` segnaposto con il valore dell'attributo **ID oggetto** dell'account utente **az104-02-aaduser1** copiato in precedenza in questa attività):

   ```powershell
   
    $scope = (Get-AzRoleDefinition -Name 'Support Request Contributor (Custom)').AssignableScopes | Where-Object {$_ -like '*managementgroup*'}
    
    Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. Nel riquadro Cloud Shell eseguire il comando seguente per rimuovere la definizione del ruolo personalizzato:

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. Nel portale di Azure tornare al pannello **Utenti - Tutti gli utenti** del **Microsoft Entra ID** ed eliminare l'account utente **az104-02-aaduser1**.

1. Nel portale di Azure tornare nel pannello **Gruppi di gestione**. 

1. Nel pannello **Gruppi di gestione** selezionare l'icona con i **puntini di sospensione** accanto alla sottoscrizione nel gruppo di gestione **az104-02-mg1** e selezionare **Sposta** per spostare la sottoscrizione nel **gruppo di gestione radice tenant**.

   >**Nota**: è probabile che il gruppo di gestione di destinazione sia il **gruppo di gestione radice tenant**, a meno che non sia stata creata una gerarchia di gruppi di gestione personalizzata prima di eseguire questo lab.
   
1. Selezionare **Aggiorna** per verificare che la sottoscrizione sia stata spostata correttamente nel **gruppo di gestione radice tenant**.

1. Tornare nel pannello **Gruppi di gestione**, fare clic sull'icona con i **puntini di sospensione** a destra del gruppo di gestione **az104-02-mg1** e fare clic su **Elimina**.
  >                **Nota**: se non è possibile eliminare il **gruppo di gestione radice del tenant**, è probabile che la **sottoscrizione di Azure** si trovi nel gruppo di gestione. È necessario spostare la **sottoscrizione di Azure** dal **gruppo di gestione radice del tenant** e quindi eliminare il gruppo.

## Verifica

In questo lab sono state eseguite le attività seguenti:

- Implementazione di gruppi di gestione
- Creazione di ruoli Controllo degli accessi in base al ruolo personalizzati 
- Assegnazione di ruoli Controllo degli accessi in base al ruolo
