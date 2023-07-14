---
demo:
  title: 'Dimostrazione 05: Amministrare la connettività tra siti'
  module: Administer Intersite Connectivity
---

# 05 - Amministrare la connettività tra siti

## Configurare il peering di reti virtuali

**Nota:** Per questa dimostrazione sono necessarie due reti virtuali.

**Riferimento**: [Connettere reti virtuali con peering reti virtuali - Esercitazione](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Configurare il peering di reti virtuali nella prima rete virtuale**

1. In  **portale di Azure** selezionare la prima rete virtuale. Esaminare il valore del peering. 

1. In **Impostazioni** selezionare **Peering** e **+ Aggiungi** un nuovo peering.

1. Configurare il peering della seconda rete virtuale. Usare le icone delle informazioni per esaminare le diverse impostazioni. 

1. Al termine del peering, esaminare **lo stato del peering**. 

**Confermare il peering di reti virtuali nella seconda rete virtuale**

1. In  **portale di Azure** selezionare la seconda rete virtuale

1. In **Impostazioni** selezionare **Peering**.

1. Si noti che è stato creato automaticamente un peering.  Si noti che lo **** stato   **del peering**è Connesso.

1. Nel lab gli studenti creeranno il peering e testeranno la connessione tra macchine virtuali. 

## Configurare il routing di rete e gli endpoint

In questa dimostrazione si apprenderà come creare una tabella di routing, definire una route personalizzata e associarla a una subnet.

**Nota:** Questa dimostrazione richiede una rete virtuale con almeno una subnet.

**Riferimento**: [Instradare il traffico di rete - esercitazione - portale di Azure](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Creare una tabella di routing**

1. Quando si ha tempo, esaminare il diagramma dell'esercitazione. Spiegare perché è necessario creare una route definita dall'utente. 

1. Accedere al portale di Azure.

1. Cercare e selezionare **Tabelle di route**. Quando **si propagano le route del gateway, è consigliabile** usare . 

1. Creare una tabella di routing, spiegare eventuali impostazioni non comuni. 

1. Attendere la distribuzione della nuova tabella di routing.

**Aggiungere una route**

1.  Selezionare la nuova tabella di routing e **quindi route.**

1.  Creare una nuova **route**. Vengono illustrati i diversi **tipi di hop** disponibili. 

1.  Creare la nuova route e attendere la distribuzione della risorsa.
 
**Associare una route a una subnet**

1.  Passare alla subnet da associare alla tabella di routing.

1.  Selezionare **Tabella di** route e scegliere la nuova tabella di routing. 

1.  **Salvare** le modifiche.

