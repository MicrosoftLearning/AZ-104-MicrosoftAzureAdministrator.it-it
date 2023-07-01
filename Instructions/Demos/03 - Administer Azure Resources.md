---
demo:
  title: 'Dimostrazione 03: Amministrare le risorse di Azure'
  module: Administer Administer Azure Resources
---
# 03 - Amministrare le risorse di Azure

## Dimostrazione - Portale di Azure

In questa dimostrazione verranno esaminati i portale di Azure.

**Riferimento**: [Gestire le impostazioni e le preferenze di portale di Azure](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**Riferimento**: [Creare un dashboard nel portale di Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**Riferimento**: [Come creare una richiesta di supporto tecnico di Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. Accedere al portale di Azure.

1. Selezionare l'icona **Supporto & risoluzione dei problemi** nel banner in alto. Esaminare i collegamenti **Risorse di supporto** . 

1. Selezionare l'icona **Impostazioni** nel banner superiore.  Esaminare **le impostazioni aspetto e visualizzazioni di avvio** . 

1. Usare il menu a sinistra e selezionare **Dashboard**. **Modificare** il dashboard usando la **raccolta riquadri**. Discutere le opzioni di personalizzazione. 

1. Chiedere se gli studenti hanno domande su come usare il portale di Azure. 

## Dimostrazione - Cloud Shell

In questa dimostrazione ci si eserciterà con Cloud Shell.

**Informazioni di riferimento**: [Avvio rapido per Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**Configurare Cloud Shell**

1.  Accedere al **portale di Azure**.

1.  Fare clic sul  **Cloud Shell**  icon nel banner in alto.

1.  Nella pagina Introduzione a Cloud Shell, fare attenzione alle selezioni per Bash o PowerShell.  Selezionare **PowerShell**.

1.  Informazioni su come Azure Cloud Shell richiede una condivisione file di Azure per rendere persistenti i file. Se necessario, configurare la condivisione di archiviazione. 

**Sperimentare con Azure PowerShell e Bash**

1. Verificare che la shell di **PowerShell** sia selezionata e provare alcuni comandi. Ad esempio, **Get-AzSubscription** e **Get-AzResourceGroup**.

1. Mostra il funzionamento del completamento automatico. Mostra come cancellare lo schermo, **cls**. 

1. Verificare che la shell **Bash** sia selezionata e provare alcuni comandi. Ad esempio, **az account list** e **az resource list**.

1. Porre domande agli studenti sull'uso dei comandi di PowerShell o Bash. 

**Sperimentare l'editor di Cloud Shell (facoltativo)**

1. Per usare l'editor cloud, selezionare l'icona **delle parentesi graffe** .

1. Selezionare un file nel riquadro di spostamento sinistro.  Ad esempio, **.profile**.

1. Nel banner superiore dell'editor notare le selezioni per Impostazioni (Dimensione testo e Font) e Carica/Scarica file.

1. Si notino i puntini di sospensione (**\...**) a destra per **Salva**, **Chiudi editor** e **Apri file**.

1. Sperimentare quando si ha tempo, quindi **chiudere** l'Editor cloud.

1. Chiudere Cloud Shell.

## Dimostrazione - Modelli di avvio rapido

In questa dimostrazione si procederà a esaminare i modelli di avvio rapido.

**Riferimento**: [Esercitazione - Creare un modello di distribuzione & - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. Per iniziare, passare alla [raccolta Modelli di avvio rapido di Azure](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager). Si noti che sono disponibili esempi JSON e Bicep. 

1. Chiedere agli studenti se sono presenti modelli specifici di interesse. In caso contrario, selezionare un modello. Ad esempio, il modello [Deploy a simple Windows VM with tags (Distribuisci una macchina virtuale Windows semplice con tag](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/) ).

1. Informazioni sul modo in cui il pulsante **Distribuisci in Azure** consente di distribuire il modello direttamente tramite il portale di Azure.

1. **Distribuire** il modello JSON e illustrare come modificare il modello e il file di parametri. Esaminare lo scopo dei file. Quando si ha tempo, esaminare la sintassi. 

1. Tornare alla raccolta di esempi di codice e individuare un modello Bicep. Ad esempio, [Creare un account di archiviazione Standard](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/). 

1. **Distribuire** il modello Bicep e illustrare come modificare il modello e il file dei parametri. Quando si ha tempo, esaminare la sintassi. 
