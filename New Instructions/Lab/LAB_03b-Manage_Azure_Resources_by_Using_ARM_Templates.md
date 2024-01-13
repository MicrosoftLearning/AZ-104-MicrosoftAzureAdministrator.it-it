---
lab:
  title: 'Lab 03b: Gestire le risorse di Azure usando i modelli di Resource Manager'
  module: Administer Azure Resources
---

# Lab 03b - Gestire le risorse di Azure usando i modelli di ARM
# Manuale del lab per gli studenti

## Scenario laboratorio
Il team ha esaminato le funzionalità amministrative di base di Azure, ad esempio il provisioning delle risorse e l'organizzazione in base ai gruppi di risorse. Il team vuole quindi esaminare i modi per automatizzare e semplificare le distribuzioni. Le organizzazioni spesso guardano all'automazione per ridurre il sovraccarico amministrativo, ridurre l'errore umano o aumentare la coerenza e come modo per consentire agli amministratori di lavorare su attività più complesse o creative.

**Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi. 

## Obiettivi

Contenuto del lab:

+ Attività 1: Creare un modello di Resource Manager per la distribuzione di un disco gestito di Azure
+ Attività 2: Modificare un modello di Resource Manager e quindi creare un disco gestito di Azure usando il modello
+ Attività 3: Esaminare la distribuzione del disco gestito basata sul modello di ARM

## Tempo stimato: 30 minuti

## Diagramma dell'architettura

![image](./media/az104-lab03b-architecture-diagram.png)

### Istruzioni

## Esercizio 1

## Attività 1: Creare un modello di Resource Manager per la distribuzione di un disco gestito di Azure
In questa attività si userà il portale di Azure per generare un modello di Resource Manager. È quindi possibile scaricare il modello da usare nelle distribuzioni future. Un'organizzazione che prevede di distribuire centinaia o migliaia di dischi potrebbe sfruttare uno o più modelli per automatizzare le distribuzioni. In questa attività viene usato il portale di Azure. È anche possibile decompilare il modello in Azure Bicep.

1. Accedere al [**portale di Azure**](http://portal.azure.com).

1. Nella portale di Azure cercare e selezionare `Disks`.

1. Nella pagina Dischi selezionare **Crea**.

1. Nella pagina Crea un disco gestito usare le informazioni seguenti per creare un disco.
    
    | Impostazione | Valore |
    | --- | --- |
    | Subscription | Sottoscrizione in uso | 
    | Gruppo di risorse | `az104-rg1` (Se necessario, selezionare **Crea nuovo**.
    | Disk name | `az104-disk1` | 
    | Area geografica | **Stati Uniti orientali** |
    | Zona di disponibilità | **La ridondanza dell'infrastruttura non è richiesta** | 
    | Source type | **Nessuno** |
    | Dimensione | **32Gb** | 
    | Prestazioni | **Unità disco rigido Standard** |

    ![image](./media/az104-lab03b-createdisk.png)

1. Fare clic su **Rivedi e crea** *una sola volta*. Non **** distribuire effettivamente la risorsa.

1. Nella scheda Rivedi e crea fare clic su **Scarica un modello per l'automazione**.

1. Esaminare le informazioni visualizzate nel modello, inclusi i parametri e le risorse creati.

1. Fare clic su **Scarica** e salvare il modello nel computer.

1. Estrarre il contenuto del file scaricato nella cartella Download** del **computer.

    
1. Chiudere tutte le finestre di **Esplora file**.

1. Nella portale di Azure annullare la distribuzione del disco gestito.

## Attività 2: Modificare un modello di Resource Manager e quindi creare un disco gestito di Azure usando il modello
In questa attività si userà il modello di Resource Manager creato per distribuire un nuovo disco gestito. Questa attività descrive il processo generale di distribuzione basata su modelli, in modo da poter ripetere facilmente e rapidamente le distribuzioni. Se è necessario modificare un parametro o due, è possibile modificare facilmente il modello in futuro.

1. Nella portale di Azure cercare e selezionare `Deploy a custom template`.

1. Nel pannello **Distribuzione personalizzata** fare clic su **Creare un modello personalizzato nell'editor**.

1. Nel pannello **Modifica modello** fare clic su **Carica file** e caricare il file **template.json** scaricato nell'attività precedente.

1. Nel riquadro dell'editor rimuovere le righe seguenti:

   ```json
    "sourceResourceId": {
            "type": "string"
    },
   ```
   
   ```json
      "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    >**Nota**: questi parametri vengono rimossi perché non sono applicabili alla distribuzione corrente. In particolare, i parametri sourceResourceId, sourceUri, osType e hyperVGeneration parameters sono applicabili alla creazione di un disco di Azure da un file VHD esistente.

1. Fare clic su **Salva** per salvare le modifiche.

1. Tornare nel pannello **Distribuzione personalizzata** e fare clic su **Modifica parametri**. 

1. Nel pannello **Modifica modello** fare clic su **Carica file** e caricare il file **parameters.json** scaricato nell'attività precedente, quindi fare clic su **Salva** per salvare le modifiche.

1. Tornare nel pannello **Distribuzione personalizzata** e specificare le impostazioni seguenti:

    | Impostazione | Valore |
    | --- |--- |
    | Subscription | *Nome della sottoscrizione di Azure usata in questo lab* |
    | Gruppo di risorse | `az104-rg1` (Se necessario, selezionare **Crea nuovo**)|
    | Area | nome di qualsiasi area di Azure disponibile nella sottoscrizione in uso in questo lab, in **genere Stati Uniti** orientali. |
    | Nome disco | `az104-disk1` |
    | Ufficio | Il valore del parametro location annotato nell'attività precedente |
    | Sku | **Standard_LRS** |
    | Dimensioni disco (GB) | **32** |
    | Opzione Crea | **empty** |
    | Set di crittografia dischi | **EncryptionAtRestWithPlatformKey** |
    | Modalità autenticazione accesso ai dati | None |
    | Criteri di accesso alla rete | **AllowAll** |
    | Accesso alla rete pubblica | Disabilitata |

    ![image](./media/az104-lab03b-customdeploy.png)

1. Selezionare **Rivedi e crea** e quindi **Crea**.

1. Verificare che la distribuzione sia stata completata correttamente.

## Attività 3: Esaminare la distribuzione del disco gestito basata sul modello di ARM
In questa attività si verifica che la distribuzione sia stata completata correttamente. Tutte le distribuzioni precedenti sono documentate nel gruppo di risorse a cui è stata destinata la distribuzione. Questa revisione mostrerà i dettagli relativi al tempo e alla durata della distribuzione, che possono essere utili per la risoluzione dei problemi. Spesso è consigliabile esaminare le prime distribuzioni basate su modelli per garantire il successo prima di usare i modelli per operazioni su larga scala.

1. Accedere al portale di Azure e selezionare **Gruppi di risorse**. 

1. Nell'elenco dei gruppi di risorse fare clic su **az104-rg1**.

1. **Nel pannello del gruppo di risorse az104-rg1** fare clic su **Distribuzioni** nella **sezione Impostazioni**.

1. **Nel pannello az104-rg1 - Distribuzioni** fare clic sulla prima voce nell'elenco delle distribuzioni ed esaminare il contenuto dei **pannelli Input** e **Modello**.


## Revisione

Complimenti. In questo lab è stato usato il portale di Azure per creare un modello di Resource Manager per un disco gestito e quindi è stato usato il modello per distribuire un nuovo disco.