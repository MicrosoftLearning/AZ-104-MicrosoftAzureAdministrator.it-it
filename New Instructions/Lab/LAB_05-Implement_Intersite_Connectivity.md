---
lab:
  title: 'Lab 05: Implementare Connessione ivity tra siti'
  module: Administer Intersite Connectivity
---

# Lab 05 - Implementare la connettività tra siti

## Introduzione al lab

In questo lab verranno esaminate le comunicazioni tra reti virtuali. Si implementerà il peering di rete virtuale ed si eseguiranno comandi remoti per testare le connessioni.   

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali. 

## Tempo stimato: 30 minuti

## Scenario laboratorio 

L'organizzazione segmenta le app e i servizi IT di base (ad esempio i servizi DNS e di sicurezza) di altre parti dell'azienda, incluso il reparto di produzione. Tuttavia, in alcuni scenari, le app e i servizi nell'area principale devono comunicare con app e servizi nell'area di produzione. In questo lab viene configurata la connettività tra le aree segmentate. Questo è uno scenario comune per separare la produzione dallo sviluppo o separare una filiale da un'altra.

## Simulazioni di lab interattive

Esistono diverse simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Connessione due reti virtuali di Azure usando il peering](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering) di rete virtuale globale. Testare la connessione tra due macchine virtuali in reti virtuali diverse. Creare un peering di rete virtuale e un retest.
+ [Implementare la connettività](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209) tra siti. Eseguire un modello per creare un'infrastruttura di rete virtuale con diverse macchine virtuali. Configurare i peering di rete virtuale e testare le connessioni. 

## Diagramma dell'architettura

![Diagramma dell'architettura di Lab 05](../media/az104-lab05-architecture-diagram.png)

## Attività

+ Attività 1: Creare una macchina virtuale e una rete virtuale di servizi di base.
+ Attività 2: Creare una macchina virtuale e una rete virtuale di servizi di produzione.
+ Attività 3: Testare la connessione tra le macchine virtuali. 
+ Attività 4: Creare peering reti virtuali tra le reti virtuali. 
+ Attività 5: Ripetere la connessione tra le macchine virtuali. 
 

## Attività 1: Creare una macchina virtuale e una rete virtuale di servizi di base

In questa attività viene creata una rete virtuale di servizi di base con una macchina virtuale. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Virtual Machines`.

1. Nella pagina macchine virtuali selezionare **Crea** e quindi selezionare **Macchina** virtuale di Azure.

1. Nella scheda Informazioni di base usare le informazioni seguenti per completare il modulo e quindi selezionare **Avanti: Dischi >**. Per qualsiasi impostazione non specificata, lasciare il valore predefinito.
 
    | Impostazione | Valore | 
    | --- | --- |
    | Subscription |  *sottoscrizione in uso* |
    | Gruppo di risorse |  `az104-rg5` (Se necessario, selezionare **Crea nuovo**. Usare questo gruppo per tutte le risorse del lab.
    | Virtual machine name |    `CoreServicesVM` |
    | Area geografica | **Stati Uniti orientali** |
    | Opzioni di disponibilità | La ridondanza dell'infrastruttura non è richiesta |
    | Immagine | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Dimensione | **Standard_DS2_v3** |
    | Username | `localadmin` | 
    | Password | **Specificare una password complessa** |

    ![Screenshot della pagina di creazione di macchine virtuali di base. ](../media/az104-lab05-createcorevm.png)
   
1. Nella scheda Dischi impostare il tipo di **disco del sistema operativo su HDD** Standard e quindi selezionare **Avanti: Rete >**.

1. Nella scheda Rete selezionare **Crea nuovo** per Rete virtuale.

1. Usare le informazioni seguenti per configurare la rete virtuale e quindi selezionare **OK**. Se necessario, rimuovere o sostituire l'intervallo di indirizzi esistente.

    | Impostazione | valore | 
    | --- | --- |
    | Nome | `CoreServicesVNet` (Crea nuovo) |
    | Spazio indirizzi | `10.0.0.0/16`  |
    | Nome della subnet | `Core` | 
    | Intervallo di indirizzi subnet | `10.0.0.0/24` |

1. Selezionare la **scheda Monitoraggio** . Per Diagnostica di avvio selezionare **Disabilita**.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Non è necessario attendere la creazione delle risorse. Continuare con l'attività successiva.

    >**Nota:** in questa attività è stata creata la rete virtuale durante la creazione della macchina virtuale? 

## Attività 2: Creare una macchina virtuale e una rete virtuale di servizi di produzione

In questa attività viene creata una rete virtuale di servizi di produzione con una macchina virtuale. 

1. Dal portale di Azure cercare e passare a **Macchine virtuali**.

1. Nella pagina macchine virtuali selezionare **Crea** e quindi selezionare **Macchina** virtuale di Azure.

1. Nella scheda Informazioni di base usare le informazioni seguenti per completare il modulo e quindi selezionare **Avanti: Dischi >**. Per qualsiasi impostazione non specificata, lasciare il valore predefinito.
 
    | Impostazione | Valore | 
    | --- | --- |
    | Subscription |  *sottoscrizione in uso* |
    | Gruppo di risorse |  `az104-rg5` |
    | Virtual machine name |    `ManufacturingVM` |
    | Area geografica | **Stati Uniti orientali** |
    | Opzioni di disponibilità | La ridondanza dell'infrastruttura non è richiesta |
    | Immagine | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Dimensione | **Standard_DS2_v3** | 
    | Username | `localadmin` | 
    | Password | **Specificare una password complessa** |

1. Nella scheda Dischi impostare il tipo di **disco del sistema operativo su HDD** Standard e quindi selezionare **Avanti: Rete >**.

1. Nella scheda Rete selezionare **Crea nuovo** per Rete virtuale.

1. Usare le informazioni seguenti per configurare la rete virtuale e quindi selezionare **OK**.  Se necessario, rimuovere o sostituire l'intervallo di indirizzi esistente.

    | Impostazione | valore | 
    | --- | --- |
    | Nome | `ManufacturingVNet` |
    | Spazio indirizzi | `172.16.0.0/16`  |
    | Nome della subnet | `Manufacturing` |
    | Intervallo di indirizzi subnet | `172.16.0.0/24` |

1. Selezionare la **scheda Monitoraggio** . Per Diagnostica di avvio selezionare **Disabilita**.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

## Attività 3: Testare la connessione tra le macchine virtuali

In questa attività viene testata la connessione tra le macchine virtuali in reti virtuali diverse.

### Verificare l'indirizzo IP privato di CoreServicesVM

1. Nella portale di Azure cercare e selezionare la `CoreServicesVM` macchina virtuale.

1. Nella sezione Rete** del **pannello **Panoramica** registrare l'indirizzo **** IP privato del computer. Queste informazioni sono necessarie per testare la connessione.
   
### Testare la connessione a CoreServicesVM dalla **ManufacturingVM**.

1. Nel portale selezionare per e selezionare la `ManufacturingVM` macchina virtuale.

1. **Nella sezione Operazioni** selezionare il **pannello Esegui comando**.

1. Selezionare **RunPowerShellScript** ed eseguire il **comando Test-Net Connessione ion**. Assicurarsi di usare l'indirizzo IP privato di **CoreServicesVM**.

   ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
   ```
   
1. L'esecuzione dello script può richiedere alcuni minuti. Nella parte superiore della pagina viene visualizzato un messaggio *informativo Esecuzione script in corso.*
   
1. La connessione di test non riesce. Le macchine virtuali in reti virtuali diverse devono, per impostazione predefinita, non essere in grado di comunicare.
   
   ![Finestra di PowerShell con Test-Net Connessione ion non riuscita.](../media/az104-lab05-fail.png)

 
## Attività 4: Creare peering reti virtuali tra le reti virtuali

In questa attività vengono creati peering di rete virtuale per abilitare le comunicazioni tra reti virtuali.

1. Nella portale di Azure selezionare **Rete virtuale** e quindi CoreServicesVnet****.

1. In CoreServicesVnet, in **Impostazioni** selezionare **Peer**.

1. In CoreServicesVnet | Peer selezionare **+ Aggiungi**.

1. Usare le informazioni nella tabella seguente per creare il peering.

    | **Sezione**                          | **Opzione**                                    | **valore**                             |
    | ------------------------------------ | --------------------------------------------- | ------------------------------------- |
    | Questa rete virtuale                 |                                               |                                       |
    |                                      | Nome del collegamento di peering                             | `CoreServicesVnet-to-ManufacturingVnet` |
    |                                      | Consentire a CoreServicesVNet di accedere alla rete virtuale con peering            | selezionato (impostazione predefinita)                       |
    |                                      | Consentire a CoreServicesVNet di ricevere traffico inoltrato dalla rete virtuale con peering | Opzione selezionata                       |
    |                                      | Abilitare CoreServicesVNet per l'uso del gateway remoto delle reti virtuali con peering       | Non selezionato (impostazione predefinita)                        |
    | Rete virtuale remota               |                                               |                                       |
    |                                      | Nome del collegamento di peering                             | `ManufacturingVnet-to-CoreServicesVnet` |
    |                                      | Modello di distribuzione della rete virtuale              | **Resource Manager**                      |
    |                                      | Conosco l'ID della risorsa                         | Non selezionato                          |
    |                                      | Subscription                                  | *sottoscrizione in uso*    |
    |                                      | Rete virtuale                               | **ManufacturingVnet**                     |
    |                                      | Consentire a ManufacturingVNet di accedere a CoreServicesVNet  | selezionato (impostazione predefinita)                       |
    |                                      | Consentire a ManufacturingVNet di ricevere traffico inoltrato da CoreServicesVNet | Opzione selezionata                        |
    |                                      | Abilitare ManufacturingVNet per l'uso del gateway remoto di CoreServicesVNet       | Non selezionato (impostazione predefinita)                        |

1. Esaminare le impostazioni e selezionare **Aggiungi**.

    ![Screenshot della pagina di peering.](../media/az104-lab05-peering.png)
 
1. In CoreServicesVnet | Peer verificare che il peering **CoreServicesVnet-to-ManufacturingVnet** sia elencato. Aggiornare la pagina per assicurarsi che lo **stato** del peering sia **Connessione.**

1. Passare a **ManufacturingVnet** e verificare che sia elencato il **peering ManufacturingVnet-to-CoreServicesVnet** . Verificare che lo **stato** del peering sia **Connessione ed**.

 
## Attività 5: Testare la connessione tra le macchine virtuali

In questa attività si verifica che le macchine virtuali in reti virtuali diverse possano comunicare tra loro.

1. Cercare e selezionare **ManufacturingVM**.

1. **Nella sezione Operazioni** selezionare il **pannello Esegui comando**.

1. Selezionare **RunPowerShellScript** e aggiungere il comando Test-Net Connessione ion. Assicurarsi di usare l'indirizzo IP privato di **CoreServicesVM**.

      ```Powershell
     Test-NetConnection <CoreServicesVM private IP address> -port 3389
      ```

1. L'esecuzione dello script può richiedere alcuni minuti. Nella parte superiore della pagina viene visualizzata un'icona *informativa Esecuzione script in corso.*
    
1. La connessione di test deve avere esito positivo. 
   ![Finestra di PowerShell con Test-Net Connessione ion completata](../media/az104-lab05-success.png)


## Esaminare i punti principali del lab

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Per impostazione predefinita, le risorse in reti virtuali diverse non possono comunicare.
+ Il peering di reti virtuali consente di connettere facilmente due reti virtuali in Azure.
+ Le reti virtuali con peering vengono visualizzate come una a scopo di connettività.
+ Il traffico tra macchine virtuali nelle reti virtuali con peering usa l'infrastruttura backbone Microsoft.

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.

