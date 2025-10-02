---
lab:
  title: 'Lab 01: Gestire le identità di Microsoft Entra ID'
  module: Administer Identity
---

# Lab 01 - Gestire le identità di Microsoft Entra ID

## Introduzione al lab

Questa è la prima in una serie di lab per gli amministratori di Azure. Questo lab fornisce informazioni su utenti e gruppi. Gli utenti e i gruppi sono i blocchi predefiniti di base per una soluzione di gestione delle identità. 

Questo lab richiede una sottoscrizione di Azure. Il tipo di sottoscrizione può influire sulla disponibilità delle funzionalità in questo lab. È possibile modificare l'area, ma i passaggi vengono scritti usando **Stati Uniti orientali**. 


## Tempo stimato: 30 minuti

## Scenario laboratorio

L'organizzazione sta creando un nuovo ambiente lab per i test di pre-produzione di app e servizi.  Vengono assunti alcuni tecnici per gestire l'ambiente lab, incluse le macchine virtuali. Per consentire ai tecnici di eseguire l'autenticazione tramite Microsoft Entra ID, è stato necessario eseguire il provisioning di utenti e gruppi. Per ridurre al minimo il sovraccarico amministrativo, l'appartenenza ai gruppi deve essere aggiornata automaticamente in base alle posizioni. 

## Diagramma dell'architettura

![Diagramma dell'architettura del lab 01.](../media/az104-lab01-architecture.png)

## Competenze mansione

+ Attività 1: Creare e configurare account utente.
+ Attività 2: Creare gruppi e aggiungere membri.

## Attività 1: Creare e configurare account utente

In questa attività verranno creati e configurati account utente. Gli account utente archivieranno i dati utente, come nome, reparto, posizione e informazioni di contatto.

1. Accedere al **portale di Azure** - `https://portal.azure.com`.

1. Per passare al portale, selezionare **Annulla** nella **schermata iniziale di Benvenuto in Azure** . 

    >**Nota:** Il portale di Azure viene usato in tutti i lab. Se non si ha una versione di Azure, cercare e selezionare `Quickstart Center`. Prendersi alcuni minuti per guardare il video **Guida introduttiva al portale di Azure**. Anche se in precedenza è stato usato il portale, sono disponibili alcuni suggerimenti e trucchi per esplorare e personalizzare l'interfaccia.
    
1. Cercare e selezionare `Microsoft Entra ID`. Microsoft Entra ID è una soluzione di gestione delle identità e degli accessi basata sul cloud di Azure. Prendersi alcuni minuti per acquisire familiarità con alcune delle funzionalità elencate nel riquadro a sinistra. 

1. Selezionare il pannello **Panoramica**, quindi la scheda **Gestisci tenant**. 

    >**Suggerimenti utili** Un tenant è un'istanza specifica di Microsoft Entra ID contenente account e gruppi. A seconda della situazione, è possibile creare più tenant e **passare** da un tenant all'altro. 

1. Tornare alla **pagina Entra ID** premendo indietro nel browser o selezionando l'opzione nel menu di navigazione.

1. Quando si ha tempo, esplorare altre opzioni, ad **esempio licenze** e **reimpostazione** della password.
   
### Creare un nuovo utente

1. Nel pannello **Gestisci** selezionare **Utenti**, quindi nell'elenco **a discesa Nuovo utente** selezionare **Crea nuovo utente**. 

1. Creare un nuovo utente con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni). Nella scheda **Proprietà** si notino tutti i diversi tipi di informazioni che possono essere inclusi nell'account utente. 

    | Impostazione | valore |
    | --- | --- |
    | Nome dell'entità utente | `az104-user1` |
    | Nome visualizzato | `az104-user1` |
    | Genera automaticamente la password | **checked** |
    | Account abilitato | **checked** |
    | Posizione (scheda Proprietà) | `IT Lab Administrator` |
    | Reparto (scheda Proprietà) | `IT` |
    | Percorso utilizzo (scheda Proprietà) | **Stati Uniti** |

1. Dopo aver verificato, selezionare **Rivedi e crea**, quindi **Crea**.

1. Aggiornare la pagina e verificare che il nuovo utente sia stato creato. 

### Invitare un utente esterno

1. Nell'elenco a discesa **Nuovo utente** selezionare **Invita un utente esterno**. 

    | Impostazione | Valore |
    | --- | --- |
    | E-mail | Indirizzo di posta elettronica |
    | Nome visualizzato | nome dell'utente |
    | Inviare messaggio di invito | **Selezionare la casella** |
    | Message | `Welcome to Azure and our group project` |

1. Passare alla scheda **Proprietà**. Completare le informazioni di base, inclusi questi campi. 

    | Impostazione | Valore |
    | --- | --- |
    | Posizione  | `IT Lab Administrator` |
    | Reparto  | `IT` |
    | Percorso utilizzo (scheda Proprietà) | **Stati Uniti** |

1. Selezionare **Rivedi e invita**, quindi **Invita**.

1. **Aggiornare** la pagina e verificare che l'utente invitato sia stato creato. Si dovrebbe ricevere il messaggio di posta elettronica di invito in breve tempo. 

    >**Nota:** È improbabile che si creino account utente singolarmente. Si conosce il modo in cui l'organizzazione prevede di creare e gestire gli account utente?
    
## Attività 2: Creare gruppi e aggiungere membri

In questa attività si crea un account di gruppo. Gli account di gruppo possono includere account utente o dispositivi. Questi sono due modi di base in cui i membri vengono assegnati ai gruppi: In modo statico e dinamico. I gruppi statici richiedono agli amministratori di aggiungere e rimuovere i membri manualmente.  I gruppi dinamici vengono aggiornati automaticamente in base alle proprietà di un account utente o di un dispositivo. Ad esempio, la posizione. 

1. Nel portale di Azure, cercare e selezionare `Microsoft Entra ID`. Nel pannello **Gestisci** selezionare **Gruppi**. 

1. Prendersi un minuto per acquisire familiarità con le impostazioni del gruppo nel riquadro a sinistra.

   + La **scadenza** consente di configurare una durata del gruppo in giorni. Dopo tale periodo, il gruppo deve essere rinnovato dal proprietario.
   + I **criteri di denominazione** consentono di configurare parole bloccate e di aggiungere un prefisso o un suffisso ai nomi gruppo.

1. Nel pannello **Tutti i gruppi** selezionare **+ Nuovo gruppo** e creare un nuovo gruppo.     

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di gruppo | **Sicurezza** |
    | Nome del gruppo | `IT Lab Administrators` |
    | Descrizione gruppo | `Administrators that manage the IT lab` |
    | Tipo di appartenenza | **Assegnate** |

    >**Nota**: Per l'appartenenza dinamica è necessaria una licenza Entra ID Premium P1 o P2. Se sono disponibili altri **tipi di appartenenza**, le opzioni verranno visualizzate nell'elenco a discesa. 
    
    ![Screenshot della creazione di un gruppo assegnato.](../media/az104-lab01-create-assigned-group.png)

1. Selezionare **Nessun proprietario selezionato**.

1. **Nella pagina Aggiungi proprietari** cercare e **selezionare** se stessi (visualizzati nell'angolo in alto a destra) come proprietario. Si noti che è possibile avere più di un proprietario. 

1. Selezionare **Nessun membro selezionato**.

1. Nel riquadro **Aggiungi membri** cercare e **selezionare** **az104-user1** e l'**utente guest** invitato. Aggiungere entrambi gli utenti al gruppo. 

1. Selezionare **Crea** per distribuire il gruppo.

1. **Aggiornare** la pagina e verificare che il gruppo sia stato creato.

1. Selezionare il nuovo gruppo e verificare le informazioni relative a **membri** e **proprietari**.

>**Nota:** È possibile gestire un numero elevato di gruppi. L'organizzazione ha un piano per la creazione di gruppi e l'aggiunta di membri?
   
## Estendere l'apprendimento con Copilot

Copilot può essere utile per imparare a usare gli strumenti di scripting di Azure. Copilot può essere utile anche in aree non coperte nel lab o dove occorrono altre informazioni. Aprire un browser Edge e scegliere Copilot (in alto a destra) o passare a *copilot.microsoft.com*. Dedicare qualche minuto alla prova di queste richieste.
+ Quali sono i comandi di Azure PowerShell e dell'interfaccia della riga di comando necessari per creare un gruppo di sicurezza denominato Amministratori IT? Specificare la pagina di riferimento ufficiale del comando.  
+ Fornire una strategia dettagliata per la gestione di utenti e gruppi in Microsoft Entra ID.
+ Quali sono i passaggi del portale di Azure per creare in blocco utenti e gruppi?
+ Fornire una tabella di confronto fra gli account utente interni ed esterni di Microsoft Entra ID. 


## Altre informazioni con la formazione autogestita

+ [Conoscere Microsoft Entra ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/). Confrontare Microsoft Entra ID con Active Directory Domain Services, apprendere informazioni su Microsoft Entra ID P1 e P2 ed esplorare Microsoft Entra Domain Services per la gestione di dispositivi e app aggiunti a un dominio nel cloud.
+ [Creare utenti e gruppi in Microsoft Entra ID](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Creare utenti in Microsoft Entra ID. Informazioni su tipi di gruppi diversi. Creare un gruppo e aggiungere membri. Gestire account guest Business to Business.
+ [Consentire agli utenti di reimpostare la password con la reimpostazione della password self-service di Microsoft Entra](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Valutare la funzionalità di reimpostazione della password self-service per consentire agli utenti dell'organizzazione di reimpostare una password o sbloccare un account. Impostare, configurare e testare la reimpostazione della password self-service.


## Punti chiave

Congratulazioni per aver completato il lab. Ecco i concetti chiave per questo lab:

+ Un tenant rappresenta l'organizzazione e consente di gestire un'istanza specifica dei servizi cloud Microsoft per gli utenti interni ed esterni.
+ Microsoft Entra ID ha account utente e guest. Ogni account ha un livello di accesso specifico per l'ambito del lavoro previsto.
+ I gruppi combinano utenti o dispositivi correlati. Esistono due tipi di gruppi, tra cui Sicurezza e Microsoft 365.
+ L'appartenenza al gruppo può essere assegnata in modo statico o dinamico.

