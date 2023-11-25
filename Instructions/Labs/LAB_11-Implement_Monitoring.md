---
lab:
  title: 'Lab 11: Implementare il monitoraggio'
  module: Administer Monitoring
---

# Lab 11 - Implementare il monitoraggio
# Manuale del lab per gli studenti

## Scenario laboratorio

È necessario valutare le funzionalità di Azure che offrono informazioni dettagliate sulle prestazioni e la configurazione delle risorse di Azure, concentrandosi in particolare sulle macchine virtuali di Azure. A tale scopo, si intende esaminare le funzionalità di Monitoraggio di Azure, incluso Log Analytics.

**Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi. 

## Obiettivi

Contenuto del lab:

+ Attività 1: Effettuare il provisioning dell'ambiente lab
+ Attività 2: Registrare i provider di risorse Microsoft.Insights e Microsoft.AlertsManagement
+ Attività 3: Creare e configurare un'area di lavoro Azure Log Analytics e soluzioni basate su Automazione di Azure
+ Attività 4: Esaminare le impostazioni di monitoraggio predefinite delle macchine virtuali di Azure
+ Attività 5: Configurare le impostazioni di diagnostica delle macchine virtuali di Azure
+ Attività 6: Esaminare la funzionalità di Monitoraggio di Azure
+ Attività 7: Esaminare la funzionalità di Azure Log Analytics

## Tempo stimato: 45 minuti

## Diagramma dell'architettura

![image](../media/lab11.png)

### Istruzioni

## Esercizio 1

## Attività 1: Effettuare il provisioning dell'ambiente lab

In questa attività si distribuirà una macchina virtuale che verrà usata per testare gli scenari di monitoraggio.

1. Accedi al [portale di Azure](https://portal.azure.com).

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**.

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**.

1. Sulla barra degli strumenti del riquadro Cloud Shell fare clic sull'icona**Carica/Scarica file**, nel menu a discesa fare clic su **Carica** e caricare i file **\\Allfiles\\Labs\\11\\az104-11-vm-template.json** e **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** nella home directory di Cloud Shell.

1. Nel riquadro Cloud Shell eseguire il comando seguente per creare il gruppo di risorse che ospiterà le macchine virtuali. Sostituire il segnaposto `[Azure_region]` con il nome di un'area di Azure in cui si intende distribuire le macchine virtuali di Azure:

    >**Nota:** assicurarsi di scegliere una delle aree elencate come **Area dell'area di lavoro Log Analytics** nella [documentazione relativa ai mapping delle aree di lavoro](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) a cui si fa riferimento

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Nel riquadro Cloud Shell eseguire il codice seguente per creare la prima rete virtuale e distribuire in tale rete una macchina virtuale usando il modello e i file di parametri caricati:

    >**Nota**: verrà richiesto di fornire una password Amministrazione.
    
   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    >**Nota**: non attendere il completamento della distribuzione, ma procedere con l'attività successiva. La distribuzione dovrebbe richiedere 3 minuti circa.

## Attività 2: Registrare i provider di risorse Microsoft.Insights e Microsoft.AlertsManagement.

1. Nel riquadro Cloud Shell eseguire il comando seguente per registrare i provider di risorse Microsoft.Insights e Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. Ridurre a icona il riquadro Cloud Shell, ma non chiuderlo.

## Attività 3: Creare e configurare un'area di lavoro Azure Log Analytics e soluzioni basate su Automazione di Azure

In questa attività verranno create e configurate un'area di lavoro Azure Log Analytics e soluzioni basate su Automazione di Azure

1. Nel portale di Azure cercare e selezionare **Aree di lavoro Log Analytics** e nel pannello **Aree di lavoro Log Analytics** fare clic su **+ Crea**.

1. Nella scheda **Dati principali** del pannello **Crea area di lavoro Log Analytics** immettere le impostazioni seguenti, fare clic su **Verifica e crea** e quindi su **Crea**:

    | Impostazioni | Valore |
    | --- | --- |
    | Abbonamento | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | nome di un nuovo gruppo di risorse **az104-11-rg1** |
    | Area di lavoro Log Analytics | qualsiasi nome univoco |
    | Region | nome dell'area di Azure in cui è stata distribuita la macchina virtuale nell'attività precedente |

    >**Nota**: assicurarsi di specificare la stessa area in cui sono state distribuite le macchine virtuali nell'attività precedente.

    >**Nota**: attendere il completamento della distribuzione. La distribuzione dovrebbe richiedere 1 minuto circa.

1. Nel portale di Azure cercare e selezionare **Account di Automazione** e nel pannello **Account di Automazione** fare clic su **+ Crea**.

1. Nel pannello **Creare un account di Automazione** specificare le impostazioni seguenti, quindi fare clic su **Verifica e crea** e al momento della convalida fare clic su **Crea**:

    | Impostazioni | valore |
    | --- | --- |
    | Nome dell'account di Automazione | qualsiasi nome univoco |
    | Abbonamento | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | **az104-11-rg1** |
    | Region | nome dell'area di Azure determinata in base alla [documentazione dei mapping dell'area di lavoro](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) |

    >**Nota:** assicurarsi di specificare l'area di Azure in base alla [documentazione sui mapping dell'area di lavoro](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)

    >**Nota**: attendere il completamento della distribuzione. La distribuzione può richiedere 3 minuti circa.

1. Fare clic su **Vai alla risorsa**.

1. Nella sezione **Gestione della configurazione** del pannello Account di Automazione fare clic su **Inventario**.

1. Nell'elenco a discesa **Area di lavoro Log Analytics** del riquadro **Inventario** selezionare l'area di lavoro Log Analytics creata in precedenza in questa attività e fare clic su **Abilita**.

    >**Nota:** attendere il completamento dell'installazione della soluzione Log Analytics corrispondente. L'operazione potrebbe richiedere circa 3 minuti.

    >**Nota:** viene installata automaticamente anche la soluzione **Rilevamento modifiche**.

1. Nel pannello Account di Automazione, nella sezione ** Gestione aggiornamenti** fare clic su **Gestione aggiornamenti**, quindi su **Abilita**.

    >**Nota**: attendere il completamento dell'installazione. L'operazione potrebbe richiedere circa cinque minuti.

## Attività 4: Esaminare le impostazioni di monitoraggio predefinite delle macchine virtuali di Azure

In questa attività si esamineranno le impostazioni di monitoraggio predefinite delle macchine virtuali di Azure

1. Nel portale di Azure cercare e selezionare **Macchine virtuali** e nel pannello **Macchine virtuali** fare clic su **az104-11-vm0**.

1. Nella sezione **Monitoraggio** del pannello **az104-11-vm0** fare clic su **Metriche**.

1. Nel pannello **az104-11-vm0 \| Metriche**, nel grafico predefinito, si noti che l'unico **spazio dei nomi delle metriche** disponibile è **Host macchina virtuale**.

    >**Nota:** questo comportamento è previsto, perché non sono ancora state configurate impostazioni di diagnostica a livello di guest. È tuttavia possibile abilitare le metriche della memoria guest direttamente dall'elenco a discesa **Spazio dei nomi delle metriche**. Le metriche verranno abilitate più avanti in questo esercizio.

1. Nell'elenco a discesa **Metrica** esaminare l'elenco delle metriche disponibili.

    >**Nota:** l'elenco include una gamma di metriche relative a CPU, disco e rete che possono essere raccolte dall'host macchina virtuale, senza avere accesso alle metriche a livello di guest.

1. Nell'elenco a discesa **Metrica** selezionare **Percentuale CPU**, nell'elenco a discesa **Aggregazione** selezionare **Media** ed esaminare il grafico risultante.

## Attività 5: Configurare le impostazioni di diagnostica delle macchine virtuali di Azure

In questa attività si configureranno le impostazioni di diagnostica delle macchine virtuali di Azure.

1. Nella sezione **Monitoraggio** del pannello **az104-11-vm0** fare clic su **Impostazioni di diagnostica**.

1. Nella **scheda Panoramica** del **pannello az104-11-vm0 \| Diagnostic settings (Impostazioni** di diagnostica) selezionare un **account** di archiviazione di diagnostica e quindi fare clic su **Abilita monitoraggio** a livello di guest.

    >**Nota**: attendere l'installazione dell'estensione delle impostazioni di diagnostica. L'operazione potrebbe richiedere circa 3 minuti.

1. Passare alla scheda **Contatori delle prestazioni** del pannello **az104-11-vm0 \| Impostazioni di diagnostica** ed esaminare i contatori disponibili.

    >**Nota:** per impostazione predefinita, sono abilitati i contatori di CPU, memoria, disco e rete. È possibile passare alla visualizzazione **personalizzata** per un elenco più dettagliato.

1. Passare alla scheda **Log** del pannello **az104-11-vm0 \| Impostazioni di diagnostica** ed esaminare le opzioni di raccolta del registro eventi disponibili.

    >**Nota:** per impostazione predefinita, la raccolta dei log include le voci critiche, di errore e di avviso del registro applicazioni e del registro di sistema, nonché le voci di errore di controllo del registro di sicurezza. Qui è anche possibile passare alla visualizzazione **personalizzata** per informazioni più dettagliate sulle impostazioni di configurazione.

1. Nella sezione **Monitoraggio** del pannello **az104-11-vm0** fare clic su **Log** e quindi su **Abilita**.

1. Nel pannello az104-11-vm0 - Log** prendere nota che **l'agente** **di Monitoraggio di Azure verrà installato e quindi fare clic su **Configura**.  

    >**Nota**: non attendere il completamento dell'operazione, ma procedere con il passaggio successivo. L'operazione potrebbe richiedere circa 5 minuti.

1. Nel pannello **az104-11-vm0 \| Log**, sezione **Monitoraggio**, fare clic su **Metriche**.

1. Nel pannello **az104-11-vm0 \| Metriche**, nel grafico predefinito, si noti che a questo punto l'elenco a discesa **Spazio dei nomi delle metriche** oltre alla voce **Host macchina virtuale** include anche la voce **Guest (versione classica)**.

    >**Nota:** questo comportamento è previsto, perché sono stati abilitate le impostazioni di diagnostica a livello di guest. È anche possibile scegliere **Abilita le nuove metriche di memoria guest**.

1. Nell'elenco a discesa ** Spazio dei nomi delle metriche** selezionare la voce **Guest (versione classica)**.

1. Nell'elenco a discesa **Metrica** esaminare l'elenco delle metriche disponibili.

    >**Nota**: l'elenco include metriche aggiuntive a livello di guest non disponibili quando ci si basa solo sul monitoraggio a livello di host.

1. Nell'elenco a discesa **Metrica** selezionare **Memoria\\Byte disponibili**, nell'elenco a discesa **Aggregazione** selezionare **Max** ed esaminare il grafico risultante.

## Attività 6: Esaminare la funzionalità di Monitoraggio di Azure

1. Nel portale di Azure cercare e selezionare **Monitoraggio** e nel pannello **Monitoraggio \| Panoramica** fare clic su **Metriche**.

1. Nella scheda **Sfoglia** del pannello **Selezionare un ambito** passare al gruppo di risorse **az104-11-rg0**, espanderlo, selezionare la casella di controllo accanto alla voce della macchina virtuale **az104-11-vm0** all'interno del gruppo di risorse e fare clic su **Applica**.

    >**Nota**: offre la stessa visualizzazione e le stesse opzioni disponibili nel pannello **az104-11-vm0 - Metriche**.

1. Nell'elenco a discesa **Metrica** selezionare **Percentuale CPU**, nell'elenco a discesa **Aggregazione** selezionare **Media** ed esaminare il grafico risultante.

1. Nel pannello **Monitoraggio \| Metriche**, nel riquadro **Percentuale CPU media per az104-11-vm0**, fare clic su **Nuova regola di avviso**.

    >**Nota**: la creazione di una regola di avviso da Metriche non è supportata per le metriche dello spazio dei nomi delle metriche Guest (versione classica). Questa operazione può essere eseguita usando i modelli Azure Resource Manager, come descritto nel documento [Inviare le metriche del sistema operativo guest all'archivio delle metriche di Monitoraggio di Azure usando un modello Resource Manager per una macchina virtuale Windows](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)

1. Nella sezione **Condizione** del pannello **Crea regola di avviso** fare clic sulla voce della condizione esistente.

1. Nel pannello **Configura logica dei segnali**, nell'elenco dei segnali, nella sezione **Logica avvisi** specificare le impostazioni seguenti (lasciare gli altri valori predefiniti) e fare clic su **Fine**:

    | Impostazioni | valore |
    | --- | --- |
    | Threshold | **Statico** |
    | Tipo di aggregazione | **Media** |
    | Operatore | **Maggiore di** |
    | Valore soglia | **2** |
    | Controllare ogni | **1 minuto** |
    | Periodo di ricerca posticipata| **1 minuto** |

1. Fare clic su **Avanti: Azioni > **, nella sezione **Gruppo di azioni** del pannello **Crea regola di avviso** fare clic sul pulsante **+ Crea gruppo di azioni**.

1. Nella scheda **Dati principali** del pannello **Crea gruppo di azioni** specificare le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni), quindi selezionare **Avanti: Notifiche >**:

    | Impostazioni | Valore |
    | --- | --- |
    | Abbonamento | Nome della sottoscrizione di Azure usata in questo lab |
    | Gruppo di risorse | **az104-11-rg1** |
    | Nome gruppo di azioni | **az104-11-ag1** |
    | Display name | **az104-11-ag1** |

1. Nella scheda **Notifiche** del pannello **Crea gruppo di azioni** selezionare **Posta elettronica/SMS/Push/Voce** nell'elenco a discesa **Tipo di notifica**. Nella casella di testo **Nome** digitare **admin email**. Fare clic sul pulsante **Modifica dettagli** (icona a forma di matita).

1. Nel pannello **Posta elettronica/Messaggio SMS/Push/Voce** selezionare la casella di controllo **Posta elettronica**, digitare l'indirizzo di posta elettronica nella casella di testo **Posta elettronica**, lasciare i valori predefiniti per le altre impostazioni, fare clic su **OK** e nella scheda **Notifiche** del pannello **Crea gruppo di azioni** selezionare **Avanti: Azioni >**.

1. Nella scheda **Azioni** del pannello **Crea gruppo di azioni** esaminare gli elementi disponibili nell'elenco a discesa **Tipo di azione** senza apportare modifiche e selezionare **Revisione e creazione**.

1. Nella scheda **Revisione e creazione** del pannello **Crea gruppo di azioni** selezionare **Crea**.

1. Nel pannello **Crea regola di avviso** fare clic su **Avanti: Dettagli > ** e nella sezione **Dettagli regola di avviso** specificare le impostazioni seguenti (lasciare le altre con i valori predefiniti):

    | Impostazioni | valore |
    | --- | --- |
    | Nome regola di avviso | **Percentuale CPU superiore alla soglia di test** |
    | Descrizione della regola di avviso | **Percentuale CPU superiore alla soglia di test** |
    | Gravità | **Gravità 3** |
    | Abilita alla creazione | **Sì** |

1. Fare clic su **Rivedi e crea** e quindi nella scheda **Rivedi e crea** fare clic su **Crea**.

    >**Nota**: possono essere necessari fino a 10 minuti prima che una regola di avviso per una metrica diventi attiva.

1. Nel portale di Azure cercare e selezionare **Macchine virtuali** e nel pannello **Macchine virtuali** fare clic su **az104-11-vm0**.

1. Nel pannello **az104-11-vm0** fare clic su **Connetti**, nel menu a discesa fare clic su **RDP**, nel pannello **Connetti tramite RDP** selezionare **Scarica file RDP** e seguire le istruzioni per avviare la sessione di Desktop remoto.

    >**Nota**: questo passaggio si riferisce alla connessione tramite Desktop remoto da un computer Windows. In un computer Mac è possibile usare il client Desktop remoto da Mac App Store e nei computer Linux è possibile usare un software client RDP open source.

    >**Nota**: è possibile ignorare eventuali richieste di avviso durante la connessione alle macchine virtuali di destinazione.

1. Quando richiesto, accedere usando il nome utente e la password dello **studente** presenti nel file dei parametri.

1. Nella sessione di Desktop remoto fare clic su **Start**, espandere la cartella **Sistema Windows** e fare clic su **Prompt dei comandi**.

1. Dal prompt dei comandi eseguire il comando seguente per attivare un maggiore utilizzo della CPU nella macchina virtuale di Azure **az104-11-vm0**:

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**Nota:** verrà avviato il ciclo infinito che dovrebbe aumentare l'utilizzo della CPU oltre la soglia della regola di avviso appena creata.

1. Lasciare aperta la sessione di Desktop remoto e tornare alla finestra del browser che visualizza il portale di Azure nel computer lab.

1. Nella finestra del portale di Azure tornare al pannello **Monitoraggio** e fare clic su **Avvisi**.

1. Prendere nota del numero di avvisi **Gravità 3** e quindi fare clic sulla riga **Gravità 3**.

    >**Nota**: potrebbe essere necessario attendere alcuni minuti e fare clic su **Aggiorna**.

1. Nel pannello **Tutti gli avvisi** esaminare gli avvisi generati.

## Attività 7: Esaminare la funzionalità di Azure Log Analytics

1. Nel portale di Azure tornare al pannello **Monitoraggio** e fare clic su **Log**.

    >**Nota**: potrebbe essere necessario fare clic su **Attività iniziali** se è la prima volta che si accede a Log Analytics.

1. Se necessario, fare clic su **Seleziona ambito**, nel pannello **Selezionare un ambito** selezionare la scheda **Recenti**, selezionare **az104-11-vm0** e fare clic su **Applica**.

1. Nella finestra delle query incollare la query seguente, fare clic su **Esegui** ed esaminare il grafico risultante:

   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

    > **Nota**: la query non deve contenere errori (indicati da blocchi rossi sulla barra di scorrimento destra). Se non è possibile incollare la query senza errori direttamente dalle istruzioni, incollare il codice della query in un editor di testo, ad esempio il Blocco note, quindi copiarlo e incollarlo nella finestra di query da tale editor.


1. Fare clic su **Query** nella barra degli strumenti, nel riquadro **Query** individuare **Rileva disponibilità macchina virtuale** e fare doppio clic sul riquadro per compilare la finestra della query, fare clic sul pulsante **Esegui** nel riquadro ed esaminare i risultati.

1. Nella scheda **Nuova query 1** selezionare l'intestazione **Tabelle** ed esaminare l'elenco delle tabelle nella sezione **Macchine virtuali**.

    >**Nota:** i nomi di diverse tabelle corrispondono alle soluzioni installate in precedenza in questo lab.

1. Passare il puntatore del mouse sulla voce **VMComputer** e fare clic sull'icona **Visualizza anteprima dati**.

1. Se sono disponibili dati, nel riquadro **Aggiorna** fare clic su **Usa nell'editor**.

    >**Nota:** potrebbe essere necessario attendere alcuni minuti prima che i dati aggiornati diventino disponibili.

## Pulire le risorse

>**Nota**: ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate garantisce che non verranno addebitati costi imprevisti.

>**Nota**: non è necessario preoccuparsi se le risorse del lab non possono essere rimosse immediatamente. A volte le risorse hanno dipendenze e l'eliminazione può richiedere più tempo. Si tratta di un'attività comune dell'amministratore per monitorare l'utilizzo delle risorse, quindi è sufficiente esaminare periodicamente le risorse nel portale per verificare il funzionamento della pulizia. 

1. Nel portale di Azure aprire la sessione di **PowerShell** all'interno del riquadro **Cloud Shell**.

1. Elencare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. Eliminare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Nota**: il comando viene eseguito in modo asincrono, in base a quanto determinato dal parametro -AsJob, quindi, sebbene sia possibile eseguire un altro comando di PowerShell immediatamente dopo nella stessa sessione di PowerShell, i gruppi di risorse verranno rimossi dopo alcuni minuti.

## Rivedi

In questo lab sono state eseguite le attività seguenti:

+ Provisioning dell'ambiente lab
+ Creazione e configurazione di un'area di lavoro Azure Log Analytics e di soluzioni basate su Automazione di Azure
+ Esame delle impostazioni di monitoraggio predefinite delle macchine virtuali di Azure
+ Configurazione delle impostazioni di diagnostica delle macchine virtuali di Azure
+ Esame della funzionalità di Monitoraggio di Azure
+ Esame della funzionalità di Azure Log Analytics
