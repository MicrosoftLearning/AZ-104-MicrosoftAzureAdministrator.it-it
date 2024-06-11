---
lab:
  title: 'Lab 06: Implementare Gestione del traffico'
  module: Administer Network Traffic Management
---

# Lab 06 - Implementare Gestione traffico

## Introduzione al lab

In questo lab si apprenderà come configurare e testare un Azure Load Balancer e un gateway applicazione di Azure.

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti orientali**.

## Tempo stimato: 50 minuti

## Scenario laboratorio

L'organizzazione ha un sito Web pubblico. È necessario bilanciare il carico delle richieste pubbliche in ingresso tra macchine virtuali diverse. È anche necessario fornire immagini e video da macchine virtuali diverse. Si prevede di implementare Azure Load Balancer e gateway applicazione di Azure. Tutte le risorse si trovano nella stessa area.

## Simulazioni interattive del lab

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di eseguire uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti concetti fondamentali sono identici. Non è necessaria una sottoscrizione di Azure.

+ [Creare e configurare bilanciamento del carico di Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20an%20Azure%20load%20balancer). Creare una rete virtuale, server back-end, bilanciamento del carico e quindi testare il bilanciamento del carico.
+ [Distribuire il gateway applicazione di Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Deploy%20Azure%20Application%20Gateway). Creare un gateway applicazione, creare macchine virtuali, creare il pool back-end e quindi testare il gateway.
+ [Implementare gestione del traffico](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010). Implementare una rete hub-spoke completa, comprendente macchine virtuali, reti virtuali, peering, bilanciamento del carico e gateway applicazione.

## Competenze mansione

+ Attività 1: Usare un modello per effettuare il provisioning di un'infrastruttura.
+ Attività 2: Configurare un Azure Load Balancer.
+ Attività 3: Configurare un gateway applicazione di Azure.

## Attività 1: Usare un modello per effettuare il provisioning di un'infrastruttura

In questa attività si userà un modello per distribuire una rete virtuale, un gruppo di sicurezza di rete e due macchine virtuali.

1. Scaricare i file lab **\\Allfiles\\Lab06** (modelli e parametri).

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `Deploy a custom template`.

1. Nella pagina di distribuzione personalizzata, selezionare **Creare un modello personalizzato nell’editor**.

1. Nella pagina di modifica del modello, selezionare **Carica file**.

1. Individuare e selezionare il file **\\Allfiles\\Lab06\\az104-06-vms-template.json**, quindi selezionare **Apri**.

1. Seleziona **Salva**.

1. Selezionare **Modifica parametri** e caricare il file **\\Allfiles\\Lab06\\az104-06-vms-parameters.json**.

1. Seleziona **Salva**.

1. Usare le informazioni seguenti per completare i campi nella pagina di distribuzione personalizzata, lasciando tutti gli altri campi con il valore predefinito.

    | Impostazione       | Valore         |
    | ---           | ---           |
    | Subscription  | sottoscrizione di Azure |
    | Gruppo di risorse | `az104-rg6` (Se necessario, selezionare **Crea nuovo**) |
    | Password      | Specificare una password sicura |

    >**Nota**: Se viene visualizzato un errore indicante che le dimensioni della macchina virtuale non sono disponibili, selezionare uno SKU disponibile nella sottoscrizione, che disponga di almeno 2 core.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

    >**Nota**: Prima di passare all'attività successiva, attendere il completamento della distribuzione. La distribuzione dovrebbe richiedere circa 5 minuti.

    >**Nota**: Esaminare le risorse distribuite. Sarà presente una rete virtuale con tre subnet. Ogni subnet disporrà di una macchina virtuale.

## Attività 2: Configurare Azure Load Balancer

In questa attività si implementa Azure Load Balancer davanti alle due macchine virtuali di Azure nella rete virtuale. I servizi di bilanciamento del carico in Azure offrono connettività di livello 4 tra le risorse, ad esempio le macchine virtuali. La configurazione di Load Balancer include un indirizzo IP front-end per accettare connessioni, un pool back-end e le regole che definiscono il modo in cui le connessioni devono attraversare il servizio di bilanciamento del carico.

## Diagramma dell'architettura - Load Balancer

>**Nota**: Si noti che Load Balancer distribuisce tra due macchine virtuali nella stessa rete virtuale.

![Diagramma delle attività del lab.](../media/az104-lab06-lb-architecture.png)

1. Nel portale di Azure cercare e selezionare `Load balancers` e nel pannello **Servizi di bilanciamento del carico** fare clic su **+ Crea**.

1. Creare un servizio di bilanciamento del carico con le impostazioni seguenti (lasciare le altre con i rispettivi valori predefiniti) quindi fare clic su **Avanti: Configurazione IP front-end**:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | sottoscrizione di Azure |
    | Gruppo di risorse | **az104-rg6** |
    | Nome | `az104-lb` |
    | Paese | La **stessa** area in cui sono state distribuite le VM |
    | SKU  | **Standard** |
    | Type | **Pubblica** |
    | Livello | **Regional** |

     ![Screenshot della pagina Crea bilanciamento del carico.](../media/az104-lab06-create-lb1.png)

1. Nella scheda **Configurazione IP front-end** fare clic su **Aggiungi una configurazione IP front-end** e usare le impostazioni seguenti:  

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-fe` |
    | Tipo di IP | Indirizzo IP |
    | Bilanciamento del carico del gateway | None |
    | Indirizzo IP pubblico | Selezionare **Crea nuovo** (usare le istruzioni nel passaggio successivo) |

1. Nella finestra popup **Aggiungi indirizzo IP pubblico**, usare le impostazioni seguenti, poi fare clic su **OK** e quindi scegliere **Aggiungi**. Al termine, fare clic su **Avanti: Pool back-end**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-lbpip` |
    | SKU | Standard |
    | Livello | Regional |
    | Assegnazione | Statico |
    | Preferenza routing | **Microsoft.Network** |

    >**Nota:** Lo SKU Standard fornisce un indirizzo IP statico. Gli indirizzi IP statici vengono assegnati alla risorsa e rilasciati quando questa viene eliminata.  

1. Nella scheda **Pool back-end** fare clic su **Aggiungi un pool back-end** con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni. Fare clic su **+ Aggiungi** (due volte), quindi su **Avanti: Regole in ingresso**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-be` |
    | Rete virtuale | **az104-06-vnet1** |
    | Configurazione pool back-end | **NIC** |
    | Fare clic su **Aggiungi** per aggiungere una macchina virtuale |  |
    | az104-06-vm0 | **Selezionare la casella** |
    | az104-06-vm1 | **Selezionare la casella** |

1. Quando si ha tempo, esaminare le altre schede, quindi fare clic su **Rivedi e crea**. Assicurarsi che non siano presenti errori di convalida, quindi fare clic su **Crea**.

1. Attendere che il servizio di bilanciamento del carico venga distribuito e quindi fare clic su **Vai alla risorsa**.

**Aggiungere una regola per determinare la modalità di distribuzione del traffico in ingresso**

1. Nel pannello **Impostazioni** selezionare **Regole di bilanciamento del carico**.

1. Seleziona **+ Aggiungi**. Aggiungere una regola di bilanciamento del carico con le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni.  Quando si configura la regola, usare le icone informative per ottenere informazioni su ciascuna impostazione. Al termine, fare clic su **Salva**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-lbrule` |
    | Versione IP | **IPv4** |
    | Indirizzo IP front-end | **az104-fe** |
    | Pool back-end | **az104-be** |
    | Protocollo | **TCP** |
    | Porta | `80` |
    | Porta back-end | `80` |
    | Probe di integrità | **  Crea nuovo** |
    | Nome | `az104-hp` |
    | Protocollo | **TCP** |
    | Porta | `80` |
    | Intervallo | `5` |
    | Chiudere la finestra di creazione di un probe di integrità | **Salva** |
    | Persistenza della sessione | **Nessuno** |
    | Timeout di inattività (minuti) | `4` |
    | Reimpostazione TCP | **Disabilitato** |
    | IP mobile | **Disabilitato** |
    | SNAT (Network Address Translation) di origine in uscita | **Consigliato** |

1. Selezionare **Configurazione IP front-end** nella pagina Load Balancer. Copiare l'indirizzo IP pubblico.

1. Aprire un'altra scheda del browser e passare all'indirizzo IP. Verificare che nella finestra del browser sia visualizzato il messaggio **Hello World from az104-06-vm0** o **Hello World from az104-06-vm1**.

1. Aggiornare la finestra per verificare che il messaggio venga modificato in modo da fare riferimento all'altra macchina virtuale. Ciò indica che il servizio di bilanciamento del carico esegue la rotazione tra le macchine virtuali.

    > **Nota**: potrebbe essere necessario aggiornare più volte la visualizzazione o aprire una nuova finestra del browser in modalità InPrivate.

## Attività 3: Configurare un gateway applicazione di Azure

In questa attività si implementa un gateway applicazione di Azure davanti a due macchine virtuali di Azure. Un gateway applicazione fornisce bilanciamento del carico di livello 7, Web application firewall (WAF), terminazione SSL e crittografia end-to-end per le risorse definite nel pool back-end. Il gateway applicazione instrada le immagini a una macchina virtuale e i video all'altra macchina virtuale.

## Diagramma dell'architettura - Gateway applicazione

>**Nota**: Questo gateway applicazione lavora nella stessa rete virtuale di Load Balancer. Questo potrebbe non essere tipico in un ambiente di produzione.

![Diagramma delle attività del lab.](../media/az104-lab06-gw-architecture.png)

1. Nel portale di Azure cercare e selezionare `Virtual networks`.

1. Nel pannello **Reti virtuali**, nell'elenco di reti virtuali, fare clic su **az104-06-vnet1**.

1. Nel pannello della rete virtuale **az104-06-vnet1**, nella sezione **Impostazioni**, fare clic su **Subnet** e quindi su **+ Subnet**.

1. Aggiungere una subnet con le impostazioni seguenti (lasciare le altre con i rispettivi valori predefiniti).

    | Impostazione | valore |
    | --- | --- |
    | Nome | `subnet-appgw` |
    | Intervallo di indirizzi subnet | `10.60.3.224/27` |

1. Fare clic su **Salva**

    > **Nota**: Questa subnet verrà usata dal gateway applicazione di Azure. Il gateway applicazione richiede una subnet dedicata di dimensioni /27 o superiori.

1. Nel portale di Azure cercare e selezionare `Application gateways` e nel pannello **Gateway applicazione** fare clic su **+ Crea**.

1. Nella scheda **Generale** specificare ora le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | --- |
    | Subscription | sottoscrizione di Azure |
    | Gruppo di risorse | `az104-rg6` |
    | Nome del gateway applicazione | `az104-appgw` |
    | Paese | La **stessa** area di Azure usata nell'attività 1 |
    | Livello | **Standard V2** |
    | Abilitare la scalabilità automatica | **No** |
    | Numero minimo di istanze | `2` |
    | Zona di disponibilità | **Zona 1** |
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

    >**Nota:** Gateway applicazione può avere un indirizzo IP sia pubblico sia privato.
 
1. Fare clic su **Avanti: Back-end >** e quindi **Aggiungi un pool back-end**. Specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-appgwbe` |
    | Aggiunta di uni pool back-end senza destinazioni | **No** |
    | Macchina virtuale | **az104-rg6-nic1 (10.60.1.4)** |
    | Macchina virtuale | **az104-rg6-nic2 (10.60.2.4)** |

1. Fare clic su **Aggiungi un pool back-end**. Si tratta del pool back-end per **immagini**. Specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-imagebe` |
    | Aggiunta di uni pool back-end senza destinazioni | **No** |
    | Macchina virtuale | **az104-rg6-nic1 (10.60.1.4)** |

1. Fare clic su **Aggiungi un pool back-end**. Questo è il pool back-end per **video**. Specificare le impostazioni seguenti, senza modificare i valori predefiniti per le altre impostazioni. Al termine, fare clic su **Aggiungi**.

    | Impostazione | valore |
    | --- | --- |
    | Nome | `az104-videobe` |
    | Aggiunta di uni pool back-end senza destinazioni | **No** |
    | Macchina virtuale | **az104-rg6-nic2 (10.60.2.4)** |

1. Selezionare **Avanti: Configurazione >** e quindi **Aggiungi una regola di gestione**. Completare le informazioni.

    | Impostazione | Valore |
    | --- | --- |
    | Nome regola | `az104-gwrule` |
    | Priorità | `10` |
    | Nome listener | `az104-listener` |
    | IP front-end | **IPv4 pubblico** |
    | Protocollo | **HTTP** |
    | Porta | `80` |
    | Tipo di listener | **Base** |

1. Passare alla scheda **destinazioni back-end**. Dopo aver completato le informazioni di base, selezionare **Aggiungi**.

   | Impostazione | Valore |
    | --- | --- |
    | Destinazione back-end | `az104-appgwbe` |
    | Impostazioni back-end | `az104-http` (crea nuovo) |

   >**Nota:** Leggere le informazioni relativa a **Affinità basata su cookie** e **Svuotamento della connessione**.

1. Nella sezione **Routing basato su percorso** selezionare **Aggiungi più destinazioni per creare una regola basata sul percorso**. Verranno create due regole. Fare clic su **Aggiungi** dopo la prima regola e quindi su **Aggiungi** dopo la seconda regola. 

    **Regola: routing a back-end immagini**

    | Impostazione | Valore |
    | --- | --- |
    | Percorso | `/image/*` |
    | Nome di destinazione | `images` |
    | Impostazioni back-end | **az104-http** |
    | Destinazione back-end | `az104-imagebe` |

    **Regola: routing a back-end video**

    | Impostazione | Valore |
    | --- | --- |
    | Percorso | `/video/*` |
    | Nome di destinazione | `videos` |
    | Impostazioni back-end | **az104-http** |
    | Destinazione back-end | `az104-videobe` |

1. Assicurarsi di **salvare** e controllare le modifiche, quindi selezionare **Avanti: Tag >**. Non sono necessarie modifiche.

1. Selezionare **Avanti: Rivedi e crea >**, quindi fare clic su **Crea**.

    > **Nota**: attendere la creazione dell'istanza del gateway applicazione. L'operazione richiederà circa 5-10 minuti. Durante l'attesa, è consigliabile esaminare alcuni dei collegamenti alla fine di questa pagina, per eseguire il training autogestito.

1. Dopo la distribuzione del gateway applicazione, cercare e selezionare **az104-appgw**.

1. Nella risorsa **Gateway applicazione**, nella sezione **Monitoraggio** selezionare **Integrità back-end**.

1. Verificare che entrambi i server nel pool back-end visualizzino **Integro**.

1. Nel pannello **Panoramica**, copiare il valore di **Indirizzo IP pubblico front-end**.

1. Avviare un'altra finestra del browser e testare l’URL: `http://<frontend ip address>/image/`.

1. Verificare di essere indirizzati al server immagini (vm1).

1. Avviare un'altra finestra del browser e testare l’URL: `http://<frontend ip address>/video/`.

1. Verificare di essere indirizzati al server video (vm2).

> **Nota**: potrebbe essere necessario aggiornare più volte la visualizzazione o aprire una nuova finestra del browser in modalità InPrivate.

## Pulire le risorse

Se si usa la **sottoscrizione personale** dedicare qualche minuto all’eliminazione delle risorse del lab. In tal modo, vengono liberate risorse e i costi vengono ridotti al minimo. Il modo più semplice per eliminare queste risorse del lab consiste nell'eliminazione del gruppo di risorse del lab. 

+ Nel portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse**, **Immettere il nome del gruppo di risorse** e quindi fare clic su **Elimina**.
+ Usando Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usando l’interfaccia della riga di comando, `az group delete --name resourceGroupName`.

## Estendere l'apprendimento con Copilot

Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a *copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.

+ Confrontare e contrapporre Azure Load Balancer con gateway applicazione di Azure.
+ Come è possibile risolvere i problemi di connettività in ingresso di Azure Load Balancer?
+ Quali sono i passaggi di base per la configurazione di gateway applicazione di Azure?
+ Creare una tabella che evidenzia le soluzioni di bilanciamento del carico di Azure. Includere le colonne: Protocolli supportati, Bilanciamento del carico privato, Bilanciamento del carico globale, Criteri di routing, Ambienti supportati, Svuotamento della connessione, Affinità di sessione, Bilanciamento del carico basato su host e percorso, Offload TLS, Accelerazione del sito, Sicurezza, Memorizzazione nella cache e Compressione.

## Altre informazioni con la formazione autogestita

+ [Migliorare la scalabilità e la resilienza delle applicazioni tramite Azure Load Balancer](https://learn.microsoft.com/training/modules/improve-app-scalability-resiliency-with-load-balancer/). Presentazione dei diversi servizi di bilanciamento del carico in Azure e di come scegliere la soluzione di bilanciamento del carico di Azure appropriata per soddisfare i requisiti.
+ [Bilanciare il carico del traffico del servizio Web con il gateway applicazione](https://learn.microsoft.com/training/modules/load-balance-web-traffic-with-application-gateway/). È possibile migliorare la resilienza dell'applicazione distribuendo il carico tra più server e usare il routing basato sul percorso per indirizzare il traffico Web.

## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab.

+ Al livello di trasporto (livello OSI 4 - TCP e UDP), Azure Load Balancer è un'ottima scelta per distribuire il traffico di rete tra più macchine virtuali.
+ Si usano i bilanciamenti del carico pubblici per bilanciare il carico del traffico Internet verso le macchine virtuali. Il bilanciamento del carico interno (o privato) viene usato se gli indirizzi IP privati sono necessari solo sul front-end.
+ Il bilanciamento del carico Basic è destinato ad applicazioni su scala ridotta che non necessitano di disponibilità elevata o ridondanza. Il bilanciamento del carico Standard è finalizzato per prestazioni elevate e latenza ultra bassa.
+ Il gateway applicazione di Azure è un servizio di bilanciamento del carico del traffico Web (livello OSI 7) che consente di gestire il traffico verso le applicazioni Web.
+ Il livello Standard di gateway applicazione offre tutte le funzionalità L7, incluso il bilanciamento del carico. Il livello WAF aggiunge un firewall per verificare la presenza di traffico dannoso.
+ Un gateway applicazione può prendere decisioni di routing basate su attributi aggiuntivi di una richiesta HTTP, ad esempio il percorso URI o le intestazioni host.
