---
lab:
  title: 'Lab 09a: Implementare App Web'
  module: Administer PaaS Compute Options
---

# Lab 09a - Implementare app Web


## Introduzione al lab

In questo lab vengono fornite informazioni sulle app Web di Azure. Si apprenderà come configurare un'app Web per visualizzare un'applicazione Hello World in un repository GitHub esterno. Si apprenderà come creare uno slot di staging e scambiare con lo slot di produzione. Si apprenderà anche la scalabilità automatica per soddisfare le modifiche della domanda.

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

## Tempo stimato: 30 minuti

## Scenario laboratorio

L'organizzazione è interessata alle app Web di Azure per ospitare i siti Web dell'organizzazione. I siti Web sono attualmente ospitati nei data center locali dell'azienda. I siti Web sono in esecuzione su server Windows usando lo stack di runtime PHP. L'hardware sta per scadere e sarà presto necessario sostituire. L'organizzazione vuole completare i test per facilitare il passaggio ad Azure prima della data di fine vita.

## Simulazioni di lab interattive

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ [Creare un'app](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%202) Web. Creare un'app Web che esegue un contenitore Docker.
    
+ [Implementare app](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013) Web di Azure. Creare un'app Web di Azure, gestire la distribuzione e ridimensionare l'app. 

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab09a-architecture-diagram.png)

## Attività

+ Attività 1: Creare un'app Web di Azure
+ Attività 2: Creare uno slot di distribuzione di staging
+ Attività 3: Configurare le impostazioni di distribuzione delle app Web
+ Attività 4: Scambiare gli slot di staging
+ Attività 5: Configurare e testare la scalabilità automatica dell'app Web di Azure

## Attività 1: Creare un'app Web di Azure

In questa attività verrà creata un'app Web di Azure. Azure offre servizi app Azure, una soluzione PAAS (Platform As a Service) per applicazioni Web, per dispositivi mobili e altre applicazioni basate sul Web. Azure App Web, un tipo di offerte di servizi app Azure, può essere usato per eseguire siti Web per la maggior parte degli ambienti di runtime, ad esempio PHP, Java, .NET e altro ancora. Se è necessario il supporto per più di un ambiente di runtime, è possibile usare servizio app con i contenitori Docker. Lo SKU selezionato determina la quantità di risorse di calcolo, archiviazione e funzionalità ricevute con l'app Web.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `App services`.

1. Selezionare **+ Crea** dal menu a discesa App **Web**. Si notino le altre scelte. 

1. Nella scheda **Informazioni di base** del pannello **Crea app Web** specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | ---|
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | `az104-rg9` (Se necessario, selezionare **Crea nuovo**) |
    | Nome dell'app Web | Qualsiasi nome univoco a livello globale |
    | Pubblica | **Codice** |
    | Stack di runtime | **PHP 8.2** |
    | Sistema operativo | **Linux** |
    | Area | **Stati Uniti orientali** |
    | Piani dei prezzi | accettare le impostazioni predefinite |
    | Ridondanza della zona | accettare le impostazioni predefinite |

 1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere che venga creata l'app Web prima di procedere all'attività successiva. L'operazione dovrebbe richiedere circa un minuto.

1. Nel pannello della distribuzione fare clic su **Vai alla risorsa**.

## Attività 2: Creare uno slot di distribuzione di staging

In questa attività verrà creato uno slot di distribuzione di staging. Gli slot di distribuzione sono funzionalità di determinati piani di servizio app che consentono di eseguire test prima di rendere disponibile l'app al pubblico (o agli utenti finali). Dopo aver eseguito il test, è possibile scambiare lo slot dallo sviluppo o dalla gestione temporanea all'ambiente di produzione. Molte organizzazioni usano slot per eseguire test di pre-produzione. Inoltre, molte organizzazioni eseguono più slot per ogni applicazione, ad esempio sviluppo, controllo di qualità, test e produzione.

1. Nel pannello dell'app Web appena distribuita fare clic sul **collegamento Dominio predefinito** per visualizzare la pagina Web predefinita in una nuova scheda del browser.

1. Chiudere la nuova scheda del browser e, di nuovo nel portale di Azure, nella sezione **Distribuzione** del pannello dell'app Web fare clic su **Add a Slot di distribuzione**.

    >**Nota**: a questo punto l'app Web include un singolo slot di distribuzione con etichetta **PRODUCTION**.

1. Fare clic su **+ Add slot** (Aggiungi slot) e aggiungere un nuovo slot con le impostazioni seguenti:

    | Impostazione | valore |
    | --- | ---|
    | Nome | `staging` |
    | Clona le impostazioni da | **Non clonare le impostazioni**|

1. Selezionare **Aggiungi**.

1. Tornare al pannello **Slot di distribuzione** dell'app Web e fare clic sulla voce che rappresenta lo slot di staging appena creato.

    >**Nota**: verrà visualizzato il pannello che mostra le proprietà dello slot di staging.

1. Esaminare il pannello dello slot di staging e notare che l'URL è diverso da quello assegnato allo slot di produzione.

## Attività 3: Configurare le impostazioni di distribuzione delle app Web

In questa attività verranno configurate le impostazioni della distribuzione Web. servizio app possono essere configurati con le impostazioni di distribuzione per consentire la distribuzione continua dal repository scelto oppure usando le credenziali FTPS e altre automazione. In questo modo, il servizio app ha la versione più recente dell'applicazione in esecuzione.

1. Nel pannello dello slot di distribuzione di staging, nella **sezione Impostazioni** selezionare **Configurazione** e quindi selezionare **Impostazioni** generali.

    >**Nota:** assicurarsi di essere nel pannello dello slot di staging (anziché nello slot di produzione).
    
1. Nell'elenco **a discesa Origine** della **scheda Impostazioni** selezionare **Git** esterno.

1. Nel campo repository immettere `https://github.com/Azure-Samples/php-docs-hello-world`

1. Nel campo ramo immettere `master`.

1. Seleziona **Salva**.

1. Nello slot di staging selezionare **Panoramica**.

1. Selezionare il **collegamento Dominio predefinito** e aprire l'URL in una nuova scheda. 

1. Verificare che lo slot di staging visualizzi **Hello World**.

## Attività 4: Scambiare gli slot di staging

In questa attività lo slot di staging verrà scambiato con lo slot di produzione. Lo scambio di uno slot consente di usare il codice testato nello slot di staging e spostarlo nell'ambiente di produzione. Il portale di Azure richiederà anche se è necessario spostare altre impostazioni dell'applicazione personalizzate per lo slot. Lo scambio degli slot è un'attività comune per i team di supporto delle applicazioni e per i team di supporto delle applicazioni, in particolare quelli che distribuiscono aggiornamenti di routine delle app e correzioni di bug.

1. Tornare al pannello che mostra lo slot di produzione dell'app Web.

1. Nella sezione**Distribuzione** fare clic su **Slot di distribuzione** e quindi sull'icona **Scambia** della barra degli strumenti.

1. Nel pannello **Scambia** esaminare le impostazioni predefinite e fare clic su **Scambia**.

1. Fare clic su **Panoramica** nel pannello slot di produzione dell'app Web e quindi sul **collegamento Dominio predefinito** per visualizzare la home page del sito Web in una nuova scheda del browser.

    >**Nota:** copiare l'URL** di dominio **predefinito necessario per i test di carico nell'attività successiva. 

1. Verificare che la pagina Web predefinita sia stata sostituita con la pagina **Hello World!** .

## Attività 5: Configurare e testare la scalabilità automatica dell'app Web di Azure

In questa attività verrà configurata la scalabilità automatica dell'app Web di Azure. La scalabilità automatica consente di mantenere prestazioni ottimali per l'app Web quando aumenta il traffico verso l'app Web.  Per la maggior parte delle applicazioni, è possibile che si conoscano metriche specifiche nell'app che dovrebbero causare la scalabilità. Potrebbe trattarsi di utilizzo della CPU, memoria o larghezza di banda.

1. Nel pannello che mostra lo slot di produzione dell'app Web, nella sezione **Impostazioni**, fare clic su **Aumenta istanze (piano di servizio app)**.

1. **Nella sezione Ridimensionamento** selezionare **Automatico**.

1. **Nel campo Burst** massimo selezionare **2**.

    ![Screenshot della pagina di scalabilità automatica.](../media/az104-lab09a-autoscale.png)

1. Seleziona **Salva**.

    >**Nota**: in un ambiente di produzione le organizzazioni spesso selezionano **Regole basate** e configurano regole per metriche specifiche o componenti di Application Insights che attivano la scalabilità automatica.

1. Nel pannello che visualizza lo slot di produzione dell'app Web selezionare **Diagnostica e risoluzione dei problemi**.

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
   
## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ app Azure Servizi consente di creare, distribuire e ridimensionare rapidamente le app Web.
+ servizio app include il supporto per molti ambienti di sviluppo, tra cui ASP.NET, Java, PHP e Python.
+ Gli slot di distribuzione consentono di creare ambienti separati per la distribuzione e il test dell'app Web.
+ È possibile ridimensionare manualmente o automaticamente un'app Web per gestire una domanda aggiuntiva. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.
    
