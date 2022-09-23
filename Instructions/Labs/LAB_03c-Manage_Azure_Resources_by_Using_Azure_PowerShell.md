---
lab:
  title: 03c - Gestire le risorse di Azure usando Azure PowerShell
  module: Administer Azure Resources
---

# <a name="lab-03c---manage-azure-resources-by-using-azure-powershell"></a>Lab 03c - Gestire le risorse di Azure usando Azure PowerShell
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

Now that you explored the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups by using the Azure portal and Azure Resource Manager templates, you need to carry out the equivalent task by using Azure PowerShell. To avoid installing Azure PowerShell modules, you will leverage PowerShell environment available in Azure Cloud Shell.

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Avviare una sessione di PowerShell in Azure Cloud Shell
+ Attività 2: Creare un gruppo di risorse e un disco gestito di Azure usando Azure PowerShell
+ Attività 3: Configurare il disco gestito usando Azure PowerShell

## <a name="estimated-timing-20-minutes"></a>Tempo stimato: 20 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab03c.png)

## <a name="instructions"></a>Istruzioni

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Always create your own secure password for any virtual machine or user account you create. If the virtual machine is created for you, use <bpt id="p1">**</bpt>Reset password<ept id="p1">**</ept> in the Portal to update the password. 

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-start-a-powershell-session-in-azure-cloud-shell"></a>Attività 1: Avviare una sessione di PowerShell in Azure Cloud Shell

In questa attività si aprirà una sessione di PowerShell in Cloud Shell. 

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**. 

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**. 

1. Quando richiesto, fare clic su **Crea archivio** e attendere che venga visualizzato il riquadro Azure Cloud Shell. 

1. Accertarsi che nel menu a discesa nell'angolo in alto a sinistra del riquadro Cloud Shell sia visualizzato **PowerShell**.

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-powershell"></a>Attività 2: Creare un gruppo di risorse e un disco gestito di Azure usando Azure PowerShell

In questa attività si creeranno un gruppo di risorse e un disco gestito di Azure usando la sessione di Azure PowerShell all'interno di Cloud Shell

1. Per creare un gruppo di risorse nella stessa area di Azure del gruppo di risorse **az104-03b-rg1** creato nel lab precedente, nella sessione di PowerShell all'interno di Cloud Shell eseguire quanto segue:

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. Per recuperare le proprietà del gruppo di risorse appena creato, eseguire quanto segue:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. Per creare un nuovo disco gestito con le stesse caratteristiche di quello creato nei lab precedenti di questo modulo, eseguire quanto segue:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. Per recuperare le proprietà del disco appena creato, eseguire quanto segue:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

#### <a name="task-3-configure-the-managed-disk-by-using-azure-powershell"></a>Attività 3: Configurare il disco gestito usando Azure PowerShell

In questa attività si gestirà la configurazione del disco gestito di Azure usando la sessione di Azure PowerShell all'interno di Cloud Shell. 

1. Per aumentare le dimensioni del disco gestito di Azure a **64 GB**, nella sessione di PowerShell all'interno di Cloud Shell eseguire quanto segue:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Per verificare che la modifica abbia avuto effetto, eseguire quanto segue:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Per verificare che lo SKU corrente sia **Standard_LRS**, eseguire quanto segue:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. Per impostare lo SKU delle prestazioni del disco su **Premium_LRS**, nella sessione di PowerShell all'interno di Cloud Shell eseguire quanto segue:

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Per verificare che la modifica abbia avuto effetto, eseguire quanto segue:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

#### <a name="clean-up-resources"></a>Pulire le risorse

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

- Avvio di una sessione di PowerShell in Azure Cloud Shell
- Creazione di un gruppo di risorse e di un disco gestito di Azure usando Azure PowerShell
- Configurazione del disco gestito usando Azure PowerShell
