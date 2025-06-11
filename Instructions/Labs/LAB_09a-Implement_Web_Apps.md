---
lab:
  title: 'Lab 09a: Implementare app Web'
  module: Administer PaaS Compute Options
---

# Lab 09a - Implementare app Web


## Introduzione al lab

Questo lab fornisce informazioni sulle app Web di Azure. Verrà illustrata la configurazione di una app Web in modo che mostri un'applicazione Hello World in un repository GitHub esterno. Si apprenderà a creare uno slot di staging e scambiarlo con lo slot di produzione. Si apprenderà anche la scalabilità automatica per soddisfare le modifiche della domanda.

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

## Tempo stimato: 20 minuti

## Scenario laboratorio

L'organizzazione è interessata alle app Web di Azure per ospitare i siti Web aziendali. I siti Web sono attualmente ospitati in un data center locale. I siti Web sono in esecuzione su server Windows usando lo stack di runtime PHP. L'hardware sta terminando il suo ciclo di vita e presto sarà necessario sostituirlo. L'organizzazione vuole evitare nuovi costi hardware usando Azure per ospitare i siti Web. 

## Simulazioni interattive del lab

>**Nota**: le simulazioni lab fornite in precedenza sono state ritirate.

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab09a-architecture.png)

## Competenze mansione

+ Attività 1: Creare e configurare un’app Web di Azure.
+ Attività 2: Creare e configurare uno slot di distribuzione.
+ Attività 3: Configurare le impostazioni di distribuzione dell’app Web.
+ Attività 4: Scambiare gli slot di distribuzione.
+ Attività 5: Configurare e testare la scalabilità automatica dell'app Web di Azure.

## Attività 1: Creare e configurare un’app Web di Azure

In questa attività verrà creata un'app Web di Azure. Servizi app di Azure è una soluzione PAAS (Platform As a Service) per il Web, per i dispositivi mobili e altre applicazioni basate sul Web. Le app Web di Azure fanno parte di Servizi app di Azure che ospitano la maggior parte degli ambienti di runtime, ad esempio PHP, Java e .NET. Il piano di servizio app che si seleziona determina il calcolo, l'archiviazione e le funzionalità dell'app Web. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare `App services`.

1. Selezionare **+ Crea** dal menu a discesa, **App Web**. Si notino le altre opzioni. 

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
    | Piani dei prezzi | **Premium V3 P1V3** |
    | Ridondanza della zona | accettare i valori predefiniti |

 1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: Attendere che venga creata l'app Web prima di procedere all'attività successiva. L'operazione dovrebbe richiedere circa un minuto.
    
    >**Nota**: se la distribuzione non riesce, passare a un'altra area e riprovare. Ciò è dovuto alle quote in aree diverse.  

1. Al termine della distribuzione, selezionare **Vai alla risorsa**.

## Attività 2: Creare e configurare uno slot di distribuzione

In questa attività verrà creato uno slot di distribuzione di staging. Gli slot di distribuzione consentono di eseguire test prima di rendere disponibile l'app al pubblico (o agli utenti finali). Dopo aver eseguito il test, è possibile scambiare lo slot da sviluppo o staging a produzione. Molte organizzazioni usano gli slot per eseguire test di pre-produzione. Inoltre, molte organizzazioni eseguono più slot per ogni applicazione, ad esempio sviluppo, controllo di qualità, test e produzione.

1. Nel pannello dell'app Web appena distribuita fare clic sul collegamento del **dominio predefinito** per visualizzare la pagina Web predefinita in una nuova scheda del browser.

1. Chiudere la nuova scheda del browser e, di nuovo nel portale di Azure, nella sezione **Distribuzione** del pannello dell'app Web fare clic su **Add a Slot di distribuzione**.

1. Fare clic su **Aggiungi slot** e aggiungere un nuovo slot con le impostazioni seguenti:

    | Impostazione | valore |
    | --- | ---|
    | Nome | `staging` |
    | Clona le impostazioni da | **Non clonare le impostazioni**|

1. Selezionare **Aggiungi** per creare lo slot.

1. Aggiornare la pagina per visualizzare gli slot di produzione e gestione temporanea. 

1. Selezionare la voce che rappresenta lo slot di staging appena creato.

    >**Nota**: verrà visualizzato il pannello che mostra le proprietà dello slot di staging.

1. Esaminare il pannello dello slot di staging e notare che l'URL è diverso da quello assegnato allo slot di produzione.

## Attività 3: Configurare le impostazioni di distribuzione delle app Web

In questa attività verranno configurate le impostazioni della distribuzione Web. Le impostazioni di distribuzione consentono la distribuzione continua. Ciò garantisce che il servizio app abbia la versione più recente dell'applicazione.

1. Nello slot di staging selezionare **Centro distribuzione**, quindi Selezionare **Impostazioni**.

    >**Nota:** Assicurarsi di trovarsi nel pannello dello slot di staging invece che in quello dello slot di produzione.
    
1. Nell'elenco a discesa **Origine** selezionare **Git esterno**. Si notino le altre opzioni. 

1. Nel campo repository immettere `https://github.com/Azure-Samples/php-docs-hello-world`

1. Nel campo ramo immettere `master`.

1. Seleziona **Salva**.

1. Nello slot di staging selezionare **Panoramica**.

1. Selezionare il collegamento **Dominio predefinito** e aprire l'URL in una nuova scheda. 

1. Verificare che lo slot di staging mostri **Hello World**. 

>**Nota:** La distribuzione può richiedere un minuto. Assicurarsi di **aggiornare** la pagina dell'applicazione.

## Attività 4: Scambiare gli slot di distribuzione

In questa attività lo slot di staging verrà scambiato con lo slot di produzione. Lo scambio di uno slot consente di usare il codice testato nello slot di staging e spostarlo nell'ambiente di produzione. Il portale di Azure chiederà anche se è necessario spostare altre impostazioni applicazione che sono state personalizzate per lo slot. Lo scambio degli slot è un'attività comune per i team applicazioni e per i team di supporto delle applicazioni, in particolare quelli che distribuiscono aggiornamenti di routine delle app e correzioni di bug.

1. Tornare al pannello **Slot di distribuzione**, quindi selezionare **Scambia**.

1. Esaminare le impostazioni predefinite e fare clic su **Avvia scambio**. Attendere la notifica che lo scambio è terminato.

1. Tornare alla home page del portale. È necessario disporre sia di un'app Web di produzione che dello slot di staging.

1. `App Services` Cercare e selezionare l'app Web servizio app. Verrà restituito lo slot Di distribuzione di produzione.

1. Selezionare l'app Web servizio app e nel pannello **Panoramica** dell'app Web selezionare il **collegamento Dominio** predefinito per visualizzare la home page del sito Web.

1. Verificare che la pagina Web di produzione visualizzi ora **Hello World.** .

    >**Nota:** Copiare l'**URL** di dominio predefinito necessario per i test di carico nell'attività successiva. 

## Attività 5: Configurare e testare la scalabilità automatica dell'app Web di Azure

In questa attività verrà configurata la scalabilità automatica dell'app Web di Azure. La scalabilità automatica consente di mantenere prestazioni ottimali per l'app Web quando aumenta il traffico verso di essa. Per determinare quando deve essere ridimensionata l’app, è possibile monitorare metriche come utilizzo della CPU, memoria o larghezza di banda.

1. Nella sezione **Impostazioni** selezionare **Aumenta (piano di servizio app)**.

    >**Nota:** Assicurarsi di lavorare nello slot di produzione e non nello slot di staging.  

1. Nella sezione **Ridimensionamento** selezionare **Automatico**. Si noti l'opzione **Basato su regole**. Il ridimensionamento basato su regole può essere configurato per metriche app diverse. 

1. Nel campo **Burst massimo** selezionare **2**.

    ![Screenshot della pagina di scalabilità automatica.](../media/az104-lab09a-autoscale.png)

1. Seleziona **Salva**.

1. Selezionare **Diagnostica e risoluzione dei problemi** (riquadro a sinistra).

1. Nella casella **Test di carico dell'app** selezionare **Crea test di carico**.

    + Selezionare **+ Crea** e assegnare un **nome** al test di carico.  Il nome deve essere univoco.
    + Selezionare **Rivedi e crea** e quindi **Crea**.

1. Attendere la creazione del test di carico, quindi selezionare **Vai alla risorsa**.

1. **Nella panoramica** | **Crea aggiungendo richieste** HTTP selezionare **Crea.**

1. Nella **scheda Piano** di test fare clic su **Aggiungi richiesta**. **Nel campo** URL incollare l'URL **di dominio** predefinito. Assicurarsi che sia formattato correttamente e inizi con **https://**. Selezionare **Aggiungi** per salvare le modifiche. 

1. Selezionare **Rivedi e crea** e quindi **Crea**.

    >**Nota:** La creazione del test può richiedere alcuni minuti. Guarda le notifiche.

1. Passare al test (elencato nella home page). 

1. Aggiornare ed esaminare le metriche attive, inclusi gli utenti** virtuali, **il tempo** di risposta e **le richieste/sec**.**

1. Selezionare **Arresta** per completare l'esecuzione del test. Non è necessario attendere il completamento del test. 

## Pulire le risorse

Se si usa la **sottoscrizione personale**, dedicare qualche minuto all’eliminazione delle risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse lab. 

+ Nel portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse**, **Immettere il nome del gruppo di risorse**, quindi fare clic su **Elimina**.
+ Tramite Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usando l’interfaccia della riga di comando, `az group delete --name resourceGroupName`.

## Estendere l'apprendimento con Copilot
Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a *copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.

+ Riepilogare i passaggi per creare e configurare un'app Web di Azure.
+ Quali sono i modi in cui è possibile ridimensionare un'app Web di Azure?

## Altre informazioni con la formazione autogestita

+ [Gestire temporaneamente la distribuzione di un'app Web per il test e il rollback usando gli slot di distribuzione del servizio app](https://learn.microsoft.com/training/modules/stage-deploy-app-service-deployment-slots/). Usare gli slot di distribuzione per semplificare la distribuzione e il rollback di un'app Web nel servizio app di Azure.
+ [Ridimensionare un'app Web del servizio app per soddisfare in modo efficiente la domanda tramite l'aumento delle prestazioni e delle istanze del servizio App](https://learn.microsoft.com/training/modules/app-service-scale-up-scale-out/). Rispondere ai periodi di intensa attività aumentando le risorse disponibili in modo incrementale e successivamente riducendo queste risorse quando l'attività diminuisce, per ridurre i costi.

## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab. 

+ Servizi app di Azure consente di creare, distribuire e ridimensionare rapidamente app Web.
+ Il servizio app include il supporto per molti ambienti di sviluppo, tra cui ASP.NET, Java, PHP e Python.
+ Gli slot di distribuzione consentono di creare ambienti separati per la distribuzione e il test dell'app Web.
+ È possibile ridimensionare manualmente o automaticamente un'app Web per gestire una domanda aggiuntiva.
+ È disponibile un'ampia gamma di strumenti di diagnostica e test. 
