---
lab:
  title: 'Lab 09d: Distribuire un''app contenitore nel portale'
  module: Administer PaaS Compute Options
---

# Lab 09c: Distribuire un'app contenitore nel portale

App Contenitore di Azure consente di eseguire microservizi e applicazioni in contenitori in una piattaforma serverless. Con Le app contenitore è possibile sfruttare i vantaggi dell'esecuzione dei contenitori, lasciandosi alle spalle le preoccupazioni di configurare manualmente l'infrastruttura cloud e gli agenti di orchestrazione dei contenitori complessi.

In questo lab si crea un ambiente app contenitore sicuro e si distribuisce la prima app contenitore usando il portale di Azure.

Per iniziare, accedere al [portale di Azure](https://portal.azure.com).

## Creare un'app contenitore

Per creare l'app contenitore, iniziare dalla home page portale di Azure.

1. Cercare **App contenitore** nella barra di ricerca superiore.
1. Selezionare **App contenitore** nei risultati della ricerca.
1. Selezionare il pulsante **Crea**.

### Scheda Informazioni di base

Nella scheda *Informazioni di base* eseguire le azioni seguenti.

1. Immettere i valori seguenti nella sezione *Dettagli progetto* .

    | Impostazione | Azione |
    |---|---|
    | Subscription | Selezionare la sottoscrizione di Azure. |
    | Resource group | Selezionare **Crea nuovo** e immettere **my-container-apps**. |
    | Nome dell'app contenitore |  Immettere **my-container-app**. |

#### Creare un ambiente

Creare quindi un ambiente per l'app contenitore.

1. Selezionare l'area appropriata.

    | Impostazione | Valore |
    |--|--|
    | Region | **La tua scelta**. |

1. Nel campo *Crea ambiente app contenitore* selezionare il collegamento **Crea nuovo** .
1. Nella pagina *Crea ambiente app contenitore* nella scheda *Informazioni di base* immettere i valori seguenti:

    | Impostazione | Valore |
    |--|--|
    | Nome ambiente | Immettere **my-environment**. |
    | Ridondanza della zona | Selezionare **Disabilitato** |

1. Selezionare la scheda **Monitoraggio** per creare un'area di lavoro Log Analytics.
1. Selezionare il collegamento **Crea nuovo** nel campo *Area di lavoro Log Analytics* e immettere i valori seguenti.

    | Impostazione | Valore |
    |--|--|
    | Nome | Immettere **my-container-apps-logs**. |
  
    Il campo *Località* è precompilato con *Stati Uniti centrali* .

1. Selezionare **OK**.


### Distribuire l'app contenitore

1. Selezionare il pulsante **Rivedi e crea** nella parte inferiore della pagina.  

    Le impostazioni nell'app contenitore vengono quindi verificate. Se non vengono rilevati errori, il pulsante *Crea* è abilitato.  

    In caso di errori, qualsiasi scheda contenente errori viene contrassegnata con un punto rosso.  Passare alla scheda appropriata.  I campi contenenti un errore verranno evidenziati in rosso.  Dopo aver risolto tutti gli errori, selezionare **Rivedi e crea** di nuovo.

1. Selezionare **Crea**.

    Viene visualizzata una pagina con il messaggio *Distribuzione in corso* .  Al termine della distribuzione, verrà visualizzato il messaggio: *La distribuzione è stata completata*.
   
### Verificare la distribuzione

Selezionare **Vai alla risorsa** per visualizzare la nuova app contenitore.  Selezionare il collegamento accanto a *URL applicazione* per visualizzare l'applicazione. Verificare di avere un messaggio *di benvenuto in App Azure Container* .

## Pulire le risorse

Se non si intende continuare a usare questa applicazione, è possibile eliminare l'istanza di App Contenitore di Azure e tutti i servizi associati rimuovendo il gruppo di risorse.

1. Selezionare il gruppo di **risorse my-container-apps** nella sezione *Panoramica* .
1. Selezionare il pulsante **Elimina gruppo di risorse** nella parte superiore del gruppo di risorse *Panoramica*.
1. Immettere il nome del gruppo di **risorse my-container-apps** nella finestra di dialogo *di conferma "my-container-apps".*
1. Selezionare **Elimina**.
1. Il completamento del processo per eliminare il gruppo di risorse può richiedere alcuni minuti.
