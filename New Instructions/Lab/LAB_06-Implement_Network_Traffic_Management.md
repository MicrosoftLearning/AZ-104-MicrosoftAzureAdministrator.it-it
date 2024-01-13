---
lab:
  title: 'Lab 06: Implementare la gestione del traffico'
  module: Administer Network Traffic Management
---

# Lab 06 - Implementare Gestione traffico

## Introduzione al lab

In questo lab si apprenderà come configurare e testare un servizio di bilanciamento del carico pubblico e un gateway applicazione. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

## Tempo stimato: 40 minuti

## Scenario laboratorio

L'organizzazione ha un sito Web pubblico. È necessario bilanciare il carico delle richieste pubbliche in ingresso tra macchine virtuali diverse. È anche necessario fornire immagini e video da macchine virtuali diverse. Si prevede di implementare e Azure Load Balancer e un gateway di app Azure lication. Tutte le risorse si trovano nella stessa area. 

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ [Creare e configurare e il servizio](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20an%20Azure%20load%20balancer) di bilanciamento del carico di Azure. Creare una rete virtuale, i server back-end, il servizio di bilanciamento del carico e quindi testare il servizio di bilanciamento del carico. 
+ [Distribuire app Azure lication Gateway](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Deploy%20Azure%20Application%20Gateway). Creare un gateway applicazione, creare macchine virtuali, creare il pool back-end e testare il gateway. 
+ [Implementare la gestione](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010) del traffico. Implementare una rete hub-spoke completa, tra cui macchine virtuali, reti virtuali, peering, bilanciamento del carico e gateway applicazione.

## Attività

+ Attività 1: Effettuare il provisioning dell'ambiente lab
+ Attività 2: Implementare Azure Load Balancer
+ Attività 3: Implementare il gateway di app Azure lication



## Attività 1: Effettuare il provisioning dell'ambiente lab

In questa attività si userà un modello per distribuire una rete virtuale, un gruppo di sicurezza di rete e due macchine virtuali insieme alle schede di interfaccia di rete virtuale associate. Le macchine virtuali si troveranno in una rete virtuale hub denominata **az104-vnet1**.

1. Se necessario, scaricare il **\\file lab Allfiles\\Lab06\\az104-06-vms-loop-template.json** lab. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Deploy a custom template`.

1. Nella pagina di distribuzione personalizzata selezionare **Compila un modello personalizzato nell'editor**.

1. Nella pagina modifica modello selezionare **Carica file**.

1. Individuare e selezionare il **\\file Allfiles\\Lab06\\az104-06-vms-loop-template.json** e selezionare **Apri**.

   >**Nota: si noti che** questo file include i parametri nella parte superiore del file. Pertanto, non è necessario un file di parametri separato. 

1. Seleziona **Salva**.

1. Usare le informazioni seguenti per completare i campi nella pagina di distribuzione personalizzata, lasciando tutti gli altri campi con il valore predefinito.

    | Impostazione       | Valore         | 
    | ---           | ---           |
    | Subscription  | la propria sottoscrizione di Azure |
    | Gruppo di risorse| `az104-rg6` (Se necessario, selezionare **Crea nuovo**)
    | Area geografica        | **Stati Uniti orientali**   |
    | Dimensioni macchina virtuale       | **DS2 Standard v3** |
    | Nome utente amministratore| `localadmin` |
    | Password      | Specificare una password sicura |

     >**Nota**: se viene visualizzato un errore che indica che le dimensioni della macchina virtuale non sono disponibili, selezionare uno SKU disponibile nella sottoscrizione e che abbia almeno 2 core.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

    >**Nota**: attendere il completamento della distribuzione prima di passare all'attività successiva. La distribuzione dovrebbe essere completata in circa 5 minuti.
    
    >**Nota:** durante l'attesa, cercare e selezionare **Network Watcher**. Selezionare il **pannello Topologia** per ottenere una visualizzazione dell'infrastruttura di rete virtuale. Passare il puntatore del mouse sulle reti per visualizzare le informazioni sulla subnet e sugli indirizzi IP. 

## Attività 2: Implementare Azure Load Balancer

In questa attività verrà implementata un'istanza di Azure Load Balancer davanti alle due macchine virtuali di Azure nella rete virtuale hub. I servizi di bilanciamento del carico in Azure offrono connettività di livello 4 tra le risorse, ad esempio le macchine virtuali. La configurazione di Load Balancer include un indirizzo IP front-end per accettare connessioni, un pool back-end e regole che definiscono il modo in cui le connessioni devono attraversare il servizio di bilanciamento del carico.


## Diagramma dell'architettura - Load Balancer

>**Nota: si noti** che load Balancer distribuisce tra due macchine virtuali nella stessa rete virtuale. 


![Diagramma delle attività del lab.](../media/az104-lab06lb-architecture-diagram.png)


1. Nella portale di Azure cercare e selezionare `Load balancers` e nel pannello Servizi di bilanciamento** del **carico fare clic su **+ Crea**.

1. Creare un servizio di bilanciamento del carico con le impostazioni seguenti (lasciare le altre con i valori predefiniti) quindi fare clic su **Avanti: Configurazione IP front-end**:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | nome della sottoscrizione di Azure |
    | Gruppo di risorse | **az104-rg6** |
    | Nome | `az104-lb` |
    | Area | La **stessa** area in cui sono state distribuite le macchine virtuali |
    | SKU  | **Standard** |
    | Type | **Pubblica** |
    | Livello | **Regional** |
    
     ![Screenshot della pagina di creazione del servizio di bilanciamento del carico.](../media/az104-lab06-create-lb1.png)

1. Nella **scheda Configurazione** IP front-end fare clic su **Aggiungi una configurazione** IP front-end e usare le impostazioni seguenti:  
     
    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-fe` |
    | Tipo di IP | Indirizzo IP |
    | Indirizzo IP pubblico | Selezionare **Crea nuovo**|
    | Load Balancer Gateway | None |
    
1. **Nella finestra popup Aggiungi un indirizzo** IP pubblico usare le impostazioni seguenti prima di fare clic su **OK** e quindi su **Aggiungi**. Al termine, fare clic su **Avanti: Pool back-end**. 
     
    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-pip` |
    | SKU | Standard |
    | Livello | Regional |
    | Assegnazione | Statico |
    | Preferenza di routing | **Rete Microsoft** |

1. Nella scheda **Pool back-end** fare clic su **Aggiungi un pool back-end** con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni. Fare clic su **+ Aggiungi** (due volte) e quindi su  **Avanti: Regole** in ingresso. 

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-be` |
    | Rete virtuale | **az104-06-vnet1** |
    | Configurazione pool back-end | **NIC** | 
    | Versione IP | **IPv4** |
    | Fare clic su **Aggiungi** per aggiungere una macchina virtuale |  |
    | az104-vm0 | **Selezionare la casella** |
    | az104-vm1 | **Selezionare la casella** |

1. Nella scheda **Regole in ingresso** fare clic su **Aggiungi una regola di bilanciamento del carico**. Aggiungere una regola di bilanciamento del carico con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-lbrule` |
    | Versione IP | **IPv4** |
    | Indirizzo IP front-end | **az104-fe** |
    | Pool back-end | **az104-be** |
    | Protocollo | **TCP** |
    | Port | `80` |
    | Porta back-end | `80` |
    | Probe di integrità | **  Crea nuovo** |
    | Nome | `az104-hp` |
    | Protocollo | **TCP** |
    | Port | `80` |
    | Intervallo | `5` |
    | Chiudere la finestra di creazione di un probe di integrità | **Salva** | 
    | Persistenza della sessione | **Nessuno** |
    | Timeout di inattività (minuti) | `4` |
    | Reimpostazione TCP | **Disabilitato** |
    | IP mobile | **Disabilitato** |
    | SNAT (Network Address Translation) di origine in uscita | **Consigliato** |

1. Poiché si ha tempo, esaminare le altre schede, quindi fare clic su **Rivedi e crea**. Assicurarsi che non siano presenti errori di convalida, quindi fare clic su **Crea**. 

1. Attendere che il servizio di bilanciamento del carico venga distribuito e quindi fare clic su **Vai alla risorsa**.  

1. Selezionare **Configurazione IP front-end** nella pagina della risorsa Load Balancer. Copiare l'indirizzo IP pubblico.

1. Aprire un'altra scheda del browser e passare all'indirizzo IP. Verificare che nella finestra del browser sia visualizzato il messaggio **Hello World from az104-06-vm0** o **Hello World from az104-06-vm1**.

1. Aggiornare la finestra per verificare che il messaggio venga modificato in modo da fare riferimento all'altra macchina virtuale. Ciò indica che il servizio di bilanciamento del carico esegue la rotazione tra le macchine virtuali.

    > **Nota**: potrebbe essere necessario aggiornare più volte la visualizzazione o aprire una nuova finestra del browser in modalità InPrivate.

### Testare la connessione tra vm0 e vm1 

1. Nella portale di Azure cercare e selezionare `Network Watcher`.

1. In Network Watcher, nel menu Strumenti di diagnostica di rete selezionare **Connessione risoluzione dei problemi**.

1. Usare le informazioni seguenti per completare i campi nella **pagina di risoluzione dei problemi** di Connessione ion.

    | Campo | Valore | 
    | --- | --- |
    | Source type           | **Macchina virtuale**   |
    | Macchina virtuale       | **vm0**    | 
    | Tipo destinazione      | **Macchina virtuale**   |
    | Macchina virtuale       | **vm1**   | 
    | Versione IP preferita  | **Entrambi**              | 
    | Protocollo              | **TCP**               |
    | Porta di destinazione      | `3389`                |  
    | Porta di origine           | *Blank*         |
    | Test di diagnostica      | *Defaults*      |

    ![Portale di Azure che mostra le impostazioni di risoluzione dei problemi di Connessione.](../media/az104-lab06-connection-troubleshoot.png)

1. Selezionare **Esegui test** di diagnostica.

    >**Nota**: la restituzione dei risultati può richiedere alcuni minuti. Le selezioni dello schermo verranno disattivate durante la raccolta dei risultati. Si noti che il test** di **Connessione ivity mostra **Reachable**. Ciò ha senso perché le macchine virtuali si trovano nella stessa rete virtuale. 

## Attività 3: Implementare il gateway di app Azure lication

In questa attività si implementerà un gateway di app Azure lication davanti alle due macchine virtuali di Azure. Un gateway applicazione fornisce il bilanciamento del carico di livello 7, web application firewall (WAF), la terminazione SSL e la crittografia end-to-end per le risorse definite nel pool back-end. Il gateway applicazione instrada le immagini a una macchina virtuale e video all'altra macchina virtuale. 

## Diagramma dell'architettura - gateway applicazione

>**Nota**: questa gateway applicazione funziona nella stessa rete virtuale del servizio di bilanciamento del carico nelle attività precedenti. Questo non è tipico in un ambiente di produzione.

![Diagramma delle attività del lab.](../media/az104-lab06gw-architecture-diagram.png)

1. Nella portale di Azure cercare e selezionare `Virtual networks`.

1. Nel pannello **Reti** virtuali fare clic su **az104-vnet1** nell'elenco delle reti virtuali.

1. Nel pannello **az104-vnet1 virtual network (Rete virtuale az104-vnet1**) nella **sezione Impostazioni** fare clic su **Subnet** e quindi su **+ Subnet**.

1. Aggiungere una subnet con le impostazioni seguenti (lasciare altri con i valori predefiniti).

    | Impostazione | valore |
    | --- | --- |
    | Nome | `subnet-appgw` |
    | Intervallo di indirizzi subnet | `10.60.3.224/27` |

1. Fare clic su **Salva**

    > **Nota**: questa subnet verrà usata dalle istanze del gateway di app Azure lication. Il gateway applicazione richiede una subnet dedicata di dimensioni /27 o superiori. 

1. Nella portale di Azure cercare e selezionare `Application Gateways` e nel pannello **gateway applicazione s** fare clic su **+ Crea**.

1. Nella scheda **Generale** specificare ora le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | `az104-rg6` |
    | Nome del gateway applicazione | `az104-appgw` |
    | Area | La stessa** area di Azure usata nell'attività **1 |
    | Livello | **Standard V2** |
    | Abilitare la scalabilità automatica | **No** |
    | Numero di istanze | `2` |
    | Zona di disponibilità | **Nessuno** |
    | HTTP2 | **Disabilitato** |
    | Rete virtuale | **az104-06-vnet1** |
    | Subnet | **subnet-appgw (10.60.3.224/27)** |

    ![Screenshot della pagina Crea gateway app.](../media/az104-lab06-create-appgw.png)

1. Fare clic su **Avanti: Front-end >** e specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine fare clic su **OK**. 

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di indirizzo IP front-end | **Pubblica** |
    | Indirizzo IP pubblico| **Aggiungi nuovo** | 
    | Nome | `az104-gwpip` |
    | Zona di disponibilità | **Nessuno** |

1. Fare clic su **Avanti: Back-end >** e quindi su **Aggiungi un pool back-end**. Si tratta del pool back-end per **le immagini**. Specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-appgwbe` |
    | Aggiunta di uni pool back-end senza destinazioni | **No** |
    | Macchina virtuale | **az104-rg6-nic1 (10.60.1.4)** |
    | Macchina virtuale | **az104-rg6-nic2 (10.60.2.4)** | 

1. Fare clic su **Avanti: Configurazione >** e quindi su **+ Aggiungi una regola di routing**. Specificare le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Nome regola | `az104-gwrule` |
    | Priorità | `10` |
    | Nome listener | `az104-listener` |
    | IP front-end | **Pubblica** |
    | Protocollo | **HTTP** |
    | Porta | `80` |
    | Tipo di listener | **Base** |

    ![Screenshot della pagina crea regola del gateway app.](../media/az104-lab06-appgw-rule.png)

1. Passare alla scheda **Destinazioni back-end** e specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. 

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di destinazione | **Pool back-end** |
    | Destinazione back-end | **az104-appgwbe** |
    | Impostazioni back-end | **Aggiungi nuovo** |
    | Nome delle impostazioni back-end | `az104-http` |
    | Protocollo back-end | **HTTP** |
    | Porta back-end | `80` |
    | Impostazioni aggiuntive | **Accettare i valori predefiniti** |
    | Host name | **Accettare i valori predefiniti** |

   >**Nota:** usare le icone informative per altre informazioni sull'affinità **** basata su cookie e **sullo svuotamento** delle Connessione. 

1. Selezionare **Aggiungi più destinazioni per creare una regola** basata sul percorso. Verranno create due regole.

    **Regola : routing al back-end delle immagini**

    | Impostazione | Valore |
    | --- | --- |
    | Percorso | `images/*` |
    | Nome di destinazione | `images` |
    | Impostazioni back-end | **appgw-settings** |
    | Destinazione back-end | `az104-appgw-images` |

    **Regola : routing al back-end dei video**

    | Impostazione | Valore |
    | --- | --- |
    | Percorso | `video/*` |
    | Nome di destinazione | `videos` |
    | Impostazioni back-end | **appgw-settings** |
    | Destinazione back-end | `az104-appgw-videos` |

1. Selezionare **Aggiungi** due volte e quindi Avanti **: Tag >**.

1. Selezionare **Avanti: Rivedi e crea >** e quindi fare clic su **Crea**.

    > **Nota**: attendere la creazione dell'istanza del gateway applicazione. L'operazione richiederà circa 5-10 minuti.

1. Nella portale di Azure cercare e selezionare **az104-appgw**.

1. Nel gateway applicazione** selezionare **Integrità **** back-end.

1. Verificare che entrambi i server nel pool back-end visualizzino **Integro**. 

1. Nel pannello **az104-appgw** gateway applicazione copiare il valore dell'indirizzo **** IP pubblico front-end.

1. Avviare un'altra finestra del browser e testare l'URL - `http://<frontend ip address>/image/`.

1. Verificare di essere indirizzati al server di immagini (vm1). 

1. Avviare un'altra finestra del browser e testare l'URL - `http://<frontend ip address>/video/`.

1. Verificare di essere indirizzati al server di immagini (vm2). 

> **Nota**: potrebbe essere necessario aggiornare più volte la visualizzazione o aprire una nuova finestra del browser in modalità InPrivate.

## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Azure Load Balancer è un'ottima scelta per distribuire il traffico di rete tra più macchine virtuali a livello di trasporto (livello OSI 4 - TCP e UDP).
+ Si usano i bilanciamenti del carico pubblici per bilanciare il carico del traffico Internet verso le macchine virtuali. Il bilanciamento del carico interno (o privato) viene usato se gli indirizzi IP privati sono necessari solo sul front-end.
+ Il servizio di bilanciamento del carico Basic è destinato alle applicazioni su scala ridotta che non necessitano di disponibilità elevata o ridondanza. Il servizio di bilanciamento del carico Standard è per prestazioni elevate e latenza ultra bassa.
+ Il gateway applicazione di Azure è un servizio di bilanciamento del carico del traffico Web (livello OSI 7) che consente di gestire il traffico verso le applicazioni Web. 
+ Un gateway applicazione può prendere decisioni di routing basate su attributi aggiuntivi di una richiesta HTTP, ad esempio percorso URI o intestazioni host. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.
