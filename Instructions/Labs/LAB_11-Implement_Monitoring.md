---
lab:
  title: 'Lab 11: Implementare il monitoraggio'
  module: Administer Monitoring
---

# Lab 11 - Implementare il monitoraggio

## Introduzione al lab

Questo lab fornisce informazioni su Monitoraggio di Azure. Si apprenderà come creare un avviso e inviarlo a un gruppo di azioni. Attivare e testare l'avviso e controllare il log attività.  

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando**Stati Uniti orientali**.

## Tempo stimato: 40 minuti

## Scenario laboratorio

L'organizzazione dell’utente ha eseguito la migrazione della sua infrastruttura in Azure. È importante che gli amministratori vengano informati di eventuali modifiche significative dell'infrastruttura. Pianificare l’esame delle funzionalità di Monitoraggio di Azure, incluso Log Analytics.

## Diagramma dell'architettura

![Diagramma delle attività dell’architettura](../media/az104-lab11-architecture.png)

## Competenze mansione

+ Attività 1: Usare un modello per effettuare il provisioning di un'infrastruttura.
+ Attività 2: Creare un avviso.
+ Attività 3 : Configurare le notifiche per un gruppo di azioni.
+ Attività 4: Attivare un avviso e accertarsi che funzioni.
+ Attività 5: Creare una regola di elaborazione degli avvisi.
+ Attività 6: Usare query di log di Monitoraggio di Azure.

## Attività 1: Usare un modello per effettuare il provisioning di un'infrastruttura

In questa attività si distribuirà una macchina virtuale che verrà usata per testare gli scenari di monitoraggio.

1. Scaricare i**\\file lab Allfiles\\\\11\\az104-11-vm-template.json** nel computer.

1. Accedere al**portale di Azure** -`https://portal.azure.com`.

1. Nel portale di Azure, cercare e selezionare`Deploy a custom template`.

1. Nella pagina di distribuzione personalizzata, selezionare**Creare un modello personalizzato nell’editor**.

1. Nella pagina di modifica del modello, selezionare**Carica file**.

1. Individuare e selezionare il**\\file Allfiles\\Labs\\11\\az104-11-vm-template.json** e selezionare**Apri**.

1. Seleziona**Salva**.

1. Usare le informazioni seguenti per completare i campi di distribuzione personalizzati, lasciando tutti gli altri campi con i valori predefiniti:

    | Impostazione       | valore         | 
    | ---           | ---           |
    | Subscription  | la propria sottoscrizione di Azure |
    | Gruppo di risorse| `az104-rg11` (se necessario, selezionare**Crea nuovo**)
    | Area geografica        | **Stati Uniti orientali**   |
    | Username      | `localadmin`   |
    | Password      | Fornire una password complessa |
    
1. Selezionare**Rivedi e crea** e quindi**Crea**.

1. Attendere il completamento della distribuzione e quindi fare clic su**Vai al gruppo di risorse**.

1. Esaminare quali risorse sono state distribuite. Deve essere presente una sola rete virtuale con una sola macchina virtuale.

**Configurare Monitoraggio di Azure per le macchine virtuali (che verrà usato nell'ultima attività)**

1. Nel portale, cercare e selezionare**Monitoraggio**.

1. Esaminare tutti gli strumenti analitici, di rilevamento, valutazione e diagnosi disponibili.

1. Selezionare**Visualizza** nella casella**Informazioni dettagliate sulla macchina virtuale**, quindi selezionare**Configura informazioni dettagliate**.

1. Selezionare**Abilita** accanto alla macchina virtuale e quindi**Abilitare nel pannello Monitoraggio di Azure - Onboarding** di Insights.

1. Acquisire le impostazioni predefinite per le regole di sottoscrizione e raccolta dei dati, quindi selezionare**Configura**. 

1. L'installazione e la configurazione dell'agente di macchine virtuali richiederanno alcuni minuti; procedere con il passaggio successivo. 
   
## Attività 2: Creare un avviso

In questa attività viene creato un avviso per quando viene eliminata una macchina virtuale. 

1. Continuare nella pagina**Monitoraggio** e selezionare**Avvisi**. 

1. Selezionare**Crea +**, quindi selezionare**Regola di avviso**. 

1. Selezionare la casella relativa alla sottoscrizione e quindi selezionare**Applica**. Questo avviso verrà applicato a tutte le macchine virtuali nella sottoscrizione. In alternativa, è possibile specificare solo un computer specifico. 

1. Selezionare la scheda**Condizione**, quindi selezionare il collegamento**Visualizza tutti i segnali**.

1. Cercare e selezionare**Elimina macchina virtuale (Macchine virtuali)**. Notare gli altri segnali predefiniti. Selezionare**Applica**.

1. Nell’area**Logica avvisi** (scorrere in basso), rivedere le selezioni**Livello evento**. Lasciare il valore predefinito di**Tutti gli elementi selezionati**.

1. Rivedere le selezioni**Stato**. Lasciare il valore predefinito di**Tutti gli elementi selezionati**.

1. Lasciare aperto il riquadro**Crea una regola di avviso** per l’attività successiva.

## Attività 3: Configurare notifiche del gruppo di azioni

In questa attività, se l'avviso viene attivato, inviare una notifica e-mail al team operativo. 

1. Continua a lavorare sull’avviso. Selezionare**Usa gruppi** di azioni e quindi selezionare**Crea gruppo** di azioni nel pannello**Seleziona gruppo** di azioni.

    >**Suggerimenti utili** Puoi aggiungere fino a cinque gruppi di azioni a una regola di avviso. I gruppi di azioni vengono eseguiti simultaneamente, senza un ordine specifico. Più regole di avviso possono usare lo stesso gruppo di azioni. 

1. Nella scheda**Informazioni di base** immettere i valori indicati di seguito per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | **Dettagli di progetto** |
    | Subscription | sottoscrizione in uso |
    | Gruppo di risorse | **az104-rg11** |
    | Area | **Globale** (impostazione predefinita) |
    | **Dettagli istanza** |
    | Nome gruppo di azioni | `Alert the operations team` (deve essere univoco all'interno del gruppo di risorse) |
    | Nome visualizzato | `AlertOpsTeam` |

1. Al termine, selezionare**Avanti: Notifiche** e immettere i valori seguenti per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | Tipo di notifica | Selezionare**Posta elettronica/SMS/Push/Chiamata vocale** |
    | Nome | `VM was deleted` |

1. Selezionare**Posta elettronica**, quindi nella casella**Posta elettronica** immettere l'indirizzo di posta elettronica e selezionare**OK**. 

    >**Nota:** Dovrebbe essere ricevuta una notifica e-mail indicante che l’utente è stato aggiunto a un gruppo di azioni. Potrebbe verificarsi un ritardo di alcuni minuti, ma ciò indica che la regola è stata distribuita correttamente.

1. Selezionare**Rivedi e crea** e quindi**Crea**.
   
1. Dopo aver creato il gruppo di azioni passare alla**scheda Avanti: Dettagli >** e immettere i valori seguenti per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | Nome regola di avviso | `VM was deleted` |
    | Descrizione della regola di avviso | `A VM in your resource group was deleted` |

1. Selezionare**Rivedi e crea** per convalidare l'input e quindi selezionare**Crea**.

## Attività 4: Attivare un avviso e accertarsi che funzioni

In questa attività si attiva l'avviso e si conferma che viene inviata una notifica. 

>**Nota:** Se si elimina la macchina virtuale prima della distribuzione della regola di avviso, questa potrebbe non essere attivata. 

1. Nel portale, cercare e selezionare**Macchine virtuali**.

1. Selezionare la casella relativa alla macchina virtuale**az104-vm0**.

1. Dalla barra dei menu selezionare**Elimina**.

1. Selezionare la casella**Applica eliminazione forzata**. Selezionare la casella nella parte inferiore che conferma che si vuole eliminare le risorse e selezionare**Elimina**. 

1. Nella barra del titolo, selezionare l'icona**Notifiche** e attendere l'eliminazione di**vm0**.

1. Dovrebbe essere ricevuta una notifica e-mail indicante**Avviso importante: La macchina virtuale di avviso di Monitoraggio di Azure è stata attivata...** In caso contrario, aprire il programma e-mail e cercare un’e-mail daazure-noreply@microsoft.com.

    ![Screenshot del messaggio di posta elettronica di avviso.](../media/az104-lab11-alert-email.png)
   
1. Nel menu delle risorse del portale di Azure selezionare**Monitoraggio** e quindi selezionare**Avvisi** nel menu a sinistra.

1. Dovrebbero essere presenti tre avvisi dettagliati generati dopo l’eliminazione di**vm0**.

   >**Nota:** L'invio dell’e-mail di avviso e l'aggiornamento degli avvisi possono richiedere alcuni minuti. Per evitare l’attesa, continuare con l'attività successiva e tornare in un secondo momento. 

1. Selezionare il nome di uno degli avvisi, ad esempio**Macchina virtuale eliminata**. Viene visualizzato un riquadro**Dettagli avviso** che mostra altri dettagli sull'evento.

## Attività 5: Configurare una regola di elaborazione degli avvisi

In questa attività viene creata una regola di avviso per sopprimere le notifiche durante un periodo di manutenzione. 

1. Continuare nel pannello**Avvisi**, selezionare**Regole di elaborazione degli avvisi** e scegliere **+ Crea**. 
   
1. Selezionare la**sottoscrizione e quindi applica******.
   
1. Selezionare**Avanti: Impostazioni regola**, quindi selezionare**Elimina notifiche**.
   
1. Selezionare**Avanti: Pianificazione >**.
   
1. Per impostazione predefinita, la regola funziona sempre, a meno che non venga disabilitata o configurata una pianificazione. Verrà definita una regola per sopprimere le notifiche durante la manutenzione notturna.
Immettere queste impostazioni per la pianificazione della regola di elaborazione degli avvisi:

    | Impostazione | Valore |
    |---------|---------|
    | Applicare la regola | A un'ora specifica |
    | Inizio | Immettere la data odierna alle 22:00. |
    | Fine | Immettere la data del giorno successivo alle 07:00. |
    | Time zone | Selezionare il fuso orario locale. |

    ![Screenshot della sezione pianificazione di una regola di elaborazione degli avvisi](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Selezionare**Avanti: Dettagli >** e immettere queste impostazioni:

    | Impostazione | valore |
    |---------|---------|
    | Gruppo di risorse | **az104-rg11** |
    | Nome regola | `Planned Maintenance` |
    | Descrizione | `Suppress notifications during planned maintenance.` |

1. Selezionare**Rivedi e crea** per convalidare l'input e quindi selezionare**Crea**.

## Attività 6: Usare query di log di Monitoraggio di Azure

In questa attività si userà Monitoraggio di Azure per eseguire query sui dati acquisiti dalla macchina virtuale.

1. Nella portale di Azure cercare e selezionare`Monitor`, quindi fare clic su**Log**.

1. Se necessario, chiudere la schermata iniziale. 

1. Se necessario, selezionare un ambito, la**sottoscrizione**. Selezionare**Applica**. 

1. Nella scheda**Query**, selezionare**Macchine virtuali** (riquadro sinistro). Potrebbe essere necessario riaprire il pannello.

    ![Screenshot della scheda query.](../media/az104-lab11-queries.png)

1. Esaminare le query disponibili. Selezionare**Esegui** (passare il puntatore del mouse sulla query) per la query**Conteggio heartbeat**.

1. Quando la macchina virtuale è in esecuzione, dovrebbe essere ottenuto un conteggio heartbeat.

1. Sul lato destro della schermata selezionare l'elenco a discesa accanto a**Modalità** semplice, scegliere**modalità** KQL. Esaminare la query. Questa query usa la tabella*heartbeat*.

1. Sostituire la query con questa e fare clic su**Esegui**. Esaminare il grafico risultante. 

   ```
    InsightsMetrics
    | where TimeGenerated > ago(1h)
    | where Name == "UtilizationPercentage"
    | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
    | render timechart
   ```

    >**Nota:** se la query non viene incollata correttamente, provare a incollare nel Blocco note e quindi copiare e incollare nuovamente nel campo della query.

1. Quando si ha tempo, esaminare ed eseguire altre query. 

    >**Suggerimenti utili**: Per provare a usare altre query, è disponibile un[ambiente demo di Log Analytics](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-tutorial#open-log-analytics).
    
    >**Suggerimenti utili**: Una volta trovata una query desiderata, è possibile creare un avviso da essa. 

## Pulire le risorse

Se si usa la**sottoscrizione personale**, dedicare qualche minuto all’eliminazione delle risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse lab. 

+ Nel portale di Azure selezionare il gruppo di risorse, selezionare**Elimina il gruppo di risorse**,**Immettere il nome del gruppo di risorse**, quindi fare clic su**Elimina**.
+ Tramite Azure PowerShell,`Remove-AzResourceGroup -Name resourceGroupName`.
+ Usando l’interfaccia della riga di comando,`az group delete --name resourceGroupName`.

## Estendere l'apprendimento con Copilot
Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a*copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.

+ Quali sono i passaggi di configurazione basilari per ricevere avvisi in Azure quando una macchina virtuale è inattiva?
+ Come ricevere una notifica quando viene attivato un avviso di Azure?
+ Creare una query di Monitoraggio di Azure per fornire informazioni sulle prestazioni della CPU della macchina virtuale.

## Altre informazioni con la formazione autogestita

+ [Migliorare la risposta agli eventi imprevisti con la generazione di avvisi in Azure](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/). Rispondere agli eventi imprevisti e alle attività nell'infrastruttura tramite le funzionalità di avviso di Monitoraggio di Azure.
+ [Monitorare le macchine virtuali di Azure con Monitoraggio di Azure](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/). Monitorare le macchine virtuali di Azure usando Monitoraggio di Azure per raccogliere e analizzare le log e metriche di client e host delle macchine virtuali.

## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab. 

+ Gli avvisi consentono di rilevare e risolvere i problemi prima che gli utenti notino che si è verificato un problema con l'infrastruttura o l'applicazione.
+ È possibile creare avvisi su qualsiasi metrica o fonte di dati di log nella piattaforma di dati di Monitoraggio di Azure.
+ Una regola di avviso monitora i dati e acquisisce un segnale che indica che sta accadendo qualcosa nella risorsa specificata.
+ Se vengono soddisfatte le condizioni della regola di avviso, viene attivato un avviso. È possibile attivare diverse azioni (e-mail, SMS, push, voce).
+ I gruppi di azioni includono singoli utenti che devono ricevere una notifica di un avviso.
