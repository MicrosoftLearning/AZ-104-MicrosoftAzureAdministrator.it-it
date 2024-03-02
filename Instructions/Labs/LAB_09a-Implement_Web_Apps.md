---
lab:
  title: 'Lab 09a: Implementare App Web'
  module: Administer PaaS Compute Options
---

# Lab 09a - Implementare app Web


## Introduzione al lab

In questo lab vengono fornite informazioni sulle app Web di Azure. Si apprenderà come configurare un'app Web per visualizzare un'applicazione Hello World in un repository GitHub esterno. Si apprenderà come creare uno slot di staging e scambiare con lo slot di produzione. Si apprenderà anche la scalabilità automatica per soddisfare le modifiche della domanda.

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

## Tempo stimato: 20 minuti

## Scenario laboratorio

L'organizzazione è interessata alle app Web di Azure per ospitare i siti Web aziendali. I siti Web sono attualmente ospitati in un data center locale. I siti Web vengono eseguiti nei server Windows usando lo stack di runtime PHP. L'hardware sta per scadere e sarà presto necessario sostituirlo. L'organizzazione vuole evitare nuovi costi hardware usando Azure per ospitare i siti Web. 

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ [Creare un'app](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%202) Web. Creare un'app Web che esegue un contenitore Docker.
    
+ [Implementare app](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013) Web di Azure. Creare un'app Web di Azure, gestire la distribuzione e ridimensionare l'app. 

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab09a-architecture.png)

## Competenze mansione

+ Attività 1: Creare e configurare un'app Web di Azure.
+ Attività 2: Creare e configurare uno slot di distribuzione.
+ Attività 3: Configurare le impostazioni di distribuzione dell’app Web.
+ Attività 4: Scambiare gli slot di distribuzione.
+ Attività 5: Configurare e testare la scalabilità automatica dell'app Web di Azure.

## Attività 1: Creare e configurare un'app Web di Azure

In questa attività si crea un'app Web di Azure. app Azure Services è una soluzione PAAS (Platform As a Service) per applicazioni Web, per dispositivi mobili e altre applicazioni basate sul Web. Le app Web di Azure fanno parte app Azure Servizi che ospitano la maggior parte degli ambienti di runtime, ad esempio PHP, Java e .NET. Il piano di servizio app selezionato determina il calcolo, l'archiviazione e le funzionalità dell'app Web. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `App services`.

1. Selezionare **+ Crea** dal menu a discesa App **** Web. Si notino le altre scelte. 

1. Nella scheda **Informazioni di base** del pannello **Crea app Web** specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | ---|
    | Subscription | sottoscrizione di Azure |
    | Gruppo di risorse | `az104-rg9` (Se necessario, selezionare **Crea nuovo**) |
    | Nome dell'app Web | Qualsiasi nome univoco a livello globale |
    | Pubblica | **Codice** |
    | Stack di runtime | **PHP 8.2** |
    | Sistema operativo | **Linux** |
    | Area | **Stati Uniti orientali** |
    | Piani dei prezzi | accettare le impostazioni predefinite |
    | Ridondanza della zona | accettare le impostazioni predefinite |

 1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere che l'app Web venga creata prima di procedere con l'attività successiva. L'operazione dovrebbe richiedere circa un minuto.

1. Dopo la distribuzione, selezionare **Vai alla risorsa**.

## Attività 2: Creare e configurare uno slot di distribuzione

In questa attività verrà creato uno slot di distribuzione di staging. Gli slot di distribuzione consentono di eseguire test prima di rendere disponibile l'app al pubblico (o agli utenti finali). Dopo aver eseguito il test, è possibile scambiare lo slot dallo sviluppo o dalla gestione temporanea all'ambiente di produzione. Molte organizzazioni usano slot per eseguire test di pre-produzione. Inoltre, molte organizzazioni eseguono più slot per ogni applicazione, ad esempio sviluppo, controllo di qualità, test e produzione.

1. Nel pannello dell'app Web appena distribuita fare clic sul **collegamento Dominio predefinito** per visualizzare la pagina Web predefinita in una nuova scheda del browser.

1. Chiudere la nuova scheda del browser e portale di Azure fare clic su Slot di distribuzione nella **sezione Distribuzione** del pannello App Web.****

    >**Nota**: l'app Web, a questo punto, ha un singolo slot di distribuzione con etichetta **PRODUCTION**.

1. Fare clic su **+ Add slot** (Aggiungi slot) e aggiungere un nuovo slot con le impostazioni seguenti:

    | Impostazione | valore |
    | --- | ---|
    | Nome | `staging` |
    | Clona le impostazioni da | **Non clonare le impostazioni**|

1. Selezionare **Aggiungi**.

1. Tornare al pannello **Slot** di distribuzione dell'app Web, fare clic sulla voce che rappresenta lo slot di staging appena creato.

    >**Nota**: verrà visualizzato il pannello che mostra le proprietà dello slot di staging.

1. Esaminare il pannello dello slot di staging e notare che l'URL è diverso da quello assegnato allo slot di produzione.

## Attività 3: Configurare le impostazioni di distribuzione dell'app Web

In questa attività verranno configurate le impostazioni di distribuzione dell'app Web. Le impostazioni di distribuzione consentono la distribuzione continua. Ciò garantisce che il servizio app abbia la versione più recente dell'applicazione.

1. Nello slot di staging selezionare **Centro** distribuzione e quindi selezionare **Impostazioni**.

    >**Nota:** assicurarsi di essere nel pannello dello slot di staging (anziché nello slot di produzione).
    
1. Nell'elenco **a discesa Origine** selezionare **Git** esterno. Si notino le altre scelte. 

1. Nel campo repository immettere `https://github.com/Azure-Samples/php-docs-hello-world`

1. Nel campo ramo immettere `master`.

1. Seleziona **Salva**.

1. Nello slot di staging selezionare **Panoramica**.

1. Selezionare il **collegamento Dominio predefinito** e aprire l'URL in una nuova scheda. 

1. Verificare che lo slot di staging visualizzi **Hello World**.

>**Nota:** la distribuzione potrebbe richiedere un minuto. Assicurarsi di aggiornare **** la pagina dell'applicazione.

## Attività 4: Scambiare gli slot di distribuzione

In questa attività lo slot di staging verrà scambiato con lo slot di produzione. Lo scambio di uno slot consente di usare il codice testato nello slot di staging e spostarlo nell'ambiente di produzione. Il portale di Azure richiederà anche se è necessario spostare altre impostazioni dell'applicazione personalizzate per lo slot. Lo scambio degli slot è un'attività comune per i team di supporto delle applicazioni e per i team di supporto delle applicazioni, in particolare quelli che distribuiscono aggiornamenti di routine delle app e correzioni di bug.

1. Tornare al pannello **Slot** di distribuzione e quindi selezionare **Scambia**.

1. Esaminare le impostazioni predefinite e fare clic su **Scambia**.

1. Nel pannello **Panoramica** dell'app Web selezionare il **collegamento Dominio predefinito** per visualizzare la home page del sito Web.

1. Verificare che la pagina Web di produzione visualizzi **Hello World!** .

    >**Nota:** copiare l'URL** di dominio **predefinito necessario per i test di carico nell'attività successiva. 

## Attività 5: Configurare e testare la scalabilità automatica dell'app Web di Azure

In questa attività verrà configurata la scalabilità automatica dell'app Web di Azure. La scalabilità automatica consente di mantenere prestazioni ottimali per l'app Web quando aumenta il traffico verso l'app Web. Per determinare quando l'app deve essere ridimensionata, è possibile monitorare metriche come utilizzo della CPU, memoria o larghezza di banda.

1. **Nella sezione Impostazioni** selezionare **Scale out (piano di servizio app).**

    >**Nota:** assicurarsi di lavorare sullo slot di produzione non sullo slot di staging.  

1. **Nella sezione Ridimensionamento** selezionare **Automatico**. Si noti l'opzione **Basata su** regole. Il ridimensionamento basato su regole può essere configurato per metriche di app diverse. 

1. **Nel campo Burst** massimo selezionare **2**.

    ![Screenshot della pagina di scalabilità automatica.](../media/az104-lab09a-autoscale.png)

1. Seleziona **Salva**.

1. Selezionare **Diagnostica e risoluzione dei problemi** (riquadro sinistro).

1. Nella casella Test di carico dell'app **** selezionare **Crea test** di carico.

    + Selezionare **+ Crea** e assegnare un nome** al **test di carico.  Il nome deve essere univoco.
    + Selezionare **Rivedi e crea** e quindi **Crea**.

1. Attendere la creazione del test di carico e quindi selezionare **Vai alla risorsa**.

1. **Nella panoramica**** | Aggiungi richieste** HTTP selezionare **Crea.**

1. Per l'URL** di test, incollare l'URL ****del dominio** predefinito. Assicurarsi che sia formattato correttamente e inizi con **https://**.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

    >**Nota:** la creazione del test può richiedere alcuni minuti. 

1. Esaminare i risultati del test, tra cui **Utenti** virtuali, **Tempo** di risposta e **Richieste/sec**.

1. Selezionare **Arresta** per completare l'esecuzione del test.

## Pulire le risorse

Se si usa **la propria sottoscrizione** , è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.



## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ app Azure Servizi consente di creare, distribuire e ridimensionare rapidamente le app Web.
+ servizio app include il supporto per molti ambienti di sviluppo, tra cui ASP.NET, Java, PHP e Python.
+ Gli slot di distribuzione consentono di creare ambienti separati per la distribuzione e il test dell'app Web.
+ È possibile ridimensionare manualmente o automaticamente un'app Web per gestire una domanda aggiuntiva.
+ Sono disponibili un'ampia gamma di strumenti di diagnostica e test. 

## Altre informazioni con la formazione autogestita

+ [Preparare la distribuzione di un'app Web per il test e il rollback usando servizio app slot](https://learn.microsoft.com/training/modules/stage-deploy-app-service-deployment-slots/) di distribuzione. Usare gli slot di distribuzione per semplificare la distribuzione e il rollback di un'app Web nel servizio app di Azure.
+ [Ridimensionare un'app Web servizio app per soddisfare in modo efficiente la domanda con servizio app aumentare e aumentare le prestazioni](https://learn.microsoft.com/training/modules/app-service-scale-up-scale-out/). Rispondere ai periodi di maggiore attività aumentando in modo incrementale le risorse disponibili e quindi, per ridurre i costi, riducendo queste risorse quando l'attività scende.
