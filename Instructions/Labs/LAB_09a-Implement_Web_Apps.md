---
lab:
  title: 09a - Implementare app Web
  module: Module 09 - Serverless Computing
ms.openlocfilehash: af243b0cfa2b011dd419516139b5200ba349bcb4
ms.sourcegitcommit: c360d3abaa6e09814f051b2568340e80d0d0e953
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2022
ms.locfileid: "138356614"
---
# <a name="lab-09a---implement-web-apps"></a>Lab 09a - Implementare app Web
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

È necessario valutare l'uso di app Web di Azure per l'hosting dei siti Web di Contoso ospitati attualmente nei data center locali della società. I siti Web sono in esecuzione su server Windows che usano lo stack di runtime PHP. È inoltre necessario determinare in che modo è possibile implementare le procedure DevOps sfruttando gli slot di distribuzione di app Web di Azure.

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Creare un'app Web di Azure
+ Attività 2: Creare uno slot di distribuzione di staging
+ Attività 3: Configurare le impostazioni di distribuzione delle app Web
+ Attività 4: Distribuire codice nello slot di distribuzione di staging
+ Attività 5: Scambiare gli slot di staging
+ Attività 6: Configurare e testare la scalabilità automatica dell'app Web di Azure

## <a name="estimated-timing-30-minutes"></a>Tempo stimato: 30 minuti

## <a name="architecture-diagram"></a>Diagramma dell'architettura

![image](../media/lab09a.png)

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-create-an-azure-web-app"></a>Attività 1: Creare un'app Web di Azure

In questa attività verrà creata un'app Web di Azure.

1. Accedere al [**portale di Azure**](http://portal.azure.com).

1. Nel portale di Azure cercare e selezionare **Servizi app** e nel pannello **Servizi app** fare clic su **+ Crea**.

1. Nella scheda **Informazioni di base** del pannello **Crea app Web** specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- | ---|
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | Nome di un nuovo gruppo di risorse **az104-09a-rg1** |
    | Nome dell'app Web | Qualsiasi nome univoco a livello globale |
    | Pubblica | **Codice** |
    | Stack di runtime | **PHP 7.4** |
    | Sistema operativo | **Windows** |
    | Region | Nome di un'area di Azure in cui è possibile effettuare il provisioning di app Web di Azure |
    | App service plan (Piano di servizio app) | Accettare la configurazione predefinita |

1. Fare clic su **Rivedi e crea**. Nella scheda **Rivedi e crea** del pannello **Crea app Web** assicurarsi che la convalida abbia avuto esito positivo e fare clic su **Crea**.

    >**Nota**: attendere che venga creata l'app Web prima di procedere all'attività successiva. L'operazione dovrebbe richiedere circa un minuto.

1. Nel pannello della distribuzione fare clic su **Vai alla risorsa**.

#### <a name="task-2-create-a-staging-deployment-slot"></a>Attività 2: Creare uno slot di distribuzione di staging

In questa attività verrà creato uno slot di distribuzione di staging.

1. Nel pannello dell'app Web appena distribuita fare clic sul collegamento dell'**URL** per visualizzare la pagina Web predefinita in una nuova scheda del browser.

1. Chiudere la nuova scheda del browser e, di nuovo nel portale di Azure, nella sezione **Distribuzione** del pannello dell'app Web fare clic su **Add a Slot di distribuzione**.

    >**Nota**: a questo punto l'app Web include un singolo slot di distribuzione con etichetta **PRODUCTION**.

1. Fare clic su **+ Add slot** (Aggiungi slot) e aggiungere un nuovo slot con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | ---|
    | Nome | **staging** |
    | Clona le impostazioni da | **Non clonare le impostazioni**|

1. Tornare al pannello **Slot di distribuzione** dell'app Web e fare clic sulla voce che rappresenta lo slot di staging appena creato.

    >**Nota**: verrà visualizzato il pannello che mostra le proprietà dello slot di staging.

1. Esaminare il pannello dello slot di staging e notare che l'URL è diverso da quello assegnato allo slot di produzione.

#### <a name="task-3-configure-web-app-deployment-settings"></a>Attività 3: Configurare le impostazioni di distribuzione delle app Web

In questa attività verranno configurate le impostazioni della distribuzione Web.

1. Nel pannello dello slot di distribuzione di staging, nella sezione **Distribuzione** fare clic su **Centro distribuzione** e quindi selezionare la scheda **Impostazioni**.

    >**Nota:** assicurarsi di trovarsi nel pannello dello slot di staging invece che dello slot di produzione.
    
1. Nella scheda **Impostazioni** nell'elenco a discesa **Origine** selezionare **Archivio Git locale** e fare clic sul pulsante **Salva**

1. Nel pannello **Centro distribuzione** copiare la voce **URL di git clone** in Blocco note.

    >**Nota:** il valore URL di git clone sarà necessario nell'attività successiva di questo lab.

1. Nel pannello **Centro distribuzione** selezionare la scheda **Credenziali GIT locale/FTPS**, nella sezione **Ambito utente** specificare le impostazioni seguenti e fare clic su **Salva**.

    | Impostazione | Valore |
    | --- | ---|
    | Nome utente | Qualsiasi nome univoco a livello globale (non deve contenere il carattere `@`) |
    | Password | Qualsiasi password che soddisfi i requisiti di complessità|

    >**Nota:** la password deve contenere almeno otto caratteri, con due dei tre elementi seguenti: lettere, numeri e caratteri non alfanumerici.

    >**Nota:** queste credenziali saranno necessarie nell'attività successiva di questo lab.

#### <a name="task-4-deploy-code-to-the-staging-deployment-slot"></a>Attività 4: Distribuire codice nello slot di distribuzione di staging

In questa attività verrà distribuito codice nello slot di distribuzione.

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**.

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**.

1. Dal riquadro Cloud Shell eseguire il codice seguente per clonare il repository remoto contenente il codice per l'app Web.

   ```powershell
   git clone https://github.com/Azure-Samples/php-docs-hello-world
   ```

1. Dal riquadro Cloud Shell eseguire il codice seguente per impostare la posizione corrente sul clone appena creato del repository locale contenente il codice dell'app Web di esempio.

   ```powershell
   Set-Location -Path $HOME/php-docs-hello-world/
   ```

1. Dal riquadro Cloud Shell eseguire il codice seguente per aggiungere il repository Git remoto. Assicurarsi di sostituire i segnaposto `[deployment_user_name]` e `[git_clone_url]` con il valore del nome utente di **Credenziali distribuzione** e **URL di git clone**, rispettivamente, identificati nell'attività precedente:

   ```powershell
   git remote add [deployment_user_name] [git_clone_url]
   ```

    >**Nota**: non è necessario che il valore successivo a `git remote add` corrisponda al nome utente di **Credenziali distribuzione**, ma deve essere univoco

1. Dal riquadro Cloud Shell eseguire il codice seguente per eseguire il push del codice dell'app Web di esempio dal repository locale allo slot di distribuzione di staging dell'app Web di Azure. Assicurarsi di sostituire il segnaposto `[deployment_user_name]` con il valore del nome utente di **Credenziali distribuzione**, identificato nell'attività precedente:

   ```powershell
   git push [deployment_user_name] master
   ```

1. Se viene richiesta l'autenticazione, digitare `[deployment_user_name]` e la password corrispondente, impostata nell'attività precedente.

1. Chiudere il riquadro Cloud Shell.

1. Nel pannello dello slot di staging fare clic su **Panoramica** e quindi sul collegamento dell'**URL** per visualizzare la pagina Web predefinita in una nuova scheda del browser.

1. Verificare che nella pagina del browser sia visualizzato il messaggio **Hello World!** e chiudere la nuova scheda.

#### <a name="task-5-swap-the-staging-slots"></a>Attività 5: Scambiare gli slot di staging

In questa attività lo slot di staging verrà scambiato con lo slot di produzione

1. Tornare al pannello che mostra lo slot di produzione dell'app Web.

1. Nella sezione **Distribuzione** fare clic su **Slot di distribuzione** e quindi sull'icona **Scambia** della barra degli strumenti.

1. Nel pannello **Scambia** esaminare le impostazioni predefinite e fare clic su **Scambia**.

1. Fare clic su **Panoramica** nel pannello dello slot di produzione dell'app Web e quindi fare clic sul collegamento dell'**URL** per visualizzare la home page del sito Web in una nuova scheda del browser.

1. Verificare che la pagina Web predefinita sia stata sostituita con la pagina **Hello World!** .

#### <a name="task-6-configure-and-test-autoscaling-of-the-azure-web-app"></a>Attività 6: Configurare e testare la scalabilità automatica dell'app Web di Azure

In questa attività verrà configurata l'app di Azure e ne verrà testata la scalabilità automatica.

1. Nel pannello che mostra lo slot di produzione dell'app Web, nella sezione **Impostazioni**, fare clic su **Aumenta istanze (piano di servizio app)** .

1. Fare clic su **Scalabilità automatica personalizzata**.

    >**Nota**: è anche possibile dimensionare manualmente l'app Web.

1. Lasciare selezionata l'opzione predefinita **Ridimensiona in base a una metrica** e fare clic su **+ Aggiungi una regola**

1. Nel pannello **Regola scalabilità** specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- |--- |
    | Origine della metrica | **Risorsa corrente** |
    | Aggregazione temporale | **Massimo** |
    | Spazio dei nomi delle metriche | **Metriche standard dei piani di servizio app** |
    | Nome metrica | **Percentuale CPU** |
    | Operatore | **Maggiore di** |
    | Soglia della metrica per l'attivazione dell'azione di dimensionamento | **10** |
    | Durata (in minuti) | **1** |
    | Statistica intervallo di tempo | **Massimo** |
    | Operazione | **Aumenta numero di** |
    | Numero di istanze | **1** |
    | Disattiva regole dopo (minuti) | **5** |

    >**Nota**: ovviamente questi valori non rappresentano una configurazione realistica, perché lo scopo è quello di attivare la scalabilità automatica il prima possibile, senza un periodo di attesa prolungato.

1. Fare clic su **Aggiungi** e di nuovo nel pannello di dimensionamento del piano di servizio app specificare le impostazioni seguenti e non modificare i valori predefiniti per le altre impostazioni:

    | Impostazione | Valore |
    | --- |--- |
    | Limiti per le istanze Minimo | **1** |
    | Limiti per le istanze Massimo | **2** |
    | Limiti per le istanze Valore predefinito | **1** |

1. Fare clic su **Save** (Salva).

    >**Nota**: se si verifica un errore che indica che il provider di risorse "microsoft.insights" non è registrato, eseguire `az provider register --namespace 'Microsoft.Insights'` in Cloud Shell e riprovare a salvare le regole di scalabilità automatica.

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**.

1. Dal riquadro Cloud Shell eseguire il codice seguente per identificare l'URL dell'app Web di Azure.

   ```powershell
   $rgName = 'az104-09a-rg1'

   $webapp = Get-AzWebApp -ResourceGroupName $rgName
   ```

1. Dal riquadro Cloud Shell eseguire il codice seguente per avviare un ciclo infinito che invia le richieste HTTP all'app Web:

   ```powershell
   while ($true) { Invoke-WebRequest -Uri $webapp.DefaultHostName }
   ```

1. Ridurre a icona il pannello Cloud Shell senza chiuderlo e nella sezione **Monitoraggio** del pannello dell'app Web fare clic su **Esplora processi**.

    >**Nota**: esplora processi facilita il monitoraggio del numero di istanze e del relativo utilizzo delle risorse.

1. Monitorare l'utilizzo e il numero di istanze per qualche minuto.

    >**Nota**: potrebbe essere necessario selezionare **Aggiorna** per la pagina.

1. Dopo aver notato che il numero di istanze è aumentato a 2, riaprire il riquadro Cloud Shell e terminare lo script premendo **CTRL+C**.

1. Chiudere il riquadro Cloud Shell.

#### <a name="clean-up-resources"></a>Pulire le risorse

>**Nota**: ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate garantisce che non verranno addebitati costi imprevisti.

>**Nota**: non è necessario preoccuparsi se le risorse del lab non possono essere rimosse immediatamente. A volte le risorse hanno dipendenze e l'eliminazione può richiedere molto tempo. Si tratta di un'attività comune dell'amministratore per monitorare l'utilizzo delle risorse, quindi è sufficiente esaminare periodicamente le risorse nel portale per verificare il funzionamento della pulizia. 

1. Nel portale di Azure aprire la sessione di **PowerShell** all'interno del riquadro **Cloud Shell**.

1. Elencare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*'
   ```

1. Eliminare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: il comando viene eseguito in modo asincrono, in base a quanto determinato dal parametro -AsJob, quindi, sebbene sia possibile eseguire un altro comando di PowerShell immediatamente dopo nella stessa sessione di PowerShell, i gruppi di risorse verranno rimossi dopo alcuni minuti.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

+ Creazione di un'app Web di Azure
+ Creazione di uno slot di distribuzione di staging
+ Configurazione delle impostazioni di distribuzione delle app Web
+ Distribuzione di codice nello slot di distribuzione di staging
+ Scambio degli slot di staging
+ Configurazione e test della scalabilità automatica dell'app Web di Azure
