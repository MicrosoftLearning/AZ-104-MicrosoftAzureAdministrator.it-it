---
lab:
  title: 02b – Gestire la governance tramite Criteri di Azure
  module: Administer Governance and Compliance
---

# <a name="lab-02b---manage-governance-via-azure-policy"></a>Lab 02b – Gestire la governance tramite Criteri di Azure
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

Per migliorare la gestione delle risorse di Azure in Contoso, è stato chiesto di implementare le funzionalità seguenti:

- Assegnazione di tag a gruppi di risorse che includono solo risorse dell'infrastruttura (ad esempio account di archiviazione Cloud Shell)

- Verifica che sia possibile aggiungere solo le risorse con il tag appropriato ai gruppi di risorse dell'infrastruttura

- Correzione di eventuali risorse non conformi

Per visualizzare l'anteprima di questo lab in formato di guida interattiva, **[fare clic qui](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203)** .

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Creare e assegnare tag tramite il portale di Azure
+ Attività 2: Imporre l'assegnazione di tag tramite criteri di Azure
+ Attività 3: Applicare l'assegnazione di tag tramite Criteri di Azure

## <a name="estimated-timing-30-minutes"></a>Tempo stimato: 30 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab02b.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-assign-tags-via-the-azure-portal"></a>Attività 1: Assegnare tag tramite il portale di Azure

In questa attività si creerà e si assegnerà un tag a un gruppo di risorse di Azure tramite il portale di Azure.

1. Nel portale di Azure avviare una sessione di **PowerShell** all'interno di **Cloud Shell**.

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**. 

1. Nel riquadro Cloud Shell eseguire quanto segue per identificare il nome dell'account di archiviazione usato da Cloud Shell:

   ```powershell
   df
   ```

1. Nell'output del comando prendere nota della prima parte del percorso completo che indica il montaggio dell'unità home di Cloud Shell (contrassegnato qui come `xxxxxxxxxxxxxx`:

   ```
   //xxxxxxxxxxxxxx.file.core.windows.net/cloudshell   (..)  /usr/csuser/clouddrive
   ```

1. Nel portale di Azure cercare e selezionare **Account di archiviazione**, quindi fare clic sulla voce dell'elenco che rappresenta l'account di archiviazione identificato nel passaggio precedente.

1. Nel pannello dell'account di archiviazione fare clic sul collegamento che rappresenta il nome del gruppo di risorse contenente l'account di archiviazione.

    **Nota**: prendere nota del gruppo di risorse in cui è contenuto l'account di archiviazione, perché sarà necessario più avanti nel lab.

1. Nel pannello del gruppo di risorse fare clic su **modifica** accanto a **Tag** per creare nuovi tag.

1. Creare un tag con le impostazioni seguenti e applicare la modifica:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **Ruolo** |
    | Valore | **Infra** |

1. Navigate back to the storage account blade. Review the <bpt id="p1">**</bpt>Overview<ept id="p1">**</ept> information and note that the new tag was not automatically assigned to the storage account. 

#### <a name="task-2-enforce-tagging-via-an-azure-policy"></a>Attività 2: Imporre l'assegnazione di tag tramite criteri di Azure

In questa attività si assegnerà il criterio predefinito *Richiedi un tag con il relativo valore sulle risorse* al gruppo di risorse e si valuterà il risultato. 

1. Nel portale di Azure cercare e selezionare **Criteri**. 

1. In the <bpt id="p1">**</bpt>Authoring<ept id="p1">**</ept> section, click <bpt id="p2">**</bpt>Definitions<ept id="p2">**</ept>. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the <bpt id="p1">**</bpt>Tags<ept id="p1">**</ept> entry (and de-selecting all other entries) in the <bpt id="p2">**</bpt>Category<ept id="p2">**</ept> drop-down list. 

1. Fare clic sulla voce che rappresenta il criterio predefinito **Richiedi un tag con il relativo valore sulle risorse** ed esaminarne la definizione.

1. Nel pannello della definizione del criterio predefinito **Richiedi un tag con il relativo valore sulle risorse** fare clic su **Assegna**.

1. Specificare un valore per **Ambito** facendo clic sul pulsante con i puntini di sospensione e selezionando le opzioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | Nome del gruppo di risorse contenente l'account Cloud Shell identificato nell'attività precedente |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A scope determines the resources or resource groups where the policy assignment takes effect. You could assign policies on the management group, subscription, or resource group level. You also have the option of specifying exclusions, such as individual subscriptions, resource groups, or resources (depending on the assignment scope). 

1. Nella scheda **Informazioni di base** configurare le proprietà dell'assegnazione specificando le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'assegnazione | **Require Role tag with Infra value**|
    | Descrizione | **Require Role tag with Infra value for all resources in the Cloud Shell resource group**|
    | Imposizione dei criteri | Attivato |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The <bpt id="p2">**</bpt>Assignment name<ept id="p2">**</ept> is automatically populated with the policy name you selected, but you can change it. You can also add an optional <bpt id="p1">**</bpt>Description<ept id="p1">**</ept>. <bpt id="p1">**</bpt>Assigned by<ept id="p1">**</ept> is automatically populated based on the user name creating the assignment. 

1. Fare clic su **Avanti** e impostare **Parametri** sui valori seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Nome del tag | **Ruolo** |
    | Valore del tag | **Infra** |

1. Fare clic su **Avanti** ed esaminare la scheda **Correzione**. Lasciare deselezionata la casella di controllo **Crea un'identità gestita**. 

    >**Nota**: questa impostazione può essere usata quando il criterio o l'iniziativa include l'effetto **deployIfNotExists** o **Modify**.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: a questo punto si verificherà se la nuova assegnazione del criterio è effettiva provando a creare un altro account di archiviazione di Azure nel gruppo di risorse senza aggiungere esplicitamente il tag necessario. 
    
    >**Nota**: l'applicazione del criterio potrebbe richiedere da 5 a 15 minuti.

1. Tornare nel pannello del gruppo di risorse che ospita l'account di archiviazione usato per l'unità home di Cloud Shell, identificato nell'attività precedente.

1. Nel pannello del gruppo di risorse fare clic su **+ Crea**, quindi cercare **Account di archiviazione** e fare clic su **+ Crea**. 

1. Nella scheda **Informazioni di base** del pannello **Crea account di archiviazione** verificare di usare il gruppo di risorse a cui è stato applicato il criterio e specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni), fare clic su **Rivedi e crea** e quindi su **Crea**:

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'account di archiviazione | Qualsiasi combinazione univoca globale di 3-24 lettere minuscole e numeri, a partire da una lettera |

1. Once you create the deployment, you should see the <bpt id="p1">**</bpt>Deployment failed<ept id="p1">**</ept> message in the <bpt id="p2">**</bpt>Notifications<ept id="p2">**</ept> list of the portal. From the <bpt id="p1">**</bpt>Notifications<ept id="p1">**</ept> list, navigate to the deployment overview and click the <bpt id="p2">**</bpt>Deployment failed. Click here for details<ept id="p2">**</ept> message to identify the reason for the failure. 

    >**Nota**: verificare se il messaggio di errore indica che la distribuzione della risorsa non è consentita dai criteri. 

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By clicking the <bpt id="p2">**</bpt>Raw Error<ept id="p2">**</ept> tab, you can find more details about the error, including the name of the role definition <bpt id="p3">**</bpt>Require Role tag with Infra value<ept id="p3">**</ept>. The deployment failed because the storage account you attempted to create did not have a tag named <bpt id="p1">**</bpt>Role<ept id="p1">**</ept> with its value set to <bpt id="p2">**</bpt>Infra<ept id="p2">**</ept>.

#### <a name="task-3-apply-tagging-via-an-azure-policy"></a>Attività 3: Applicare l'assegnazione di tag tramite Criteri di Azure

In questa attività verrà usata una definizione di criteri diversa per correggere eventuali risorse non conformi. 

1. Nel portale di Azure cercare e selezionare **Criteri**. 

1. Nella sezione **Creazione** fare clic su **Assegnazioni**. 

1. Nell'elenco di assegnazioni fare clic sull'icona con i puntini di sospensione nella riga che rappresenta l'assegnazione di criteri **Require Role tag with Infra value** e usare la voce di menu **Elimina assegnazione** per eliminare l'assegnazione.

1. Fare clic su **Assegna criteri** e specificare un valore per **Ambito** facendo clic sul pulsante con i puntini di sospensione e selezionando le opzioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | Il nome del gruppo di risorse contenente l'account Cloud Shell identificato nella prima attività |

1. Per specificare il valore di **Definizione dei criteri**, fare clic sul pulsante con i puntini di sospensione e quindi cercare e selezionare **Eredita un tag dal gruppo di risorse se mancante**.

1. Nella scheda **Informazioni di base** configurare le rimanenti proprietà dell'assegnazione specificando le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'assegnazione | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing**|
    | Descrizione | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing**|
    | Imposizione dei criteri | Attivato |

1. Fare clic su **Avanti** e impostare **Parametri** sui valori seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Nome del tag | **Ruolo** |

1. Fare clic su **Avanti** e quindi, nella scheda **Correzione**, configurare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Creare un'attività di correzione | Enabled |
    | Criterio da correggere | **Eredita un tag dalla sottoscrizione se mancante** |

    >**Nota**: questa definizione dei criteri include l'effetto **Modify**.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: per verificare se la nuova assegnazione del criterio è effettiva, si creerà un altro account di archiviazione di Azure nello stesso gruppo di risorse senza aggiungere esplicitamente il tag necessario. 
    
    >**Nota**: l'applicazione del criterio potrebbe richiedere da 5 a 15 minuti.

1. Tornare nel pannello del gruppo di risorse che ospita l'account di archiviazione usato per l'unità home di Cloud Shell, identificato nella prima attività.

1. Nel pannello del gruppo di risorse fare clic su **+ Crea**, quindi cercare **Account di archiviazione** e fare clic su **+ Crea**. 

1. Nella scheda **Informazioni di base** del pannello **Crea account di archiviazione** verificare di usare il gruppo di risorse a cui è stato applicato il criterio e specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni), quindi fare clic su **Rivedi e crea**:

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'account di archiviazione | Qualsiasi combinazione univoca globale di 3-24 lettere minuscole e numeri, a partire da una lettera |

1. Verificare che questa volta la convalida sia stata superata e fare clic su **Crea**.

1. Dopo aver effettuato il provisioning del nuovo account di archiviazione, fare clic sul pulsante **Vai alla risorsa**, quindi nel pannello **Panoramica** dell'account di archiviazione appena creato si noti che il tag **Role** con il valore **Infra** è stato assegnato automaticamente alla risorsa.

#### <a name="task-4-clean-up-resources"></a>Attività 4: Eseguire la pulizia delle risorse

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges, although keep in mind that Azure policies do not incur extra cost.
   
   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. Nel portale cercare e selezionare **Criteri**.

1. Nella sezione **Creazione** fare clic su **Assegnazioni**, fare clic sull'icona con i puntini di sospensione a destra dell'assegnazione creata nell'attività precedente, quindi fare clic su **Elimina assegnazione**. 

1. Nel portale cercare e selezionare **Account di archiviazione**.

1. In the list of storage accounts, select the resource group corresponding to the storage account you created in the last task of this lab. Select <bpt id="p1">**</bpt>Tags<ept id="p1">**</ept> and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept> (Trash can to the right) to the <bpt id="p3">**</bpt>Role:Infra<ept id="p3">**</ept> tag and press <bpt id="p4">**</bpt>Apply<ept id="p4">**</ept>. 

1. Click <bpt id="p1">**</bpt>Overview<ept id="p1">**</ept> and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept> on the top of the storage account blade. When prompted for the confirmation, in the <bpt id="p1">**</bpt>Delete storage account<ept id="p1">**</ept> blade, type the name of the storage account to confirm and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept>. 

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

- Creazione e assegnazione di tag tramite il portale di Azure
- Imposizione dei tag tramite un criterio di Azure
- Applicazione dei tag tramite un criterio di Azure
