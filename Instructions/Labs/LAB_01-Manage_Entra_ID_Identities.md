---
lab:
  title: 'Lab 01: Gestire le identità id di Microsoft Entra'
  module: Administer Identity
---

# Lab 01 - Gestire le identità id di Microsoft Entra

## Introduzione al lab

Si tratta della prima di una serie di lab per Azure Amministrazione istrators. In questo lab vengono fornite informazioni su utenti e gruppi. Gli utenti e i gruppi sono i blocchi predefiniti di base per una soluzione di gestione delle identità. 

## Tempo stimato: 30 minuti

## Scenario laboratorio

L'organizzazione sta creando un nuovo ambiente lab per i test di pre-produzione di app e servizi.  Alcuni tecnici vengono assunti per gestire l'ambiente lab, incluse le macchine virtuali. Per consentire agli ingegneri di eseguire l'autenticazione tramite Microsoft Entra ID, è stato eseguito il provisioning di utenti e gruppi. Per ridurre al minimo il sovraccarico amministrativo, l'appartenenza ai gruppi deve essere aggiornata automaticamente in base ai titoli dei processi. 

## Simulazione interattiva del lab

Questo lab usa una simulazione di lab interattiva. La simulazione consente di fare clic su uno scenario simile al proprio ritmo. Esistono differenze tra la simulazione interattiva e questo lab, ma molti dei concetti di base sono gli stessi. Non è necessaria una sottoscrizione di Azure.

>**Nota:** questa simulazione viene aggiornata. Microsoft Entra ID è il nuovo nome per Azure Active Directory (Azure AD). 

+ [Gestisci identità ID](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201) Entra. Creare e configurare gli utenti e assegnarli ai gruppi. Creare un tenant di Azure e gestire gli account guest. 

## Diagramma dell'architettura
![Diagramma dell'architettura lab 01.](../media/az104-lab01-architecture.png)

## Competenze mansione

+ Attività 1: Creare e configurare gli account utente.
+ Attività 2: Creare gruppi e aggiungere membri.

## Attività 1: Creare e configurare gli account utente

In questa attività verranno creati e configurati gli account utente. Gli account utente archivieranno i dati utente, ad esempio nome, reparto, posizione e informazioni di contatto.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

    >**Nota:** il portale di Azure viene usato in tutti i lab. Se non si ha una versione di Azure, cercare e selezionare `Quickstart Center`. Dedicare alcuni minuti a guardare il **video Introduttivo nel video portale di Azure**. Anche se in precedenza è stato usato il portale, sono disponibili alcuni suggerimenti e trucchi per esplorare e personalizzare l'interfaccia.
    
1. Cercare e selezionare `Microsoft Entra ID`. Microsoft Entra ID è la soluzione di gestione delle identità e degli accessi basata sul cloud di Azure. Prendere alcuni minuti per acquisire familiarità con alcune delle funzionalità elencate nel riquadro sinistro. 

1. Selezionare il **pannello Panoramica** e quindi la **scheda Gestisci tenant** . 

    >**Lo sapevi?** Un tenant è un'istanza specifica dell'ID Microsoft Entra contenente account e gruppi. A seconda della situazione, è possibile creare più tenant e **passare** da un tenant all'altro. 

1. Tornare alla **pagina Entra ID** e selezionare Licenze****. Da qui è possibile acquistare una licenza, gestire le licenze disponibili e assegnare licenze a utenti e gruppi. Selezionare **Funzionalità** con licenza per vedere cosa è disponibile.
   
### Creare un nuovo utente

1. Selezionare **Utenti** e quindi nell'elenco **a discesa Nuovo utente** selezionare **Crea nuovo utente**. 

1. Creare un nuovo utente con le impostazioni seguenti (lasciare le impostazioni predefinite per altri utenti). Nella **scheda Proprietà** si notino tutti i diversi tipi di informazioni che possono essere inclusi nell'account utente. 

    | Impostazione | valore |
    | --- | --- |
    | Nome dell'entità utente | `az104-user1` |
    | Nome visualizzato | `az104-user1` |
    | Genera automaticamente la password | **checked** |
    | Account abilitato | **checked** |
    | Titolo processo (scheda Proprietà) | `IT Lab Administrator` |
    | Reparto (scheda Proprietà) | `IT` |
    | Percorso utilizzo (scheda Proprietà) | **Stati Uniti** |

1. Al termine della revisione, selezionare **Rivedi e crea** e quindi **Crea**.

1. Aggiornare la pagina e verificare che il nuovo utente sia stato creato. 

### Invitare un utente esterno

1. Nell'elenco **a discesa Nuovo utente** selezionare **Invita un utente** esterno. 

    | Impostazione | Valore |
    | --- | --- |
    | E-mail | Indirizzo di posta elettronica |
    | Nome visualizzato | nome dell'utente |
    | Invia messaggio di invito | **Selezionare la casella** |
    | Message | `Welcome to Azure and our group project` |

1. Passare alla **scheda Proprietà** . Completare le informazioni di base, inclusi questi campi. 

    | Impostazione | Valore |
    | --- | --- |
    | Posizione  | `IT Lab Administrator` |
    | Department  | `IT` |
    | Percorso utilizzo (scheda Proprietà) | **Stati Uniti** |

1. Selezionare **Rivedi e invita** e quindi **Invita**.

1. **Aggiornare** la pagina e verificare che l'utente invitato sia stato creato. Si dovrebbe ricevere il messaggio di posta elettronica di invito a breve. 

    >**Nota:** è improbabile che si creino account utente singolarmente. Si sa come l'organizzazione prevede di creare e gestire gli account utente?
    
## Attività 2: Creare gruppi e aggiungere membri

In questa attività si crea un account di gruppo. Gli account di gruppo possono includere account utente o dispositivi. Questi sono due modi di base in cui i membri vengono assegnati ai gruppi: staticamente e dinamicamente. I gruppi statici richiedono agli amministratori di aggiungere e rimuovere i membri manualmente.  I gruppi dinamici vengono aggiornati automaticamente in base alle proprietà di un account utente o di un dispositivo. Ad esempio, il titolo di lavoro. 

1. Nella portale di Azure cercare e selezionare `Groups`.

1. Acquisire familiarità con le impostazioni del gruppo nel riquadro sinistro.

   + **La scadenza** consente di configurare una durata del gruppo in giorni. Dopo tale periodo, il gruppo deve essere rinnovato dal proprietario.
   + **I criteri di denominazione** consentono di configurare parole bloccate e di aggiungere un prefisso o un suffisso ai nomi di gruppo.

1. Nel pannello **Tutti i gruppi** selezionare **+ Nuovo gruppo** e creare un nuovo gruppo.     

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di gruppo | **Sicurezza** |
    | Nome del gruppo | `IT Lab Administrators` |
    | Descrizione gruppo | `Administrators that manage the IT lab` |
    | Tipo di appartenenza | **Assegnate** |

    >**Nota**: per l'appartenenza dinamica è necessaria una licenza Entra ID Premium P1 o P2. Se sono disponibili altri **tipi di** appartenenza, le opzioni verranno visualizzate nell'elenco a discesa. 
    
    ![Screenshot della creazione di un gruppo assegnato.](../media/az104-lab01-create-assigned-group.png)

1. Selezionare **Nessun proprietario selezionato**.

1. **Nella pagina Aggiungi proprietari** cercare e **selezionare** se stessi come proprietario. Si noti che è possibile avere più di un proprietario. 

1. Selezionare **Nessun membro selezionato**.

1. **Nel riquadro Aggiungi membri** cercare e **selezionare** **az104-user1** e l'utente **** guest invitato. Aggiungere entrambi gli utenti al gruppo. 

1. Selezionare **Crea** per distribuire il gruppo.

1. **Aggiornare** la pagina e verificare che il gruppo sia stato creato.

1. Selezionare il nuovo gruppo ed esaminare le **informazioni membri** e **proprietari** .

>**Nota:** è possibile gestire un numero elevato di gruppi. L'organizzazione ha un piano per la creazione di gruppi e l'aggiunta di membri?
   
## Pulire le risorse

Se si usa **la propria sottoscrizione** , è necessario un minuto per eliminare le risorse del lab. In questo modo le risorse vengono liberate e i costi vengono ridotti al minimo. Il modo più semplice per eliminare le risorse del lab consiste nell'eliminare il gruppo di risorse del lab. 

+ Nella portale di Azure selezionare il gruppo di risorse, selezionare **Elimina il gruppo di risorse, **Immettere il nome** del gruppo** di risorse e quindi fare clic su **Elimina**.
+ Uso di Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Uso dell'interfaccia della riga di comando di `az group delete --name resourceGroupName`.
  
## Punti chiave

Congratulazioni per il completamento del lab. Ecco alcuni passaggi principali per questo lab:

+ Un tenant rappresenta l'organizzazione e consente di gestire un'istanza specifica dei servizi cloud Microsoft per gli utenti interni ed esterni.
+ Microsoft Entra ID ha account utente e guest. Ogni account ha un livello di accesso specifico per l'ambito del lavoro previsto.
+ I gruppi combinano utenti o dispositivi correlati. Esistono due tipi di gruppi, tra cui Sicurezza e Microsoft 365.
+ L'appartenenza al gruppo può essere assegnata in modo statico o dinamico.


## Altre informazioni con la formazione autogestita

+ [Informazioni sull'ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/) Microsoft Entra. Confrontare Microsoft Entra ID con Active Directory DS, informazioni su Microsoft Entra ID P1 e P2 ed esplorare Microsoft Entra Domain Services per la gestione di dispositivi e app aggiunti a un dominio nel cloud.
+ [Creare utenti e gruppi di Azure in Microsoft Entra ID](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Creare utenti in Microsoft Entra ID. Informazioni su tipi di gruppi diversi. Creare un gruppo e aggiungere membri. Gestire account guest Business to Business.
+ [Consentire agli utenti di reimpostare la password con la reimpostazione della password self-service di Microsoft Entra](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Valutare la funzionalità di reimpostazione della password self-service per consentire agli utenti dell'organizzazione di reimpostare una password o sbloccare un account. Impostare, configurare e testare la reimpostazione della password self-service.



