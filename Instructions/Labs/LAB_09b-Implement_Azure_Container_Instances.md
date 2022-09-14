---
lab:
  title: 09b - Implementare Istanze di Azure Container
  module: Administer Serverless Computing
---

# <a name="lab-09b---implement-azure-container-instances"></a>Lab 09b - Implementare Istanze di Azure Container
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

Contoso wants to find a new platform for its virtualized workloads. You identified a number of container images that can be leveraged to accomplish this objective. Since you want to minimize container management, you plan to evaluate the use of Azure Container Instances for deployment of Docker images.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

- Attività 1: Distribuire un'immagine Docker usando Istanza di Azure Container
- Attività 2: Esaminare la funzionalità di Istanza di Azure Container

## <a name="estimated-timing-20-minutes"></a>Tempo stimato: 20 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab09b.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-deploy-a-docker-image-by-using-the-azure-container-instance"></a>Attività 1: Distribuire un'immagine Docker usando Istanza di Azure Container

In questa attività verrà creata una nuova istanza di contenitore per l'applicazione Web.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Nel portale di Azure trovare **Istanze di Container** e nel pannello **Istanze di Container** fare clic su **+ Crea**.

1. Nella scheda **Dati principali** del pannello **Crea istanza di Container** specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | ---- | ---- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | Nome di un nuovo gruppo di risorse **az104-09b-rg1** |
    | Nome contenitore | **az104-9b-c1** |
    | Region | Nome di un'area in cui è possibile effettuare il provisioning delle istanze di Azure Container |
    | Origine immagine | **Immagini di avvio rapido** |
    | Immagine | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Fare clic su **Avanti: Rete >** e nella scheda **Rete** del pannello **Crea istanza di Container** specificare le impostazioni seguenti, lasciando i valori predefiniti per le altre:

    | Impostazione | Valore |
    | --- | --- |
    | Etichetta del nome DNS | Qualsiasi nome host DNS valido e univoco a livello globale |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Your container will be publicly reachable at dns-name-label.region.azurecontainer.io. If you receive a <bpt id="p1">**</bpt>DNS name label not available<ept id="p1">**</ept> error message, specify a different value.

1. Fare clic su **Avanti: Avanzate >** , esaminare le impostazioni nella scheda **Avanzate** del pannello **Crea istanza di Container** senza apportare modifiche, fare clic su **Verifica e crea**, verificare che la convalida sia stata superata e fare clic su **Crea**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. This should take about 3 minutes.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: While you wait, you may be interested in viewing the <bpt id="p2">[</bpt>code behind the sample application<ept id="p2">](https://github.com/Azure-Samples/aci-helloworld)</ept>. To view it, browse the <ph id="ph1">\\</ph>app folder.

#### <a name="task-2-review-the-functionality-of-the-azure-container-instance"></a>Attività 2: Esaminare la funzionalità di Istanza di Azure Container

In questa attività verrà esaminata la distribuzione dell'istanza di contenitore.

1. Nel pannello della distribuzione fare clic sul collegamento **Vai alla risorsa**.

1. Nel pannello **Panoramica** dell'istanza di contenitore verificare che **Stato** indichi **In esecuzione**.

1. Copiare il valore **FQDN**dell'istanza di contenitore, aprire una nuova scheda del browser e passare all'URL corrispondente.

1. Verificare che venga visualizzata la pagina **Benvenuti in Istanze di Azure Container**.

1. Chiudere la nuova scheda del browser e nel portale di Azure, nella sezione **Impostazioni** del pannello dell'istanza del contenitore, fare clic su **Contenitori** e quindi su **Log**.

1. Verificare che siano visualizzate le voci di log che rappresentano la richiesta HTTP GET generata visualizzando l'applicazione nel browser.

#### <a name="clean-up-resources"></a>Pulire le risorse

>Contoso vuole trovare una nuova piattaforma per i carichi di lavoro virtualizzati.

>Sono state identificate diverse immagini del contenitore che possono essere sfruttate per raggiungere questo obiettivo. 

1. Nel portale di Azure aprire la sessione di **PowerShell** all'interno del riquadro **Cloud Shell**.

1. Elencare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Eliminare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: il comando viene eseguito in modo asincrono, in base a quanto determinato dal parametro -AsJob, quindi, sebbene sia possibile eseguire un altro comando di PowerShell immediatamente dopo nella stessa sessione di PowerShell, i gruppi di risorse verranno rimossi dopo alcuni minuti.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

- Distribuzione di un'immagine Docker usando Istanza di Azure Container
- Esame della funzionalità di Istanza di Azure Container
