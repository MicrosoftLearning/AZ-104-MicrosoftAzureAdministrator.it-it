---
lab:
  title: 'Lab 05: Implementare la connettività tra siti'
  module: Administer Intersite Connectivity
---

# Lab 05 - Implementare la connettività tra siti

## Introduzione al lab

In questo lab si esplora la comunicazione tra reti virtuali. Si implementano il peering di rete virtuale e si testano le connessioni. Si creerà anche una route personalizzata. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali. 

## Tempo stimato: 50 minuti
    
## Scenario laboratorio 

L'organizzazione segmenta le app e i servizi IT di base (ad esempio i servizi DNS e di sicurezza) di altre parti dell'azienda, incluso il reparto di produzione. Tuttavia, in alcuni scenari, le app e i servizi nell'area principale devono comunicare con app e servizi nell'area di produzione. In questo lab viene configurata la connettività tra le aree segmentate. Questo è uno scenario comune per separare la produzione dallo sviluppo o separare una filiale da un'altra.  

## Simulazioni di lab interattive

Esistono diverse simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Connessione due reti virtuali di Azure usando il peering](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering) di rete virtuale globale. Testare la connessione tra due macchine virtuali in reti virtuali diverse. Creare un peering di rete virtuale e un retest.

+ [Configurare il monitoraggio per le reti](https://learn.microsoft.com/training/modules/configure-monitoring-virtual-networks/) virtuali. Informazioni su come usare Monitoraggio connessione di Azure Network Watcher, log dei flussi, diagnostica del gruppo di sicurezza di rete e acquisizione di pacchetti per monitorare la connettività tra le risorse di rete IaaS di Azure.

+ [Implementare la connettività](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209) tra siti. Eseguire un modello per creare un'infrastruttura di rete virtuale con diverse macchine virtuali. Configurare i peering di rete virtuale e testare le connessioni. 

## Diagramma dell'architettura

![Diagramma dell'architettura di Lab 05](../media/az104-lab05-architecture.png)

## Competenze mansione

+ Attività 1: Creare una macchina virtuale in una rete virtuale.
+ Attività 2: Creare una macchina virtuale in una rete virtuale diversa.
+ Attività 3: Usare Network Watcher per testare la connessione tra macchine virtuali. 
+ Attività 4: Configurare i peering di rete virtuale tra reti virtuali diverse.
+ Attività 5: Usare Azure PowerShell per testare la connessione tra macchine virtuali.
+ Attività 6: Creare una route personalizzata. 

## Attività 1: Creare una macchina virtuale e una rete virtuale di servizi di base

In questa attività si crea una rete virtuale di servizi di base con una macchina virtuale. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Virtual Machines`.

1. Nella pagina macchine virtuali selezionare **Crea** e quindi selezionare **Macchina** virtuale di Azure.

1. Nella scheda Informazioni di base usare le informazioni seguenti per completare il modulo e quindi selezionare **Avanti: Dischi >**. Per qualsiasi impostazione non specificata, lasciare il valore predefinito.
 
    | Impostazione | Valore | 
    | --- | --- |
    | Subscription |  *sottoscrizione in uso* |
    | Gruppo di risorse |  `az104-rg5` (se necessario, **Crea nuovo**. )
    | Virtual machine name |    `CoreServicesVM` |
    | Paese | **(Stati Uniti) Stati Uniti orientali** |
    | Opzioni di disponibilità | La ridondanza dell'infrastruttura non è richiesta |
    | Tipo di sicurezza | **Standard** |
    | Immagine | **Windows Server 2019 Datacenter: x64 Gen2** (si notino le altre opzioni) |
    | Dimensione | **Standard_DS2_v3** |
    | Username | `localadmin` | 
    | Password | **Specificare una password complessa** |
    | Porte in ingresso pubbliche | **Nessuno** |

    ![Screenshot della pagina di creazione di macchine virtuali di base. ](../media/az104-lab05-createcorevm.png)
   
1. Nella **scheda Dischi accettare** le impostazioni predefinite e quindi selezionare **Avanti: Rete >**.

1. Nella **scheda Rete** selezionare **Crea nuovo** per Rete virtuale.

1. Usare le informazioni seguenti per configurare la rete virtuale e quindi selezionare **OK**. Se necessario, rimuovere o sostituire le informazioni esistenti.

    | Impostazione | valore | 
    | --- | --- |
    | Nome | `CoreServicesVNet` (Crea nuovo) |
    | Intervallo di indirizzi | `10.0.0.0/16`  |
    | Nome della subnet | `Core` | 
    | Intervallo di indirizzi subnet | `10.0.0.0/24` |

1. Selezionare la **scheda Monitoraggio** . Per Diagnostica di avvio selezionare **Disabilita**.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Non è necessario attendere la creazione delle risorse. Continuare con l'attività successiva.

    >**Nota:** in questa attività è stata creata la rete virtuale durante la creazione della macchina virtuale? È anche possibile creare l'infrastruttura di rete virtuale e quindi aggiungere le macchine virtuali. 

## Attività 2: Creare una macchina virtuale in una rete virtuale diversa

In questa attività si crea una rete virtuale di servizi di produzione con una macchina virtuale. 

1. Dal portale di Azure cercare e passare a **Macchine virtuali**.

1. Nella pagina macchine virtuali selezionare **Crea** e quindi selezionare **Macchina** virtuale di Azure.

1. Nella scheda Informazioni di base usare le informazioni seguenti per completare il modulo e quindi selezionare **Avanti: Dischi >**. Per qualsiasi impostazione non specificata, lasciare il valore predefinito.
 
    | Impostazione | Valore | 
    | --- | --- |
    | Subscription |  *sottoscrizione in uso* |
    | Gruppo di risorse |  `az104-rg5` |
    | Virtual machine name |    `ManufacturingVM` |
    | Paese | **(Stati Uniti) Stati Uniti orientali** |
    | Tipo di sicurezza | **Standard** |
    | Opzioni di disponibilità | La ridondanza dell'infrastruttura non è richiesta |
    | Immagine | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Dimensione | **Standard_DS2_v3** | 
    | Username | `localadmin` | 
    | Password | **Specificare una password complessa** |
    | Porte in ingresso pubbliche | **Nessuno** |

1. Nella **scheda Dischi accettare** le impostazioni predefinite e quindi selezionare **Avanti: Rete >**.

1. Nella scheda Rete selezionare **Crea nuovo** per Rete virtuale.

1. Usare le informazioni seguenti per configurare la rete virtuale e quindi selezionare **OK**.  Se necessario, rimuovere o sostituire l'intervallo di indirizzi esistente.

    | Impostazione | valore | 
    | --- | --- |
    | Nome | `ManufacturingVNet` |
    | Intervallo di indirizzi | `172.16.0.0/16`  |
    | Nome della subnet | `Manufacturing` |
    | Intervallo di indirizzi subnet | `172.16.0.0/24` |

1. Selezionare la **scheda Monitoraggio** . Per Diagnostica di avvio selezionare **Disabilita**.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

## Attività 3: Usare Network Watcher per testare la connessione tra macchine virtuali 


In questa attività si verifica che le risorse nelle reti virtuali con peering possano comunicare tra loro. Network Watcher verrà usato per testare la connessione. Prima di continuare, assicurarsi che entrambe le macchine virtuali siano state distribuite e siano in esecuzione. 

1. Nella portale di Azure cercare e selezionare `Network Watcher`.

1. In Network Watcher, nel menu Strumenti di diagnostica di rete selezionare **Connessione risoluzione dei problemi**.

1. Usare le informazioni seguenti per completare i campi nella **pagina di risoluzione dei problemi** di Connessione ion.

    | Campo | Valore | 
    | --- | --- |
    | Source type           | **Macchina virtuale**   |
    | Macchina virtuale       | **CoreServicesVM**    | 
    | Tipo destinazione      | **Macchina virtuale**   |
    | Macchina virtuale       | **ManufacturingVM**   | 
    | Versione IP preferita  | **Entrambi**              | 
    | Protocollo              | **TCP**               |
    | Porta di destinazione      | `3389`                |  
    | Porta di origine           | *Blank*         |
    | Test di diagnostica      | *Defaults*      |

    ![Portale di Azure che mostra le impostazioni di risoluzione dei problemi di Connessione.](../media/az104-lab05-connection-troubleshoot.png)

1. Selezionare **Esegui test diagnostici**.

    >**Nota**: la restituzione dei risultati può richiedere alcuni minuti. Le selezioni dello schermo verranno disattivate durante la raccolta dei risultati. Si noti che il test** di **Connessione ivity mostra **UnReachable**. Ciò ha senso perché le macchine virtuali si trovano in reti virtuali diverse. 

 
## Attività 4: Configurare i peering di rete virtuale tra reti virtuali

In questa attività viene creato un peering di rete virtuale per abilitare le comunicazioni tra le risorse nelle reti virtuali. 

1. Nella portale di Azure selezionare la `CoreServicesVnet` rete virtuale.

1. In CoreServicesVnet, in **Impostazioni** selezionare **Peer**.

1. In CoreServicesVnet | Peer selezionare **+ Aggiungi**.

1. Usare le informazioni nella tabella seguente per creare il peering.

| **Parametro**                                    | **valore**                             |
| --------------------------------------------- | ------------------------------------- |
| **Questa rete virtuale**                                       |                                       |
| Nome del collegamento di peering                             | `CoreServicesVnet-to-ManufacturingVnet` |
| Consentire a CoreServicesVNet di accedere alla rete virtuale con peering            | selezionato (impostazione predefinita)                       |
| Consentire a CoreServicesVNet di ricevere traffico inoltrato dalla rete virtuale con peering | Opzione selezionata                       |
| Consentire al gateway in CoreServicesVNet di inoltrare il traffico alla rete virtuale con peering | Non selezionato (impostazione predefinita) |
| Abilitare CoreServicesVNet per l'uso del gateway remoto delle reti virtuali con peering       | Non selezionato (impostazione predefinita)                        |
| **Rete virtuale remota**                                   |                                       |
| Nome del collegamento di peering                             | `ManufacturingVnet-to-CoreServicesVnet` |
| Modello di distribuzione della rete virtuale              | **Resource Manager**                      |
| Conosco l'ID della risorsa                         | Non selezionato                          |
| Subscription                                  | *sottoscrizione in uso*    |
| Rete virtuale                               | **ManufacturingVnet**                     |
| Consentire a ManufacturingVNet di accedere a CoreServicesVNet  | selezionato (impostazione predefinita)                       |
| Consentire a ManufacturingVNet di ricevere traffico inoltrato da CoreServicesVNet | Opzione selezionata                        |
| Consentire al gateway in CoreServicesVNet di inoltrare il traffico alla rete virtuale con peering | Non selezionato (impostazione predefinita) |
| Abilitare ManufacturingVNet per l'uso del gateway remoto di CoreServicesVNet       | Non selezionato (impostazione predefinita)                        |

1. Esaminare le impostazioni e selezionare **Aggiungi**.

![Screenshot della pagina di peering.](../media/az104-lab05-peering.png)

 
1. In CoreServicesVnet | Peer verificare che il peering **CoreServicesVnet-to-ManufacturingVnet** sia elencato. Aggiornare la pagina per assicurarsi che lo **stato** del peering sia **Connessione.**

1. Passare a **ManufacturingVnet** e verificare che sia elencato il **peering ManufacturingVnet-to-CoreServicesVnet** . Verificare che lo **stato** del peering sia **Connessione ed**. Può essere necessario **aggiornare** la pagina. 


## Attività 5: Usare Azure PowerShell per testare la connessione tra macchine virtuali

In questa attività si ripete la connessione tra le macchine virtuali in reti virtuali diverse. 

### Verificare l'indirizzo IP privato di CoreServicesVM

1. Nella portale di Azure cercare e selezionare la `CoreServicesVM` macchina virtuale.

1. Nella sezione Rete** del **pannello **Panoramica** registrare l'indirizzo **** IP privato del computer. Queste informazioni sono necessarie per testare la connessione.
   
### Testare la connessione a CoreServicesVM dalla **ManufacturingVM**.

>**Lo sapevi?** Esistono molti modi per controllare le connessioni. In questa attività si usa il **comando** Esegui. È anche possibile continuare a usare Network Watcher. In alternativa, è possibile usare un [Connessione](https://learn.microsoft.com/azure/virtual-machines/windows/connect-rdp#connect-to-the-virtual-machine) Desktop remoto per accedere alla macchina virtuale. Dopo la connessione, usare **test-connection**. Man mano che hai tempo, prova RDP. 

1. Passare alla `ManufacturingVM` macchina virtuale.

1. Nel pannello **Operazioni** selezionare il **pannello Esegui comando** .

1. Selezionare **RunPowerShellScript** ed eseguire il **comando Test-Net Connessione ion**. Assicurarsi di usare l'indirizzo IP privato di **CoreServicesVM**.

    ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
    ```
1. Il timeout dello script potrebbe richiedere alcuni minuti. Nella parte superiore della pagina viene visualizzato un messaggio *informativo Esecuzione script in corso.*

   
1. La connessione di test dovrebbe avere esito positivo perché il peering è stato configurato. Il nome del computer e l'indirizzo remoto in questo elemento grafico potrebbero essere diversi. 
   
   ![Finestra di PowerShell con Test-Net Connessione ion completata.](../media/az104-lab05-success.png)

## Attività 6: Creare una route personalizzata 

In questa attività si vuole controllare il traffico di rete tra la subnet perimetrale e la subnet dei servizi principali interni. Un'appliance di rete virtuale verrà installata nella subnet dei servizi di base e tutto il traffico deve essere instradato lì. 

1. Cercare selezionare .`CoreServicesVnet`

1. Selezionare **Subnet** e quindi **+ Crea**. Assicurarsi di **salvare** le modifiche. 

    | Impostazione | valore | 
    | --- | --- |
    | Nome | `perimeter` |
    | Intervallo di indirizzi subnet | `10.0.1.0/24`  |

   
1. Nella portale di Azure cercare e selezionare `Route tables`e quindi selezionare **Crea**. 

    | Impostazione | Valore | 
    | --- | --- |
    | Subscription | sottoscrizione in uso |
    | Gruppo di risorse | `az104-rg5`  |
    | Area geografica | **Stati Uniti orientali** |
    | Nome | `rt-CoreServices` |
    | Propaga route del gateway | **No** |

1. Dopo la distribuzione della tabella di route, selezionare **Vai alla risorsa**.

1. Selezionare **Route e quindi **+ Aggiungi****. Creare una route dall'appliance virtuale di rete futura alla rete virtuale CoreServices. 

    | Impostazione | Valore | 
    | --- | --- |
    | Nome route | `PerimetertoCore` |
    | Tipo destinazione | **Indirizzi IP** |
    | Indirizzi IP di destinazione | `10.0.0.0/16` (rete virtuale dei servizi di base) |
    | Tipo hop successivo | **** Appliance virtuale (si notino le altre opzioni) |
    | Indirizzo hop successivo | `10.0.1.7` (appliance virtuale di rete futura) |

1. Selezionare **+ Aggiungi** al termine della route. L'ultima operazione da eseguire è associare la route alla subnet.

1. Selezionare **Subnet** e quindi **Associa**. Completare la configurazione.

    | Impostazione | Valore | 
    | --- | --- |
    | Rete virtuale | **CoreServicesVnet** |
    | Subnet | **Core** |    

>**Nota**: è stata creata una route definita dall'utente per indirizzare il traffico dalla rete perimetrale alla nuova appliance virtuale di rete.  

## Pulire le risorse

Se si usa **la propria sottoscrizione** , è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.


## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Per impostazione predefinita, le risorse in reti virtuali diverse non possono comunicare.
+ Il peering di reti virtuali consente di connettere facilmente due reti virtuali in Azure.
+ Le reti virtuali con peering vengono visualizzate come una a scopo di connettività.
+ Il traffico tra macchine virtuali nelle reti virtuali con peering usa l'infrastruttura backbone Microsoft.
+ Le route definite dal sistema vengono create automaticamente per ogni subnet in una rete virtuale. Le route definite dall'utente eseguono l'override o aggiungono alle route di sistema predefinite. 
+ Azure Network Watcher offre una suite di strumenti per monitorare, diagnosticare e visualizzare metriche e log per le risorse IaaS di Azure.

## Altre informazioni con la formazione autogestita

+ [Distribuire i servizi tra reti virtuali di Azure e integrarli usando il peering di](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/) rete virtuale. Usare il peering di rete virtuale per consentire le comunicazioni tra le reti virtuali in modo sicuro e con la massima semplicità.
+ [Gestire e controllare il flusso del traffico nella distribuzione di Azure con route](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). Informazioni su come controllare il traffico di rete virtuale di Azure implementando route personalizzate.
