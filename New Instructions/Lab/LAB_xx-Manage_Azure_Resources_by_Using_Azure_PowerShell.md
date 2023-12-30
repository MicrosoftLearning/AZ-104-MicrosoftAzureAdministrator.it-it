---
lab:
  title: 'Lab 03c: Gestire le risorse di Azure usando Azure PowerShell (facoltativo)'
  module: Administer Azure Resources
---

# Lab 03c - Gestire le risorse di Azure usando Azure PowerShell
# Manuale del lab per gli studenti

## Scenario laboratorio

L'utente e il team hanno esaminato le funzionalità amministrative di base di Azure nelle portale di Azure, ad esempio il provisioning delle risorse e l'organizzazione in base ai gruppi di risorse. Sono stati esaminati anche i modelli di Azure Resource Manager. Successivamente, si vogliono provare le attività equivalenti usando Azure PowerShell. In questo lab si userà un ambiente PowerShell disponibile in Azure Cloud Shell.

**Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi. 

## Obiettivi

Contenuto del lab:

+ Attività 1: Avviare una sessione di PowerShell in Azure Cloud Shell
+ Attività 2: Creare un gruppo di risorse e un disco gestito di Azure usando Azure PowerShell
+ Attività 3: Configurare il disco gestito usando Azure PowerShell

## Tempo stimato: 20 minuti

## Diagramma dell'architettura

![Diagramma dell'architettura.](../media/az104-lab03c-architecture-diagram.png)

### Istruzioni

## Esercizio 1

## Attività 1: Avviare una sessione di PowerShell in Azure Cloud Shell

In questa attività si aprirà una sessione di PowerShell in Cloud Shell. Cloud Shell è integrato nel portale di Azure e consente di eseguire comandi di PowerShell o dell'interfaccia della riga di comando di Azure direttamente nel portale senza dover installare alcun elemento. L'account di archiviazione mantiene la cronologia dei comandi e fornisce un percorso per l'archiviazione degli script.

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra. In alternativa, passare direttamente a `https://shell.azure.com`.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**. 

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**. 

1. Quando richiesto, fare clic su **Crea archivio** e attendere che venga visualizzato il riquadro Azure Cloud Shell. 

1. Accertarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia visualizzato **PowerShell**.

## Attività 2: Creare un gruppo di risorse e un disco gestito di Azure usando Azure PowerShell

In questa attività si creerà un gruppo di risorse e un disco gestito di Azure usando la sessione di Azure PowerShell in Cloud Shell. Questa attività è progettata per illustrare come usare Cloud Shell e Azure PowerShell per creare script di attività comuni. È quindi possibile attivare script usando Automazione di Azure, App per la logica o altri strumenti di automazione.

1. PowerShell usa un *formato sostantivo verbo**-* per i cmdlet. Ad esempio, il cmdlet per creare un nuovo gruppo di risorse è **New-AzResourceGroup**. Per visualizzare come usare il cmdlet, eseguire il comando seguente:

   ```powershell
   Get-Help New-AzResourceGroup
   ```


1. Per creare un gruppo di risorse dalla sessione di PowerShell in Cloud Shell, eseguire i comandi seguenti. Si noti che i comandi che iniziano con un segno di dollaro ($) creano variabili che è possibile usare nei comandi successivi.

   ```powershell
   $location = 'eastus'

   $rgName = 'az104-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
   >**Nota**: se **az104-rg1** esiste già, usare un nome diverso nel **parametro $rgName** . 

   ![Screenshot della creazione di un gruppo di risorse. ](../media/az104-lab03c-createrg.png)

1. Per recuperare le proprietà del gruppo di risorse appena creato, eseguire il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```

1. Analogamente alla creazione di un nuovo gruppo di risorse, per creare un nuovo disco, usare il **cmdlet New-AzDisk** . Per visualizzare come usare il cmdlet, eseguire il comando seguente:

   ```powershell
   Get-Help New-AzDisk
   ```

1. Per creare un nuovo disco gestito denominato **az104-disk1**, eseguire i comandi seguenti:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -SkuName Standard_LRS

   $diskName = 'az104-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

   ![Screenshot della creazione del disco. ](../media/az104-lab03c-createdisk.png)

1. Per recuperare le proprietà del disco appena creato, eseguire il comando seguente:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## Attività 3: Configurare il disco gestito usando Azure PowerShell

In questa attività si configurerà il disco gestito di Azure usando la sessione di Azure PowerShell in Cloud Shell. Questa attività è progettata per illustrare come usare Cloud Shell e Azure PowerShell per creare script di attività comuni.

>**Lo sapevi?**  È anche possibile eseguire script usando Automazione di Azure, App per la logica o altri strumenti di automazione.

1. Per aumentare le dimensioni del disco gestito di Azure a **64 GB**, nella sessione di PowerShell all'interno di Cloud Shell eseguire quanto segue:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Per verificare che la modifica sia stata applicata, eseguire il comando seguente:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Per verificare lo SKU corrente come **Standard_LRS**, eseguire il comando seguente:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

   ![Screenshot dello SKU di aggiornamento.](../media/az104-lab03c-updatesku.png)

1. Per modificare lo SKU delle prestazioni del disco in **Premium_LRS**, dalla sessione di PowerShell in Cloud Shell eseguire il comando seguente:

   ```powershell
   New-AzDiskUpdateConfig -SkuName Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Per verificare che la modifica sia stata applicata, eseguire il comando seguente:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

   ![Screenshot della versione finale dello SKU di aggiornamento.](../media/az104-lab03c-updatesku2.png)

>**Hai notato?** I cmdlet di PowerShell usano verbi comuni, ad **esempio Get**, **New** e **Update**. I cmdlet che iniziano con  **Get** in genere recuperano informazioni sulla configurazione. I cmdlet che iniziano con **New** creano in genere nuove risorse. I cmdlet che iniziano con **Update** in genere configurano le risorse esistenti.

## Revisione

Complimenti. È stata avviata correttamente una sessione di Azure Cloud Shell, è stato creato un gruppo di risorse usando PowerShell, è stato creato un disco gestito usando PowerShell e configurato il disco gestito usando PowerShell.
