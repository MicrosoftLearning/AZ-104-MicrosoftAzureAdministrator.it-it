---
demo:
  title: 'Dimostrazione 06: gestione del traffico di rete Amministrazione ister'
  module: Administer Network Traffic Management
---


# 06 - Gestione del traffico di rete Amministrazione ister

## Configurare Azure Load Balancer

In questa dimostrazione si apprenderà come creare un servizio di bilanciamento del carico pubblico. 

**Nota:**  questa dimostrazione richiede una rete virtuale con almeno una subnet. 

**Riferimento**: Guida introduttiva: [Creare un servizio di bilanciamento del carico pubblico per bilanciare il carico delle macchine virtuali usando il portale di Azure](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**Mostra la funzionalità di scelta della guida del portale**

1. Accedere al portale di Azure.

1. Cercare e selezionare **Bilanciamento del carico: consente di scegliere**.

1. Usare la procedura guidata per esaminare scenari diversi.
   
**Creare un servizio di bilanciamento del carico**

1. Continuare nella portale di Azure.

1. Cercare e selezionare **Bilanciamento del** carico. **Creare** un servizio di bilanciamento del carico. 

1. Nella **scheda Informazioni di base** discutere **sku**, **tipo** e **livello**.

1. Nella scheda Configurazione** IP front-end esaminare l'uso **di un indirizzo IP pubblico.

1. Nella **scheda Pool** back-end selezionare la rete virtuale con intervallo di indirizzi IP.

1. Nella **scheda Regole** in ingresso creare una regola di bilanciamento del carico. Illustrare parametri come **Protocollo**, **Porte**, **Probe** di integrità e **Persistenza della** sessione. 


## Configurare il gateway applicazione di Azure

In questa dimostrazione si apprenderà come creare un gateway di app Azure lication. 

**Nota**: per semplificare le operazioni, creare nuove reti virtuali e subnet durante l'esecuzione della configurazione. 

**Riferimento**: Guida introduttiva: [Indirizzare il traffico Web con app Azure lication Gateway - portale di Azure](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Creare il gateway di app Azure lication**

1. Accedere al portale di Azure.

1. Cercare e selezionare **app Azure gateway di pubblicazione**.

1. **Creare** un nuovo gateway.

1. Nella **scheda Informazioni di base** vengono illustrati **i** livelli, **la scalabilità automatica** e **i conteggi** delle istanze.

1. **Nella scheda Front-end** discutere i tipi di indirizzi IP.

1. Nella **scheda Back-end** discutere quando usare un pool back-end vuoto.

1. Nella **scheda Configurazione** vengono illustrate le regole di routing. Confrontare le regole del servizio di bilanciamento del carico.

1. Spiegare che dopo la creazione del gateway, si aggiungerebbero quindi destinazioni back-end e test. 
