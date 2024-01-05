# Lab 04 - Implementare la rete virtuale

## Introduzione al lab

Questo lab è il primo di tre lab incentrati sulla rete virtuale. In questo lab vengono illustrate le nozioni di base della rete virtuale e della subnet. Si apprenderà anche come proteggere la rete con gruppi di sicurezza di rete e gruppi di sicurezza delle applicazioni. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti** orientali ed **Europa** occidentale. 

## Tempo stimato: 40 minuti

## Scenario laboratorio 

L'organizzazione globale prevede di implementare reti virtuali. Queste reti si trovano negli Stati Uniti orientali, nell'Europa occidentale e nell'Asia sud-orientale. L'obiettivo immediato è quello di contenere tutte le risorse esistenti. Tuttavia, l'organizzazione è in una fase di crescita e vuole garantire una capacità aggiuntiva per la crescita.

La rete virtuale **CoreServicesVnet** viene distribuita nell'area **Stati Uniti orientali**. Questa rete virtuale ha il maggior numero di risorse. La rete ha connettività alle reti locali tramite una connessione VPN. Questa rete include servizi Web, database e altri sistemi chiave per le operazioni dell'azienda. I servizi condivisi, ad esempio controller di dominio e DNS, si trovano qui. È prevista una forte crescita, quindi è necessario uno spazio degli indirizzi di grandi dimensioni per questa rete virtuale.

La rete virtuale **ManufacturingVnet** viene distribuita nell'area **Europa occidentale**, vicino a dove si trovano gli impianti di produzione dell'organizzazione. Questa rete virtuale contiene sistemi per le operazioni delle strutture di produzione. L'organizzazione prevede un numero elevato di dispositivi connessi interni per i propri sistemi per recuperare i dati da, ad esempio la temperatura, e richiede uno spazio indirizzi IP in cui può espandersi.

## Simulazioni di lab interattive

Esistono diverse simulazioni di lab interattive che potrebbero risultare utili per questo argomento. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure. 

+ [Proteggere il traffico](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013) di rete. Creare una macchina virtuale, una rete virtuale e un gruppo di sicurezza di rete. Aggiungere regole del gruppo di sicurezza di rete per consentire e impedire il traffico.
  
+ [Creare una semplice rete](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204) virtuale. Creare reti virtuali con due macchine virtuali. Illustrare le macchine virtuali in grado di comunicare. 

+ [Progettare e implementare una rete virtuale in Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure). Creare un gruppo di risorse e creare reti virtuali con subnet.  

+ [Implementare la rete](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208) virtuale. Creare e configurare una rete virtuale, distribuire macchine virtuali, configurare i gruppi di sicurezza di rete e configurare DNS di Azure.

## Diagramma dell'architettura

![Layout di rete](../media/az104-lab04-arch-diagram.png)

Queste reti virtuali e subnet sono strutturate in modo da supportare le risorse esistenti, ma consentono la crescita prevista. Verranno ora create le reti virtuali e le subnet che saranno alla base dell'infrastruttura di rete.

>**Lo sapevi?**: è consigliabile evitare intervalli di indirizzi IP sovrapposti per ridurre i problemi e semplificare la risoluzione dei problemi. La sovrapposizione è un problema nell'intera rete, sia nel cloud che in locale. Molte organizzazioni progettano uno schema di indirizzamento IP a livello aziendale per evitare sovrapposizioni e pianificare la crescita futura.

## Attività

+ Attività 1: Creare un gruppo di risorse.
+ Attività 2: Creare la rete virtuale CoreServicesVnet e le subnet.
+ Attività 3: Creare la rete virtuale ManufacturingVnet e le subnet.
+ Attività 4: Configurare la comunicazione tra un gruppo di sicurezza delle applicazioni e un gruppo di sicurezza di rete. 


## Attività 1: Creare un gruppo di risorse

### Creare un gruppo di risorse per tutte le risorse in questo lab. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Cercare e selezionare **Gruppi** di risorse, quindi selezionare **+ Crea**.  

1. Creare il gruppo di risorse con queste impostazioni. 

    | **Tab**         | **Opzione**                                 | **valore**            |
    | --------------- | ------------------------------------------ | -------------------- |
    | Informazioni di base          | Gruppo di risorse                             | `az104-rg4` |
    |                 | Area                                     | (Stati Uniti) **Stati Uniti orientali**     |
    | Tag            | Nessuna modifica necessaria                        |                      |
   
1. Al termine, selezionare **Rivedi e crea** e quindi **Crea**.
   
## Attività 2: Creare la rete virtuale CoreServicesVnet e le subnet

L'organizzazione pianifica una grande crescita per i servizi di base. In questa attività si creano la rete virtuale e le subnet associate per supportare le risorse esistenti e la crescita pianificata.

1. Cercare e selezionare **Rete virtuale.**

1. Selezionare **Crea** nella pagina Reti virtuali e completare gli **** indirizzi** Basics e **IPv4. 

1. Usare le informazioni contenute nella tabella seguente per creare la rete virtuale CoreServicesVnet.  

    | **Tab**      | **Opzione**         | **valore**            |
    | ------------ | ------------------ | -------------------- |
    | Informazioni di base       | Gruppo di risorse     | **az104-rg4** |
    |              | Nome               | `CoreServicesVnet`     |
    |              | Area             | (Stati Uniti) **Stati Uniti orientali**         |
    | Indirizzi IP | Spazio indirizzi IPv4 | `10.20.0.0/16` (Eliminare o sovrascrivere lo spazio indirizzi IP)     |


1. Nell'area subnet eliminare la **subnet predefinita** .

1. Selezionare **+ Aggiungi una subnet**. Completare le informazioni sul nome e sull'indirizzo per ogni subnet. Assicurarsi di selezionare **Aggiungi** per ogni nuova subnet. 

    | **Subnet**             | **Opzione**           | **valore**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nome subnet          | `SharedServicesSubnet`   |
    |                        | Indirizzo iniziale     | `10.20.10.0`          |
    |                        | Dimensione                 | `/24` |
    | DatabaseSubnet         | Nome subnet          | `DatabaseSubnet`         |
    |                        | Indirizzo iniziale     | `10.20.20.0`        |
    |                        | Dimensione                 | `/24` |

1. Per completare la creazione di CoreServicesVnet e delle subnet associate, selezionare **Rivedi e crea**.

1. Verificare che la configurazione abbia superato la convalida e quindi selezionare **Crea**.

1. Attendere che la rete virtuale venga distribuita e quindi selezionare **Vai alla risorsa**. 

1. **Nella sezione Automazione** selezionare **Esporta modello** e quindi attendere che venga generato il modello.

1. **Scaricare** il modello.

1. Passare al computer locale alla **cartella Download** ed **Estrarre tutti i** file nel file ZIP scaricato. 

1. Prima di procedere, assicurarsi di avere due file **template.json** e **parameters.json**. Esaminare i file e le informazioni su CoreServicesVnet. Questo modello verrà usato per creare ManufacturingVnet nell'attività successiva. 
 
## Attività 3: Creare la rete virtuale ManufacturingVnet e le subnet

In questa attività si creano la rete virtuale ManufacturingVnet e le subnet associate. L'organizzazione prevede una crescita per gli uffici di produzione, in modo che le subnet siano ridimensionate per la crescita prevista.

1. Individuare il **file template.json** esportato nell'attività precedente. Deve trovarsi nella **cartella Download** .

1. Modificare il file usando l'editor preferito. Se si usa Visual Studio Code, assicurarsi di lavorare in una **finestra** attendibile e non nella **modalità** con restrizioni.

   >**Nota:** per questa attività viene illustrato come modificare e ridistribuire un modello. In caso di confusione, viene fornito il modello finito. 

### Apportare modifiche alla rete virtuale ManufacturingVnet

1. Sostituire tutte le occorrenze di **CoreServicesVnet** con `ManufacturingVnet`. 

1. Sostituire tutte le occorrenze di **eastus** con `westeurope`. 

1. Sostituire tutte le occorrenze di **10.20.0.0/16** con `10.30.0.0/16`. 

### Apportare modifiche alle subnet ManufacturingVnet

1. Modificare tutte le occorrenze di **SharedServicesSubnet** in `SensorSubnet1`.

1. Modificare tutte le occorrenze di **10.20.10.0/24** in `10.30.20.0/24`.

1. Modificare tutte le occorrenze di **DatabaseSubnet** in `SensorSubnet2`.

1. Modificare tutte le occorrenze di **10.20.20.0/24** in `10.30.21.0/24`.

1. Leggere nuovamente il file e assicurarsi che tutto sia corretto.

1. Assicurarsi di **salvare** le modifiche.

### Distribuire il modello personalizzato

1. Nel portale cercare e selezionare **Distribuisci un modello** personalizzato.

1. Selezionare **Compila un modello personalizzato nell'editor** e quindi **Carica file**.

1. Selezionare il **file templates.json** con le modifiche di produzione e quindi selezionare **Salva**.

1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Attendere che il modello venga distribuito, quindi confermare (nel portale) che la rete virtuale Manufacturing è stata creata. 
   
## Attività 4: Configurare la comunicazione tra un gruppo di sicurezza delle applicazioni e un gruppo di sicurezza di rete 

In questa attività vengono creati un gruppo di sicurezza delle applicazioni e un gruppo di sicurezza di rete. Il gruppo di sicurezza di rete avrà una regola di sicurezza in ingresso che consente il traffico dal gruppo di sicurezza di Azure. 

### Creare il gruppo di sicurezza delle applicazioni (ASG)

1. Nella portale di Azure cercare e selezionare **Gruppi** di sicurezza delle applicazioni.

1. Fare clic su **Crea** e specificare le informazioni di base.

    | Impostazione | Valore |
    | -- | -- |
    | Subscription | *sottoscrizione in uso* |
    | Gruppo di risorse | **az104-rg4** |
    | Nome | `asg-web` |
    | Area | **Europa occidentale**  |

1. Fare clic su **Rivedi e crea** e quindi dopo la convalida fare clic su **Crea**.

### Creare il gruppo di sicurezza di rete e associarlo alla subnet asg

1. Nella portale di Azure cercare e selezionare **Gruppi** di sicurezza di rete.

1. Selezionare **+ Crea** e fornire informazioni nella **scheda Informazioni di base** . 

    | Impostazione | Valore |
    | -- | -- |
    | Subscription | *sottoscrizione in uso* |
    | Gruppo di risorse | **az104-rg4** |
    | Nome | `myNSGSecure` |
    | Area | **(Stati Uniti) Stati Uniti orientali**  |

1. Fare clic su **Rivedi e crea** e quindi dopo la convalida fare clic su **Crea**.

1. Dopo aver creato il gruppo di sicurezza di rete, fare clic su **Vai alla risorsa**.

1. In **Impostazioni** fare clic su Subnet** e quindi su ****Associa**.

    | Impostazione | Valore |
    | -- | -- |
    | Rete virtuale | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** |

1. Fare clic su **OK** per salvare l'associazione.

### Configurare una regola di sicurezza in ingresso

1. Nell'area **Impostazioni** selezionare **Regole** di sicurezza in ingresso.

1. Esaminare le regole in ingresso predefinite. Si noti che solo altre reti virtuali e servizi di bilanciamento del carico sono autorizzati ad accedere.

1. Seleziona **+ Aggiungi**.

1. Nel pannello **Aggiungi regola** porta in ingresso usare le informazioni seguenti per aggiungere la regola della porta in ingresso e quindi selezionare **Aggiungi**.

    | Impostazione | Valore |
    | -- | -- |
    | Origine | **qualsiasi** |
    | Intervalli porte di origine |  ***** |
    | Destinazione | **Gruppo di sicurezza delle applicazioni** |
    | Gruppi di sicurezza delle applicazioni di destinazione | **asg-web** |
    | Service | **Personalizzato** (si notino le altre opzioni) |
    | Intervalli porte di destinazione | **80,443** |
    | Protocollo | **TCP** |
    | Azione | **Consenti** |
    | Priorità | **100** |
    | Nome | **AllowASG** |

1. Dopo aver creato la regola del gruppo di sicurezza di rete, esaminare le regole** di sicurezza in uscita predefinite**.

## Punti chiave

Congratulazioni per il completamento del lab. Ecco le principali considerazioni per questo lab. 

+ Una rete virtuale è una rappresentazione della rete dell'utente nel cloud. 
+ Quando si progettano reti virtuali, è consigliabile evitare intervalli di indirizzi IP sovrapposti. In questo modo si riducono i problemi e si semplifica la risoluzione dei problemi.
+ Una subnet è un intervallo di indirizzi IP all'interno della rete virtuale. È possibile dividere la rete virtuale in più subnet per motivi di organizzazione e sicurezza.
+ Un gruppo di sicurezza di rete contiene regole di sicurezza che consentono o negano il traffico di rete. Esistono regole predefinite in ingresso e in uscita che è possibile personalizzare in base alle proprie esigenze.
+ I gruppi di sicurezza delle applicazioni vengono usati per proteggere i gruppi di server con una funzione comune, ad esempio server Web o server di database. 

## Pulire le risorse

Se si usa la propria sottoscrizione, è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.
