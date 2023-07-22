---
lab:
  title: 'Lab 09c: Implementare app Azure Container'
  module: Administer PaaS Compute Options
---

# Lab 09c: Implementare app Azure Container
# Manuale del lab per gli studenti

## Scenario del lab
App Contenitore di Azure consente di eseguire microservizi e applicazioni in contenitori in una piattaforma serverless. Con Le app per i contenitori si ottengono i vantaggi dell'esecuzione dei contenitori, lasciandosi alle spalle le preoccupazioni di configurare manualmente l'infrastruttura cloud e gli agenti di orchestrazione contenitori complessi.

## Obiettivi

In questo lab si eseguiranno le attività seguenti:
- Attività 1: Creare un'app e un ambiente contenitore
- Attività 2: Distribuire l'app contenitore
- Attività 3: Testare e verificare la distribuzione dell'app contenitore

Iniziare accedendo alla [portale di Azure](https://portal.azure.com).

## Tempo stimato: 20 minuti

## Attività 1: Creare un'app e un ambiente contenitore

Per creare l'app contenitore, iniziare nella home page portale di Azure.

1. `Container Apps` Cercare nella barra di ricerca superiore.
1. Selezionare **App contenitore** nei risultati della ricerca.
1. Selezionare il pulsante **Crea**.

### Scheda Informazioni di base

Nella scheda *Nozioni di base* eseguire le azioni seguenti.

1. Immettere i valori seguenti nella sezione *Dettagli progetto* .

    | Impostazione | Azione |
    |---|---|
    | Subscription | Selezionare la sottoscrizione di Azure. |
    | Resource group | Selezionare **Crea nuovo** e immettere `az104-09c-rg1`. |
    | Nome dell'app contenitore |  Immettere `my-container-app`. |

#### Creare un ambiente

Creare quindi un ambiente per l'app contenitore.

1. Selezionare l'area appropriata.

    | Impostazione | Valore |
    |--|--|
    | Region | **La tua scelta**. |

1. Nel campo *Crea app contenitore* selezionare il collegamento **Crea nuovo** .
1. Nella pagina *Crea ambiente app contenitore* nella scheda *Nozioni di base* immettere i valori seguenti:

    | Impostazione | Valore |
    |--|--|
    | Nome ambiente | Immettere `my-environment`. |
    | Ridondanza della zona | Selezionare **Disabilitato** |

1. Selezionare la scheda **Monitoraggio** per creare un'area di lavoro Log Analytics.
1. Selezionare il **collegamento Crea nuovo** *nell'area di lavoro Log Analytics* e immettere i valori seguenti.

    | Impostazione | Valore |
    |--|--|
    | Nome | Immettere `my-container-apps-logs` |
  
    Il campo *Località* è precompilato con l'area.

1. Selezionare **OK**.


## Attività 2: Distribuire l'app contenitore

1. Selezionare il pulsante **Rivedi e crea** nella parte inferiore della pagina.  

    Le impostazioni nell'app contenitore vengono quindi verificate. Se non vengono trovati errori, il pulsante *Crea* è abilitato.  

    Se si verificano errori, qualsiasi scheda contenente errori è contrassegnata con un punto rosso.  Passare alla scheda appropriata.  I campi contenenti un errore verranno evidenziati in rosso.  Dopo aver corretto tutti gli errori, selezionare **Rivedi e crea** di nuovo.

1. Selezionare **Crea**.

    Viene visualizzata una pagina con il messaggio *Distribuzione in corso* .  Al termine della distribuzione, verrà visualizzato il messaggio: *la distribuzione è stata completata*.
   
## Attività 3: Testare e verificare la distribuzione dell'app contenitore

Selezionare **Vai alla risorsa** per visualizzare la nuova app contenitore.  Selezionare il collegamento accanto *all'URL applicazione* per visualizzare l'applicazione. Verificare di avere un messaggio *Di benvenuto in App Azure Container* .

## Pulire le risorse

Se non si continuerà a usare questa applicazione, è possibile eliminare l'istanza di App Azure Container e tutti i servizi associati rimuovendo il gruppo di risorse.

1. Selezionare il gruppo di risorse **my-container-apps** nella sezione *Panoramica* .
1. Selezionare il pulsante **Elimina gruppo di risorse** nella parte superiore del gruppo di risorse *Panoramica*.
1. Immettere il nome del gruppo di risorse e confermare di voler eliminare l'app. 
1. Selezionare **Elimina**.
1. Il completamento del processo per eliminare il gruppo di risorse può richiedere alcuni minuti.
