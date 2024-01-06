---
lab:
  title: 'Lab 03: Gestire le risorse di Azure usando i modelli di Azure Resource Manager'
  module: Administer Azure Resources
---

# Lab 03 - Gestire le risorse di Azure usando i modelli di Azure Resource Manager

## Introduzione al lab

In questo lab si apprenderà come definire l'infrastruttura delle risorse usando i modelli di Azure Resource Manager. I modelli garantiscono la coerenza e consentono di creare più risorse contemporaneamente. Si apprenderà come distribuire modelli con il portale di Azure, Azure PowerShell o l'interfaccia della riga di comando. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali. 

## Tempo stimato: 40 minuti

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Creare una macchina virtuale con un modello](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Distribuire una macchina virtuale con un modello di avvio rapido.
  
+ [Gestire le risorse di Azure usando i modelli](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205) di Azure Resource Manager. Esaminare, creare e distribuire dischi gestiti con un modello.

## Scenario laboratorio

Il team ha esaminato le funzionalità amministrative di base di Azure, ad esempio il provisioning delle risorse e l'organizzazione in base ai gruppi di risorse. Il team vuole quindi esaminare i modi per automatizzare e semplificare le distribuzioni. Le organizzazioni spesso cercano l'automazione per ridurre il sovraccarico amministrativo, ridurre l'errore umano o aumentare la coerenza e come modo per consentire agli amministratori di lavorare su attività più complesse o creative.

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab03b-architecture-diagram.png)

## Attività

+ Attività 1: Creare un modello di Azure Resource Manager per la distribuzione di un disco gestito di Azure.
+ Attività 2: Modificare un modello di Azure Resource Manager e quindi creare un disco gestito di Azure usando il modello.
+ Attività 3: Esaminare la distribuzione basata su modello di Azure Resource Manager del disco gestito.
+ Attività 4: Distribuire un modello con Azure PowerShell (opzione 1).
+ Attività 5: Distribuire un modello con l'interfaccia della riga di comando (opzione 2). 
+ Attività 6: Distribuire un disco gestito usando Azure Bicep.



## Attività 1: Creare un modello di Azure Resource Manager per la distribuzione di un disco gestito di Azure

In questa attività si usa il portale di Azure per generare un modello di Azure Resource Manager. È quindi possibile scaricare il modello da usare nelle distribuzioni future. Un'organizzazione che prevede di distribuire centinaia o migliaia di dischi potrebbe sfruttare uno o più modelli per automatizzare le distribuzioni. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Disks`.

1. Nella pagina Dischi selezionare **Crea**.

1. Nella pagina Crea un disco gestito usare le informazioni seguenti per creare un disco.
    
    | Impostazione | Valore |
    | --- | --- |
    | Subscription | *sottoscrizione in uso* | 
    | Gruppo di risorse | `az104-rg3` (Se necessario, selezionare **Crea nuovo**.
    | Disk name | `az104-disk1` | 
    | Area geografica | **Stati Uniti orientali** |
    | Zona di disponibilità | **La ridondanza dell'infrastruttura non è richiesta** | 
    | Source type | **Nessuno** |
    | Prestazioni | **Unità disco rigido Standard** |
    | Dimensione | **32 Gib** | 


1. Fare clic su **Rivedi e crea** *una sola volta*. Non **** distribuire la risorsa.

1. Dopo la convalida, fare clic su **Scarica un modello per l'automazione** (nella parte inferiore della pagina).

1. Esaminare le informazioni visualizzate nel modello. Esaminare sia la **scheda Modello** che **Parametri** .

1. Fare clic su **Scarica** e salvare i modelli nell'unità locale. Verrà creato un file compresso compresso. 

1. Estrarre il contenuto del file scaricato nella cartella Download** del **computer. Si noti che sono presenti due file JSON (modello e parametri). 

1. Chiudere tutte le finestre di **Esplora file**.

1. Nella portale di Azure annullare la distribuzione del disco gestito.

   >**Nota:**  è possibile esportare un intero gruppo di risorse o solo risorse specifiche all'interno di tale gruppo di risorse.

## Attività 2: Modificare un modello di Azure Resource Manager e quindi creare un disco gestito di Azure usando il modello

In questa attività si usa il modello creato per distribuire un nuovo disco gestito. Questa attività descrive il processo generale di distribuzione basata su modelli, in modo da poter ripetere facilmente e rapidamente le distribuzioni. Se è necessario modificare un parametro o due, è possibile modificare facilmente il modello in futuro.

1. Nella portale di Azure cercare e selezionare `Deploy a custom template`.

1. Nel pannello **Distribuzione** personalizzata si noti che è possibile usare un **modello** di avvio rapido. Esistono molti modelli predefiniti, come illustrato nel menu a discesa. 

1. Invece di usare una guida introduttiva, selezionare **Compila un modello personalizzato nell'editor**.

1. Nel pannello **Modifica modello** fare clic su **Carica file** e caricare il file **template.json** scaricato nell'attività precedente.

1. Nel riquadro dell'editor rimuovere le righe seguenti. Assicurarsi di rimuovere la parentesi chiusa e la virgola. 

   ```
    "sourceResourceId": {
            "type": "string"
    },
   ```
   
   ```
      "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    >**Nota**: questi parametri vengono rimossi perché non sono applicabili alla distribuzione corrente. I parametri SourceResourceId, sourceUri, osType e hyperVGeneration sono applicabili alla creazione di un disco di Azure da un file VHD esistente.

1. Fare clic su **Salva** per salvare le modifiche.

1. Tornare nel pannello **Distribuzione personalizzata** e fare clic su **Modifica parametri**. 

1. Nel pannello **Modifica modello** fare clic su **Carica file** e caricare il file **parameters.json** scaricato nell'attività precedente, quindi fare clic su **Salva** per salvare le modifiche.

1. Tornare nel pannello **Distribuzione personalizzata** e specificare le impostazioni seguenti:

    | Impostazione | Valore |
    | --- |--- |
    | Subscription | *sottoscrizione in uso* |
    | Gruppo di risorse | `az104-rg3` |
    | Area geografica | **Stati Uniti orientali** |
    | Nome disco | `az104-disk1` |
    | Ufficio | Il valore del parametro location annotato nell'attività precedente |
    | Sku | **Standard_LRS** |
    | Dimensioni disco (GB) | **32** |
    | Opzione Crea | **empty** |
    | Set di crittografia dischi | **EncryptionAtRestWithPlatformKey** |
    | Modalità autenticazione accesso ai dati | None |
    | Criteri di accesso alla rete | **AllowAll** |
    | Accesso alla rete pubblica | Disabilitata |

1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Verificare che la distribuzione sia stata completata correttamente.

## Attività 3: Esaminare la distribuzione basata su modello di Azure Resource Manager del disco gestito

In questa attività si verifica che la distribuzione sia stata completata correttamente. Tutte le distribuzioni precedenti sono documentate nel gruppo di risorse a cui è stata destinata la distribuzione. Questa revisione mostra i dettagli relativi al tempo e alla durata della distribuzione, che possono essere utili per la risoluzione dei problemi. Spesso è consigliabile esaminare le prime distribuzioni basate su modelli per garantire il successo prima di usare i modelli per operazioni su larga scala.

1. Accedere al portale di Azure e selezionare **Gruppi di risorse**. 

1. Nell'elenco dei gruppi di risorse fare clic su **az104-rg3**.

1. Nel pannello **az104-rg3** resource group fare clic su **Distribuzioni** nella **sezione Impostazioni**.

1. **Nel pannello az104-rg3 - Distribuzioni** fare clic sulla prima voce nell'elenco delle distribuzioni ed esaminare il contenuto dei **pannelli Input** e **Modello**.

1. Nel portale di Azure cercare e selezionare **Dischi**.

1. Si noti che il disco gestito è stato creato.

    >**Nota:** è anche possibile distribuire modelli dalla riga di comando. L'attività 4 illustra come eseguire la distribuzione con PowerShell. L'attività 5 illustra come eseguire la distribuzione usando l'interfaccia della riga di comando.


## Attività 4. Distribuire un modello con Azure PowerShell (opzione 1).

1. Aprire Cloud Shell e selezionare **PowerShell**.

1. Se necessario, usare le **impostazioni avanzate** per creare l'archiviazione su disco per Cloud Shell.

1. In Cloud Shell usare l'icona **Carica** per caricare i file di modello e parametri. Sarà necessario caricare ogni file separatamente.

1. Verificare che i file siano disponibili nell'archiviazione di Cloud Shell.

    ```powershell
    dir
    ```

1. In Cloud Shell selezionare l'icona **Editor** (parentesi graffe) e passare al file JSON dei parametri.

1. Apportare una modifica. Ad esempio, modificare il nome del disco in **az104-disk2**. Usare **CTRL+S** o il menu Altro** in alto a destra **per salvare le modifiche. 

    >**Nota**: è possibile impostare come destinazione la distribuzione del modello in un gruppo di risorse, una sottoscrizione, un gruppo di gestione o un tenant. A seconda dell'ambito della distribuzione, vengono usati comandi diversi.

1. Per eseguire la distribuzione in un gruppo di risorse, usare **New-AzResourceGroupDeployment**.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. Verificare che il comando venga completato e ProvisioningState sia **Succeeded**.

1. È possibile verificare che il disco sia stato creato controllando il portale o usando il **comando Get-AzDisk** . 
   
## Attività 5: Distribuire un modello con l'interfaccia della riga di comando (opzione 2)

1. Aprire Cloud Shell e selezionare **Bash**.

1. Se necessario, usare le **impostazioni avanzate** per creare l'archiviazione su disco per Cloud Shell.

1. In Cloud Shell usare l'icona **Carica** per caricare i file di modello e parametri. Sarà necessario caricare ogni file separatamente.

1. Verificare che i file siano disponibili nell'archiviazione di Cloud Shell.

    ```sh
    ls
    ```

1. In Cloud Shell selezionare l'icona **Editor** (parentesi graffe) e passare al file JSON dei parametri.

1. Apportare una modifica. Ad esempio, modificare il nome del disco in **az104-disk3**. Usare **CTRL+S** o il menu Altro** in alto a destra **per salvare le modifiche. 

    >**Nota**: è possibile impostare come destinazione la distribuzione del modello in un gruppo di risorse, una sottoscrizione, un gruppo di gestione o un tenant. A seconda dell'ambito della distribuzione, vengono usati comandi diversi.

1. Per eseguire la distribuzione in un gruppo di risorse, usare **az deployment group create**.

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
1. Verificare che il comando venga completato e ProvisioningState sia **Succeeded**.

1. È possibile verificare che il disco sia stato creato controllando il portale o usando il **comando az disk list** .
   
## Attività 6: Distribuire una risorsa usando Azure Bicep

In questa attività si userà un file Bicep per distribuire un account di archiviazione nel gruppo di risorse. Bicep è uno strumento di automazione dichiarativa basato su modelli di Resource Manager, ma è più facile da leggere e usare.

1. Aprire una sessione Bash** di Cloud Shell**. 

1. Selezionare **Carica**. Individuare la directory \Allfiles\Lab03 e selezionare il file **modello Bicep azuredeploy.bicep**.

1. Per verificare che il file sia stato caricato, selezionare l'icona delle doppie parentesi quadre nel menu di Cloud Shell per aprire l'editor predefinito.

1. Il file deve essere elencato nell'albero di spostamento a sinistra. Selezionarlo per verificarne il contenuto.

   ![Screenshot del file bicep nell'editor di Cloud Shell.](../media/az104-lab03d-editor.png)

1. Leggere il file modello bicep. Si noti come viene definita la risorsa di archiviazione (stg). Si noti come vengono configurati i parametri e i valori consentiti.
   
1. Per distribuire il file Bicep nel **gruppo di risorse az104-rg3** , eseguire quanto segue:

   ```sh
   az deployment group create --resource-group az104-rg3 --template-file azuredeploy.bicep
   ```

1. Quando viene richiesto un valore stringa di archiviazione, immettere `az104`. Il file modello userà la stringa con una stringa generata in modo casuale per creare un nome di distribuzione univoco.

1. Chiudere Cloud Shell e tornare al portale di Azure completo. 

1. Cercare e selezionare **Archiviazione Account**. Verificare che sia stato creato un account di archiviazione denominato **az104** nel **gruppo di risorse az104-rg3** .

## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ I modelli di Azure Resource Manager consentono di distribuire, gestire e monitorare tutte le risorse per la soluzione come gruppo, anziché gestire queste risorse singolarmente.
+ Un modello di Azure Resource Manager è un file JSON (JavaScript Object Notation) che consente di gestire l'infrastruttura in modo dichiarativo anziché con gli script.
+ Anziché passare parametri come valori inline nel modello, è possibile usare un file JSON separato che contiene i valori dei parametri.
+ I modelli di Azure Resource Manager possono essere distribuiti in diversi modi, tra cui la portale di Azure, Azure PowerShell e l'interfaccia della riga di comando.

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.
