---
lab:
  title: 'Lab 11: Implementare il monitoraggio'
  module: Administer Monitoring
---

# Lab 11 - Implementare il monitoraggio

## Introduzione al lab

In questo lab vengono fornite informazioni su Monitoraggio di Azure. Si apprenderà come creare un avviso da inviare a un gruppo di azioni. Attivare l'avviso e controllare il log attività.  

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando Stati Uniti orientali.

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

1. Se necessario, scaricare i **\\file lab Allfiles\\\\11\\az104-11-vm-template.json** e **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** nel computer.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Nella portale di Azure cercare e selezionare `Deploy a custom template`.

1. Nella pagina di distribuzione personalizzata selezionare **Compila un modello personalizzato nell'editor**.

1. Nella pagina modifica modello selezionare **Carica file**.

1. Individuare e selezionare il **file Allfiles\\Labs\\11\\az104-11-vm-template.json** e selezionare **Apri**.\\

1. Seleziona **Salva**.

1. Nella pagina di distribuzione personalizzata selezionare **Modifica parametri**.

1. Nella pagina modifica parametri selezionare **Carica file**. Individuare e selezionare il **file Allfiles\\Labs\\11\\az104-11-vm-parameters.json** e selezionare **Apri**.\\

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

1. Esaminare le risorse distribuite, tra cui una macchina virtuale e una rete virtuale.

    >**Nota:** più avanti nel lab verrà usato Monitoraggio di Azure, quindi è necessario un minuto per installare l'agente di macchine virtuali

1. Nel portale cercare e selezionare **Monitoraggio**.

1. Esaminare tutti gli strumenti analitici, di rilevamento, valutazione e diagnosi disponibili.

1. Selezionare **Visualizzazione** informazioni dettagliate macchina virtuale e quindi selezionare **Configura informazioni dettagliate**.

1. Selezionare la macchina virtuale e quindi **Abilitare** (due volte).

1. L'installazione e la configurazione dell'agente richiederanno alcuni minuti, procedere con il passaggio successivo. 
   
## Attività 2: Creare l'avviso del log attività di Azure

1. Nella portale di Azure cercare e selezionare **Monitoraggio**. 

1. Nel menu Monitoraggio selezionare **Avvisi**. 

1. Selezionare Crea e** selezionare ****Regola** di avviso. Verrà visualizzato il riquadro **Crea una regola di avviso** con la sezione **Ambito** aperta e il riquadro **Selezionare una risorsa** aperto a destra.

1. **Nel riquadro Selezionare una risorsa** il **campo Filtra per sottoscrizione** deve essere già popolato. Nell'elenco a discesa **Filtra per tipo di risorsa** cercare e selezionare **Macchine virtuali**.

1. Si vuole ricevere un avviso quando viene eliminata una macchina virtuale nel gruppo di risorse. Selezionare la casella per il **gruppo di risorse az104-rg11** e quindi selezionare **Applica**.

1. Selezionare la **scheda Condizione** e quindi selezionare il **collegamento Visualizza tutti i segnali** .

1. Cercare e selezionare **Elimina macchina virtuale (Macchine virtuali).** Selezionare **Applica**.

1. Poiché si vogliono ricevere avvisi di tutti i tipi, per tutte le impostazioni di **Logica avvisi** lasciare il valore predefinito **Tutti selezionati**. Lasciare aperto il riquadro **Crea una regola di avviso** per la sezione successiva.

## Attività 3: Aggiungere un'azione di avviso di posta elettronica

Per l'avviso di Monitoraggio di Azure precedente non sono state aggiunte azioni. Sono solo stati esaminati gli avvisi attivati nel portale di Azure. Le azioni consentono di inviare un messaggio di posta elettronica per le notifiche, attivare una funzione di Azure o chiamare un webhook. In questo esercizio viene aggiunto un avviso di posta elettronica quando vengono eliminate le macchine virtuali.

1. Nel riquadro **Crea una regola di avviso**, selezionare il pulsante **Avanti: Azioni** e selezionare **Crea gruppo di azioni**. 

1. Nella scheda **Informazioni di base** immettere i valori indicati di seguito per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | **Dettagli di progetto** |
    | Subscription | sottoscrizione in uso |
    | Gruppo di risorse | **az104-rg11** |
    | Area | **Globale** (impostazione predefinita) |
    | **Dettagli istanza** |
    | Nome gruppo di azioni | **Avvisi per il team operativo** |
    | Nome visualizzato | **AlertOpsTeam** |

1. Selezionare **Avanti: Notifiche** e immettere i valori seguenti per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | Tipo di notifica | Selezionare **Posta elettronica/SMS/Push/Chiamata vocale** |
    | Nome | **Macchina virtuale eliminata** |

1. Selezionare **Posta elettronica**, quindi nella casella **Posta elettronica** immettere l'indirizzo di posta elettronica e selezionare **OK**. 

1. Selezionare **Rivedi e crea** per convalidare le impostazioni.

1. Seleziona **Crea**.
 
1. Verrà nuovamente visualizzato il riquadro **Crea una regola di avviso**. Selezionare il pulsante **Avanti: Dettagli** e immettere i valori seguenti per ogni impostazione.

    | Impostazione | Valore |
    |---------|---------|
    | Nome regola di avviso | **Macchina virtuale eliminata** |
    | Descrizione | **Una macchina virtuale nel gruppo di risorse è stata eliminata** |

1. Espandere la sezione **Opzioni avanzate** e verificare che l'opzione **Abilita regola di avviso alla creazione** sia selezionata.

1. Selezionare **Rivedi e crea** per convalidare l'input e quindi selezionare **Crea**.

    >**Nota:** i destinatari aggiunti al gruppo di azioni configurato (team operativo) ricevono una notifica:

    - Quando vengono aggiunti al gruppo di azioni
    - Quando l'avviso è attivo
    - Quando l'avviso viene attivato

## Attività 4: Attivare l'avviso

Per attivare un avviso, eliminare la macchina virtuale nel gruppo di risorse.

>**Nota:** per attivare una regola di avviso del log attività possono essere necessari fino a cinque minuti. In questo esercizio, se si elimina la macchina virtuale prima della distribuzione della regola, la regola di avviso potrebbe non essere attivata. 

1. Nel menu del portale di Azure o nella **home page** selezionare **Crea una risorsa**.

1. Selezionare la casella per la **macchina virtuale az104-vm0** .

1. Dalla barra dei menu selezionare **Elimina**.

1. Digitare "sì" nel campo **Conferma eliminazione** e quindi selezionare **Elimina**.

1. Nella barra del titolo selezionare l'icona **Notifiche** e attendere l'eliminazione di **vm1**.

1. Dovrebbe essere stato ricevuto un messaggio di posta elettronica di notifica con un messaggio simile al seguente: **Avviso importante: è stato attivato l'avviso di Monitoraggio di Azure sull'eliminazione della macchina virtuale...** In caso contrario, aprire il programma di posta elettronica e cercare un messaggio di posta elettronica da azure-noreply@microsoft.com.

    ![Screenshot del messaggio di posta elettronica di avviso.](../media/az104-lab11-alert-email.png)

1. Nel menu delle risorse del portale di Azure selezionare **Monitoraggio** e quindi selezionare **Avvisi** nel menu a sinistra.

1. Dovrebbero essere presenti tre avvisi dettagliati generati dopo l’eliminazione di **vm1**.

1. Selezionare il nome di uno degli avvisi, ad esempio **Macchina virtuale eliminata**. Viene visualizzato un riquadro **Dettagli avviso** che mostra altri dettagli sull'evento.

## Attività 5: Aggiungere una regola di avviso

Verrà programmata una manutenzione pianificata una tantum, durante la notte. Inizia la sera e continua fino al mattino successivo.

1. Nel menu delle risorse del portale di Azure selezionare **Monitoraggio**, selezionare **Avvisi** nel menu a sinistra e selezionare **Regole di elaborazione degli avvisi** nella barra dei menu.
   
1. Seleziona **+ Crea**.
   
1. Selezionare la casella corrispondente al gruppo di risorse sandbox come ambito della regola di elaborazione degli avvisi e quindi selezionare **Applica**.
   
1. Selezionare **Avanti: Impostazioni regola**, quindi selezionare **Elimina notifiche**.
   
1. Al termine, selezionare **Avanti: Pianificazione**.
   
1. Per impostazione predefinita, la regola funziona sempre, a meno che non venga disabilitata. Verrà definita la regola per eliminare le notifiche per una manutenzione pianificata notturna una tantum.
Immettere queste impostazioni per la pianificazione della regola di elaborazione degli avvisi:

    | Impostazione | Valore |
    |---------|---------|
    |Applicare la regola |A un'ora specifica|
    |Inizio|Immettere la data odierna alle 22:00.|
    |Fine|Immettere la data di domani alle 7:00.|
    |Time zone|Selezionare il fuso orario locale.|

    ![Screenshot della sezione pianificazione di una regola di elaborazione degli avvisi](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Selezionare **Avanti: Dettagli** e immettere le impostazioni seguenti:

    | Impostazione | Valore |
    |---------|---------|
    |Gruppo di risorse |Selezionare il gruppo di risorse sandbox. |
    |Nome regola|**Manutenzione pianificata**|
    |Descrizione|**Eliminare le notifiche durante la manutenzione pianificata.**|

1. Selezionare **Rivedi e crea** per convalidare l'input e quindi selezionare **Crea**.

## Attività 6: Usare query di log di Monitoraggio di Azure

In questa attività si userà Monitoraggio di Azure per eseguire query sui dati acquisiti dalla macchina virtuale.

1. Nella portale di Azure cercare e selezionare `Monitor` il pannello Fare clic su **Log**.

    >**Nota**: potrebbe essere necessario fare clic su **Attività iniziali** se è la prima volta che si accede a Log Analytics. Se viene ancora visualizzato un **pulsante Abilita** , attendere il completamento della distribuzione precedente.

1. Se necessario, fare clic su **Seleziona ambito**, nel pannello **Selezionare un ambito** espandere la sottoscrizione, espandere il gruppo **di risorse az104-rg1**, quindi selezionare **az104-vm0** e fare clic su **Applica**.

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

    > **Nota**: la query non deve contenere errori (indicati da blocchi rossi sulla barra di scorrimento destra). Se la query non verrà incollata senza errori, incollare il codice della query in un editor di testo, ad esempio Blocco note, quindi copiarlo e incollarlo nella finestra di query da questa posizione.


1. Fare clic su **Query** sulla barra degli strumenti, 

    >**Nota**: a seconda della risoluzione dello schermo, **le query** potrebbero essere nascoste dietro un'elipses.

1. Cancellare eventuali filtri esistenti. Usando la ricerca di query, cercare `Track VM Availability using Heartbeat` e quindi selezionare **Esegui**.

1. Selezionare la **scheda Risultati** della query ed esaminare i risultati della query.

## Esaminare i punti principali del lab

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Gli avvisi consentono di rilevare e risolvere i problemi prima che gli utenti notino che si è verificato un problema con l'infrastruttura o l'applicazione.

+ È possibile creare avvisi su qualsiasi metrica o fonte di dati di log nella piattaforma di dati di Monitoraggio di Azure.

+ Una regola di avviso monitora i dati e acquisisce un segnale che indica che sta accadendo qualcosa nella risorsa specificata.

+ Se vengono soddisfatte le condizioni della regola di avviso, viene attivato un avviso. Diverse azioni (posta elettronica, SMS, push, voce) possono essere avviate e inviate a un gruppo di azioni. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.

+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.

+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.

