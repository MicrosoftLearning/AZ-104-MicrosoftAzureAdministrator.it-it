---
lab:
  title: 'Lab 04: Implementare Rete virtuale ing'
  module: Administer Virtual Networking
---

# Lab 04 - Implementare la rete virtuale

## Requisiti del lab

Questo lab richiede una sottoscrizione di Azure. È necessario essere in grado di creare reti virtuali e subnet. 

## Tempo stimato: 20 minuti

## Scenario laboratorio 

L'organizzazione globale prevede di implementare reti virtuali. Queste reti si trovano negli Stati Uniti orientali, nell'Europa occidentale e nell'Asia sud-orientale. L'obiettivo immediato è quello di contenere tutte le risorse esistenti. Tuttavia, l'organizzazione è in una fase di crescita e vuole garantire una capacità aggiuntiva per la crescita.

La rete virtuale **CoreServicesVnet** viene distribuita nell'area **Stati Uniti orientali**. Questa rete virtuale ha il maggior numero di risorse. La rete ha connettività alle reti locali tramite una connessione VPN. Questa rete include servizi Web, database e altri sistemi chiave per le operazioni dell'azienda. I servizi condivisi, ad esempio controller di dominio e DNS, si trovano qui. È prevista una forte crescita, quindi è necessario uno spazio degli indirizzi di grandi dimensioni per questa rete virtuale.

La rete virtuale **ManufacturingVnet** viene distribuita nell'area **Europa occidentale**, vicino a dove si trovano gli impianti di produzione dell'organizzazione. Questa rete virtuale contiene sistemi per le operazioni delle strutture di produzione. L'organizzazione prevede un numero elevato di dispositivi connessi interni per i propri sistemi per recuperare i dati da, ad esempio la temperatura, e richiede uno spazio indirizzi IP in cui può espandersi.

La rete virtuale **ResearchVnet** viene distribuita nell'area **Asia sud-orientale**, vicino a dove si trova il team di ricerca e sviluppo dell'organizzazione. Il team di ricerca e sviluppo usa questa rete virtuale. Questo team ha un piccolo set stabile di risorse per cui non è prevista alcuna crescita. Il team necessita di un numero ridotto di indirizzi IP per alcune macchine virtuali usate per svolgere il proprio lavoro.

## Simulazione interattiva del lab

Per questo argomento è disponibile una **[simulazione](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure)** interattiva del lab. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab ospitato, ma i concetti di base e le idee dimostrati sono gli stessi. Non è necessaria una sottoscrizione di Azure. 
## Attività

+ Attività 1: Creare un gruppo di risorse.
+ Attività 2: Creare la rete virtuale CoreServicesVnet e le subnet.
+ Attività 3: Creare la rete virtuale ManufacturingVnet e le subnet.
+ Attività 4: Creare la rete virtuale ResearchVnet e le subnet.
+ Attività 5: Verificare la creazione di reti virtuali e subnet.

## Diagramma dell'architettura
![Layout di rete](../media/az104-lab04-diagram.png)

| **Rete virtuale** | **Area**   | **Spazio indirizzi di rete virtuale** | **Subnet**                | **Subnet**    |
| ------------------- | ------------ | --------------------------------- | ------------------------- | ------------- |
| CoreServicesVnet    | Stati Uniti orientali      | 10.20.0.0/16                      |                           |               |
|                     |              |                                   | GatewaySubnet             | 10.20.0.0/27  |
|                     |              |                                   | SharedServicesSubnet      | 10.20.10.0/24 |
|                     |              |                                   | DatabaseSubnet            | 10.20.20.0/24 |
|                     |              |                                   | PublicWebServiceSubnet    | 10.20.30.0/24 |
| ManufacturingVnet   | Europa occidentale  | 10.30.0.0/16                      |                           |               |
|                     |              |                                   | ManufacturingSystemSubnet | 10.30.10.0/24 |
|                     |              |                                   | SensorSubnet1             | 10.30.20.0/24 |
|                     |              |                                   | SensorSubnet2             | 10.30.21.0/24 |
|                     |              |                                   | SensorSubnet3             | 10.30.22.0/24 |
| ResearchVnet        |Asia sud-orientale| 10.40.0.0/16                      |                           |               |
|                     |              |                                   | ResearchSystemSubnet      | 10.40.0.0/24  |


Queste reti virtuali e subnet sono strutturate in modo da supportare le risorse esistenti, ma consentono la crescita prevista. Verranno ora create le reti virtuali e le subnet che saranno alla base dell'infrastruttura di rete.

>**Lo sapevi?**: è consigliabile evitare intervalli di indirizzi IP sovrapposti per ridurre i problemi e semplificare la risoluzione dei problemi. La sovrapposizione è un problema nell'intera rete, sia nel cloud che in locale. Molte organizzazioni progettano uno schema di indirizzamento IP a livello aziendale per evitare sovrapposizioni e pianificare la crescita futura.



## Attività 1: Creare un gruppo di risorse

### Creare un gruppo di risorse per tutte le risorse in questo lab. 

1. Accedere al **portale di Azure** - `http://portal.azure.com`.

1. Cercare e selezionare **Gruppi** di risorse, quindi selezionare **+ Crea**.  

1. Creare il gruppo di risorse con queste impostazioni. 

| **Tab**         | **Opzione**                                 | **valore**            |
| --------------- | ------------------------------------------ | -------------------- |
| Informazioni di base          | Gruppo di risorse                             | `az104-rg4` |
|                 | Area                                     | (Stati Uniti) **Stati Uniti orientali**     |
| Tag            | Nessuna modifica necessaria                        |                      |
| Rivedi e crea | Rivedere le impostazioni e selezionare **Crea** |                      |


1. Aggiornare la **pagina Gruppi** di risorse e verificare che **az104-rg4** venga visualizzato nell'elenco.

## Attività 2: Creare la rete virtuale CoreServicesVnet e le subnet

L'organizzazione pianifica una grande crescita per i servizi di base. In questa attività si creano la rete virtuale e le subnet associate per supportare le risorse esistenti e la crescita pianificata.

1. Cercare e selezionare **Rete virtuale.**

    ![Risultati della barra di ricerca globale di una rete virtuale nella home page del portale di Azure.](../media/az104-lab04-vnet-search.png)

1. Selezionare **Crea** nella pagina Reti virtuali.  

    ![Creazione guidata di una rete virtuale.](../media/az104-lab04-createvnet.png)

3. Usare le informazioni contenute nella tabella seguente per creare la rete virtuale CoreServicesVnet.  

| **Tab**      | **Opzione**         | **valore**            |
| ------------ | ------------------ | -------------------- |
| Informazioni di base       | Gruppo di risorse     | **az104-rg1** |
|              | Nome               | `CoreServicesVnet`     |
|              | Area             | (Stati Uniti) **Stati Uniti orientali**         |
| Indirizzi IP | Spazio indirizzi IPv4 | `10.20.0.0/16`         |

    >**Note:** Remove or overwrite the default IP Address space.
    
![Configurazione degli indirizzi IP per la distribuzione della rete virtuale di Azure](../media/az104-lab04-address-space.png)

 4. Creare le subnet CoreServicesVnet. Per iniziare a creare ogni subnet, selezionare **+ Aggiungi subnet**. Per completare la creazione di ogni subnet, selezionare **Aggiungi**.

| **Subnet**             | **Opzione**           | **valore**              |
| ---------------------- | -------------------- | ---------------------- |
| GatewaySubnet          | Nome subnet          | `GatewaySubnet`          |
|                        | Intervallo di indirizzi subnet | `10.20.0.0/27`           |
| SharedServicesSubnet   | Nome subnet          | `SharedServicesSubnet`   |
|                        | Intervallo di indirizzi subnet | `10.20.10.0/24`          |
| DatabaseSubnet         | Nome subnet          | `DatabaseSubnet`         |
|                        | Intervallo di indirizzi subnet | `10.20.20.0/24  `        |
| PublicWebServiceSubnet | Nome subnet          | `PublicWebServiceSubnet` |
|                        | Intervallo di indirizzi subnet | `10.20.30.0/24`          |

 1. Per completare la creazione di CoreServicesVnet e delle subnet associate, selezionare **Rivedi e crea**.

 1. Verificare che la configurazione abbia superato la convalida e quindi selezionare **Crea**.
 
## Attività 3: Creare la rete virtuale ManufacturingVnet e le subnet

In questa attività si continua a creare una rete virtuale aggiuntiva e le subnet associate. L'organizzazione prevede una crescita per gli uffici di produzione, in modo che le subnet siano ridimensionate per la crescita prevista.

    >**Note**: If you need help, use the detailed steps in Task 2. 

| **Tab**      | **Opzione**         | **valore**             |
| ------------ | ------------------ | --------------------- |
| Informazioni di base       | Gruppo di risorse     | **az104-rg1**  |
|              | Nome               | `ManufacturingVnet`     |
|              | Area             | (Europa) **Europa occidentale**  |
| Indirizzi IP | Spazio indirizzi IPv4 | `10.30.0.0/16`          |



| **Subnet**                | **Opzione**           | **valore**                 |
| ------------------------- | -------------------- | ------------------------- |
| ManufacturingSystemSubnet | Nome subnet          | `ManufacturingSystemSubnet` |
|                           | Intervallo di indirizzi subnet | `10.30.10.0/24`             |
| SensorSubnet1             | Nome subnet          | `SensorSubnet1`             |
|                           | Intervallo di indirizzi subnet | `10.30.20.0/24`             |
| SensorSubnet2             | Nome subnet          | `SensorSubnet2`             |
|                           | Intervallo di indirizzi subnet | `10.30.21.0/24`             |
| SensorSubnet3             | Nome subnet          | `SensorSubnet3`             |
|                           | Intervallo di indirizzi subnet | `10.30.22.0/24`             |
 

## Attività 4: Creare la rete virtuale ResearchVnet e le subnet

In questa attività si creano la rete virtuale finale e la subnet associata. L'organizzazione non pianifica la crescita e ha esigenze limitate per gli uffici di ricerca e sviluppo.

>**Nota**: se è necessaria assistenza, usare i passaggi dettagliati nell'attività 2. 

| **Tab**      | **Opzione**         | **valore**            |
| ------------ | ------------------ | -------------------- |
| Informazioni di base       | Gruppo di risorse     | **az104-rg1** |
|              | Nome               | `ResearchVnet`         |
|              | Area             | **Asia sud-orientale**       |
| Indirizzi IP | Spazio indirizzi IPv4 | `10.40.0.0/16`         |

| **Subnet**           | **Opzione**           | **valore**            |
| -------------------- | -------------------- | -------------------- |
| ResearchSystemSubnet | Nome subnet          | `ResearchSystemSubnet` |
|                      | Intervallo di indirizzi subnet | `10.40.0.0/24 `        |
 

## Attività 5: Verificare la creazione delle reti virtuali e delle subnet

In questa attività si verifica di avere tutte le reti virtuali e le subnet necessarie per soddisfare i requisiti dell'organizzazione.

1. Nella home page portale di Azure cercare e selezionare **Tutte le risorse**.

2. Verificare che CoreServicesVnet, ManufacturingVnet e ResearchVnet siano elencate.

3. Selezionare **CoreServicesVnet**. 

4. In CoreServicesVnet, in **Impostazioni** selezionare **Subnet**.

5. In CoreServicesVnet | Subnet verificare che le subnet create siano elencate e che gli intervalli di indirizzi IP siano corretti.

   ![Elenco di subnet in CoreServicesVnet.](../media/az104-lab04-subnets.png)

6. Ripetere i passaggi da 3 a 5 per ogni rete virtuale. Ricordarsi di modificare la selezione della rete virtuale per assicurarsi di verificarne tutte.

## Revisione

Complimenti. È stato creato un gruppo di risorse, tre reti virtuali e subnet associate.
