---
lab:
  title: 'Lab 11: Implementare il monitoraggio'
  module: Administer Monitoring
---

# Lab 11 - Implementare il monitoraggio

## Introduzione al lab

In questo lab vengono fornite informazioni su Monitoraggio di Azure. Si apprenderà come creare un avviso e inviarlo a un gruppo di azioni. Attivare e testare l'avviso e controllare il log attività.  

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali.

## Tempo stimato: 40 minuti

## Scenario laboratorio

L'organizzazione ha eseguito la migrazione dell'infrastruttura ad Azure. È importante che i Amministrazione istrator vengano informati di eventuali modifiche significative dell'infrastruttura. Si prevede di esaminare le funzionalità di Monitoraggio di Azure, incluso Log Analytics.

## Simulazione interattiva del lab

È disponibile una simulazione di lab interattiva che può risultare utile per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

+ [Implementare il monitoraggio.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017) Creare un'area di lavoro Log Analytics e soluzioni di automazione di Azure. Esaminare le impostazioni di monitoraggio e diagnostica per le macchine virtuali. Esaminare le funzionalità di Monitoraggio di Azure e Log Analytics. 

## Diagramma dell'architettura

![Diagramma delle attività di architettura](../media/az104-lab11-architecture-diagram.png)

## Attività

+ Attività 1: Effettuare il provisioning dell'ambiente lab.
+ Attività 2: Creare l'avviso del log attività di Azure.
+ Attività 3: Attivare l'avviso.
+ Attività 4: Aggiungere una regola di avviso.
+ Attività 5: Usare query di log di Monitoraggio di Azure.

## Attività 1: Effettuare il provisioning dell'ambiente lab

In questa attività si distribuirà una macchina virtuale che verrà usata per testare gli scenari di monitoraggio.

1. Se necessario, scaricare i **\\file lab Allfiles\\Lab11\\az104-11-vm-template.json** nel computer.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Nella portale di Azure cercare e selezionare `Deploy a custom template`.

1. Nella pagina di distribuzione personalizzata selezionare **Compila un modello personalizzato nell'editor**.

1. Nella pagina modifica modello selezionare **Carica file**.

1. Individuare e selezionare il **\\file Allfiles\\Labs11\\az104-11-vm-template.json** e selezionare **Apri**.

1. Seleziona **Salva**.

1. Usare le informazioni seguenti per completare i campi di distribuzione personalizzati, lasciando tutti gli altri campi con i valori predefiniti:

    | Impostazione       | Valore         | 
    | ---           | ---           |
    | Subscription  | la propria sottoscrizione di Azure |
    | Gruppo di risorse| `az104-rg11` (Se necessario, selezionare **Crea nuovo**)
    | Area geografica        | **Stati Uniti orientali**   |
    | Username      | `Student`   |
    | Password      | Specificare una password complessa |
    
1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Attendere il completamento della distribuzione, quindi fare clic su **Vai al gruppo** di risorse.

1. Esaminare le risorse distribuite. Deve essere presente una rete virtuale con una macchina virtuale.

**Configurare Monitoraggio di Azure per le macchine virtuali (verrà usato nell'ultima attività)**

1. Nel portale cercare e selezionare **Monitoraggio**.

1. Esaminare tutti gli strumenti analitici, di rilevamento, valutazione e diagnosi disponibili.

1. Selezionare **Visualizza** nella **casella Informazioni dettagliate** macchina virtuale e quindi selezionare **Configura informazioni dettagliate**.

1. Selezionare la macchina virtuale e quindi **Abilitare** (due volte).

1. Prendere le impostazioni predefinite per le regole di sottoscrizione e raccolta dati, quindi selezionare **Configura**. 

1. L'installazione e la configurazione dell'agente di macchine virtuali richiederanno alcuni minuti, procedere con il passaggio successivo. 
   
## Attività 2: Creare l'avviso del log attività di Azure

In questa attività viene creato un avviso per quando viene eliminata una macchina virtuale. 

1. Nella portale di Azure cercare e selezionare **Monitoraggio**. 

1. Nel menu Monitoraggio selezionare **Avvisi**. 

1. Selezionare Crea e** selezionare ****Regola** di avviso. 

1. Selezionare la casella per il **gruppo di risorse az104-rg11** e quindi selezionare **Applica**. Questo avviso verrà applicato a tutte le macchine virtuali nel gruppo di risorse. In alternativa, è possibile specificare solo un computer specifico. 

1. Selezionare la **scheda Condizione** e quindi selezionare il **collegamento Visualizza tutti i segnali** .

1. Cercare e selezionare **Elimina macchina virtuale (Macchine virtuali).** Si notino gli altri segnali predefiniti. Selezionare **Applica**.

1. Poiché si vogliono ricevere avvisi di tutti i tipi, per tutte le impostazioni di **Logica avvisi** lasciare il valore predefinito **Tutti selezionati**.

1. Lasciare aperto il **riquadro Crea una regola** di avviso per l'attività successiva.

## Attività 3: Aggiungere un'azione di avviso di posta elettronica

In questa attività, se l'avviso viene attivato un messaggio di posta elettronica inviato al team operativo. 

1. Nel riquadro **Crea una regola di avviso**, selezionare il pulsante **Avanti: Azioni** e selezionare **Crea gruppo di azioni**. 

1. Nella scheda **Informazioni di base** immettere i valori indicati di seguito per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | **Dettagli di progetto** |
    | Subscription | sottoscrizione in uso |
    | Gruppo di risorse | **az104-rg11** |
    | Area | **Globale** (impostazione predefinita) |
    | **Dettagli istanza** |
    | Nome gruppo di azioni | `Alert the operations team` (deve essere univoco nel gruppo di risorse) |
    | Nome visualizzato | `AlertOpsTeam` |

1. Selezionare **Avanti: Notifiche** e immettere i valori seguenti per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | Tipo di notifica | Selezionare **Posta elettronica/SMS/Push/Chiamata vocale** |
    | Nome | **Macchina virtuale eliminata** |

1. Selezionare **Posta elettronica**, quindi nella casella **Posta elettronica** immettere l'indirizzo di posta elettronica e selezionare **OK**. 

1. **Nel riquadro Crea una regola** di avviso selezionare il **pulsante Avanti: Dettagli** e immettere i valori seguenti per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | Nome regola di avviso | **Macchina virtuale eliminata** |
    | Descrizione | **Una macchina virtuale nel gruppo di risorse è stata eliminata** |

1. Selezionare **Rivedi e crea** per convalidare l'input e quindi selezionare **Crea**.

    >**Nota:** dovresti ricevere una notifica tramite posta elettronica che informa che sei stato aggiunto a un gruppo di azioni. Potrebbe verificarsi un ritardo di alcuni minuti, ma si tratta di un segno sicuro della distribuzione della regola. 

## Attività 4: Attivare l'avviso

In questa attività si attiva l'avviso e si conferma che viene inviata una notifica. 

>**Nota:** se si elimina la macchina virtuale prima della distribuzione della regola di avviso, la regola di avviso potrebbe non essere attivata. 

1. Nel portale cercare e selezionare **Macchine** virtuali.

1. Selezionare la casella per la **macchina virtuale az104-vm0** .

1. Dalla barra dei menu selezionare **Elimina**.

1. Selezionare la casella **Applica l'eliminazione** forzata. Selezionare la casella per confermare l'eliminazione, quindi selezionare **Elimina**. 

1. Nella barra del titolo selezionare l'icona **Notifiche** e attendere l'eliminazione **di vm0** .

1. Si dovrebbe ricevere un messaggio di posta elettronica di notifica che legge, **Avviso importante: la macchina virtuale di avviso di Monitoraggio di Azure è stata eliminata è stata attivata...** In caso contrario, aprire il programma di posta elettronica e cercare un messaggio di posta elettronica da azure-noreply@microsoft.com.

    ![Screenshot del messaggio di posta elettronica di avviso.](../media/az104-lab11-alert-email.png)

   >**Nota:** l'invio del messaggio di posta elettronica di avviso e l'aggiornamento degli avvisi nel portale può richiedere alcuni minuti. Se non si vuole attendere, continuare con l'attività successiva e quindi tornare. 
   
1. Nel menu delle risorse del portale di Azure selezionare **Monitoraggio** e quindi selezionare **Avvisi** nel menu a sinistra.

1. Dovrebbero essere presenti tre avvisi dettagliato generati eliminando **vm0**.

1. Selezionare il nome di uno degli avvisi, ad esempio **Macchina virtuale eliminata**. Viene visualizzato un riquadro **Dettagli avviso** che mostra altri dettagli sull'evento.

## Attività 5: Aggiungere una regola di avviso

In questa attività si crea una regola di avviso per eliminare le notifiche durante un periodo di manutenzione. 

1. Continuare nel pannello **Avvisi** , selezionare **Regole** di elaborazione avvisi e quindi **+ Crea**. 
   
1. Selezionare il **gruppo** di risorse, quindi selezionare **Applica**.
   
1. Selezionare **Avanti: Impostazioni regola**, quindi selezionare **Elimina notifiche**.
   
1. Al termine, selezionare **Avanti: Pianificazione**.
   
1. Per impostazione predefinita, la regola funziona sempre, a meno che non venga disabilitata o configurata una pianificazione. Si definirà una regola per eliminare le notifiche durante la manutenzione notturna.
Immettere queste impostazioni per la pianificazione della regola di elaborazione degli avvisi:

    | Impostazione | Valore |
    |---------|---------|
    | Applicare la regola | A un'ora specifica |
    | Inizio | Immettere la data odierna alle 10.00. |
    | Fine | Immettere la data di domani alle 7.00. |
    | Time zone | Selezionare il fuso orario locale. |

    ![Screenshot della sezione pianificazione di una regola di elaborazione degli avvisi](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Selezionare **Avanti: Dettagli** e immettere le impostazioni seguenti:

    | Impostazione | Valore |
    |---------|---------|
    | Gruppo di risorse | **az104-rg11** |
    | Nome regola | `Planned Maintenance` |
    | Descrizione | `Suppress notifications during planned maintenance.` |

1. Selezionare **Rivedi e crea** per convalidare l'input e quindi selezionare **Crea**.

## Attività 6: Usare query di log di Monitoraggio di Azure

In questa attività si userà Monitoraggio di Azure per eseguire query sui dati acquisiti dalla macchina virtuale.

1. Nella portale di Azure cercare e selezionare `Monitor` il pannello Fare clic su **Log**.

1. Nella **finestra Query** selezionare **Macchine** virtuali (riquadro sinistro). Se necessario, chiudere la schermata iniziale. 

1. Revoca le query disponibili. **Eseguire** (passare il puntatore del mouse sulla query) la **query Conteggio heartbeat.**

1. Quando la macchina virtuale è in esecuzione, dovrebbe essere visualizzato un numero di heartbeat.

1. Esaminare la query. Questa query usa la *tabella heartbeat* . 

1. Sostituire la query con questa e quindi fare clic su **Esegui**. Esaminare il grafico risultante.
   
   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```
1. Quando si ha tempo, esaminare ed eseguire altre query.

    >**Nota: si noti** che uou può configurare una regola di avviso basata su una query. 

## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Gli avvisi consentono di rilevare e risolvere i problemi prima che gli utenti notino che si è verificato un problema con l'infrastruttura o l'applicazione.
+ È possibile creare avvisi su qualsiasi metrica o fonte di dati di log nella piattaforma di dati di Monitoraggio di Azure.
+ Una regola di avviso monitora i dati e acquisisce un segnale che indica che sta accadendo qualcosa nella risorsa specificata.
+ Se vengono soddisfatte le condizioni della regola di avviso, viene attivato un avviso. È possibile attivare diverse azioni (posta elettronica, SMS, push, voce).
+ I gruppi di azioni includono utenti che devono ricevere una notifica di un avviso, in modo da poter intervenire. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.

+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.

