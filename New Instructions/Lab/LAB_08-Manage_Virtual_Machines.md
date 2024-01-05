---
lab:
  title: 'Lab 08: Gestire Macchine virtuali'
  module: Administer Virtual Machines
---

# Lab 08 - Gestire le macchine virtuali

## Introduzione al lab

In questo lab viene confrontato il ridimensionamento manuale delle macchine virtuali con il ridimensionamento automatico delle macchine virtuali. Si apprenderà come configurare e ridimensionare una singola macchina virtuale. Si apprenderà come creare un set di scalabilità di macchine virtuali e configurare la scalabilità automatica.

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

## Tempo stimato: 50 minuti

## Scenario laboratorio

L'organizzazione vuole esplorare la distribuzione e la configurazione di macchine virtuali di Azure. Prima di tutto, è necessario determinare le diverse opzioni di resilienza e scalabilità delle risorse di calcolo e di archiviazione che è possibile implementare quando si usano le macchine virtuali di Azure. Successivamente, è necessario esaminare le opzioni di resilienza e scalabilità delle risorse di calcolo e di archiviazioni disponibili quando si usano i set di scalabilità di macchine virtuali.

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ [Creare una macchina virtuale nel portale](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%201). Creare una macchina virtuale, connettersi e installare il ruolo del server Web. 
+ [Distribuire una macchina virtuale con un modello](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Esplorare la raccolta Avvio rapido e individuare un modello di macchina virtuale. Distribuire il modello e verificare la distribuzione.
+ [Creare una macchina virtuale con PowerShell](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2010). Usare Azure PowerShell per distribuire una macchina virtuale. Esaminare le raccomandazioni di Azure Advisor. 
+ [Creare una macchina virtuale con l'interfaccia della riga di comando](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2011). Usare l'interfaccia della riga di comando per distribuire una macchina virtuale. Esaminare le raccomandazioni di Azure Advisor. 

## Attività

+ Attività 1: Distribuire macchine virtuali di Azure resilienti alla zona usando il portale di Azure
+ Attività 2: Gestire il ridimensionamento delle risorse di calcolo e archiviazione per le macchine virtuali
+ Attività 3: Implementare Azure set di scalabilità di macchine virtuali
+ Attività 4: Ridimensionare i set di scalabilità di macchine virtuali di Azure
+ Attività 5: Creare una macchina virtuale con Azure PowerShell (opzione 1)
+ Attività 6: Creare una macchina virtuale usando l'interfaccia della riga di comando (opzione 2) 




## Attività 1 e 2: Diagramma dell'architettura di Azure Macchine virtuali

![Diagramma delle attività di architettura.](../media/az104-lab08a-architecture-diagram.png)

## Attività 1: Distribuire macchine virtuali di Azure resilienti alla zona usando il portale di Azure

In questa attività si distribuiranno due macchine virtuali di Azure in zone di disponibilità diverse usando il portale di Azure. Le zone di disponibilità offrono il massimo livello di tempo di attività per le macchine virtuali al 99,99%. Per ottenere questo contratto di servizio, è necessario distribuire almeno due macchine virtuali in zone di disponibilità diverse.

1. Accedere al portale di Azure - `https://portal.azure.com`.

1. Cercare e selezionare `Virtual machines` e nel pannello **Macchine** virtuali fare clic su **+ Crea** e quindi selezionare nell'elenco a discesa **+ Macchina** virtuale di Azure.

1. Nella **scheda Informazioni di base** del **pannello Crea una macchina** virtuale, nel **menu a discesa Zona di disponibilità** posizionare un segno di spunta accanto a **Zona 2**. Questa opzione deve selezionare sia La zona 1** che ****la zona 2**.

    >**Nota**: verranno distribuite due macchine virtuali nell'area selezionata, una in ogni zona. Si ottiene il contratto di servizio con tempo di attività del 99,99% perché sono distribuite almeno due macchine virtuali in almeno due zone. Nello scenario in cui potrebbe essere necessaria una sola macchina virtuale, è consigliabile distribuire la macchina virtuale in una zona per assicurarsi che il disco e le risorse corrispondenti si trovino nella stessa zona.

1. Nella scheda Informazioni di base usare le impostazioni seguenti per completare i campi (lasciare gli altri con i valori predefiniti):

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | nome della sottoscrizione di Azure |
    | Gruppo di risorse |  **az104-rg8** (se necessario, fare clic su **Crea nuovo**) |
    | Nomi delle macchine virtuali | `az104-vm1` e `az104-vm2` (Dopo aver selezionato entrambe le zone di disponibilità, selezionare **Modifica nomi** nel campo Nome macchina virtuale. |
    | Area geografica | **Stati Uniti orientali** |
    | Opzioni di disponibilità | **Zona di disponibilità** |
    | Zona di disponibilità | **Zona 1, 2** (leggere la nota sull'uso dei set di scalabilità di macchine virtuali) |
    | Tipo di sicurezza | **Standard** |
    | Image | **Ubuntu Server 20.04 LTS - x64 Gen2** |
    | Istanza Spot di Azure | **unchecked** |
    | Dimensione | **Standard D2s v3** |
    | Tipo di autenticazione | **Password** |
    | Username | `localadmin` |
    | Password | **Specificare una password sicura** |
    | Porte in ingresso pubbliche | **Nessuno** |
    | Usare una licenza esistente di Windows Server? | **Non selezionato** |

    ![Screenshot della pagina crea macchina virtuale.](../media/az104-lab08-create-vm.png)

1. Fare clic su **Avanti: Dischi** e quindi nella scheda **Dischi** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di disco del sistema operativo | **SSD Premium** |
    | Abilita compatibilità disco Ultra | **Non selezionato** |

1. Fare clic su **Avanti: Rete >** accettare le impostazioni predefinite, ma non fornire un servizio di bilanciamento del carico. 
   
    | Opzioni di bilanciamento del carico | **Nessuno** |
    
1. Selezionare **Avanti: Gestione >** e quindi nella scheda **Gestione** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Opzioni di orchestrazione patch | **Azure orchestrato** |  

1. Selezionare **Avanti: Monitoraggio>** e nella scheda **Monitoraggio** del pannello **Crea macchina virtuale** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Diagnostica di avvio | **Disabilita** |

1. Fare clic su **Avanti: Avanzate >**, quindi nella scheda **Avanzate** del pannello **Crea macchina virtuale** esaminare le impostazioni disponibili senza modificarle e fare clic su **Rivedi e crea**.

1. Nel pannello **Rivedi e crea** fare clic su **Crea**.

    >**Nota:** monitorare i **messaggi di notifica** e attendere il completamento della distribuzione. 

## Attività 2: Gestire il ridimensionamento delle risorse di calcolo e archiviazione per le macchine virtuali

In questa attività si ridimensiona il calcolo per una macchina virtuale modificandone le dimensioni in uno SKU diverso. Azure offre flessibilità nella selezione delle dimensioni delle macchine virtuali, in modo da poter modificare una macchina virtuale per periodi di tempo se richiede più o meno risorse di calcolo e memoria allocate. Questo concetto viene esteso ai dischi, in cui è possibile modificare le prestazioni del disco o aumentare la capacità allocata.

1. Nella portale di Azure cercare e selezionare **az104-vm1**.

1. Nel pannello **az104-vm1** macchina virtuale fare clic su **Dimensioni** e impostare le dimensioni della macchina virtuale su **DS1_v2** e fare clic su **Ridimensiona**

    >**Nota**: scegliere un'altra dimensione se **Standard DS1_v2** non è disponibile.

    ![Screenshot del ridimensionamento della macchina virtuale.](../media/az104-lab08-resize-vm.png)

1. Nel pannello **az104-vm1** macchina virtuale fare clic su Dischi **, in **Dischi dati** fare clic **su **+ Crea e collegare un nuovo disco**.

1. Creare un disco gestito con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Disk name | `vm1-disk1` |
    | Tipo di archiviazione | **Unità disco rigido Standard** |
    | Dimensioni (GB) | `32` |

1. Fare clic su **Applica**.

1. Dopo aver creato il disco, fare clic su **Scollega** e quindi su **Applica**.
    
    >**Nota**: potrebbe essere necessario scorrere verso destra per visualizzare l'icona *di scollegamento* .

1. Nella portale di Azure cercare e selezionare `Disks`.

1. Nell'elenco dei dischi selezionare l'oggetto **vm1-disk1** .

1. Da vm1-disk1 selezionare **Dimensioni e prestazioni**.

1. In Dimensioni e prestazioni impostare il tipo di **archiviazione su SSD** Standard e quindi fare clic su **Salva**.

    >**Nota**: non è possibile modificare il tipo di archiviazione del disco mentre è collegato o mentre la macchina virtuale è in esecuzione. 

1. Tornare alla **macchina virtuale az104-vm1** e selezionare **Dischi**.

1. Verificare che il disco sia ora **HDD** Standard.

## Attività 3 e 4: Diagramma dell'architettura di Azure set di scalabilità di macchine virtuali

![Diagramma delle attività di architettura.](../media/az104-lab08b-architecture-diagram.png)

## Attività 3: Implementare Azure set di scalabilità di macchine virtuali

In questa attività si distribuirà un set di scalabilità di macchine virtuali di Azure tra zone di disponibilità. Con le singole macchine virtuali, è necessario un'altra automazione per distribuire e configurare macchine virtuali aggiuntive se l'applicazione necessita di risorse di calcolo aggiuntive. I set di scalabilità di macchine virtuali riducono il sovraccarico amministrativo dell'automazione consentendo di configurare metriche o condizioni che consentono al set di scalabilità di aumentare o ridurre automaticamente il numero di macchine virtuali nel set.

1. Nella portale di Azure cercare e selezionare `Virtual machine scale sets` e nel pannello **Set di scalabilità** di macchine virtuali fare clic su **+ Crea**.

1. Nella **scheda Informazioni di base** del **pannello Crea un set** di scalabilità di macchine virtuali specificare le impostazioni seguenti (lasciare gli altri con i valori predefiniti) e fare clic su **Avanti : Spot >**:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | nome della sottoscrizione di Azure  |
    | Gruppo di risorse | **az104-rg8**  |
    | Nome del set di scalabilità di macchine virtuali | `vmss1` |
    | Area | **Stati Uniti** orientali (o un'area nelle vicinanze) |
    | Zona di disponibilità | **Zone 1, 2, 3** |
    | Modalità di orchestrazione | **Uniforme** |
    | Tipo di sicurezza | **Standard** | 
    | Image | **Windows Server 2019 Datacenter - x64 Gen2** |
    | Eseguire con sconto di Spot Azure | **Non selezionato** |
    | Dimensione | **Standard D2s_v3** |
    | Username | `localadmin` |
    | Password | **Specificare una password sicura**  |
    | Si dispone già di una licenza di Windows Server? | **Non selezionato** |

    >**Nota**: per l'elenco delle aree di Azure che supportano la distribuzione di macchine virtuali Windows nelle zone di disponibilità, vedere [Che cosa sono le zone di disponibilità in Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

    ![Screenshot della pagina crea vmss. ](../media/az104-lab08-create-vmss.png)

1. Nella **scheda Spot** accettare le impostazioni predefinite e selezionare **Avanti: Dischi >**.

1. Nella **scheda Dischi** accettare i valori predefiniti e fare clic su **Avanti : Rete >**.

1. Nella **scheda Rete** fare clic sul **collegamento Crea rete** virtuale sotto la **casella di testo Rete** virtuale e creare una nuova rete virtuale con le impostazioni seguenti (lasciare altri valori predefiniti).  Al termine selezionare **OK**. 

    | Impostazione | valore |
    | --- | --- |
    | Nome | `vmss-vnet` |
    | Intervallo di indirizzi | `10.82.0.0/20` |
    | Nome subnet | `subnet0` |
    | Intervallo di subnet | `10.82.0.0/24` |

1. **Nella scheda Rete** fare clic sull'icona **Modifica interfaccia** di rete a destra della voce dell'interfaccia di rete.

1. Nel pannello **Modifica interfaccia di rete**, nella sezione **Gruppo di sicurezza di rete della scheda di interfaccia di rete**, fare clic su **Avanzate** e quindi su **Crea nuovo** sotto l'elenco a discesa **Configura gruppo di sicurezza di rete**.

1. Nel pannello **Crea gruppo di sicurezza di rete** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | valore |
    | --- | --- |
    | Nome | **vmss1-nsg** |

1. Fare clic su **Aggiungi una regola in ingresso** e aggiungere una regola di sicurezza in ingresso con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Origine | **Any** |
    | Intervalli porte di origine | * |
    | Destinazione | **Any** |
    | Service | **HTTP** |
    | Azione | **Consenti** |
    | Priorità | **1010** |
    | Nome | `allow-http` |

1. Fare clic su **Aggiungi** e quindi, di nuovo nel pannello **Crea gruppo di sicurezza di rete**, fare clic su **OK**.

1. Nella sezione Indirizzo** IP pubblico del **pannello **Modifica interfaccia** di rete fare clic su **Abilitato** e fare clic su **OK**.

1. **Nella scheda Rete**, nella sezione Bilanciamento** del **carico, specificare quanto segue (lasciare gli altri con i valori predefiniti).

    | Impostazione | Valore |
    | --- | --- |
    | Opzioni di bilanciamento del carico | **Azure Load Balancer** |
    | Selezionare un servizio di bilanciamento del carico | **Creare un servizio di bilanciamento del carico** |
    
1.  Nella pagina Crea un servizio di bilanciamento** del **carico specificare il nome del servizio di bilanciamento del carico e accettare le impostazioni predefinite. Al termine, fare clic su Crea** e quindi **su **Avanti : Ridimensionamento >**.
    
    | Impostazione | Valore |
    | --- | --- |
    | Nome del servizio di bilanciamento del carico | `vmss-lb` |

1. Nella **scheda Ridimensionamento** specificare le impostazioni seguenti (lasciare altri valori predefiniti) e fare clic su **Avanti : Gestione >**:

    | Impostazione | Valore |
    | --- | --- |
    | Numero di istanze iniziale | `2` |
    | Criteri di ridimensionamento | **Manualee** |

1. Nella **scheda Gestione** specificare le impostazioni seguenti (lasciare altri valori predefiniti):

    | Impostazione | Valore |
    | --- | --- |
    | Diagnostica di avvio | **Disabilita** |
    
1. Fare clic su **Avanti: Integrità >**:

1. Nella scheda Integrità **** esaminare le impostazioni predefinite senza apportare modifiche e fare clic su **Avanti : Avanzate >**.

1. Nella **scheda Avanzate** fare clic su **Rivedi e crea**.

1. **Nella scheda Rivedi e crea** verificare che la convalida sia stata superata e fare clic su **Crea**.

    >**Nota**: attendere il completamento della distribuzione del set di scalabilità di macchine virtuali. L'operazione dovrebbe richiedere circa 5 minuti.


## Attività 4: Ridimensionare i set di scalabilità di macchine virtuali di Azure

In questa attività si ridimensiona il set di scalabilità di macchine virtuali usando una regola di scalabilità personalizzata.

1. Selezionare **Vai alla risorsa** o cercare e selezionare il **set di scalabilità vmss1** .

1. Scegliere **Proporzioni** dal menu nella parte sinistra della finestra del set di scalabilità.

1. Si noti che la **modalità** di scalabilità può essere **ridimensionata in base alle metriche o **alla scalabilità** a un numero** di istanze specifico. Nei set di scalabilità con un numero ridotto di istanze di macchine virtuali, l'aumento o la riduzione del numero di istanze può essere ottimale. Nei set di scalabilità con un numero elevato di istanze di macchine virtuali, il ridimensionamento basato sulle metriche potrebbe essere più appropriato.

1. Selezionare il pulsante **su Scalabilità** automatica personalizzata. Selezionare quindi **Aggiungi una regola**. 

### Regola di aumento del numero di istanze

1. Si creerà una regola di scalabilità orizzontale che aumenta automaticamente il numero di istanze di macchine virtuali. Questa regola aumenta il numero di istanze quando il carico medio della CPU è maggiore del 70% in un periodo di 10 minuti. Quando la regola viene attivata, il numero di istanze di macchine virtuali viene aumentato del 20%. Fare clic su **Aggiungi** dopo aver effettuato le selezioni. 

    | Impostazione | Valore |
    | --- | --- |
    | Origine della metrica | **Risorsa corrente (vmss1)** |
    | Spazio dei nomi delle metriche | **Host macchina virtuale** |
    | Nome metrica | **Percentuale CPU** (rivedere le altre scelte) |
    | Operatore | **Maggiore di** |
    | Soglia della metrica per l'attivazione dell'azione di dimensionamento | **70** |
    | Durata (minuti) | **10** |
    | Statistica intervallo di tempo | **Media** |
    | Operazione | **Aumentare la percentuale per** (rivedere altre scelte) |
    | Disattiva regole dopo (minuti) | **5** |
    | Percentuale | **20** |
    
    ![Screenshot della pagina aggiungi regola di ridimensionamento.](../media/az104-lab08-scale-rule.png)

### Regola di scalabilità orizzontale

1. Durante le serate o i fine settimana, la domanda può diminuire, quindi è importante creare una regola di ridimensionamento.

1. Si creerà una regola che riduce il numero di istanze di macchine virtuali in un set di scalabilità. Il numero di istanze deve diminuire quando il carico medio della CPU scende al di sotto del 30% in un periodo di 10 minuti. Quando la regola viene attivata, il numero di istanze di macchine virtuali viene diminuito del 20%. Modificare le impostazioni e quindi selezionare **Aggiungi**.

    | Impostazione | Valore |
    | --- | --- |
    | Operatore | **Minore di** |
    | Threshold | **30** |
    | Operazione | **ridurre la percentuale per** (esaminare le altre scelte) |
    | Numero di istanze | **20** |

### Impostare i limiti dell'istanza

1. Quando vengono applicate le regole di scalabilità automatica, i limiti dell'istanza assicurano che non si esesca oltre il numero massimo di istanze o che il numero di istanze sia superiore al numero minimo di istanze.

1. **I limiti** delle istanze vengono visualizzati nella **pagina Ridimensionamento** dopo le regole. 

    | Impostazione | Valore |
    | --- | --- |
    | Requisiti minimi | **2** |
    | Massimo | **10** |
    | Valori predefiniti | **2** |

1. Assicurarsi di **salvare** le modifiche

1. Nella **pagina vmss1** selezionare **Istanze**. In questo modo è possibile monitorare il numero di istanze di macchina virtuale. 

## Attività 5: Creare una macchina virtuale con Azure PowerShell (opzione 1)

1. Accedere al portale di Azure - `https://portal.azure.com`.

1. Usare il menu per avviare una **sessione di Cloud Shell** . In alternativa, passare direttamente a `https://shell.azure.com`.

1. Se necessario, configurare Cloud Shell. Assicurarsi di selezionare **PowerShell**.

1. Eseguire il comando seguente per creare una macchina virtuale. Quando richiesto, specificare un nome utente e una password per la macchina virtuale. Durante l'attesa, vedere le informazioni di riferimento sui [comandi New-AzVM](https://learn.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-11.1.0) per tutti i parametri associati alla creazione di una macchina virtuale.

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' 
    -Credential '(Get-Credential)' `

1. Once the command completes, use **Get-AzVM** to list the virtual machines in your resource group. 

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-rg8'
    -Status

1. Verify your new virtual machine is listed and the **Status** is **Running**.

1. Use **Stop-AzVM** to deallocate your virtual machine. Type **Yes** to confirm. 

    ```
    Stop-AzVM `
    -ResourceGroupName 'az104-rg8'
    -Name 'myPSVM' `

1. Usare **Get-AzVM** con il **parametro -Status** per verificare che il computer sia **deallocato**.

    >**Lo sapevi?** Quando si usa Azure per arrestare la macchina virtuale, lo stato viene *deallocato*. Ciò significa che vengono rilasciati indirizzi IP pubblici non statici e si interrompe il pagamento dei costi di calcolo della macchina virtuale.

## Attività 6: Creare una macchina virtuale usando l'interfaccia della riga di comando (opzione 2)

1. Accedere al portale di Azure - `https://portal.azure.com`.

1. Usare il menu per avviare una **sessione di Cloud Shell** . In alternativa, passare direttamente a `https://shell.azure.com`.

1. Se necessario, configurare Cloud Shell. Assicurarsi di selezionare **Bash**.

1. Eseguire il comando seguente per creare una macchina virtuale. Quando richiesto, specificare un nome utente e una password per la macchina virtuale. Durante l'attesa, vedere il riferimento al [comando az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) per tutti i parametri associati alla creazione di una macchina virtuale.


    ```sh
    az vm create --name myCLIVM --resource-group az104-rg8 --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys 
    
1. Once the command completes, use **az vm show** to verify your machine was created.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-rg8 --show-details

1. Verify the **powerState** is **VM Running**.

1. Use **az vm deallocate** to deallocate your virtual machine. Type **Yes** to confirm. 

    ```sh
    az vm deallocate --resource-group az104-rg8 --name myCLIVM

1. Use **az vm show** to ensure the **powerState** is **VM deallocated**.

    >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure virtual machines are on-demand, scalable computing resources.
+ Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings. 
+ Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
+ The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration. 
+ In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.

## Cleanup your resources

If you are working with your own subscription take a minute to delete the lab resources. This will ensure resources are freed up and cost is minimized. The easiest way to delete the lab resources is to delete the lab resource group. 

+ In the Azure portal, select the resource group, select **Delete the resource group**, **Enter resource group name**, and then click **Delete**.
+ Using Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Using the CLI, `az group delete --name resourceGroupName`.


