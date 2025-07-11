---
lab:
  title: 'Lab 04: Implementare la rete virtuale'
  module: Implement Virtual Networking
---

# Lab 04 - Implementare la rete virtuale

## Introduzione al lab

Questo lab è il primo di tre lab incentrati sulla rete virtuale. In questo lab vengono illustrate le nozioni di base della rete virtuale e della subnet. Si apprenderà come proteggere la rete con gruppi di sicurezza di rete e gruppi di sicurezza delle applicazioni. Vengono inoltre fornite informazioni sulle zone e i record DNS. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti orientali**.

## Tempo stimato: 50 minuti

## Scenario laboratorio 

L'organizzazione globale prevede di implementare reti virtuali. L'obiettivo immediato è quello di contenere tutte le risorse esistenti. Tuttavia, l'organizzazione è in una fase di crescita e vuole garantire una capacità aggiuntiva per la crescita.

La **rete virtuale CoreServicesVnet** ha il maggior numero di risorse. È prevista una forte crescita, quindi è necessario uno spazio degli indirizzi di grandi dimensioni per questa rete virtuale.

La rete virtuale **ManufacturingVnet** contiene sistemi per le operazioni degli impianti di produzione. L'organizzazione prevede un numero elevato di dispositivi connessi interni per i sistemi da cui recuperare i dati. 

## Simulazioni interattive del lab

>**Nota**: le simulazioni lab fornite in precedenza sono state ritirate.

## Diagramma dell'architettura

![Layout di rete](../media/az104-lab04-architecture.png)

Queste reti virtuali e subnet sono strutturate in modo da supportare le risorse esistenti, consentendo tuttavia la crescita prevista. Verranno ora create le reti virtuali e le subnet che saranno alla base dell'infrastruttura di rete.

>**Suggerimenti utili**: È consigliabile evitare intervalli di indirizzi IP sovrapposti per ridurre i problemi e semplificare la risoluzione dei problemi. La sovrapposizione è un problema nell'intera rete, sia nel cloud che in locale. Molte organizzazioni progettano uno schema di indirizzamento IP a livello aziendale per evitare sovrapposizioni e pianificare la crescita futura.

## Competenze mansione

+ Attività 1: Creare una rete virtuale con subnet usando il portale.
+ Attività 2: Creare una rete virtuale e subnet usando un modello.
+ Attività 3: Creare e configurare la comunicazione tra un gruppo di sicurezza delle applicazioni e un gruppo di sicurezza di rete.
+ Attività 4: Configurare zone DNS di Azure pubbliche o private.
  
## Attività 1: Creare una rete virtuale con subnet usando il portale

L'organizzazione pianifica una grande crescita per i servizi di base. In questa attività si creano la rete virtuale e le subnet associate per supportare le risorse esistenti e la crescita pianificata. In questa attività si userà il portale di Azure. 

1. Accedere al **portale di Azure** - `https://portal.azure.com`.
   
1. Cercare e selezionare `Virtual Networks`.

1. Selezionare **Crea** nella pagina Reti virtuali.

1. Completare la scheda **Informazioni di base** per CoreServicesVnet.  

    |  **Opzione**         | **valore**            |
    | ------------------ | -------------------- |
    | Gruppo di risorse     | `az104-rg4` (se necessario, crearne uno nuovo) |
    | Nome               | `CoreServicesVnet`     |
    | Paese             | (Stati Uniti) **Stati Uniti orientali**         |

1. Passare alla scheda **Indirizzi IP**.

    |  **Opzione**         | **valore**            |
    | ------------------ | -------------------- |
    | Spazio indirizzi IPv4 | Sostituire lo spazio indirizzi IPv4 prepopolato con `10.20.0.0/16` (separare le voci)  |

1. Selezionare **+ Aggiungi una subnet**. Completare le informazioni sul nome e sull'indirizzo per ogni subnet. Assicurarsi di selezionare **Aggiungi** per ogni nuova subnet. Assicurarsi di eliminare la subnet predefinita, prima o dopo la creazione delle altre subnet.

    | **Subnet**             | **Opzione**           | **valore**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nome subnet          | `SharedServicesSubnet`   |
    |                        | Indirizzo iniziale     | `10.20.10.0`          |
    |                        | Dimensione                 | `/24` |
    | DatabaseSubnet         | Nome subnet          | `DatabaseSubnet`         |
    |                        | Indirizzo iniziale     | `10.20.20.0`        |
    |                        | Dimensione                 | `/24` |

    >**Nota:** Ogni rete virtuale deve avere almeno una subnet. Promemoria che cinque indirizzi IP saranno sempre riservati, quindi tenerne conto nella pianificazione. 

1. Per completare la creazione di CoreServicesVnet e delle subnet associate, selezionare **Rivedi e crea**.

1. Verificare che la configurazione abbia superato la convalida e quindi selezionare **Crea**.

1. Attendere che venga distribuita la rete virtuale e quindi selezionare **Vai alla risorsa**.

1. Verificare lo **spazio indirizzi** e le **subnet**. Si notino le altre opzioni nel pannello **Impostazioni**. 

1. Nella sezione **Automazione** selezionare **Esporta modello** e quindi attendere che venga generato il modello.

1. **Scaricare** il modello.

1. Passare al computer locale nella cartella **Download** ed **Estrarre tutti** i file nel file ZIP scaricato. 

1. Prima di procedere, assicurarsi di avere il file **template.json**. Questo modello verrà usato per creare ManufacturingVnet nell'attività successiva. 
 
## Attività 2: Creare una rete virtuale e subnet usando un modello

In questa attività si creano la rete virtuale ManufacturingVnet e le subnet associate. L'organizzazione prevede una crescita per gli uffici di produzione, in modo che le subnet siano ridimensionate per la crescita prevista. Per questa attività si usa un modello per creare le risorse. 

1. Individuare il file **template.json** esportato nell'attività precedente. Deve trovarsi nella cartella **Download**.

1. Modificare il file usando l'editor preferito. Molti editor hanno una funzionalità di *modifica di tutte le occorrenze*. Se si usa Visual Studio Code, assicurarsi di lavorare in una **finestra attendibile** e non nella **modalità con restrizioni**. Consultare il diagramma dell'architettura per verificare i dettagli. 

### Apportare modifiche alla rete virtuale ManufacturingVnet

1. Sostituire tutte le occorrenze di **CoreServicesVnet** con `ManufacturingVnet`. 

1. Sostituire tutte le occorrenze di **10.20.0.0** con `10.30.0.0`. 

### Apportare modifiche alle subnet ManufacturingVnet

1. Modificare tutte le occorrenze di **SharedServicesSubnet** in `SensorSubnet1`.

1. Modificare tutte le occorrenze di **10.20.10.0/24** in `10.30.20.0/24`.

1. Modificare tutte le occorrenze di **DatabaseSubnet** in `SensorSubnet2`.

1. Modificare tutte le occorrenze di **10.20.20.0/24** in `10.30.21.0/24`.

1. Leggere nuovamente il file e assicurarsi che tutto sia corretto. Usare il diagramma dell'architettura per i nomi delle risorse e gli indirizzi IP. 

1. Assicurarsi di **salvare** le modifiche.

>**Nota:** Nella directory dei file lab è presente un file modello completato. 

### Apportare modifiche al file dei parametri

1. Individuare il file **parameters.json** esportato nell'attività precedente. Deve trovarsi nella cartella **Download**.

1. Modificare il file usando l'editor preferito.

1. Sostituire l'unica occorrenza di **CoreServicesVnet** con `ManufacturingVnet`.

1. Selezionare **Salva** per salvare le modifiche.
   
### Distribuire il modello personalizzato

1. Nel portale cercare e selezionare `Deploy a custom template`.

1. Selezionare **Compila un modello personalizzato nell'editor** e quindi **Carica file**.

1. Selezionare il **file template.json** con le modifiche di produzione e quindi selezionare **Salva**.

1. Selezionare **Modifica parametri** e quindi **Carica file**.

1. Selezionare il **file parameters.json** con le modifiche di produzione e quindi selezionare **Salva**.

1. Verificare che il gruppo di risorse az104-rg4 **** sia selezionato. 

1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Attendere che il modello venga distribuito, quindi verificare che (nel portale) siano state create la rete virtuale di produzione e le subnet.

>**Nota:** Se è necessario distribuire più di una volta, è possibile che alcune risorse siano state completate correttamente e che la distribuzione abbia esito negativo. È possibile rimuovere manualmente tali risorse e riprovare. 
   
## Attività 3: Creare e configurare la comunicazione tra un gruppo di sicurezza delle applicazioni e un gruppo di sicurezza di rete

In questa attività vengono creati un gruppo di sicurezza delle applicazioni e un gruppo di sicurezza di rete. Il gruppo di sicurezza di rete avrà una regola di sicurezza in ingresso che consente il traffico dal gruppo di sicurezza di Azure. Il gruppo di sicurezza di rete avrà anche una regola in uscita che nega l'accesso a Internet. 

### Creare il gruppo di sicurezza delle applicazioni (ASG)

1. Nel portale di Azure, cercare e selezionare `Application security groups`.

1. Fare clic su **Crea** e specificare le informazioni di base.

    | Impostazione | Valore |
    | -- | -- |
    | Subscription | *sottoscrizione in uso* |
    | Gruppo di risorse | **az104-rg4** |
    | Nome | `asg-web` |
    | Area geografica | **Stati Uniti orientali**  |

1. Fare clic su **Rivedi e crea** e, dopo la convalida, su **Crea**.

>**Nota:** a questo punto, associare il gruppo di sicurezza di azure alle macchine virtuali. Questi computer saranno interessati dalla regola NSG in ingresso creata nell'attività successiva.  

### Creare il gruppo di sicurezza di rete e associarlo a CoreServicesVnet

1. Nel portale di Azure, cercare e selezionare `Network security groups`.

1. Selezionare **+ Crea** e fornire informazioni nella scheda **Informazioni di base**. 

    | Impostazione | Valore |
    | -- | -- |
    | Subscription | *sottoscrizione in uso* |
    | Gruppo di risorse | **az104-rg4** |
    | Nome | `myNSGSecure` |
    | Area geografica | **Stati Uniti orientali**  |

1. Fare clic su **Rivedi e crea** e, dopo la convalida, su **Crea**.

1. Dopo aver distribuito il gruppo di sicurezza di rete, fare clic su **Vai alla risorsa**.

1. In **Impostazioni** fare clic su **Subnet** e quindi su **Associa**.

    | Impostazione | Valore |
    | -- | -- |
    | Rete virtuale | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** |

1. Fare clic su **OK** per salvare l'associazione.

### Configurare una regola di sicurezza in ingresso per consentire il traffico del gruppo di sicurezza del servizio app

1. Continuare a lavorare con il gruppo di sicurezza di rete. Nell'area **Impostazioni** selezionare **Regole di sicurezza in ingresso**.

1. Esaminare le regole in ingresso predefinite. Si noti che solo altre reti virtuali e servizi di bilanciamento del carico sono autorizzati ad accedere.

1. Seleziona **+ Aggiungi**.

1. Nel pannello **Aggiungi regola di sicurezza in ingresso**, usare le informazioni seguenti per aggiungere una regola di porta in ingresso. Questa regola consente il traffico del gruppo di sicurezza di Azure. Al termine, selezionare **Aggiungi**.

    | Impostazione | Valore |
    | -- | -- |
    | Origine | **Gruppo di sicurezza delle applicazioni** |
    | Gruppi di sicurezza delle applicazioni di origine | **asg-web** |
    | Intervalli porte di origine |  * |
    | Destinazione | **Any** |
    | Service | **Personalizzato** (si notino le altre opzioni) |
    | Intervalli porte di destinazione | **80,443** |
    | Protocollo | **TCP** |
    | Azione | **Consenti** |
    | Priorità | **100** |
    | Nome | `AllowASG` |

### Configurare una regola del gruppo di sicurezza di rete in uscita che nega l'accesso a Internet

1. Dopo aver creato la regola del gruppo di sicurezza di rete in ingresso, selezionare **Regole di sicurezza in uscita**. 

1. Si noti la regola **AllowInternetOutboundRule**. Si noti anche che la regola non può essere eliminata e la priorità è 65001.

1. Selezionare **+ Aggiungi** e quindi configurare una regola in uscita che nega l'accesso a Internet. Al termine, selezionare **Aggiungi**.

    | Impostazione | Valore |
    | -- | -- |
    | Origine | **Any** |
    | Intervalli porte di origine |  * |
    | Destinazione | **Tag di servizio** |
    | Tag del servizio di destinazione | **Internet** |
    | Servizioo | **Personalizzazione** |
    | Intervalli porte di destinazione | **8080** |
    | Protocollo | **Any** |
    | Azione | **Nega** |
    | Priorità | **4096** |
    | Nome | **DenyAnyCustom8080Outbound** |


## Attività 4: Configurare zone DNS di Azure pubbliche e private

In questa attività verranno create e configurate zone DNS pubbliche e private. 

### Configurare una zona DNS pubblica

È possibile configurare DNS di Azure per la risoluzione dei nomi host nel dominio pubblico. Ad esempio, se è stato acquistato il nome di dominio contoso.xyz da un registrar di nomi di dominio, è possibile configurare DNS di Azure per ospitare il dominio `contoso.com` e risolvere www.contoso.xyz all'indirizzo IP del server Web o dell'app Web.

1. Nel portale cercare e selezionare `DNS zones`.

1. Seleziona **+ Crea**.

1. Configurare la scheda **Informazioni di base**.

    | Proprietà | valore    |
    |:---------|:---------|
    | Subscription | **Selezionare la sottoscrizione** |
    | Gruppo di risorse | **az-104-rg4** |
    | Nome | `contoso.com` (se riservato regolare il nome) |
    | Paese |**Stati Uniti orientali** (esaminare l'icona informativa) |

1. Selezionare **Rivedi e crea** e quindi **Crea**.
   
1. Attendere che la zona DNS venga distribuita e quindi selezionare **Vai alla risorsa**.

1. Nel pannello **Panoramica** si notino i nomi dei quattro server dei nomi DNS di Azure assegnati alla zona. **Copiare** uno degli indirizzi del server dei nomi. Sarà necessaria in un passaggio successivo. 
  
1. Espandere il pannello **Gestione** DNS e selezionare **Recordset**. Fare clic su **+Aggiungi**. 

    | Proprietà | valore    |
    |:---------|:---------|
    | Nome | **www** |
    | Type | **A** |
    | TTL | **1** |
    | Indirizzo IP | **10.1.1.4** |

>**Nota:**  In uno scenario reale immettere l'indirizzo IP pubblico del server Web.

1. Selezionare **Aggiungi** e verificare che il dominio abbia un set di record A denominato **www**.

1. Aprire un prompt dei comandi ed eseguire il comando seguente. Se il nome di dominio è stato modificato, apportare una modifica. 

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. Verificare che il nome host www.contoso.com venga risolto nell'indirizzo IP specificato. Ciò conferma che la risoluzione dei nomi funziona correttamente.

### Configura una zona DNS privato

Una zona DNS privata fornisce servizi di risoluzione dei nomi all'interno delle reti virtuali. Una zona DNS privata è accessibile solo dalle reti virtuali a cui è collegata e non è possibile accedervi da Internet. 

1. Nel portale cercare e selezionare `Private dns zones`.

1. Seleziona **+ Crea**.

1. Nella scheda **Informazioni di base** di Crea zona DNS privata immettere le informazioni elencate nella tabella seguente:

    | Proprietà | valore    |
    |:---------|:---------|
    | Subscription | **Selezionare la sottoscrizione** |
    | Gruppo di risorse | **az-104-rg4** |
    | Nome | `private.contoso.com` (regolare se è necessario rinominare) |
    | Area geografica |**Stati Uniti orientali** |

1. Selezionare **Rivedi e crea** e quindi **Crea**.
   
1. Attendere che la zona DNS venga distribuita e quindi selezionare **Vai alla risorsa**.

1. Si noti che nel pannello **Panoramica** non sono presenti record del server dei nomi. 

1. Espandere il pannello **Gestione** DNS e quindi selezionare **Collegamenti** di rete virtuale. Configurare il collegamento. 

    | Proprietà | valore    |
    |:---------|:---------|
    | Nome collegamento | `manufacturing-link` |
    | Rete virtuale | `ManufacturingVnet` |

1. Selezionare **Crea** e attendere la creazione del collegamento. 

1. Nel pannello **Gestione** DNS selezionare **+ Recordset**. Ora si aggiunge un record per ogni macchina virtuale che richiede il supporto per la risoluzione dei nomi privata.

    | Proprietà | valore    |
    |:---------|:---------|
    | Nome | **sensorvm** |
    | Type | **A** |
    | TTL | **1** |
    | Indirizzo IP | **10.1.1.4** |

 >**Nota:**  In uno scenario reale immettere l'indirizzo IP per una macchina virtuale di produzione specifica.

## Pulire le risorse

Se si usa la **sottoscrizione personale**, dedicare qualche minuto all’eliminazione delle risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse lab. 

+ Nel portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse**, **Immettere il nome del gruppo di risorse**, quindi fare clic su **Elimina**.
+ Tramite Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usando l’interfaccia della riga di comando, `az group delete --name resourceGroupName`.

## Estendere l'apprendimento con Copilot

Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a *copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.
+ Condividere le prime 10 procedure consigliate per la distribuzione e la configurazione di una rete virtuale in Azure.
+ Come si usano i comandi di Azure PowerShell e dell'interfaccia della riga di comando di Azure per creare una rete virtuale con un indirizzo IP pubblico e una subnet. 
+ Spiegare le regole in ingresso e in uscita del gruppo di sicurezza di rete di Azure e come vengono usate.
+ Qual è la differenza tra i gruppi di sicurezza di rete di Azure e i gruppi di sicurezza delle applicazioni di Azure? Condividere esempi di quando usare ognuno di questi gruppi. 
+ Fornire una guida dettagliata su come risolvere eventuali problemi di rete riscontrati durante la distribuzione di una rete in Azure. Condividere anche il processo di pensiero usato per ogni passaggio della risoluzione dei problemi.

## Altre informazioni con la formazione autogestita

+ [Introduzione alle reti virtuali di Azure](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). Progettare e implementare l'infrastruttura di base di Rete di Azure, ad esempio reti virtuali, indirizzi IP pubblici e privati, DNS, peering di reti virtuali, routing e NAT virtuale di Azure.
+ [Progettare uno schema di indirizzamento IP](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/). Identificare le funzionalità di indirizzamento IP privato e pubblico delle reti virtuali locali e di Azure.
+ [Proteggere e isolare l'accesso alle risorse di Azure usando gruppi di sicurezza di rete ed endpoint di servizio](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/). I gruppi di sicurezza e gli endpoint aiutano a proteggere le macchine virtuali e i servizi di Azure dall'accesso non autorizzato alla rete.
+ [Ospitare il dominio in DNS di Azure](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). Creare una zona DNS per il nome di dominio. Crea record DNS per eseguire il mapping del dominio a un indirizzo IP. Verificare che il nome di dominio venga risolto nel server Web.
  
## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab. 

+ Una rete virtuale è una rappresentazione della rete dell'utente nel cloud. 
+ Quando si progettano reti virtuali, è consigliabile evitare intervalli di indirizzi IP sovrapposti. In questo modo si riducono i problemi e si semplifica la risoluzione dei problemi.
+ Una subnet è un intervallo di indirizzi IP all'interno della rete virtuale. È possibile dividere la rete virtuale in più subnet per motivi di organizzazione e sicurezza.
+ Un gruppo di sicurezza di rete contiene regole di sicurezza che consentono o negano il traffico di rete. Esistono regole predefinite in ingresso e in uscita che è possibile personalizzare in base alle proprie esigenze.
+ I gruppi di sicurezza delle applicazioni vengono usati per proteggere i gruppi di server con una funzione comune, ad esempio server Web o server di database.
+ DNS di Azure è un servizio di hosting per i domini DNS che fornisce la risoluzione dei nomi. È possibile configurare DNS di Azure per la risoluzione dei nomi host nel dominio pubblico.  È anche possibile usare zone DNS private per assegnare nomi DNS alle macchine virtuali nelle reti virtuali di Azure.
