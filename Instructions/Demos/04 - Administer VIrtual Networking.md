---
demo:
  title: 'Dimostrazione 04: Amministrazione ister Rete virtuale ing'
  module: Administer Virtual Networking
---

# 04 - Amministrazione ister Rete virtuale ing

## Configurare le reti virtuali

In questa dimostrazione verranno create reti virtuali.

**Riferimento**: Guida introduttiva: [Creare una rete virtuale - portale di Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## Creare una rete virtuale nel portale

1.  Accedere al portale di Azure e cercare ** Rete virtuale s**.

1.  Creare una rete virtuale, spiegando le impostazioni di base man mano che si procede. Verificare che sia stata creata almeno una subnet. 

1.  Verificare che la rete virtuale sia stata creata.

## Configurare i gruppi di sicurezza di rete

In questa dimostrazione verranno esaminati i gruppi di sicurezza di rete e gli endpoint di servizio.

**Riferimento**: [Limitare l'accesso alle risorse PaaS - Esercitazione - portale di Azure](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**Creare un gruppo di sicurezza di rete**

1. Accedere al portale di Azure.

1. Cercare e selezionare i **gruppi di** sicurezza di rete.

1. Creare un gruppo di sicurezza di rete che spiega le impostazioni man mano che si procede. 
 
1. Attendere che il gruppo di sicurezza di rete venga distribuito.

**Esplorare le regole in ingresso e in uscita**

1. Selezionare il nuovo gruppo di criteri di rete.

1. Illustrare come il gruppo di sicurezza di rete può essere associato a subnet o interfacce di rete.

1. Illustrare lo scopo delle regole in ingresso e in uscita.  

1. Esaminare le regole in ingresso e in uscita predefinite. 

1. Creare una nuova regola, spiegando le impostazioni man mano che si procede. Illustra in particolare la selezione del servizio (ad esempio HTTPS) e le impostazioni di priorità. 

## Configurare DNS di Azure

In questa dimostrazione verrà esaminato DNS di Azure.

**Riferimento**: Esercitazione: [Ospitare il dominio e il sottodominio - DNS di Azure](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Creare una zona DNS**

1. Accedere al portale di Azure.

1. Cercare il **servizio zone** DNS.

1. Creare una **zona** DNS e spiegare lo scopo della zona. Per un nome è possibile usare contoso.internal.com.

1.  Attendere il completamento della creazione della zona DNS. Potrebbe essere necessario aggiornare **** la pagina.

**Aggiungere un set di record DNS**

**Riferimento**: Esercitazione: [Creare un record alias per fare riferimento a un record di risorse di zona](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Dopo aver creato la zona, selezionare ** +Set di** record.

1. Usare l'elenco **a discesa Tipo** per visualizzare i diversi tipi di record. Esaminare il modo in cui vengono usati i diversi tipi di record. Si noti che le informazioni sui record cambiano quando si selezionano tipi di record diversi.

1. Creare un **record A** come esempio. 

