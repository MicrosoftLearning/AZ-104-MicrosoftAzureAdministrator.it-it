---
lab:
  title: 'Lab 09b: Implementare contenitori di Azure'
  module: Administer PaaS Compute Options
---

# Lab 09b - Implementare contenitori di Azure

## Introduzione al lab

In questo lab si apprenderà come implementare Istanze di Azure Container e App Azure Container. Si apprenderà come distribuire un'istanza di Azure Container per visualizzare un'app Hello World.
Si apprenderà come distribuire l'app Azure Container predefinita. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

## Tempo stimato: 30 minuti

## Scenario laboratorio

L'organizzazione ha un'applicazione Web che viene eseguita in una macchina virtuale nel data center locale. L'organizzazione vuole spostare tutte le applicazioni nel cloud, ma non vuole gestire un numero elevato di server. Si decide di valutare Istanze di Azure Container e Docker. Si vuole anche distribuire e testare un'app contenitore di Azure.

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ [Distribuire Istanze di Azure Container](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203). Creare, configurare e distribuire un contenitore Docker con Istanze di Azure Container. 
+ [Implementare Istanze di Azure Container](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014).  Distribuire un'immagine Docker usando Istanze di Azure Container. 

## Attività

- Attività 1: Distribuire un'istanza di Azure Container usando un'immagine Docker
- Attività 2: Esaminare la funzionalità di Istanza di Azure Container
- Attività 3: Creare un'app e un ambiente Azure Container
- Attività 4: Distribuire e testare l'app contenitore

## Attività 1 e 2: diagramma dell'architettura Istanze di Azure Container

![Diagramma delle attività.](../media/az104-lab09baci-architecture-diagram.png)

## Attività 1: Distribuire un'istanza di Azure Container usando un'immagine Docker

In questa attività verrà creata una nuova istanza di contenitore per l'applicazione Web. Docker è una piattaforma che consente di creare pacchetti ed eseguire applicazioni in ambienti isolati denominati contenitori. Istanze di Azure Container fornisce l'ambiente di calcolo per un'immagine del contenitore.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Nella portale di Azure cercare e selezionare `Container instances` e quindi **nel pannello Istanze** di Contenitore fare clic su **+ Crea**.

1. Nella scheda **Dati principali** del pannello **Crea istanza di Container** specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | ---- | ---- |
    | Subscription | nome della sottoscrizione di Azure |
    | Gruppo di risorse | `az104-rg9` (Se necessario, selezionare **Crea nuovo**) |
    | Nome contenitore | `az104-c1` |
    | Area | **Stati Uniti** orientali (o un'area disponibile nelle vicinanze)|
    | Origine immagine | **Immagini di avvio rapido** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Fare clic su **Avanti: Rete >** e nella scheda **Rete** del pannello **Crea istanza di Container** specificare le impostazioni seguenti, lasciando i valori predefiniti per le altre:

    | Impostazione | Valore |
    | --- | --- |
    | Etichetta del nome DNS | Qualsiasi nome host DNS valido e univoco a livello globale |

    >**Nota**: il contenitore sarà raggiungibile pubblicamente all'indirizzo dns-name-label.region.azurecontainer.io. Se viene visualizzato un messaggio di errore **L'etichetta del nome DNS non è disponibile**, specificare un valore diverso.

1. Fare clic su **Avanti: Avanzate >**, esaminare le impostazioni nella **scheda Avanzate** del **pannello Crea istanza** del contenitore senza apportare modifiche.

 1. Fare clic su **Rivedi e crea**, verificare che la convalida sia stata superata e quindi selezionare **Crea**.

    >**Nota**: attendere il completamento della distribuzione. L'operazione dovrebbe richiedere 2-3 minuti.

    >**Nota**: durante l'attesa può essere interessante visualizzare il [codice alla base di questa applicazione di esempio](https://github.com/Azure-Samples/aci-helloworld). Per visualizzarlo, passare alla cartella \\app.

## Attività 2: Esaminare la funzionalità di Istanza di Azure Container

In questa attività verrà esaminata la distribuzione dell'istanza di contenitore. Per impostazione predefinita, l'istanza di Azure Container sarà accessibile sulla porta 80. Dopo aver distribuito l'istanza, è possibile passare al contenitore usando il nome DNS fornito nell'attività precedente.

1. Nel pannello della distribuzione fare clic sul collegamento **Vai alla risorsa**.

1. Nel pannello **Panoramica** dell'istanza di contenitore verificare che **Stato** indichi **In esecuzione**.

1. Copiare il valore **FQDN**dell'istanza di contenitore, aprire una nuova scheda del browser e passare all'URL corrispondente.

     ![Screenshot della pagina di panoramica di ACI nel portale.](../media/az104-lab09b-aci-overview.png)

1. Verificare che venga visualizzata la pagina **Benvenuti in Istanze di Azure Container**.

1. Chiudere la nuova scheda del browser e nel portale di Azure, nella sezione **Impostazioni** del pannello dell'istanza del contenitore, fare clic su **Contenitori** e quindi su **Log**.

1. Verificare che siano visualizzate le voci di log che rappresentano la richiesta HTTP GET generata visualizzando l'applicazione nel browser.

## Attività 3 e 4: Diagramma dell'architettura delle app di Azure Container

![Diagramma delle attività.](../media/az104-lab09baca-architecture-diagram.png)

## Attività 3: Creare un'app e un ambiente contenitore

App Contenitore di Azure considerano ulteriormente il concetto di cluster Kubernetes gestito e gestisce ulteriormente l'ambiente del cluster, oltre a fornire altri servizi gestiti all'interno del cluster. A differenza di un cluster Azure Kubernetes, in cui è comunque necessario gestire il cluster, un'istanza di App Contenitore di Azure rimuove alcune delle complessità per configurare un cluster Kubernetes.

1. Nella portale di Azure cercare e selezionare `Container Apps`.

1. In **App** contenitore selezionare **Crea**.

1. Usare le informazioni seguenti per compilare i dettagli nella **scheda Informazioni di base** e quindi selezionare **Avanti: Contenitore >**.

    | Impostazione | Azione |
    |---|---|
    | Abbonamento | Selezionare la Sottoscrizione di Azure |
    | Gruppo di risorse | `az104-rg9` |
    | Nome dell'app contenitore |  `my-app` |
    | Area    | **Stati Uniti** orientali (o un'area disponibile nelle vicinanze) |
    | Ambiente app contenitore | Lasciare il valore predefinito |

1. Assicurarsi che **l'opzione Usa immagine** di avvio rapido sia abilitata e che l'immagine di avvio rapido sia impostata su **Contenitore Simple hello world**.

1. Selezionare il **pulsante Rivedi e crea** nella parte inferiore della pagina. 

    >**Nota**: se sono presenti errori, qualsiasi scheda contenente errori viene contrassegnata con un punto rosso.  Passare alla scheda appropriata.  I campi contenenti un errore verranno evidenziati in rosso.  Dopo aver corretto tutti gli errori, selezionare **Rivedi e crea** di nuovo.

1. Seleziona **Crea**.

    >**Nota:** viene visualizzata una pagina con il messaggio *Distribuzione in corso* .  Al termine della distribuzione, verrà visualizzato il messaggio: *La distribuzione è stata completata*.
 
## Attività 4: Testare e verificare la distribuzione dell'app contenitore

Per impostazione predefinita, l'app contenitore di Azure creata accetterà il traffico sulla porta 80 usando l'applicazione Hello World di esempio. App Azure Container fornirà un nome DNS per l'applicazione. Copiare e passare a questo URL per assicurarsi che l'applicazione sia attiva e in esecuzione.

1. Selezionare Vai alla risorsa** per visualizzare **la nuova app contenitore.

1. Selezionare il collegamento accanto a *URL* applicazione per visualizzare l'applicazione.

    ![Screenshot della pagina di panoramica di ACA nel portale.](../media/az104-lab09b-aca-overview.png)

1. Verificare che l'app **App Azure Container sia in tempo reale** .

## Esaminare i punti principali del lab

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Istanze di Azure Container (ACI) è un servizio che consente di distribuire contenitori nel cloud pubblico di Microsoft Azure. ACI non richiede il provisioning o la gestione di un'infrastruttura sottostante. Il servizio supporta sia contenitori Linux che contenitori Windows.
+ App Azure Container (ACA) è una piattaforma serverless che consente di mantenere meno infrastruttura e risparmiare sui costi durante l'esecuzione di applicazioni in contenitori. Invece di preoccuparsi della configurazione del server, dell'orchestrazione dei contenitori e dei dettagli della distribuzione, App contenitore fornisce tutte le risorse server aggiornate necessarie per mantenere le applicazioni stabili e sicure.
+ I carichi di lavoro in Istanze di Azure Container vengono in genere avviati e arrestati da un certo tipo di processo o trigger e sono in genere di breve durata. I carichi di lavoro in ACA sono in genere processi a esecuzione prolungata, ad esempio un'app Web.

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.

+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.
