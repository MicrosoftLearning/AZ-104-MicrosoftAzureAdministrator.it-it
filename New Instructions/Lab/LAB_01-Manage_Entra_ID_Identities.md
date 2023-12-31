---
lab:
  title: 'Lab 01: Gestire le identità id di Microsoft Entra'
  module: Administer Identity
---

# Lab 01 - Gestire le identità id di Microsoft Entra

## Introduzione al lab

Si tratta della prima di una serie di lab per Azure Amministrazione istrators. In questo lab vengono fornite informazioni su utenti e gruppi. Gli utenti e i gruppi sono i blocchi predefiniti di base per una soluzione di gestione delle identità. Si ha anche familiarità con gli strumenti di amministrazione di base. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono visualizzati negli Stati Uniti orientali.

## Tempo stimato: 40 minuti

## Scenario laboratorio

L'organizzazione sta creando un nuovo ambiente lab per i test di pre-produzione di app e servizi.  Alcuni tecnici vengono assunti per gestire l'ambiente lab, incluse le macchine virtuali. Per consentire agli ingegneri di eseguire l'autenticazione tramite Microsoft Entra ID, è stato eseguito il provisioning di utenti e account di gruppo. Per ridurre al minimo il sovraccarico amministrativo, l'appartenenza ai gruppi deve essere aggiornata automaticamente in base ai titoli dei processi. È anche necessario sapere come eliminare gli utenti per impedire l'accesso dopo che un tecnico lascia l'organizzazione.

## Simulazione interattiva del lab

Esistono simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Gestisci identità ID](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201) Entra*. Creare e configurare gli utenti e assegnarli ai gruppi. Creare un tenant di Azure e gestire gli account guest. 

## Diagramma dell'architettura
![Diagramma dell'architettura lab 01.](../media/az104-lab01-user-and-groups2.png)

## Attività

+ Attività 1: Creare un gruppo di risorse.
+ Attività 2: Creare e configurare gli account utente.
+ Attività 3: Creare gruppi e aggiungere membri.
+ Attività 4: acquisire familiarità con Cloud Shell.
+ Attività 5: Esercitarsi con Azure PowerShell.
+ Attività 6: Esercitarsi con la shell Bash.

## Attività 1: Creare un gruppo di risorse

In questa attività si crea un gruppo di risorse. Un gruppo di risorse è un raggruppamento di risorse correlate. Ad esempio, tutte le risorse per un progetto, un reparto o un'applicazione. 

>**Nota:** per ogni lab di questo corso si creerà un nuovo gruppo di risorse. In questo modo è possibile individuare e gestire rapidamente le risorse del lab. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

    >**Nota:** il portale di Azure viene usato in tutti i lab. Se non si ha una versione di Azure, digitare `Quickstart Center` nella casella di ricerca superiore. Poi qualche minuto per guardare il **video Introduttivo nel video portale di Azure**. Anche se in precedenza è stato usato il portale, sono disponibili alcuni suggerimenti e trucchi per spostarsi e personalizzare l'interafacea. 
   
1. Nella portale di Azure cercare e selezionare `Resource groups`.
   
1. Nel pannello **Gruppi** di risorse fare clic su **+ Crea** e specificare le informazioni necessarie. 

    | Impostazione | Valore |
    | --- | --- |
    | Nome della sottoscrizione | sottoscrizione in uso |
    | Nome gruppo di risorse | `az104-rg1` |
    | Ufficio | **Stati Uniti orientali** |

    >**Nota:** tutti i lab usano **Stati Uniti** orientali. Guardare il video Selezionare l'area **migliore** nel **Centro** avvio rapido per informazioni su cosa prendere in considerazione quando si seleziona un'area.  
    
1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere che il gruppo di risorse venga distribuito. Usare l'icona **notifica** (in alto a destra) per tenere traccia dello stato di avanzamento della distribuzione.

1. Selezionare Vai alla risorsa **, aggiornare **la pagina e verificare che il nuovo gruppo di risorse venga visualizzato nell'elenco dei gruppi di risorse.

    ![Screenshot dell'elenco di gruppi di risorse.](../media/az104-lab01-create-resource-group.png)

## Attività 2: Creare e configurare gli account utente

In questa attività verranno creati e configurati gli account utente. Gli account utente archivieranno i dati utente, ad esempio nome, reparto, posizione e informazioni di contatto.

1. Continuare nella portale di Azure. 

1. Cercare e selezionare `Microsoft Entra ID`.

1. Nel pannello Microsoft Entra ID scorrere verso il basso fino alla **sezione Gestisci** , fare clic su **Impostazioni** utente ed esaminare le opzioni di configurazione disponibili.

1. Tornare nel pannello **Utenti - Tutti gli utenti** e quindi fare clic su **+ Nuovo utente**.

1. Creare un nuovo utente con le impostazioni seguenti (lasciare le impostazioni predefinite per altri utenti). Si notino tutti i diversi tipi di dati che possono essere inclusi nell'account utente. 

    | Impostazione | valore |
    | --- | --- |
    | Nome dell'entità utente | `az104-user1` |
    | Nome visualizzato | `az104-user1` |
    | Genera automaticamente la password | de-select |
    | Password iniziale | **Specificare una password sicura** |
    | Titolo processo (scheda Proprietà) | `Cloud Administrator` |
    | Reparto (scheda Proprietà) | `IT` |
    | Percorso utilizzo (scheda Proprietà) | **Stati Uniti** |

### Attività 4: Creare gruppi e aggiungere membri

In questa attività si crea un gruppo. I gruppi vengono usati per account utente o dispositivi. Alcuni gruppi hanno membri assegnati in modo statico. Alcuni gruppi hanno membri assegnati dinamicamente. I gruppi dinamici vengono aggiornati automaticamente in base alle proprietà degli account utente o dei dispositivi. I gruppi statici richiedono un sovraccarico amministrativo maggiore (gli amministratori devono aggiungere e rimuovere manualmente i membri).

1. Nella portale di Azure cercare e selezionare **Gruppi**.

1. Si notino le informazioni sul gruppo, ad **esempio Tipo di** appartenenza, **Origine** e **Tipo**. Si noti anche il numero di membri nel gruppo. 

1. Selezionare **+ Nuovo gruppo** e creare un nuovo gruppo. 

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di gruppo | **Sicurezza** |
    | Nome del gruppo | `IT Lab Administrators` (regolare il nome se questo non è disponibile) |
    | Descrizione gruppo | `Administrators that manage the IT lab` |
    | Tipo di appartenenza | **Assegnate** |

    >**Nota**: l'elenco **a discesa Tipo di** appartenenza potrebbe essere disattivato. Qui è possibile passare da un gruppo assegnato a un gruppo dinamico. Questo richiede una licenza Entra ID Premium P1 o P2.

    ![Screenshot della creazione di un gruppo assegnato.](../media/az104-lab01-create-assigned-group.png)

1. Fare clic su **Nessun membro selezionato**.

1. Nel pannello **Aggiungi membri** cercare l'account utente. **Selezionare** l'account utente da aggiungere al gruppo. 

1. Fare clic su **Crea** per completare la creazione del gruppo. 

## Attività 5: acquisire familiarità con Cloud Shell.

In questa attività si lavora con Azure Cloud Shell. Azure Cloud Shell è un terminale interattivo, autenticato e accessibile dal browser per la gestione delle risorse di Azure. Offre la possibilità di scegliere l'esperienza shell più adatta al proprio modo di lavorare, Bash o PowerShell. 

1. Selezionare l'icona **di Cloud Shell** in alto a destra nel portale di Azure. In alternativa, è possibile passare direttamente a `https://shell.azure.com`.

1. Quando viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**. Bash viene usato nell'attività successiva.

    >**Lo sapevi?**  Se si lavora principalmente con sistemi Linux, l'interfaccia della riga di comando di Azure è più naturale. Se si lavora principalmente con i sistemi Windows, Azure PowerShell è più naturale. 

1. Nella **schermata Non è installato** spazio di archiviazione selezionare **Mostra impostazioni** avanzate e specificare le informazioni necessarie. Al termine, selezionare **Crea archiviazione**. 

    | Impostazione | Valori |
    |  -- | -- |
    | Gruppo di risorse | **Creare nuovo gruppo di risorse** |
    | Archiviazione account (creare un nuovo account usando un nome univoco globale (ad esempio cloudshellstoragemystorage)) | **cloudshellxxxxxxx** |
    | Condivisione file (crea nuovo) | **shellstorage** |

    >**Nota:** se si lavora in un ambiente lab ospitato, è necessario configurare l'archiviazione di Cloud Shell ogni volta che viene creato un nuovo ambiente lab.

    >**Nota: l'attività** 6 consente di esercitarsi con Azure PowerShell. L'attività 7 consente di esercitarsi con l'interfaccia della riga di comando. È possibile eseguire entrambe le attività o solo quella a cui si è più interessati. 

## Attività 6: Procedura con Azure PowerShell

In questa attività si creano un gruppo di risorse e un gruppo di Azure AD usando la sessione di Azure PowerShell in Cloud Shell.

    >**Note:** Use the arrow keys to move through the command history. Use the tab key to autocomplete commands and parameters.

1. Continuare a lavorare in Cloud Shell. In qualsiasi momento usare **cls** per cancellare la finestra di comando.

1. Azure PowerShell usa un *formato sostantivo verbo**-* per i cmdlet. Ad esempio, il cmdlet per creare un nuovo gruppo di risorse è **New-AzResourceGroup**. Per visualizzare come usare il cmdlet, eseguire il comando Get-Help.

   ```powershell
   Get-Help New-AzResourceGroup -detailed
   ```
1. Per creare un gruppo di risorse dalla sessione di PowerShell in Cloud Shell, eseguire i comandi seguenti. Si noti che i comandi che iniziano con un segno di dollaro ($) creano variabili che è possibile usare nei comandi successivi. Assicurarsi di ricevere un messaggio riuscito. 

   ```powershell
   $location = 'eastus'
   $rgName = 'az104-rg-ps'
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Per recuperare le proprietà del gruppo di risorse appena creato, eseguire il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```

1. Si proverà ora a creare un nuovo gruppo di Azure.

   ```powershell
   Get-Help New-AzureADGroup -detailed
   ```

1. Usando l'esempio nella Guida, provare questi comandi. Si noti che è prima necessario connettersi ad Azure AD.

   ```powershell
   Connect-AzureAD 
   New-AzureADGroup -DisplayName "MyPSgroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "MyPSgroup"
   ```

1. Tornare al portale di Azure. Verificare di avere un nuovo gruppo di risorse e un nuovo gruppo di Azure. Potrebbe essere necessario aggiornare le pagine. 

## Attività 7: Esercitarsi con la shell Bash

In questa attività si creano un gruppo di risorse e un gruppo di Azure usando la sessione dell'interfaccia della riga di comando di Azure in Cloud Shell.

1. Procedere in Cloud Shell. Usare l'elenco a discesa per passare a **Bash**.

    >**Nota:** usare i tasti di direzione per spostarsi nella cronologia dei comandi. Usare il tasto TAB per completare automaticamente i comandi e i parametri. 

1. L'interfaccia della riga di comando di Azure usa una sintassi facile da leggere. Ad esempio, per interagire con i gruppi di risorse, il comando è **az group**.  

   ```sh
   az group --help
   ```

1. L'opzione **di creazione** sembra promettente. Si noti che i nomi in maiuscolo creano variabili a cui è possibile fare riferimento nei comandi successivi. 

   ```sh
   RGNAME='az104-rg1-cli'
   LOCATION='eastus'
   az group create --name $RGNAME --location $LOCATION
   ```
   
1. Per verificare e recuperare le proprietà per il gruppo di risorse appena creato, provare il **comando show** . 

   ```sh
   az group show --name $RGNAME
   ```
1. A questo punto si userà la Guida per altre informazioni sulla creazione di un gruppo di Azure.

    ```sh
    az ad group --help
    ```

1. **Creare** il gruppo ed **elencare** i gruppi da verificare.

   ```sh
   az ad group create --display-name MyCLIgroup --mail-nickname MyCLIgroup
   az ad group list
   ```

1. Tornare al portale di Azure. Verificare di avere un nuovo gruppo di risorse e un nuovo gruppo di Azure. Potrebbe essere necessario aggiornare le pagine.   
    
## Esaminare i punti principali del lab

Congratulazioni per il completamento del lab. Ecco alcuni passaggi principali per questo lab:

+ Il portale di Azure è un buon modo per iniziare a creare e gestire le risorse di Azure. Amministrazione istrator può personalizzare il portale e condividere i dashboard.
+ I gruppi di risorse sono un modo per raggruppare le risorse correlate. È possibile usare un gruppo di risorse per un progetto, un reparto o un'applicazione. In questo modo è facile gestire e monitorare un gruppo di risorse correlate. 
+ Microsoft Entra ID supporta diversi tipi di account utente. Ogni tipo di account utente ha un livello di accesso specifico per l'ambito del lavoro previsto. 
+ Raggruppare gli account insieme a utenti o dispositivi correlati. L'appartenenza al gruppo può essere assegnata in modo statico o dinamico. 
+ Cloud Shell è un terminale interattivo autenticato per la gestione delle risorse di Azure. Cloud Shell fornisce l'accesso a Bash o Azure PowerShell.
+ Azure PowerShell e Bash consentono di creare risorse tramite script. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.

+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.

