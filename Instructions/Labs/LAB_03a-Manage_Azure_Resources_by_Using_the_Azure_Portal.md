---
lab:
  title: 03a - Gestire le risorse di Azure tramite il portale di Azure
  module: Module 03 - Azure Administration
---

# <a name="lab-03a---manage-azure-resources-by-using-the-azure-portal"></a>Lab 03a - Gestire le risorse di Azure tramite il portale di Azure
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

You need to explore the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups, including moving resources between resource groups. You also want to explore options for protecting disk resources from being accidentally deleted, while still allowing for modifying their performance characteristics and size.

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Creare gruppi di risorse e distribuire risorse al loro interno
+ Attività 2: Spostare le risorse tra gruppi di risorse
+ Attività 3: Implementare e testare i blocchi delle risorse

## <a name="estimated-timing-20-minutes"></a>Tempo stimato: 20 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab03a.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-create-resource-groups-and-deploy-resources-to-resource-groups"></a>Attività 1: Creare gruppi di risorse e distribuire risorse al loro interno

In questa attività si userà il portale di Azure per creare gruppi di risorse e un disco al loro interno.

1. Accedere al [**portale di Azure**](http://portal.azure.com).

1. Nel portale di Azure cercare e selezionare **Dischi**, fare clic su **+ Crea** e specificare le impostazioni seguenti:

    |Impostazione|Valore|
    |---|---|
    |Subscription| Nome della sottoscrizione di Azure in cui è stato creato il gruppo di risorse |
    |Gruppo di risorse| Il nome di un nuovo gruppo di risorse **az104-03a-rg1** |
    |Nome del disco| **az104-03a-disk1** |
    |Region| **(Stati Uniti) Stati Uniti orientali** |
    |Zona di disponibilità| **Nessuno** |
    |Tipo di origine| **Nessuno** |

    >**Nota**: quando si crea una risorsa, è possibile creare un nuovo gruppo di risorse oppure usarne uno esistente.

1. Impostare il tipo e le dimensioni del disco rispettivamente su **HDD Standard** e su **32 GiB**.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait until the disk is created. This should take less than a minute.

#### <a name="task-2-move-resources-between-resource-groups"></a>Attività 2: Spostare le risorse tra gruppi di risorse 

In questa attività la risorsa disco creata nell'attività precedente verrà spostata in un nuovo gruppo di risorse. 

1. Cercare e selezionare **Gruppi di risorse**. 

1. Nel pannello **Gruppi di risorse** fare clic sulla voce che rappresenta il gruppo di risorse **az104-03a-rg1** creato nell'attività precedente.

1. Nel pannello **Panoramica** del gruppo di risorse selezionare nell'elenco delle risorse la voce che rappresenta il disco appena creato, fare clic su **Sposta** sulla barra degli strumenti e quindi, nell'elenco a discesa, selezionare **Sposta in un altro gruppo di risorse**.

    >**Nota**: questo metodo consente di spostare più risorse contemporaneamente. 

1. Below the <bpt id="p1">**</bpt>Resource group<ept id="p1">**</ept> text box, click <bpt id="p2">**</bpt>Create new<ept id="p2">**</ept> then type <bpt id="p3">**</bpt>az104-03a-rg2<ept id="p3">**</ept> in the text box. On the Review tab, select the checkbox <bpt id="p1">**</bpt>I understand that tools and scripts associated with moved resources will not work until I update them to use new resource IDs<ept id="p1">**</ept>, and click <bpt id="p2">**</bpt>Move<ept id="p2">**</ept>.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the move to complete but instead proceed to the next task. The move might take about 10 minutes. You can determine that the operation was completed by monitoring activity log entries of the source or target resource group. Revisit this step once you complete the next task.

#### <a name="task-3-implement-resource-locks"></a>Attività 3: Implementare blocchi delle risorse

In questa attività si applicherà un blocco a un gruppo di risorse di Azure contenente una risorsa disco.

1. Nel portale di Azure cercare e selezionare **Dischi**, fare clic su **+ Crea** e specificare le impostazioni seguenti:

    |Impostazione|Valore|
    |---|---|
    |Subscription| Il nome della sottoscrizione usata in questo lab |
    |Gruppo di risorse| Fare clic su **Crea nuovo gruppo di risorse** e assegnare il nome **az104-03a-rg3** |
    |Nome del disco| **az104-03a-disk2** |
    |Region| Il nome dell'area di Azure in cui sono stati creati gli altri gruppi di risorse di questo lab |
    |Zona di disponibilità| **Nessuno** |
    |Tipo di origine| **Nessuno** |

1. Impostare il tipo e le dimensioni del disco rispettivamente su **HDD Standard** e su **32 GiB**.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

1. Fare clic su **Vai alla risorsa**.

1. Nella pagina Panoramica del disco fare clic sul nome del gruppo di risorse, **az104-03a-rg3**.

1. Nel pannello del gruppo di risorse **az104-03a-rg3** fare clic su **Blocchi**, quindi su **+ Aggiungi** e specificare le impostazioni seguenti:

    |Impostazione|Valore|
    |---|---|
    |Nome del blocco| **az104-03a-delete-lock** |
    |Tipo di blocco| **Elimina** |
    
1. Fare clic su **OK**.    

1. Nel pannello del gruppo di risorse **az104-03a-rg3** fare clic su **Panoramica** e selezionare nell'elenco delle risorse la voce che rappresenta il disco creato in precedenza in questa attività, quindi fare clic su **Elimina** sulla barra degli strumenti. 

1. Quando viene visualizzato il messaggio **Eliminare le risorse selezionate?** , digitare **Sì** nella casella di testo **Conferma eliminazione** e quindi fare clic su **Elimina**.

1. Verrà visualizzato un messaggio di errore che avvisa che l'operazione di eliminazione non è riuscita. 

    >**Nota**: come indica il messaggio di errore, questo comportamento è previsto a causa del blocco di eliminazione applicato a livello di gruppo di risorse.

1. Tornare nell'elenco delle risorse del gruppo **az104-03a-rg3** e fare clic sulla voce che rappresenta la risorsa **az104-03a-disk2**. 

1. On the <bpt id="p1">**</bpt>az104-03a-disk2<ept id="p1">**</ept> blade, in the <bpt id="p2">**</bpt>Settings<ept id="p2">**</ept> section, click <bpt id="p3">**</bpt>Size + performance<ept id="p3">**</ept>, set the disk type and size to <bpt id="p4">**</bpt>Premium SSD<ept id="p4">**</ept> and <bpt id="p5">**</bpt>64 GiB<ept id="p5">**</ept>, respectively, and click <bpt id="p6">**</bpt>Resize<ept id="p6">**</ept> to apply the change. Verify that the change was successful.

    >**Nota**: questo comportamento è previsto, perché il blocco a livello di gruppo di risorse si applica solo alle operazioni di eliminazione. 

#### <a name="clean-up-resources"></a>Pulire le risorse

   >È necessario esplorare le funzionalità di amministrazione di base di Azure associate al provisioning delle risorse e organizzarle in base ai gruppi di risorse, incluso lo spostamento di risorse tra gruppi.

1. Passare al pannello del gruppo di risorse **az104-03a-rg3**, visualizzare il relativo pannello **Blocchi** e rimuovere il blocco **az104-03a-delete-lock** facendo clic sul collegamento **Elimina** a destra della voce **Elimina blocco**.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

- Creazione di gruppi di risorse e distribuzione di risorse al loro interno
- Spostamento di risorse tra gruppi di risorse
- Implementazione e test dei blocchi delle risorse
