---
demo:
  title: 'Dimostrazione 08: Amministrazione ister azure Macchine virtuali'
  module: Administer Azure Virtual Machines
---


# 08 - Amministrazione ister Azure Macchine virtuali

## Dimostrazione- Creare Macchine virtuali nel portale

In questa dimostrazione si creerà e si accederà a una macchina virtuale di Azure nel portale.

**Riferimenti**

[Guida introduttiva: Creare una macchina virtuale Windows nel portale di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[Avvio rapido: Creare una macchina virtuale Linux nel portale di Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[Connessione a una macchina virtuale con Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**Creare la macchina virtuale**

**Nota: **questi passaggi riguardano solo alcuni parametri della macchina virtuale. È possibile esplorare e coprire altre aree. È possibile creare una macchina virtuale Windows o Linux, a seconda del gruppo di destinatari.

1. Usare il portale di Azure.

1. **Cercare Macchine virtuali**. 

1. Creare una macchina virtuale di base. Esaminare le opzioni di disponibilità, le immagini e le regole in ingresso.

1. Discutere l'importanza della creazione di un account amministratore sicuro.

1. Creare la macchina virtuale e attendere la distribuzione della risorsa.  

**Connettersi alla macchina virtuale**

1. Esistono diversi modi per **Connessione** alla macchina virtuale. 

1. Per un server Windows è possibile usare **RDP**, come illustrato nella guida introduttiva. 

1. Per un server Linux è possibile **usare SSH**, come illustrato nella guida introduttiva. 

1. Per entrambi i server è possibile connettersi al **servizio Bastion** (Avvio rapido). Esaminare il motivo per cui Bastion è preferibile a RDP o SSH. 

## Configurare la disponibilità delle macchine virtuali

In questa dimostrazione verranno esaminate le opzioni relative alle macchine virtuali.

**Riferimenti**

[Creare macchine virtuali in un set di scalabilità usando portale di Azure](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. Usare il portale di Azure.

1. Cercare e selezionare **set di scalabilità di macchine virtuali**. 

1. Creare un **set di scalabilità di macchine virtuali**. Esaminare lo scopo dei set di scalabilità di macchine virtuali. Esaminare la differenza tra le **modalità di orchestrazione Uniform** e **Flessibile** . Spiegare la selezione può influire sulle opzioni di ridimensionamento. 

1. Passare alla **scheda Ridimensionamento** . 

1. Esaminare il modo in cui **vengono usati i criteri di scalabilità** e **scalabilità** manuale. 

1. Passare a criteri **di ridimensionamento personalizzati** . 

1. Esaminare il modo in cui **viene usata la soglia della CPU (%)** per aumentare e ridimensionare le istanze delle macchine virtuali. 

