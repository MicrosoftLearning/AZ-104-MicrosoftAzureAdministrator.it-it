---
lab:
  title: 'Lab 06: Implementare la gestione del traffico'
  module: Administer Network Traffic Management
---

# Lab 06 - Implementare Gestione del traffico
# Manuale del lab per studenti

## Scenario del lab

Si è stati incaricati di testare la gestione del traffico di rete per le macchine virtuali di Azure nella topologia di rete hub-spoke, che Contoso considera di implementare nell'ambiente Azure (anziché creare la topologia mesh, testata nel lab precedente). Questo test deve includere l'implementazione della connettività tra spoke basandosi su route definite dall'utente che forzano il flusso del traffico tramite l'hub, nonché la distribuzione del traffico tra macchine virtuali usando i servizi di bilanciamento del carico di livello 4 e 7. A questo scopo, si prevede di usare Azure Load Balancer (livello 4) e il gateway applicazione di Azure (livello 7).

                **Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi. 

>**Nota**: per impostazione predefinita, questo lab richiede un totale di 8 vCPU disponibili nella serie Standard_Dsv3 nell'area scelta per la distribuzione, perché prevede la distribuzione di quattro macchine virtuali di Azure di Standard_D2s_v3 SKU. Se gli studenti usano account di valutazione con un limite di 4 vCPU è possibile usare una dimensione di macchina virtuale che richieda una sola vCPU (ad esempio Standard_B1s).

## Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Effettuare il provisioning dell'ambiente lab
+ Attività 2: Configurare la topologia di rete hub-spoke
+ Attività 3: Testare la transitività del peering di reti virtuali
+ Attività 4: Configurare il routing nella topologia hub-spoke
+ Attività 5: Implementare Azure Load Balancer
+ Attività 6: Implementare il gateway applicazione di Azure

## Tempo stimato: 60 minuti

## Diagramma dell'architettura

![image](../media/lab06.png)


### Istruzioni

## Esercizio 1

## Attività 1: Effettuare il provisioning dell'ambiente lab

In questa attività si distribuiranno quattro macchine virtuali nella stessa area di Azure. Le prime due risiederanno in una rete virtuale hub, mentre ognuna delle due rimanenti risiederà in una rete virtuale spoke separata.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**.

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**.

1. Sulla barra degli strumenti del riquadro Cloud Shell fare clic sull'icona **Carica/Scarica file**, nel menu a discesa fare clic su **Carica** e caricare i file **\\Allfiles\\Labs\\06\\az104-06-vms-loop-template.json** e **\\Allfiles\\Labs\\06\\az104-06-vms-loop-parameters.json** nella home directory di Cloud Shell.

1. Nel riquadro Cloud Shell eseguire il comando seguente per creare il primo gruppo di risorse che ospiterà l'ambiente lab (sostituire il segnaposto "[Azure_region]" con il nome di un'area di Azure in cui si intende distribuire le macchine virtuali di Azure). È possibile usare il cmdlet "(Get-AzLocation).Location" per ottenere l'elenco delle aree:

    ```powershell 
    $location = '[Azure_region]'
    ```
    
    Ora il nome del gruppo di risorse:
    ```powershell
    $rgName = 'az104-06-rg1'
    ```
    
    Infine, creare il gruppo di risorse nella posizione desiderata:
    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```


1. Nel riquadro Cloud Shell eseguire il comando seguente per creare tre reti virtuali e quattro macchine virtuali di Azure in tali reti usando il modello e i file di parametri caricati:

    >**Nota**: verrà richiesto di specificare una password di Amministrazione.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-06-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-06-vms-loop-parameters.json
   ```

    >**Nota**: attendere il completamento della distribuzione prima di proseguire al passaggio successivo. L'operazione richiede circa 5 minuti.

    >**Nota**: se viene visualizzato un errore che indica che le dimensioni della macchina virtuale non sono disponibili, chiedere assistenza all'insegnante e provare questi passaggi.
    > 1. Fare clic sul pulsante `{}` in CloudShell, selezionare il file **az104-06-vms-loop-parameters.json** nella barra laterale sinistra e prendere nota del valore del parametro `vmSize`.
    > 1. Controllare il percorso in cui viene distribuito il gruppo di risorse 'az104-06-rg1'. È possibile eseguire `az group show -n az104-06-rg1 --query location` in CloudShell per ottenerlo.
    > 1. Eseguire `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` in CloudShell.
    > 1. Sostituire il valore del parametro `vmSize` con uno dei valori restituiti dal comando appena eseguito. Se non vengono restituiti valori, potrebbe essere necessario scegliere un'area diversa in cui eseguire la distribuzione. Si potrebbe anche scegliere un nome di famiglia diverso, ad esempio "Standard_B1s".
    > 1. Ora ridistribuire i modelli eseguendo di nuovo il comando `New-AzResourceGroupDeployment`. È possibile premere il pulsante Su alcune volte per visualizzare l'ultimo comando eseguito.

1. Nel riquadro Cloud Shell eseguire il comando seguente per installare l'estensione Network Watcher nelle macchine virtuali di Azure distribuite nel passaggio precedente:

   ```powershell
   $rgName = 'az104-06-rg1'
   $location = (Get-AzResourceGroup -ResourceGroupName $rgName).location
   $vmNames = (Get-AzVM -ResourceGroupName $rgName).Name

   foreach ($vmName in $vmNames) {
     Set-AzVMExtension `
     -ResourceGroupName $rgName `
     -Location $location `
     -VMName $vmName `
     -Name 'networkWatcherAgent' `
     -Publisher 'Microsoft.Azure.NetworkWatcher' `
     -Type 'NetworkWatcherAgentWindows' `
     -TypeHandlerVersion '1.4'
   }
   ```

    >**Nota**: attendere il completamento della distribuzione prima di proseguire al passaggio successivo. L'operazione richiede circa 5 minuti.



1. Chiudere il riquadro Cloud Shell.

## Attività 2: Configurare la topologia di rete hub-spoke

In questa attività verrà configurato il peering locale tra le reti virtuali distribuite nelle attività precedenti per creare una topologia di rete hub-spoke.

1. Nel portale di Azure cercare e selezionare **Reti virtuali**.

1. Esaminare le reti virtuali create nell'attività precedente.

    >**Nota**: il modello usato per la distribuzione delle tre reti virtuali garantisce che gli intervalli di indirizzi IP delle tre reti virtuali non si sovrappongano.

1. Nell'elenco di reti virtuali selezionare **az104-06-vnet2**.

1. Nel pannello **az104-06-vnet2** selezionare **Proprietà**. 

1. Nel pannello **az104-06-vnet2 \| Proprietà** registrare il valore della proprietà **ID risorsa**.

1. Tornare all'elenco delle reti virtuali e selezionare **az104-06-vnet3**.

1. Nel pannello **az104-06-vnet3** selezionare **Proprietà**. 

1. Nel pannello **az104-06-vnet3 \| Proprietà** registrare il valore della proprietà **ID risorsa**.

    >**Nota:** più avanti in questa attività saranno necessari i valori della proprietà ResourceID per entrambe le reti virtuali.

    >**Nota:** si tratta di una soluzione alternativa che risolve il problema con il portale di Azure che in alcuni casi non visualizza la rete virtuale di cui è stato appena effettuato il provisioning durante la creazione di peering di reti virtuali.

1. Nell'elenco di reti virtuali fare clic su **az104-06-vnet01**.

1. Nel pannello della rete virtuale **az104-06-vnet01**, nella sezione **Impostazioni**, fare clic su **Peering** e quindi su **+ Aggiungi**.

1. Aggiungere un peering con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni, quindi fare clic su **Aggiungi**:

    | Impostazione | Valore |
    | --- | --- |
    | Questa rete virtuale: Nome del collegamento di peering | **az104-06-vnet01_to_az104-06-vnet2** |
    | Consenti 'az104-06-vnet01' di accedere alla rete virtuale peered | **Verificare che la casella sia selezionata (impostazione predefinita)** |
    | Consentire al gateway in 'az104-06-vnet01' di inoltrare il traffico alla rete virtuale con peering | **Verificare che la casella sia selezionata** 
    | Rete virtuale remota: Nome del collegamento di peering | **az104-06-vnet2_to_az104-06-vnet01** |
    | Modello di distribuzione della rete virtuale | **Resource Manager** |
    | Conosco l'ID della risorsa | Enabled |
    | ID risorsa | Valore del parametro resourceID di **az104-06-vnet2** registrato in precedenza in questa attività. |
    | Consenti az104-06-vnet2 di accedere a az104-06-vnet01 | **Verificare che la casella sia selezionata (impostazione predefinita)** |
    | Consenti az104-06-vnet2 di ricevere il traffico inoltrato da az104-06-vnet01 | **Verificare che la casella sia selezionata** |

    >**Nota**: attendere il completamento dell'operazione.

    >**Nota**: questo passaggio stabilisce due peering locali, uno da az104-06-vnet01 ad az104-06-vnet2 e l'altro da az104-06-vnet2 ad az104-06-vnet01.

1. Nel pannello della rete virtuale **az104-06-vnet01**, nella sezione **Impostazioni**, fare clic su **Peering** e quindi su **+ Aggiungi**.

1. Aggiungere un peering con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni, quindi fare clic su **Aggiungi**:

    | Impostazione | Valore |
    | --- | --- |
    | Questa rete virtuale: Nome del collegamento di peering | **az104-06-vnet01_to_az104-06-vnet3** |
    | Impostazioni per consentire l'accesso, il traffico inoltrato e il gateway | **Verificare che tutte le caselle siano controllate** |
    | Rete virtuale remota: Nome del collegamento di peering | **az104-06-vnet3_to_az104-06-vnet01** |
    | Modello di distribuzione della rete virtuale | **Resource Manager** |
    | Conosco l'ID della risorsa | Enabled |
    | ID risorsa | Valore del parametro resourceID di **az104-06-vnet3** registrato in precedenza in questa attività |
    | Impostazioni per consentire l'accesso, il traffico inoltrato e il gateway | **Verificare che tutte le caselle siano controllate** |

    >**Nota**: attendere il completamento dell'operazione.
    
    >**Nota**: questo passaggio stabilisce due peering locali, uno da az104-06-vnet01 ad az104-06-vnet3 e l'altro da az104-06-vnet3 ad az104-06-vnet01. Questa operazione completa la configurazione della topologia hub-spoke (con due reti virtuali spoke).

## Attività 3: Testare la transitività del peering di reti virtuali

In questa attività si testerà la transitività del peering di reti virtuali usando Network Watcher.

1. Nel portale di Azure cercare e selezionare **Network Watcher**.

1. Nel pannello **Network Watcher** espandere l'elenco delle aree di Azure e verificare che il servizio sia abilitato nell'area in uso. 

1. Nel pannello **Network Watcher** passare a **Risoluzione dei problemi di connessione**.

1. Nel pannello **Network Watcher - Risoluzione dei problemi di connessione** avviare un controllo con le impostazioni seguenti, lasciando i valori predefiniti per le altre impostazioni:

    > **Nota**: la visualizzazione del gruppo di risorse può richiedere alcuni minuti. Se non si vuole attendere, provare a eseguire questa operazione: eliminare Network Watcher, creare una nuova istanza di Network Watcher e quindi riprovare Risoluzione dei problemi di connessione. 

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg1** |
    | Tipo di origine | **Macchina virtuale** |
    | Macchina virtuale | **az104-06-vm0** |
    | Destination | **Specificare manualmente** |
    | URI, FQDN o IPv4 | **10.62.0.4** |
    | Protocollo | **TCP** |
    | Porta di destinazione | **3389** |

    > **Nota**: **10.62.0.4** rappresenta l'indirizzo IP privato di **az104-06-vm2**

1. Fare clic su **Esegui test di diagnostica** e attendere che i risultati del controllo di connettività vengano restituiti. Verificare che lo stato sia **Success**. Esaminare il percorso di rete e notare che la connessione era diretta, senza hop intermedi tra le macchine virtuali.

    > **Nota**: questo comportamento è previsto, perché la rete virtuale hub è collegata direttamente alla prima rete virtuale spoke.

1. Nel pannello **Network Watcher - Risoluzione dei problemi di connessione** avviare un controllo con le impostazioni seguenti, lasciando i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg1** |
    | Tipo di origine | **Macchina virtuale** |
    | Macchina virtuale | **az104-06-vm0** |
    | Destination | **Specificare manualmente** |
    | URI, FQDN o IPv4 | **10.63.0.4** |
    | Protocollo | **TCP** |
    | Porta di destinazione | **3389** |

    > **Nota**: **10.63.0.4** rappresenta l'indirizzo IP privato di **az104-06-vm3**

1. Fare clic su **Esegui test di diagnostica** e attendere che i risultati del controllo di connettività vengano restituiti. Verificare che lo stato sia **Success**. Esaminare il percorso di rete e notare che la connessione era diretta, senza hop intermedi tra le macchine virtuali.

    > **Nota**: questo comportamento è previsto, perché la rete virtuale hub è collegata direttamente alla seconda rete virtuale spoke.

1. Nel pannello **Network Watcher - Risoluzione dei problemi di connessione** avviare un controllo con le impostazioni seguenti, lasciando i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg1** |
    | Tipo di origine | **Macchina virtuale** |
    | Macchina virtuale | **az104-06-vm2** |
    | Destination | **Specificare manualmente** |
    | URI, FQDN o IPv4 | **10.63.0.4** |
    | Protocollo | **TCP** |
    | Porta di destinazione | **3389** |

1. Fare clic su **Esegui test di diagnostica** e attendere che i risultati del controllo di connettività vengano restituiti. Si noti che lo stato è **Fail**.

    > **Nota:** questo comportamento è previsto, perché le due reti virtuali spoke non sono associate tra loro (il peering di reti virtuali non è transitivo).

## Attività 4: Configurare il routing nella topologia hub-spoke

In questa attività verrà configurato e testato il routing tra le due reti virtuali spoke abilitando l'inoltro IP nell'interfaccia di rete della macchina virtuale **az104-06-vm0** abilitando il routing all'interno del sistema operativo e configurando route definite dall'utente nella rete virtuale spoke.

1. Nel portale di Azure cercare e selezionare **Macchine virtuali**.

1. Nel pannello **Macchine virtuali**, nell'elenco di macchine virtuali, fare clic su **az104-06-vm0**.

1. Nel pannello della macchina virtuale **az104-06-vm0**, nella sezione **Impostazioni**, fare clic su **Rete**.

1. Fare clic sul collegamento **az104-06-nic0** accanto all'etichetta **Interfaccia di rete** e quindi nel pannello dell'interfaccia di rete **az104-06-nic0** fare clic su **Configurazioni IP** nella sezione **Impostazioni**.

1. Impostare **Inoltro IP** su **Abilitato** e salvare la modifica.

   > **Nota**: questa impostazione è necessaria per il funzionamento di **az104-06-vm0** come router, che instraderà il traffico tra due reti virtuali spoke.

   > **Nota**: a questo punto è necessario configurare il sistema operativo della macchina virtuale **az104-06-vm0** per supportare il routing.

1. Nel portale di Azure tornare al pannello della macchina virtuale di Azure **az104-06-vm0** e fare clic su **Panoramica**.

1. Nel pannello **az104-06-vm0** fare clic su **Esegui comando** nella sezione **Operazioni** e nell'elenco dei comandi fare clic su **RunPowerShellScript**.

1. Nel pannello **Esegui script di comandi** digitare quanto segue e fare clic su **Esegui** per installare il ruolo Remote Access Windows Server.

   ```powershell
   Install-WindowsFeature RemoteAccess -IncludeManagementTools
   ```

   > **Nota:** attendere la conferma del completamento del comando.

1. Nel pannello **Esegui script di comandi** digitare quanto segue e fare clic su **Esegui** per installare il servizio del ruolo Routing.

   ```powershell
   Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature

   Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"

   Install-RemoteAccess -VpnType RoutingOnly

   Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
   ```

   > **Nota:** attendere la conferma del completamento del comando.

   > **Nota**: è ora necessario creare e configurare route definite dall'utente nelle reti virtuali spoke.

1. Nel portale di Azure cercare selezionare **Tabelle di route** e nel pannello **Tabelle di route** fare clic su **+ Crea**.

1. Creare una tabella di route con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg1** |
    | Location | Nome dell'area di Azure in cui sono state create le reti virtuali |
    | Nome | **az104-06-rt23** |
    | Propaga route del gateway | **No** |

1. Fare clic su **Rivedi e crea**. Attendere il completamento della convalida e fare clic su **Crea** per inviare la distribuzione.

   > **Nota**: attendere il completamento della creazione della tabella di route. L'operazione richiede circa 3 minuti.

1. Fare clic su **Vai alla risorsa**.

1. Nella sezione **Impostazioni** della tabella di route **az104-06-rt23** fare clic su **Route** e quindi su **+ Aggiungi**.

1. Aggiungere un nuova route con le impostazioni seguenti:

    | Impostazione | valore |
    | --- | --- |
    | Nome route | **az104-06-route-vnet2-to-vnet3** |
    | Destinazione prefisso indirizzo | **Indirizzi IP** |
    | Indirizzi IP/Intervalli CIDR di destinazione | **10.63.0.0/20** |
    | Tipo hop successivo | **Appliance virtuale** |
    | Indirizzo hop successivo | **10.60.0.4** |

1. Fare clic su **Aggiungi**

1. Nella sezione **Impostazioni** della tabella di route **az104-06-rt23** fare clic su **Subnet** e quindi su **+ Associa**.

1. Associare la tabella di route **az104-06-rt23** alla subnet seguente:

    | Impostazione | valore |
    | --- | --- |
    | Rete virtuale | **az104-06-vnet2** |
    | Subnet | **subnet0** |

1. Fare clic su **Aggiungi**

1. Tornare al pannello **Tabelle di route** e fare clic su **+ Crea**.

1. Creare una tabella di route con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg1** |
    | Region | Nome dell'area di Azure in cui sono state create le reti virtuali |
    | Nome | **az104-06-rt32** |
    | Propaga route del gateway | **No** |

1. Fare clic su Rivedi e crea. Attendere il completamento della convalida e fare clic su Crea per inviare la distribuzione.

   > **Nota**: attendere il completamento della creazione della tabella di route. L'operazione richiede circa 3 minuti.

1. Fare clic su **Vai alla risorsa**.

1. Nella sezione **Impostazioni** della tabella di route **az104-06-rt32** fare clic su **Route** e quindi su **+ Aggiungi**.

1. Aggiungere un nuova route con le impostazioni seguenti:

    | Impostazione | valore |
    | --- | --- |
    | Nome route | **az104-06-route-vnet3-to-vnet2** |
    | Destinazione prefisso indirizzo | **Indirizzi IP** |
    | Indirizzi IP/Intervalli CIDR di destinazione | **10.62.0.0/20** |
    | Tipo hop successivo | **Appliance virtuale** |
    | Indirizzo hop successivo | **10.60.0.4** |

1. Fare clic su **OK**.

1. Nella sezione **Impostazioni** della tabella di route **az104-06-rt32** fare clic su **Subnet** e quindi su **+ Associa**.

1. Associare la tabella di route **az104-06-rt32** alla subnet seguente:

    | Impostazione | valore |
    | --- | --- |
    | Rete virtuale | **az104-06-vnet3** |
    | Subnet | **subnet0** |

1. Fare clic su **OK**.

1. Nel portale di Azure tornare al pannello **Network Watcher - Risoluzione dei problemi di connessione**.

1. Nel pannello **Network Watcher - Risoluzione dei problemi di connessione** usare le impostazioni seguenti (lasciare altri con i relativi valori predefiniti):

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg1** |
    | Tipo di origine | **Macchina virtuale** |
    | Macchina virtuale | **az104-06-vm2** |
    | Destination | **Specificare manualmente** |
    | URI, FQDN o IPv4 | **10.63.0.4** |
    | Protocollo | **TCP** |
    | Porta di destinazione | **3389** |

1. Fare clic su **Esegui test di diagnostica** e attendere che i risultati del controllo di connettività vengano restituiti. Verificare che lo stato sia **Success**. Esaminare il percorso di rete e notare che il traffico è stato instradato tramite **10.60.0.4**, assegnato alla scheda di rete **az104-06-nic0**. Se lo stato è **Fail**, è necessario arrestare e quindi avviare az104-06-vm0.

    > **Nota:** questo comportamento è previsto perché il traffico tra reti virtuali spoke viene ora instradato tramite la macchina virtuale che si trova nella rete virtuale hub, che funziona come router.

    > **Nota:** è possibile usare **Network Watcher** per visualizzare la topologia della rete.

## Attività 5: Implementare Azure Load Balancer

In questa attività verrà implementata un'istanza di Azure Load Balancer davanti alle due macchine virtuali di Azure nella rete virtuale hub.

1. Nel portale di Azure cercare e selezionare **Servizi di bilanciamento del carico** e, nel pannello **Servizi di bilanciamento del carico**, fare clic su **+ Crea**.

1. Creare un servizio di bilanciamento del carico con le impostazioni seguenti (lasciare le altre con i valori predefiniti) quindi fare clic su **Avanti: Configurazione IP front-end**:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg4** (se necessario create) |
    | Nome | **az104-06-lb4** |
    | Region | Nome dell'area di Azure in cui sono state distribuite tutte le altre risorse in questo lab |
    | SKU  | **Standard** |
    | Tipo | **Pubblica** |
    | Livello | **Regional** |
    
1. Nella scheda **Configurazione IP front-end** fare clic su **Aggiungi una configurazione IP front-end** e usare le impostazioni seguenti:  
     
    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-06-fe4** |
    | Tipo di indirizzo IP | Indirizzo IP |
    | Indirizzo IP pubblico | Selezionare **Crea nuovo** |
    | Load Balancer gateway | Nessuno |
    
1. Nel popup **Aggiungi un indirizzo IP pubblico** usare le impostazioni seguenti prima di fare clic su **OK** e quindi **su Aggiungi**. Al termine, fare clic su **Avanti: Pool back-end**. 
     
    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-06-pip4** |
    | SKU | Standard |
    | Livello | A livello di area |
    | Assegnazione | Statico |
    | Preferenza di routing | **Rete Microsoft** |

1. Nella scheda **Pool back-end** fare clic su **Aggiungi un pool back-end** con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni. Fare clic su **+ Aggiungi** (due volte) e quindi su **Avanti: Regole in ingresso**. 

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-06-lb4-be1** |
    | Rete virtuale | **az104-06-vnet01** |
    | Configurazione pool back-end | **NIC** | 
    | Versione indirizzo IP | **IPv4** |
    | Fare clic su **Aggiungi** per aggiungere una macchina virtuale |  |
    | az104-06-vm0 | **Selezionare la casella** |
    | az104-06-vm1 | **Selezionare la casella** |


1. Nella scheda **Regole in ingresso** fare clic su **Aggiungi una regola di bilanciamento del carico**. Aggiungere una regola di bilanciamento del carico con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-06-lb4-lbrule1** |
    | Versione indirizzo IP | **IPv4** |
    | Indirizzo IP front-end | **az104-06-fe4** |
    | Pool back-end | **az104-06-lb4-be1** |    
    | Protocollo | **TCP** |
    | Porta | **80** |
    | Porta back-end | **80** |
    | Probe di integrità | **Crea nuovo** |
    | Nome | **az104-06-lb4-hp1** |
    | Protocollo | **TCP** |
    | Porta | **80** |
    | Intervallo | **5** |
    | Chiudere la finestra di creazione di un probe di integrità | **OK** | 
    | Persistenza della sessione | **Nessuno** |
    | Timeout di inattività (minuti) | **4** |
    | Reimpostazione TCP | **Disabilitato** |
    | IP mobile | **Disabilitato** |
    | SNAT (Network Address Translation) di origine in uscita | **Consigliato** |

1. Poiché si ha tempo, esaminare le altre schede, quindi fare clic su **Rivedi e crea**. Assicurarsi che non siano presenti errori di convalida, quindi fare clic su **Crea**. 

1. Attendere che il servizio di bilanciamento del carico venga distribuito e quindi fare clic su **Vai alla risorsa**.  

1. Selezionare **Configurazione IP front-end** nella pagina della risorsa Load Balancer. Copiare l'indirizzo IP,

1. Aprire un'altra scheda del browser e passare all'indirizzo IP. Verificare che nella finestra del browser sia visualizzato il messaggio **Hello World from az104-06-vm0** o **Hello World from az104-06-vm1**.

1. Aggiornare la finestra per verificare che il messaggio venga modificato in modo da fare riferimento all'altra macchina virtuale. Ciò indica che il servizio di bilanciamento del carico esegue la rotazione tra le macchine virtuali.

    > **Nota**: potrebbe essere necessario aggiornare più volte la visualizzazione o aprire una nuova finestra del browser in modalità InPrivate.

## Attività 6: Implementare il gateway applicazione di Azure

In questa attività verrà implementato un gateway applicazione di Azure davanti alle due macchine virtuali di Azure nelle reti virtuale spoke.

1. Nel portale di Azure cercare e selezionare **Reti virtuali**.

1. Nel pannello **Reti virtuali**, nell'elenco di reti virtuali, fare clic su **az104-06-vnet01**.

1. Nel pannello della rete virtuale **az104-06-vnet01**, nella sezione **Impostazioni**, fare clic su **Subnet** e quindi su **+ Subnet**.

1. Aggiungere una subnet con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **subnet-appgw** |
    | Intervallo di indirizzi subnet | **10.60.3.224/27** |

1. Fare clic su **Save** (Salva).

    > **Nota**: questa subnet verrà usata dalle istanze del gateway applicazione di Azure, che verranno distribuite più avanti in questa attività. Il gateway applicazione richiede una subnet dedicata di dimensioni /27 o superiori.

1. Nel portale di Azure cercare e selezionare **Gateway applicazione** e nel pannello **Gateway applicazione** fare clic su **+ Crea**.

1. Nella scheda **Generale** specificare ora le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | **az104-06-rg5** (Crea nuovo) |
    | Nome del gateway applicazione | **az104-06-appgw5** |
    | Region | Nome dell'area di Azure in cui sono state distribuite tutte le altre risorse in questo lab |
    | Livello | **Standard V2** |
    | Abilitare la scalabilità automatica | **No** |
    | Numero di istanze | **2** |
    | Zona di disponibilità | **Nessuno** |
    | HTTP2 | **Disabilitato** |
    | Rete virtuale | **az104-06-vnet01** |
    | Subnet | **subnet-appgw (10.60.3.224/27)** |

1. Fare clic su **Avanti: Front-end >** e specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **OK**. 

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di indirizzo IP front-end | **Pubblica** |
    | Indirizzo IP pubblico| **Aggiungi nuovo** | 
    | Nome | **az104-06-pip5** |
    | Zona di disponibilità | **Nessuno** |

1. Fare clic su **Avanti: Back-end >** e quindi su **Aggiungi un pool back-end**. Specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-06-appgw5-be1** |
    | Aggiunta di uni pool back-end senza destinazioni | **No** |
    | indirizzo IP o FQDN | **10.62.0.4** | 
    | indirizzo IP o FQDN | **10.63.0.4** |

    > **Nota**: le destinazioni rappresentano gli indirizzi IP privati delle macchine virtuali nelle reti virtuali spoke **az104-06-vm2** e **az104-06-vm3**.

1. Fare clic su **Avanti: Configurazione >** e quindi su **+ Aggiungi una regola di routing**. Specificare le seguenti impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Nome regola | **az104-06-appgw5-rl1** |
    | Priorità | **10** |
    | Nome listener | **az104-06-appgw5-rl1l1** |
    | IP front-end | **Pubblica** |
    | Protocollo | **HTTP** |
    | Porta | **80** |
    | Tipo di listener | **Base** |
    | URL pagina di errore | **No** |

1. Passare alla scheda **Destinazioni back-end** e specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi** (due volte).  

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di destinazione | **Pool back-end** |
    | Destinazione back-end | **az104-06-appgw5-be1** |
    | Impostazioni back-end | **Aggiungi nuovo** |
    | Nome delle impostazioni back-end | **az104-06-appgw5-http1** |
    | Protocollo back-end | **HTTP** |
    | Porta back-end | **80** |
    | Impostazioni aggiuntive | **Accettare i valori predefiniti** |
    | Nome host | **Accettare i valori predefiniti** |

1. Fare clic su **Avanti: Tag >** seguito da **Avanti: Revisione e creazione >** , quindi fare clic su **Crea**.

    > **Nota**: attendere la creazione dell'istanza del gateway applicazione. L'operazione potrebbe richiedere circa 8 minuti.

1. Nel portale di Azure cercare e selezionare **Gateway applicazione** e nel pannello **Gateway applicazione** fare clic su **az104-06-appgw5**.

1. Nel pannello del gateway applicazione **az104-06-appgw5** copiare il valore di **Indirizzo IP pubblico front-end**.

1. Aprire un'altra finestra del browser e passare all'indirizzo IP identificato nel passaggio precedente.

1. Verificare che nella finestra del browser sia visualizzato il messaggio **Hello World from az104-06-vm2** o **Hello World from az104-06-vm3**.

1. Aggiornare la finestra per verificare che il messaggio venga modificato in modo da fare riferimento all'altra macchina virtuale. 

    > **Nota**: potrebbe essere necessario aggiornare più volte la visualizzazione o aprire una nuova finestra del browser in modalità InPrivate.

    > **Nota**: la destinazione delle macchine virtuali in più reti virtuali non è una configurazione comune, ma è concepita per illustrare il punto in cui il gateway applicazione è in grado di impostare come destinazione macchine virtuali in più reti virtuali (nonché endpoint in altre aree di Azure o anche all'esterno di Azure), a differenza di Azure Load Balancer, che bilancia il carico tra le macchine virtuali nella stessa rete virtuale.

## Pulire le risorse

>**Nota**: ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate garantisce che non verranno addebitati costi imprevisti.

>**Nota**: non è necessario preoccuparsi se le risorse del lab non possono essere rimosse immediatamente. A volte le risorse hanno dipendenze e l'eliminazione può richiedere più tempo. Si tratta di un'attività comune dell'amministratore per monitorare l'utilizzo delle risorse, quindi è sufficiente esaminare periodicamente le risorse nel portale per verificare il funzionamento della pulizia. 

1. Nel portale di Azure aprire la sessione di **PowerShell** all'interno del riquadro **Cloud Shell**.

1. Elencare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*'
   ```

1. Eliminare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: il comando viene eseguito in modo asincrono, in base a quanto determinato dal parametro -AsJob, quindi, sebbene sia possibile eseguire un altro comando di PowerShell immediatamente dopo nella stessa sessione di PowerShell, i gruppi di risorse verranno rimossi dopo alcuni minuti.

## Verifica

In questo lab sono state eseguite le attività seguenti:

+ Provisioning dell'ambiente lab
+ Configurare la topologia di rete hub-spoke
+ Testare la transitività del peering di reti virtuali
+ Configurare il routing nella topologia hub-spoke
+ Implementare Azure Load Balancer
+ Implementare un gateway applicazione di Azure
