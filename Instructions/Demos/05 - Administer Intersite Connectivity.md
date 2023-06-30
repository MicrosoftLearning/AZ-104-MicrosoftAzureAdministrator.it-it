---
demo:
  title: 'Dimostrazione 05: Amministrare la connettività tra siti'
  module: Administer Intersite Connectivity
---

# 05 - Amministrare la connettività tra siti

## Configurare il peering di reti virtuali

**Nota:** Per questa dimostrazione saranno necessarie due reti virtuali.

**Informazioni di riferimento**: [Connettere reti virtuali con peering reti virtuali - Esercitazione](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Configurare il peering di reti virtuali nella prima rete virtuale**

1. In  **portale di Azure** selezionare la prima rete virtuale. Esaminare il valore del peering. 

1. In **Impostazioni** selezionare **Peering** e **+ Aggiungi** un nuovo peering.

1. Configurare il peering della seconda rete virtuale. Usare le icone delle informazioni per esaminare le diverse impostazioni. 

1. Al termine del peering, esaminare lo **stato del peering**. 

**Confermare il peering di reti virtuali nella seconda rete virtuale**

1. Nella  **portale di Azure** selezionare la seconda rete virtuale

1. In **Impostazioni** selezionare **Peering.**

1. Si noti che è stato creato automaticamente un peering.  Si noti che lo **** stato   **del peering**è connesso.

1. Nel lab gli studenti creeranno il peering e testeranno la connessione tra macchine virtuali. 



