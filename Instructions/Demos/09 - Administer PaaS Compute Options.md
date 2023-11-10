---
demo:
  title: 'Dimostrazione 09: opzioni di calcolo PaaS Amministrazione ister'
  module: Administer PaaS Compute Options
---

# 09 - Opzioni di calcolo PaaS Amministrazione ister

## Configurare i piani di servizio app di Azure

In questa dimostrazione verranno creati e utilizzati piani di Servizio app di Azure.

**Riferimento**: [Gestire il piano di servizio app - Servizio app Azure](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**Riferimento**: [Aumentare le prestazioni di un'app nel servizio app Azure](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**Riferimento**: [Scalabilità automatica nel servizio app Azure](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. Usare il portale di Azure. 

1. Cercare e selezionare ** servizio app piani**.

1. Creare un piano servizio app semplice. Discutere la necessità di selezionare Windows o Linux. Discutere i piani tariffari ora o nei passaggi successivi. 

1. Distribuire il nuovo piano di servizio app. 

1. Esaminare il **pannello Aumento delle prestazioni (piano servizio app).** Illustrare la differenza tra **i piani di sviluppo/test** e **produzione** . Esaminare l'elenco delle funzionalità. 

1. Esaminare il pannello **Scale out (piano servizio app).** Esaminare la differenza tra **Manuale** e **Basato su** regole. 

## Configurare i servizi app di Azure

In questa dimostrazione verrà creata una nuova app Web che esegue un contenitore Docker. Il contenitore visualizza un messaggio di benvenuto.

**Riferimento**: [Creare un'app Web](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

In questa attività verrà creata un'app Web del servizio app Azure.

1. Usare il portale di Azure. 

1. Cercare e selezionare ** servizio app s**.

1. **Creare** un'app**** Web.

    - Pubblica: **codice**. Rivedere le altre scelte.
    - Stack di runtime: **.Net**. Rivedere le altre scelte.
    - Sistema operativo: **Linux**

1. Selezionare il **piano di servizio F1** gratuito.

1. **Rivedi e crea** l'app Web. Attendere la distribuzione della risorsa.

1. **Nella pagina Panoramica** verificare che lo **stato** sia **in esecuzione**.

1. Selezionare l'URL **** e assicurarsi che la pagina segnaposto predefinita venga caricata.

1. Quando si ha tempo, esplorare le **opzioni Slot** di distribuzione.
   
## Configurare Istanze di Azure Container

In questa dimostrazione viene creato, configurato e distribuito un contenitore usando Istanze di Azure Container (ACI) dal portale di Azure. L'applicazione ACI visualizza una pagina HTML statica con l'immagine Microsoft Hello World pubblica. 

**Riferimento**: Guida introduttiva - [Distribuire un contenitore Docker nell'istanza del contenitore](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. Usare il portale di Azure.

1. Cercare e selezionare **Istanze** di Contenitore.

1. **Creare** una nuova istanza del contenitore. 

1. Compilare il **gruppo** di risorse e **il nome** del contenitore. 

1. Discutere le **opzioni origine** immagine. Usare immagini **** di avvio rapido.

1. Per l'immagine del contenitore usare mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux).For **Container image** use **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)**. In questa immagine Linux di esempio è inclusa una piccola app Web scritta in Node.js che distribuisce una pagina HTML statica.

1. Nella pagina **Rete** specificare un'**etichetta del nome DNS** per il contenitore. 

1. Lasciare tutte le altre impostazioni predefinite, quindi selezionare **Rivedi e crea**.

1. Attendere la distribuzione della risorsa.

1. **Nella pagina Panoramica** della risorsa verificare che lo **stato** sia **in esecuzione**.

1. Passare al **nome di dominio completo** per l'istanza del contenitore e assicurarsi che venga visualizzata la pagina iniziale. 

**Nota**: per evitare costi aggiuntivi, eliminare la risorsa. 

## Configurare le app di Azure Container

In questa dimostrazione si creerà e si lavorerà con App Azure Container. 

**Riferimento**: Guida introduttiva: [Distribuire la prima app contenitore usando il portale di Azure](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. Cercare e selezionare **App** contenitore.

1. Completare i dettagli** del **progetto e creare l'ambiente** delle app **contenitore.

1. **Esaminare e creare** l'app contenitore.

1. Usare il **collegamento URL** applicazione per visualizzare l'applicazione.

1. Verificare che nel browser sia visualizzato il **messaggio Benvenuto in App** Azure Container. 






