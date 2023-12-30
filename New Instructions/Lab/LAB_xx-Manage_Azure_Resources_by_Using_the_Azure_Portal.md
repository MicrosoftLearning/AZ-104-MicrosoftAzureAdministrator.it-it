---
lab:
  title: 'Lab 03a: Gestire le risorse di Azure usando il portale di Azure'
  module: Administer Azure Resources
---

# Lab 03a - Gestire le risorse di Azure usando il portale di Azure
# Manuale del lab per gli studenti

## Scenario laboratorio

È necessario esplorare le funzionalità amministrative di base di Azure associate al provisioning delle risorse e organizzarle.  Si vogliono comprendere i metodi più comuni in Azure per organizzare le risorse. Si vuole anche comprendere come spostare le risorse. Infine, si vuole testare la protezione delle risorse del disco dall'eliminazione accidentale, consentendo comunque di modificare le caratteristiche e le dimensioni delle prestazioni.

**Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi. 

## Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Creare gruppi di risorse, creare risorse e distribuire risorse in gruppi di risorse
+ Attività 2: Spostare le risorse tra gruppi di risorse
+ Attività 3: Implementare e testare i blocchi delle risorse

## Tempo stimato: 30 minuti

## Diagramma dell'architettura

![Architettura del lab diagramma 03a](../media/az104-lab03a-architecture-diagram.png)

### Istruzioni

## Esercizio 1

## Attività 1: Creare gruppi di risorse e distribuire risorse al loro interno

In questa attività si userà il portale di Azure per creare un disco gestito. Nell'attività successiva questo disco verrà spostato in un altro gruppo di risorse. 

1. Accedere al [**portale di Azure**](http://portal.azure.com).

1. Nel portale di Azure cercare e selezionare **Dischi**, fare clic su **+ Crea** e specificare le impostazioni seguenti:

    |Impostazione|Valore|
    |---|---|
    |Subscription| nome della sottoscrizione di Azure  |
    |Gruppo di risorse| selezionare **Crea nuovo** gruppo di risorse - `az104-rg3a`|
    |Disk name| `az104-disk1` |
    |Area| **(Stati Uniti) Stati Uniti orientali** |
    |Zona di disponibilità| **La ridondanza dell'infrastruttura non è richiesta** |
    |Source type| **Nessuno** |

1. Selezionare **Cambia dimensione** e scegliere **HDD** Standard e **32 GiB**. Selezionare **OK** al termine. 

    ![Screenshot della pagina crea un disco.](../media/az104-lab03a-createdisk1.png)

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

    >**Nota**: attendere che il disco venga creato. L'operazione dovrebbe richiedere meno di un minuto.

## Attività 2: Spostare le risorse tra gruppi di risorse 

In questa attività la risorsa disco creata nell'attività precedente verrà spostata in un nuovo gruppo di risorse. Le singole risorse, ad esempio i dischi, possono essere inserite solo in un singolo gruppo di risorse. Spesso, le organizzazioni applicano il controllo degli accessi in base al ruolo, l'assegnazione di tag o Criteri di Azure a livello di gruppo di risorse. Potrebbe essere necessario spostare una risorsa in un gruppo di risorse diverso se la risorsa viene riallocata a un altro progetto, se si vuole sfruttare la configurazione esistente di un gruppo di risorse (ad esempio, il controllo degli accessi in base al ruolo e l'assegnazione di tag sono già configurati) o se si prevede di rimuovere il gruppo di risorse originale.

1. Cerca e seleziona **Gruppi di risorse**. 

1. **Nel pannello Gruppi** di risorse fare clic sulla voce che rappresenta il **gruppo di risorse az104-rg3a** creato nell'attività precedente.

1. Nel pannello **Panoramica** del gruppo di risorse selezionare la voce che rappresenta il disco appena creato nell'elenco delle risorse del gruppo di risorse.

1. Usare i puntini di sospensione della barra degli strumenti (...) per selezionare **Sposta** e quindi nell'elenco a discesa selezionare **Sposta in un altro gruppo** di risorse. Si notino le scelte per passare a un'altra sottoscrizione o area geografica. 

    ![Screenshot per spostare la risorsa.](../media/az104-lab03a-moverg.png)

1. Sotto la **casella di testo Gruppo** di risorse fare clic su **Crea nuovo** e quindi digitare `az104-rg3move` nella casella di testo. Selezionare **OK** per creare il nuovo gruppo di risorse.

1. **Nella scheda Risorse da spostare** verificare che **Succeeded** sia lo stato di convalida. Il completamento della convalida potrebbe richiedere un minuto. Nel frattempo, esaminare l'elenco [delle operazioni](https://learn.microsoft.com/azure/azure-resource-manager/management/move-support-resources) di spostamento supportate. Selezionare **Avanti** per continuare. 

1. Nella scheda Revisione selezionare la casella di controllo **Dichiaro di aver compreso che gli strumenti e gli script associati alle risorse spostate non funzioneranno fino a quando non li aggiornerò con i nuovi ID di risorsa**, quindi fare clic su **Sposta**.

    >**Nota**: non attendere il completamento dello spostamento, ma procedere con l'attività successiva. Lo spostamento potrebbe richiedere circa 10 minuti. Per determinare se l'operazione è stata completata, è possibile monitorare le voci del log attività del gruppo di risorse di origine o di destinazione. Rivedere questo passaggio dopo aver completato l'attività successiva.

## Attività 3: Implementare blocchi delle risorse

In questa attività si applicherà un blocco a un gruppo di risorse di Azure contenente una risorsa disco. Un blocco di risorse può impedire qualsiasi tipo di modifiche a una risorsa, ad esempio le dimensioni di un disco. In alternativa, un blocco delle risorse può impedire eliminazioni accidentali. Questi due tipi di blocco sono *i blocchi Delete* e *i blocchi di sola* lettura.

### Creare un nuovo disco gestito. 

1. Nel portale di Azure cercare e selezionare **Dischi**, fare clic su **+ Crea** e specificare le impostazioni seguenti:

    |Impostazione|Valore|
    |---|---|
    |Subscription| Il nome della sottoscrizione usata in questo lab |
    |Gruppo di risorse| Fare clic su **Crea nuovo** gruppo di risorse e denominarlo `az104-rg3` |
    |Disk name| `az104-disk2` |
    |Area| Il nome dell'area di Azure in cui sono stati creati gli altri gruppi di risorse di questo lab |
    |Zona di disponibilità| **La ridondanza dell'infrastruttura non è richiesta** |
    |Source type| **Nessuno** |

1. Selezionare **Cambia dimensione** e scegliere **HDD** Standard e **32 GiB**. Selezionare **OK** al termine. 

    ![Screenshot dell'impostazione del tipo e delle dimensioni del disco.](,./media/az104-lab03a-createdisk1.png)

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

1. Attendere la distribuzione del disco, quindi selezionare **Vai alla risorsa**.

### Bloccare il gruppo di risorse in modo che le risorse non possano essere deselezionate. 

1. Nel pannello **Panoramica** selezionare il gruppo di risorse.

1. **Nella sezione Impostazioni** selezionare **Blocchi** e quindi + **Aggiungi**. Specificare le impostazioni seguenti e quindi selezionare **OK**. 

    |Impostazione|Valore|
    |---|---|
    |Nome del blocco| `az104-rglock` |
    |Tipo di blocco| **CANC** |

    ![Screenshot della schermata di blocco delle risorse.](../media/az104-lab03a-deletelock.png)

1. Nel pannello Panoramica del gruppo **di risorse selezionare il disco gestito e quindi selezionare **Elimina** sulla barra degli** strumenti. 

1. Nella **pagina Elimina risorse** digitare e fare clic su **Elimina** nella **casella `delete` di testo Conferma eliminazione**. Confermare la **conferma dell'eliminazione**. 

1. Verrà visualizzato un messaggio di errore che informa che l'operazione di eliminazione non è riuscita.

    >**Nota**: questo è previsto. Il messaggio indica che è presente un blocco nel gruppo di risorse. 

    ![Screenshot del messaggio di errore.](../media/az104-lab03a-deleteerror.png)

1. Selezionare la **risorsa az104-disk2** . 

1. **Nella sezione Impostazioni** fare clic su **Dimensioni e prestazioni**. Passare a **SSD** Premium e **64 GiB**. Fare clic su **Salva** per applicare la modifica.

1. Nel pannello **Panoramica** verificare che sia stato possibile modificare le dimensioni del disco. 

    >**Nota**: questo è previsto. Il blocco a livello di gruppo di risorse si applica solo alle operazioni di eliminazione. È possibile modificare le impostazioni delle risorse. 

## Revisione

Complimenti. È stato creato correttamente un gruppo di risorse e una risorsa, la risorsa è stata spostata in un altro gruppo e sono stati implementati blocchi di risorse.
