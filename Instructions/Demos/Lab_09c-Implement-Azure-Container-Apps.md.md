---
lab:
  title: 'Lab 09c: Implementare app di Azure Container'
  module: Administer PaaS Compute Options
---

# Lab 09c: Implementare app di Azure Container
# Manuale del lab per gli studenti

## Scenario laboratorio
App Contenitore di Azure consente di eseguire microservizi e applicazioni in contenitori in una piattaforma serverless. Con App contenitore si possono sfruttare i vantaggi dell'esecuzione di contenitori, senza doversi preoccupare della configurazione manuale dell'infrastruttura del cloud e di agenti di orchestrazione complessi.

## Obiettivi

In questo lab si eseguiranno le attività seguenti:
- Attività 1: Creare un'app e un ambiente contenitore
- Attività 2: Distribuire l'app contenitore
- Attività 3: Testare e verificare la distribuzione dell'app contenitore

Per iniziare, accedere al [portale di Azure](https://portal.azure.com).

## Tempo stimato: 20 minuti

## Attività 1: Creare un'app e un ambiente contenitore

Per creare l'app contenitore, iniziare dalla home page portale di Azure.

1. `Container Apps` Cercare nella barra di ricerca superiore.
1. Selezionare **App** contenitore nei risultati della ricerca.
1. Selezionare il pulsante **Crea**.

### Scheda Informazioni di base

*Nella scheda Informazioni di* base eseguire le azioni seguenti.

1. Immettere i valori seguenti nella *sezione Dettagli* progetto.

    | Impostazione | Azione |
    |---|---|
    | Subscription | Seleziona la tua sottoscrizione di Azure. |
    | Resource group | Selezionare **Crea nuovo** e immettere `az104-09c-rg1`. |
    | Nome dell'app contenitore |  Immetti `my-container-app`. |

#### Crea un ambiente

Creare quindi un ambiente per l'app contenitore.

1. Selezionare l'area appropriata.

    | Impostazione | Valore |
    |--|--|
    | Area | **La tua scelta**. |

1. *Nel campo Crea ambiente* app contenitore selezionare il **collegamento Crea nuovo**.
1. *Nella pagina Crea ambiente* app contenitore nella *scheda Informazioni di base* immettere i valori seguenti:

    | Impostazione | Valore |
    |--|--|
    | Nome ambiente | Immetti `my-environment`. |
    | Ridondanza della zona | selezionare **Disabilitato** |

1. Selezionare la **scheda Monitoraggio** per creare un'area di lavoro Log Analytics.
1. Selezionare il **collegamento Crea nuovo** nel *campo Area di lavoro* Log Analytics e immettere i valori seguenti.

    | Impostazione | valore |
    |--|--|
    | Nome | Immetti `my-container-apps-logs` |
  
    Il *campo Località* è precompilato con l'area geografica.

1. Seleziona **OK**.


## Attività 2: Distribuire l'app contenitore

1. Selezionare il **pulsante Rivedi e crea** nella parte inferiore della pagina.  

    Successivamente, le impostazioni nell'app contenitore vengono verificate. Se non vengono rilevati errori, il *pulsante Crea* è abilitato.  

    In caso di errori, qualsiasi scheda contenente errori viene contrassegnata con un punto rosso.  Passare alla scheda appropriata.  I campi contenenti un errore verranno evidenziati in rosso.  Dopo aver corretto tutti gli errori, selezionare **Rivedi e crea** di nuovo.

1. Seleziona **Crea**.

    Viene visualizzata una pagina con il messaggio *Distribuzione in corso* .  Al termine della distribuzione, verrà visualizzato il messaggio: *La distribuzione è stata completata*.
   
## Attività 3: Testare e verificare la distribuzione dell'app contenitore

Selezionare Vai alla risorsa** per visualizzare **la nuova app contenitore.  Selezionare il collegamento accanto a *URL* applicazione per visualizzare l'applicazione. Verificare di avere un *messaggio di benvenuto in App* Azure Container.

## Pulire le risorse

Se non si intende continuare a usare questa applicazione, è possibile eliminare l'istanza di App Azure Container e tutti i servizi associati rimuovendo il gruppo di risorse.

1. Selezionare il **gruppo di risorse my-container-apps** nella *sezione Panoramica* .
1. Selezionare il **pulsante Elimina gruppo** di risorse nella parte superiore della panoramica* del gruppo *di risorse.
1. Immettere il nome del gruppo di risorse e confermare l'eliminazione dell'app. 
1. Selezionare **Elimina**.
1. Il completamento del processo per eliminare il gruppo di risorse può richiedere alcuni minuti.
