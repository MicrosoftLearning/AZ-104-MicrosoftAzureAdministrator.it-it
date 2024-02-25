---
lab:
  title: 'Lab 09c: Implementare app di Azure Container'
  module: Administer PaaS Compute Options
---

# Lab 09c - Implementare app di Azure Container

## Introduzione al lab

In questo lab si apprenderà come implementare e distribuire app contenitore di Azure.

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali.

## Tempo stimato: 15 minuti

## Scenario laboratorio

L'organizzazione ha un'applicazione Web che viene eseguita in una macchina virtuale nel data center locale. L'organizzazione vuole spostare tutte le applicazioni nel cloud, ma non vuole gestire un numero elevato di server. Si decide di valutare le app di Azure Container.

## Simulazioni di lab interattive

Non sono disponibili simulazioni di lab interattive per questo argomento. 

## Competenze mansione

- Attività 1: Creare e configurare un'app e un ambiente azure Container.
- Attività 2: Testare e verificare la distribuzione dell'app Azure Container.

## Diagramma dell'architettura

![Diagramma delle attività.](../media/az104-lab09b-aca-architecture.png)

## Attività 1: Creare e configurare un'app e un ambiente azure Container

App Contenitore di Azure considerano ulteriormente il concetto di cluster Kubernetes gestito e gestisce ulteriormente l'ambiente del cluster, oltre a fornire altri servizi gestiti all'interno del cluster. A differenza di un cluster Azure Kubernetes, in cui è comunque necessario gestire il cluster, un'istanza di App Contenitore di Azure rimuove alcune delle complessità per configurare un cluster Kubernetes.

1. Nella portale di Azure cercare e selezionare `Container Apps`.

1. In **App** contenitore selezionare **Crea**.

1. Usare le informazioni seguenti per compilare i dettagli nella **scheda Informazioni di base** .*.

    | Impostazione | Azione |
    |---|---|
    | Abbonamento | Selezionare la Sottoscrizione di Azure |
    | Gruppo di risorse | `az104-rg9` |
    | Nome dell'app contenitore |  `my-app` |
    | Area    | **Stati Uniti** orientali (o un'area disponibile nelle vicinanze) |
    | Ambiente app contenitore | Lasciare il valore predefinito |

1. **Nella scheda Contenitore** assicurarsi che **l'opzione Usa immagine** di avvio rapido sia abilitata e che l'immagine di avvio rapido sia impostata su **Contenitore Hello World** semplice.

1. **Selezionare Rivedi e crea** e quindi **Crea**.

    >**Nota:** attendere che l'app contenitore venga distribuita. L'operazione richiede alcuni minuti. 
 
## Attività 2: Testare e verificare la distribuzione dell'app Azure Container

Per impostazione predefinita, l'app contenitore di Azure creata accetterà il traffico sulla porta 80 usando l'applicazione Hello World di esempio. App Azure Container fornirà un nome DNS per l'applicazione. Copiare e passare a questo URL per assicurarsi che l'applicazione sia attiva e in esecuzione.

1. Selezionare Vai alla risorsa** per visualizzare **la nuova app contenitore.

1. Selezionare il collegamento accanto a *URL* applicazione per visualizzare l'applicazione.

    ![Screenshot della pagina di panoramica di ACA nel portale.](../media/az104-lab09b-aca-overview.png)

1. Verificare che l'app **App Azure Container sia in tempo reale** .
   
## Pulire le risorse

Se si usa **la propria sottoscrizione** , è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.



## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ App Azure Container (ACA) è una piattaforma serverless che consente di mantenere meno infrastruttura e risparmiare sui costi durante l'esecuzione di applicazioni in contenitori.
+ App contenitore fornisce informazioni dettagliate sulla configurazione del server, sull'orchestrazione dei contenitori e sulla distribuzione. 
+ I carichi di lavoro in ACA sono in genere processi a esecuzione prolungata, ad esempio un'app Web.

## Altre informazioni con la formazione autogestita

+ [Configurare un'app contenitore in App](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/) Azure Container. Esamina le funzionalità e le funzionalità di App Azure Container e quindi illustra come creare, configurare, ridimensionare e gestire app contenitore usando App Contenitore di Azure.
     
