---
lab:
  title: 04 - Implementare la rete virtuale
  module: Administer Virtual Networking
---

# <a name="lab-04---implement-virtual-networking"></a>Lab 04 - Implementare la rete virtuale

# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

È necessario esplorare le funzionalità della rete virtuale di Azure. Per iniziare, si pianifica la creazione di una rete virtuale in Azure che ospiterà un paio di macchine virtuali di Azure. Poiché si prevede di implementare la segmentazione basata sulla rete, sarà necessario distribuirle in subnet diverse della rete virtuale. È inoltre consigliabile assicurarsi che gli indirizzi IP privati e pubblici non cambino nel tempo. Per soddisfare i requisiti per la sicurezza di Contoso, è necessario proteggere gli endpoint pubblici delle macchine virtuali di Azure accessibili da Internet. È infine necessario implementare la risoluzione dei nomi DNS per le macchine virtuali di Azure nella rete virtuale e da Internet.

                **Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi. 

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Creare e configurare una rete virtuale
+ Attività 2: Distribuire macchine virtuali nella rete virtuale
+ Attività 3: Configurare gli indirizzi IP privati e pubblici delle macchine virtuali di Azure
+ Attività 4: Configurare i gruppi di sicurezza di rete
+ Attività 5: Configurare DNS di Azure per la risoluzione dei nomi interni
+ Attività 6: Configurare DNS di Azure per la risoluzione dei nomi esterni

## <a name="estimated-timing-40-minutes"></a>Tempo stimato: 40 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab04.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-create-and-configure-a-virtual-network"></a>Attività 1: Creare e configurare una rete virtuale

In questa attività verrà creata una rete virtuale con più subnet mediante il portale di Azure

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Nel portale di Azure cercare e selezionare **Macchine virtuali** e nel pannello **Reti virtuali** fare clic su **+ Crea**.

1. Creare una rete virtuale con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure che verrà usata nel lab |
    | Gruppo di risorse | Nome di un **nuovo** gruppo di risorse **az104-04-rg1** |
    | Nome | **az104-04-vnet1** |
    | Region | Nome di qualsiasi area di Azure disponibile nella sottoscrizione che verrà usata in questo lab |

1. Fare clic su **Avanti : Indirizzi IP** e quindi su **Aggiungi un indirizzo IP**. Fare clic su **Aggiungi** al termine. 

    | Impostazione | Valore |
    | --- | --- |
    | Indirizzo iniziale | **10.40.0.0** |
    | Dimensioni dello spazio degli indirizzi | **/20 (4096 indirizzi)** |

1. Fare clic su **+ Aggiungi subnet**, immettere i valori seguenti, quindi fare clic su **Aggiungi**

    | Impostazione | valore |
    | --- | --- |
    | Nome della subnet | **subnet0** |
    | Intervallo di indirizzi subnet | **10.40.0.0/24** |

1. Accettare le impostazioni predefinite e fare clic su **Rivedi e crea**. Attendere il completamento della convalida e fare di nuovo clic su **Crea** per inviare la distribuzione.

    >**Nota:** attendere il completamento del provisioning della rete virtuale. L'operazione dovrebbe richiedere meno di un minuto.

1. Fare clic su **Vai alla risorsa**

1. Nel pannello della rete virtuale **az104-04-vnet1** fare clic su **Subnet** e quindi su **+ Subnet**.

1. Creare una subnet con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **subnet1** |
    | Intervallo di indirizzi (blocco CIDR) | **10.40.1.0/24** |
    | Gruppo di sicurezza di rete | **Nessuno** |
    | Tabella di route | **Nessuno** |

1. Fare clic su **Save** (Salva).

#### <a name="task-2-deploy-virtual-machines-into-the-virtual-network"></a>Attività 2: Distribuire macchine virtuali nella rete virtuale

In questa attività verranno distribuite macchine virtuali di Azure in diverse subnet della rete virtuale mediante un modello di ARM

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**.

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**.

1. Sulla barra degli strumenti del riquadro Cloud Shell fare clic sull'icona **Carica/Scarica file**, nel menu a discesa fare clic su **Carica** e caricare i file **\\Allfiles\\Labs\\04\\az104-04-vms-loop-template.json** e **\\Allfiles\\Labs\\04\\az104-04-vms-loop-parameters.json** nella home directory di Cloud Shell.

    >**Nota**: potrebbe essere necessario caricare ogni file separatamente.

1. Modificare il file dei parametri e modificare la password. Per le indicazioni relative alla modifica del file nella shell, chiedere assistenza all'insegnante. Come procedura consigliata, i segreti, ad esempio le password, devono essere archiviati in modo più sicuro in Key Vault. 

1. Nel riquadro Cloud Shell eseguire il codice seguente per distribuire due macchine virtuali usando il modello e i file di parametri:

   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```

    >**Nota**: questo metodo di distribuzione dei modelli di ARM usa Azure PowerShell. È possibile eseguire la stessa attività eseguendo il comando equivalente **az deployment create** dell'interfaccia della riga di comando di Azure. Per altre informazioni, vedere [Distribuire risorse con modelli di Resource Manager e l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli).

    >**Nota**: attendere il completamento dell'operazione prima di passare all'attività successiva. L'operazione richiede circa 2 minuti.

    >**Nota**: se viene visualizzato un errore che indica che le dimensioni della macchina virtuale non sono disponibili, chiedere assistenza all'insegnante e provare questi passaggi:
    > 1. Fare clic sul pulsante `{}` in CloudShell, selezionare **az104-04-vms-loop-parameters.json** nella barra laterale sinistra e prendere nota del valore del parametro`vmSize`.
    > 1. Controllare il percorso in cui viene distribuito il gruppo di risorse "az104-04-rg1". È possibile eseguire `az group show -n az104-04-rg1 --query location` in CloudShell per ottenerlo.
    > 1. Eseguire `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` in CloudShell. Se non viene elencato alcuno SKU, ovvero non vengono restituiti risultati, non è possibile distribuire macchine virtuali D2S in tale area. Sarà necessario trovare un'area che consenta la distribuzione di macchine virtuali D2S. Dopo aver scelto una posizione appropriata, eliminare il gruppo di risorse AZ104-04-rg1 e riavviare il lab.
    > 1. Sostituire il valore del parametro `vmSize` con uno dei valori restituiti dal comando appena eseguito.
    > 1. Ora ridistribuire i modelli eseguendo di nuovo il comando `New-AzResourceGroupDeployment`. È possibile premere il pulsante Su alcune volte per visualizzare l'ultimo comando eseguito.

1. Chiudere il riquadro Cloud Shell.

#### <a name="task-3-configure-private-and-public-ip-addresses-of-azure-vms"></a>Attività 3: Configurare gli indirizzi IP privati e pubblici delle macchine virtuali di Azure

In questa attività verrà configurata un'assegnazione statica di indirizzi IP pubblici e privati assegnati a interfacce di rete di macchine virtuali di Azure.

   >**Nota**: gli indirizzi IP privati e pubblici vengono effettivamente assegnati alle interfacce di rete, che a loro volta sono collegate a macchine virtuali di Azure, ma è piuttosto comune fare riferimento agli indirizzi IP assegnati invece alle macchine virtuali di Azure.

1. Nel portale di Azure cercare e selezionare **Gruppi di risorse** e nel pannello **Gruppi di risorse** fare clic su **az104-04-rg1**.

1. Nel pannello del gruppo di risorse **az104-04-rg1** fare clic su **az104-04-vnet1** nell'elenco delle rispettive risorse.

1. Ne pannello della rete virtuale **az104-04-vnet1** esaminare la sezione **Dispositivi connessi** e verificare che siano disponibili due interfacce di rete, **az104-04-nic0** e **az104-04-nic1**, collegate alla rete virtuale.

1. Fare clic su **az104-04-nic0** e nel pannello **az104-04-nic0** fare clic su **Configurazioni IP**.

    >**Nota**: verificare che la configurazione **ipconfig1** sia attualmente configurata con un indirizzo IP privato dinamico.

1. Nell'elenco di configurazioni IP fare clic su **ipconfig1**.

1. Nel pannello **ipconfig1** nella sezione **Impostazioni dell'indirizzo IP pubblico** selezionare **Associa**, fare clic su **+ Crea nuovo**, specificare le impostazioni seguenti e fare clic su **OK**:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-04-pip0** |
    | SKU | **Standard** |

1. Nel pannello **ipconfig1** impostare **Assegnazione** su **Statica**, lasciare il valore predefinito di **Indirizzo IP** impostato su **10.40.0.4**.

1. Nel pannello **ipconfig1** salvare le modifiche. Assicurarsi di attendere il completamento dell'operazione di salvataggio prima di procedere al passaggio successivo.

1. Tornare al pannello **az104-04-vnet1**

1. Fare clic su **az104-04-nic1** e nel pannello **az104-04-nic1** fare clic su **Configurazioni IP**.

    >**Nota**: verificare che la configurazione **ipconfig1** sia attualmente configurata con un indirizzo IP privato dinamico.

1. Nell'elenco di configurazioni IP fare clic su **ipconfig1**.

1. Nel pannello **ipconfig1** nella sezione **Impostazioni dell'indirizzo IP pubblico** selezionare **Associa**, fare clic su **+ Crea nuovo**, specificare le impostazioni seguenti e fare clic su **OK**:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-04-pip1** |
    | SKU | **Standard** |

1. Nel pannello **ipconfig1** impostare **Assegnazione** su **Statica**, lasciare il valore predefinito di **Indirizzo IP** impostato su **10.40.1.4**.

1. Nel pannello **ipconfig1** salvare le modifiche.

1. Tornare nel pannello del gruppo di risorse **az104-04-rg1**, nell'elenco delle rispettive risorse fare clic su **az104-04-vm0** e nel pannello della macchina virtuale **az104-04-vm0** prendere nota della voce relativa all'indirizzo IP.

1. Tornare nel pannello del gruppo di risorse **az104-04-rg1**, nell'elenco delle rispettive risorse fare clic su **az104-04-vm1** e nel pannello della macchina virtuale **az104-04-vm1** prendere nota della voce relativa all'indirizzo IP.

    >**Nota**: entrambi gli indirizzi IP saranno necessari nell'ultima attività di questo lab.

#### <a name="task-4-configure-network-security-groups"></a>Attività 4: Configurare i gruppi di sicurezza di rete

In questa attività verranno configurati gruppi di sicurezza di rete per consentire la connettività con restrizioni alle macchine virtuali di Azure.

1. Nel portale di Azure tornare al pannello del gruppo di risorse **az104-04-rg1** e nell'elenco delle rispettive risorse fare clic su **az104-04-vm0**.

1. Nel pannello di panoramica **az104-04-vm0** fare clic su **Connetti**, nel menu a discesa fare clic su **RDP**, nel pannello **Connetti tramite RDP** selezionare **Scarica file RDP** usando l'indirizzo IP pubblico e seguire le istruzioni per avviare la sessione di Desktop remoto.

1. Si noti che il tentativo di connessione ha esito negativo.

    >**Nota**: questo comportamento è previsto perché gli indirizzi IP pubblici di Standard SKU, per impostazione predefinita, richiedono che le interfacce di rete a cui sono assegnati siano protette da un gruppo di sicurezza di rete. Per consentire le connessioni Desktop remoto, verrà creato un gruppo di sicurezza di rete che consente esplicitamente il traffico RDP in ingresso da Internet e lo assegna alle interfacce di rete di entrambe le macchine virtuali.

1. Arrestare le macchine virtuali **az104-04-vm0** e **az104-04-vm1**.

    >**Nota**: questa operazione viene eseguita per comodità per il lab. Se le macchine virtuali sono in esecuzione quando un gruppo di sicurezza di rete viene collegato all'interfaccia di rete, per rendere effettivo il collegamento possono essere richiesti più di 30 minuti. Dopo aver creato e collegato il gruppo di sicurezza di rete, le macchine virtuali verranno riavviate e il collegamento diventerà effettivo immediatamente.

1. Nel portale di Azure cercare e selezionare **Gruppi di sicurezza di rete** e nel pannello **Gruppi di sicurezza di rete** fare clic su **+ Crea**.

1. Creare un gruppo di sicurezza di rete con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | **az104-04-rg1** |
    | Nome | **az104-04-nsg01** |
    | Region | Nome dell'area di Azure in cui sono state distribuite tutte le altre risorse in questo lab |

1. Fare clic su **Rivedi e crea**. Attendere il completamento della convalida e fare clic su **Crea** per inviare la distribuzione.

    >**Nota**: attendere il completamento della distribuzione. L'operazione richiede circa 2 minuti.

1. Nel pannello della distribuzione fare clic su **Vai alla risorsa** per aprire il pannello del gruppo di sicurezza di rete **az104-04-nsg01**.

1. Nel pannello del gruppo di sicurezza di rete **az104-04-nsg01** nella sezione **Impostazioni** fare clic su **Regole di sicurezza in ingresso**.

1. Aggiungere una regola in ingresso con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Source (Sorgente) | **qualsiasi** |
    | Intervalli di porte di origine | * |
    | Destination | **qualsiasi** |
    | Servizio | **RDP** |
    | Azione | **Consentito** |
    | Priorità | **300** |
    | Nome | **AllowRDPInBound** |

1. Nel pannello del gruppo di sicurezza di rete **az104-04-nsg01** nella sezione **Impostazioni** fare clic su **Interfacce di rete** e quindi su **+ Associa**.

1. Associare il gruppo di sicurezza di rete **az104-04-nsg01** alle interfacce di rete **az104-04-nic0** e **az104-04-nic1**.

    >**Nota**: l'applicazione delle regole del gruppo di sicurezza di rete appena creato alla scheda di interfaccia di rete può richiedere fino a 5 minuti.

1. Avviare le macchine virtuali **az104-04-vm0** e **az104-04-vm1**.

1. Tornare al pannello della macchina virtuale **az104-04-vm0**.

    >**Nota**: nei passaggi successivi si verificherà se è possibile connettersi correttamente alla macchina virtuale di destinazione.

1. Nel pannello **az104-04-vm0** fare clic su **Connetti**, fare clic su **RDP**, nel pannello **Connetti tramite RDP** selezionare **Scarica file RDP** usando l'indirizzo IP pubblico e seguire le istruzioni per avviare la sessione di Desktop remoto.

    >**Nota**: questo passaggio si riferisce alla connessione tramite Desktop remoto da un computer Windows. In un computer Mac è possibile usare il client Desktop remoto da Mac App Store e nei computer Linux è possibile usare un software client RDP open source.

    >**Nota**: è possibile ignorare eventuali richieste di avviso durante la connessione alle macchine virtuali di destinazione.

1. Quando richiesto, accedere con il nome utente e la password riportati nel file dei parametri.

    >**Nota**: lasciare aperta la sessione di Desktop remoto. Sarà necessario nell'attività successiva.

#### <a name="task-5-configure-azure-dns-for-internal-name-resolution"></a>Attività 5: Configurare DNS di Azure per la risoluzione dei nomi interni

In questa attività verrà configurata la risoluzione dei nomi DNS in una rete virtuale mediante le zone DNS privato di Azure.

1. Nel portale di Azure cercare e selezionare **Zone DNS privato** e nel pannello **Zone DNS privato** fare clic su **+ Crea**.

1. Creare un zona DNS privato con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | **az104-04-rg1** |
    | Nome | **contoso.org** |

1. Fare clic su **Rivedi e crea**. Attendere il completamento della convalida e fare di nuovo clic su **Crea** per inviare la distribuzione.

    >**Nota**: attendere il completamento della creazione di zona DNS privato. L'operazione richiede circa 2 minuti.

1. Fare clic su **Vai alla risorsa** per aprire il pannello della zona DNS privato **contoso.org**.

1. Nel pannello della zona DNS privato **contoso.org** nella sezione **Impostazioni** fare clic su **Collegamenti di rete virtuale**

1. Fare clic su **+ Aggiungi** per creare un collegamento di rete virtuale con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Nome collegamento | **az104-04-vnet1-link** |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Rete virtuale | **az104-04-vnet1** |
    | Abilita registrazione automatica | Enabled |

1. Fare clic su **OK**.

    >**Note:** attendere il completamento della creazione del collegamento di rete virtuale. L'operazione dovrebbe richiedere meno di un minuto.

1. Nella barra laterale del pannello della zona DNS privato **contoso.org** fare clic su **Panoramica**

1. Verificare che i record DNS per **az104-04-vm0** e **az104-04-vm1** vengano visualizzati nell'elenco di set di record come **Registrato automaticamente**.

    >**Nota:** potrebbe essere necessario attendere alcuni minuti e aggiornare la pagina se i set di record non sono elencati.

1. Passare alla sessione di Desktop remoto in **az104-04-vm0**, fare clic con il pulsante destro del mouse sul pulsante **Avvia** e nel menu di scelta rapida fare clic su **Windows PowerShell (amministratore)** .

1. Nella finestra della console di Windows PowerShell eseguire il codice seguente per testare la risoluzione dei nomi interni nella zona DNS privato appena creata:

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. Verificare che l'output del comando includa l'indirizzo IP privato **az104-04-vm1** (**10.40.1.4**).

#### <a name="task-6-configure-azure-dns-for-external-name-resolution"></a>Attività 6: Configurare DNS di Azure per la risoluzione dei nomi esterni

In questa attività verrà configurata la risoluzione dei nomi DNS esterni mediante le zone DNS pubblico di Azure.

1. In un Web browser aprire una nuova scheda e passare a <https://www.godaddy.com/domains/domain-name-search>.

1. Usare la ricerca del nome di dominio per identificare un nome di dominio non in uso.

1. Nel portale di Azure cercare e selezionare **Zone DNS** e nel pannello **Zone DNS** fare clic su **+ Crea**.

1. Creare un zona DNS con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | **az104-04-rg1** |
    | Nome | Nome di dominio DNS identificato in precedenza in questa attività |

1. Fare clic su **Rivedi e crea**. Attendere il completamento della convalida e fare di nuovo clic su **Crea** per inviare la distribuzione.

    >**Nota**: attendere il completamento della creazione di zona DNS. L'operazione richiede circa 2 minuti.

1. Fare clic su **Vai alla risorsa** per aprire il pannello della zona DNS appena creata.

1. Nel pannello Zona DNS fare clic su **+ Set di record**.

1. Aggiungere un set di record con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-04-vm0** |
    | Tipo | **A** |
    | Set di record alias | **No** |
    | TTL | **1** |
    | Unità TTL | **Ore** |
    | Indirizzo IP | Indirizzo IP pubblico di **az104-04-vm0** identificato nel terzo esercizio di questo lab |

1. Fare clic su **OK**.

1. Nel pannello Zona DNS fare clic su **+ Set di record**.

1. Aggiungere un set di record con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Nome | **az104-04-vm1** |
    | Tipo | **A** |
    | Set di record alias | **No** |
    | TTL | **1** |
    | Unità TTL | **Ore** |
    | Indirizzo IP | Indirizzo IP pubblico di **az104-04-vm1v** identificato nel terzo esercizio di questo lab. |

1. Fare clic su **OK**.

1. Nel pannello Zona DNS prendere nota del nome della voce **Nome del server 1**.

1. Nel portale di Azure aprire la sessione di **PowerShell** in **Cloud Shell** facendo clic sull'icona in alto a destra nel portale di Azure.

1. Dal riquadro Cloud Shell eseguire il comando seguente per testare la risoluzione dei nomi esterni del set di record DNS **az104-04-vm0** nella zona DNS appena creata (sostituire il segnaposto `[Name server 1]` con il nome di **Nome del server 1** annotato in precedenza in questa attività e il segnaposto `[domain name]` con il nome del dominio DNS creato in precedenza in questa attività):

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. Verificare che l'output del comando includa l'indirizzo IP privato **az104-04-vm0**.

1. Dal riquadro Cloud Shell eseguire il comando seguente per testare la risoluzione dei nomi esterni del set di record DNS **az104-04-vm1** nella zona DNS appena creata (sostituire il segnaposto `[Name server 1]` con il nome di **Nome del server 1** annotato in precedenza in questa attività e il segnaposto `[domain name]` con il nome del dominio DNS creato in precedenza in questa attività):

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. Verificare che l'output del comando includa l'indirizzo IP privato **az104-04-vm1**.

#### <a name="clean-up-resources"></a>Pulire le risorse

 > **Nota**: ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate garantisce che non verranno addebitati costi imprevisti.

 > **Nota**: non è necessario preoccuparsi se le risorse del lab non possono essere rimosse immediatamente. A volte le risorse hanno dipendenze e l'eliminazione può richiedere più tempo. Si tratta di un'attività comune dell'amministratore per monitorare l'utilizzo delle risorse, quindi è sufficiente esaminare periodicamente le risorse nel portale per verificare il funzionamento della pulizia. 

1. Nel portale di Azure aprire la sessione di **PowerShell** all'interno del riquadro **Cloud Shell**.

1. Elencare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. Eliminare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: il comando viene eseguito in modo asincrono, in base a quanto determinato dal parametro -AsJob, quindi, sebbene sia possibile eseguire un altro comando di PowerShell immediatamente dopo nella stessa sessione di PowerShell, i gruppi di risorse verranno rimossi dopo alcuni minuti.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

+ Creazione e configurazione di una rete virtuale
+ Distribuzione di macchine virtuali nella rete virtuale
+ Configurazione degli indirizzi IP privati e pubblici delle macchine virtuali di Azure
+ Configurazione dei gruppi di sicurezza di rete
+ Configurazione di DNS di Azure per la risoluzione dei nomi interni
+ Configurazione di DNS di Azure per la risoluzione dei nomi esterni
