---
demo:
  title: 'Dimostrazione 06: Amministrare la gestione del traffico di rete'
  module: Administer Network Traffic Management
---


# 06 - Amministrare la gestione del traffico di rete

## Configurare Azure Load Balancer

In questa dimostrazione si apprenderà come creare un servizio di bilanciamento del carico pubblico. 

**Nota:** Questa dimostrazione richiede una rete virtuale con almeno una subnet. 

**Riferimento**: [Avvio rapido: Creare un servizio di bilanciamento del carico pubblico per bilanciare il carico delle macchine virtuali usando il portale di Azure](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**Mostra la guida del portale per scegliere la funzionalità**

1. Accedere al portale di Azure.

1. Cercare e selezionare **Bilanciamento del carico- Aiutami a scegliere**.

1. Usare la procedura guidata per esaminare i diversi scenari.
   
**Creare un servizio di bilanciamento del carico**

1. Continuare nel portale di Azure.

1. Cercare e selezionare **Bilanciamento del carico**. **Creare** un servizio di bilanciamento del carico. 

1. Nella scheda **Informazioni di base** vengono illustrati **SKU**, **Tipo** e **Livello**.

1. Nella scheda **Configurazione IP front-end** esaminare l'uso di un indirizzo IP pubblico.

1. Nella scheda **Pool back-end** selezionare la rete virtuale con intervallo di indirizzi IP.

1. Nella scheda **Regole in ingresso** creare una regola di bilanciamento del carico. Vengono illustrati i parametri come **Protocollo**, **Porte**, **Probe** di integrità e **Persistenza della sessione**. 


## Configurare il gateway applicazione di Azure

In questa dimostrazione verrà illustrato come creare un gateway applicazione di Azure. 

**Nota**: per semplificare le operazioni, creare nuove reti virtuali e subnet durante l'esecuzione della configurazione. 

**Riferimento**: [Guida introduttiva: Indirizzare il traffico Web con gateway applicazione di Azure - portale di Azure](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Creare il gateway applicazione di Azure**

1. Accedere al portale di Azure.

1. Cercare e selezionare **gateway applicazione di Azure**.

1. **Creare** un nuovo gateway.

1. Nella scheda **Informazioni di base** vengono illustrati **i livelli, la** **scalabilità automatica** e **i conteggi delle istanze**.

1. Nella scheda **Front-end vengono** illustrati i tipi di indirizzo IP.

1. Nella scheda **Back-end** discutere quando usare un pool back-end vuoto.

1. Nella scheda **Configurazione** vengono illustrate le regole di routing. Confrontare le regole del servizio di bilanciamento del carico.

1. Spiegare che dopo la creazione del gateway si aggiungerebbero destinazioni back-end e test. 
