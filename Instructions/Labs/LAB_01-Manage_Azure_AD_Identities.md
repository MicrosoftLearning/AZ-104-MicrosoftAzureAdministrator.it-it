---
lab:
  title: 'Lab 01: Gestire le identità di Azure Active Directory'
  module: Administer Identity
---

# Lab 01 - Gestire le identità di Azure Active Directory

# Manuale del lab per studenti

## Scenario del lab

Per consentire agli utenti di Contoso di eseguire l'autenticazione usando Azure AD, è stato necessario eseguire il provisioning di account di utenti e gruppi. L'appartenenza ai gruppi dovrà essere aggiornata automaticamente in base alle mansioni degli utenti. È anche necessario creare un tenant di Azure AD di con un account utente di test e concedere a tale account autorizzazioni limitate per le risorse della sottoscrizione di Azure Contoso.

                **Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** che consente di eseguire questo lab in base ai propri tempi. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi.

## Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Creare e configurare gli utenti di Azure AD
+ Attività 2: Creare gruppi di Azure AD con appartenenza dinamica e assegnata
+ Attività 3: Creare un tenant di Azure Active Directory (AD) (facoltativo - problema dell'ambiente lab)
+ Attività 4: Gestire gli utenti guest di Azure AD (facoltativo - problema dell'ambiente lab)

## Tempo stimato: 30 minuti

## Diagramma dell'architettura
![image](../media/lab01.png)

### Istruzioni

## Esercizio 1

## Attività 1: Creare e configurare gli utenti di Azure AD

In questa attività verranno creati e configurati gli utenti di Azure AD.

>**Nota**: se in precedenza è stata usata la licenza di valutazione per Azure AD Premium in questo tenant di Azure AD, sarà necessario un nuovo tenant di Azure in cui eseguire l'attività 2 dopo l'attività 3.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Nel portale di Azure cercare e selezionare **Azure Active Directory**.

1. Nel pannello Azure Active Directory scorrere verso il basso fino alla sezione **Gestisci,** fare clic su **Impostazioni utente** ed esaminare le opzioni di configurazione disponibili.

1. Nel pannello Azure Active Directory, nella sezione **Gestisci**, fare clic su **Utenti** e quindi sul proprio account utente per visualizzarne le impostazioni di **Profilo**. 

1. Fare clic su **Modifica proprietà**, quindi nella scheda **Impostazioni** impostare **Percorso utilizzo** **su Stati Uniti** e fare clic su **Salva** per applicare la modifica.

    >**Nota**: questa operazione è necessaria per assegnare una licenza di Azure AD Premium P2 all'account utente più avanti in questo lab.

1. Tornare nel pannello **Utenti - Tutti gli utenti** e quindi fare clic su **+ Nuovo utente**.

1. Creare un nuovo utente con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'entità utente | **az104-01a-aaduser1** |
    | Nome visualizzato | **az104-01a-aaduser1** |
    | Generare automaticamente la password | de-select |
    | Password iniziale | **Specificare una password sicura** |
    | Titolo processo (scheda Proprietà) | **Amministratore cloud** |
    | Reparto (scheda Proprietà) | **IT** |
    | Percorso di utilizzo (scheda Proprietà) | **Stati Uniti** |

    >**Nota**: usare **Copia negli Appunti** per copiare l'intero **nome dell'entità utente** (nome utente più dominio). Questo valore sarà necessario più avanti in questa attività.

1. Nell'elenco di utenti fare clic sull'account utente appena creato per visualizzare il relativo pannello.

1. Esaminare le opzioni disponibili nella sezione **Gestisci** e notare che è possibile identificare i ruoli di Azure AD assegnati all'account utente e le relative autorizzazioni per le risorse di Azure.

1. Nella sezione **Gestisci** fare clic su **Ruoli assegnati**, quindi fare clic sul pulsante **+ Aggiungi assegnazione** e assegnare il ruolo **Amministratore utenti** a **az104-01a-aaduser1**.

    >**Nota**: i ruoli di Azure AD possono anche essere assegnati durante il provisioning di un nuovo utente.

1. Aprire una finestra **InPrivate** del browser e accedere al [portale di Azure](https://portal.azure.com) con l'account utente appena creato. Quando viene richiesto di aggiornare la password, impostarla su una password sicura di propria scelta. 

    >**Nota**: invece di digitare il nome utente (incluso il nome di dominio), è possibile incollare il contenuto degli Appunti.

1. Nella finestra **InPrivate** del browser, nel portale di Azure, cercare e selezionare **Azure Active Directory**.

    >**Nota**: anche se questo account utente può accedere al tenant di Azure Active Directory, non ha accesso alle risorse di Azure. Questo comportamento è previsto, poiché tale accesso deve essere concesso esplicitamente tramite Controllo degli accessi in base al ruolo. 

1. Nella finestra **InPrivate** del browser, nel pannello Azure AD, scorrere verso il basso fino alla sezione **Gestisci**, fare clic su **Impostazioni utente** e notare che non si hanno le autorizzazioni per modificare le opzioni di configurazione.

1. Nella finestra **InPrivate** del browser, nel pannello Azure AD, fare clic su **Utenti** e quindi su **+ Nuovo utente** nella sezione **Gestisci**.

1. Creare un nuovo utente con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'entità utente | **az104-01a-aaduser2** |
    | Nome visualizzato | **az104-01a-aaduser2** |
    | Generare automaticamente la password | de-select  |
    | Password iniziale | **Specificare una password sicura** |
    | Posizione | **Amministratore sistema** |
    | department | **IT** |
    | Località di utilizzo | **Stati Uniti** |
    
1. Disconnettersi come utente az104-01a-aaduser1 dal portale di Azure e chiudere la finestra InPrivate del browser.

## Attività 2: Creare gruppi di Azure AD con appartenenza dinamica e assegnata

In questa attività verranno creati gruppi di Azure Active Directory con appartenenza assegnata e dinamica.

1. Tornare nel portale di Azure in cui è stato eseguito l'accesso con l'**account utente**, tornare nel pannello **Panoramica** del tenant di Azure AD e quindi, nella sezione **Gestisci**, fare clic su **Licenze**.

    >**Nota**: per implementare i gruppi dinamici, sono necessarie le licenze di Azure AD Premium P1 o P2.

1. Nella sezione **Gestisci** fare clic su **Tutti i prodotti**.

1. Fare clic su **+ Prova/Acquista** e attivare la versione di valutazione gratuita di Azure AD Premium P2.

1. Aggiornare la finestra del browser per verificare che l'attivazione sia riuscita. 

 >**Nota**: per l'attivazione delle licenze possono essere richiesti fino a 10 minuti. Continuare ad aggiornare la pagina finché non viene visualizzata. Non procedere fino a quando non sono state attivate le licenze.

1. Nel pannello **Licenze - Tutti i prodotti** selezionare la voce **Azure Active Directory Premium P2** e assegnare tutte le opzioni di licenza di Azure AD Premium P2 al proprio account utente e ai due account utente appena creati.

1. Nel portale di Azure tornare nel pannello del tenant di Azure AD e fare clic su **Gruppi**.

1. Usare il pulsante **+ Nuovo gruppo** per creare un nuovo gruppo con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Tipo gruppo | **Sicurezza** |
    | Nome gruppo | **IT Cloud Administrators** |
    | Descrizione gruppo | **Contoso IT cloud administrators** |
    | Tipo di appartenenza | **Utente dinamico** |

    >**Nota:** se l'elenco a discesa **Tipo di appartenenza** è disattivato, attendere alcuni minuti e aggiornare la pagina del browser.

1. Fare clic su **Aggiungi query dinamica**.

1. Nella scheda **Configura regole** del pannello **Regole di appartenenza dinamica** creare una nuova regola con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Proprietà | **jobTitle** |
    | Operatore | **È uguale a** |
    | Valore | **Amministratore cloud** |

1. Salvare la regola facendo clic su **+ Aggiungi un'espressione** e quindi su **Salva**. Tornare nel pannello **Nuovo gruppo** e fare clic su **Crea**. 

1. Tornare nel pannello **Gruppi - Tutti i gruppi** del tenant di Azure AD, fare clic sul pulsante **+ Nuovo gruppo** e creare un nuovo gruppo con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Tipo gruppo | **Sicurezza** |
    | Nome gruppo | **IT System Administrators** |
    | Descrizione gruppo | **Contoso IT system administrators** |
    | Tipo di appartenenza | **Utente dinamico** |

1. Fare clic su **Aggiungi query dinamica**.

1. Nella scheda **Configura regole** del pannello **Regole di appartenenza dinamica** creare una nuova regola con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Proprietà | **jobTitle** |
    | Operatore | **È uguale a** |
    | Valore | **Amministratore sistema** |

1. Salvare la regola facendo clic su **+ Aggiungi un'espressione** e quindi su **Salva**. Tornare nel pannello **Nuovo gruppo** e fare clic su **Crea**. 

1. Tornare nel pannello **Gruppi - Tutti i gruppi** del tenant di Azure AD, fare clic sul pulsante **+ Nuovo gruppo** e creare un nuovo gruppo con le impostazioni seguenti:

    | Impostazione | Valore |
    | --- | --- |
    | Tipo gruppo | **Sicurezza** |
    | Nome gruppo | **IT Lab Administrators** |
    | Descrizione gruppo | **Contoso IT Lab administrators** |
    | Tipo di appartenenza | **Assegnato** |
    
1. Fare clic su **Nessun membro selezionato**.

1. Nel pannello **Aggiungi membri** cercare e selezionare i gruppi **IT Cloud Administrators** e **IT System Administrators** e quindi, di nuovo nel pannello **Nuovo gruppo**, fare clic su **Crea**.

1. Tornare nel pannello **Gruppi - Tutti i gruppi**, fare clic sulla voce che rappresenta il gruppo **IT Cloud Administrators** e quindi visualizzare il relativo pannello **Membri**. Verificare che **az104-01a-aaduser1** sia visualizzato nell'elenco dei membri del gruppo.

    >**Nota**: si potrebbero riscontrare ritardi con gli aggiornamenti del gruppi di appartenenza dinamica. Per accelerare l'aggiornamento, passare al pannello del gruppo, visualizzare il relativo pannello **Regole di appartenenza dinamica**, scegliere **Modifica** per modificare la regola riportata nella casella di testo **Sintassi delle regole** aggiungendo uno spazio alla fine, quindi fare clic su **Salva** per salvare la modifica.

1. Tornare nel pannello **Gruppi - Tutti i gruppi**, fare clic sulla voce che rappresenta il gruppo **IT System Administrators** e quindi visualizzare il relativo pannello **Membri**. Verificare che **az104-01a-aaduser2** sia visualizzato nell'elenco dei membri del gruppo.

## Attività 3: Creare un tenant di Azure Active Directory (AD) (facoltativo - problema dell'ambiente lab)

In questa attività si creerà un nuovo tenant di Azure AD.
    
1. Nel portale di Azure cercare e selezionare **Azure Active Directory**.

    >**Nota**: si verifica un problema noto con la verifica Captcha nell'ambiente lab. Se viene visualizzato l'errore **Creazione non riuscita. Troppe richieste, provare più tardi**, eseguire le operazioni seguenti:<br>
    - Provare a creare alcune volte.<br>
    - Controllare la sezione **Gestisci tenant** per assicurarsi che il tenant non sia stato creato in background. <br>
    - Aprire una nuova finestra **InPrivate** e usare il portale di Azure e provare a creare il tenant da questa finestra.<br>
     Generare il problema con il formatore, quindi usare la **[simulazione interattiva del lab per visualizzare](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** i passaggi. <br>
    - È possibile provare questa attività in un secondo momento, ma la creazione di un tenant non è necessaria in altri lab. 

1. Fare clic su **Gestisci i tenant**, quindi nella schermata successiva fare clic su **+ Crea** e specificare l'impostazione seguente:

    | Impostazione | Valore |
    | --- | --- |
    | Tipo di directory | **Azure Active Directory** |
    
1. Fare clic su **Avanti: Configurazione**

    | Impostazione | Valore |
    | --- | --- |
    | Nome organizzazione | **Contoso Lab** |
    | Nome di dominio iniziale | Qualsiasi nome DNS valido costituito da lettere minuscole e cifre e che inizia con una lettera | 
    | Paese/Area geografica | **Stati Uniti** |

   > **Nota**: il valore di **Nome di dominio iniziale** non deve essere un nome legittimo che potrebbe corrispondere alla propria organizzazione o a un'altra. Il segno di spunta verde nella casella di testo **Nome di dominio iniziale** indicherà che il nome di dominio immesso è valido e univoco.

1. Fare clic su **Rivedi e crea** e quindi su **Crea**.

1. Visualizzare il pannello del tenant di Azure AD appena creato usando il collegamento **Fare clic qui per passare al nuovo tenant: Contoso Lab** oppure il pulsante **Directory e sottoscrizione** immediatamente a destra del pulsante Cloud Shell sulla barra degli strumenti del portale di Azure.

## Attività 4: Gestire gli utenti guest di Azure AD.

In questa attività verranno creati utenti guest di Azure AD a cui verrà concesso l'accesso alle risorse di una sottoscrizione di Azure.

1. Nel portale di Azure che visualizza il tenant di Azure AD Contoso Lab, nella sezione **Gestisci**, fare clic su **Utenti** e quindi su **+ Nuovo utente**.

1. Creare un nuovo utente con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | Nome dell'entità utente | **az104-01b-aaduser1** |
    | Nome visualizzato | **az104-01b-aaduser1** |
    | Generare automaticamente la password | de-select  |
    | Password iniziale | **Specificare una password sicura** |
    | Posizione | **Amministratore sistema** |
    | department | **IT** |

1. Fare clic sul profilo appena creato.

    >**Nota**: usare **Copia negli Appunti** per copiare l'intero **nome dell'entità utente** (nome utente più dominio). Questo valore sarà necessario più avanti in questa attività.

1. Tornare al primo tenant creato in precedenza. Eseguire questa operazione usando il pulsante **Directory + Sottoscrizione** (direttamente a destra del pulsante Cloud Shell) nella barra degli strumenti di portale di Azure.

1. Tornare nel pannello **Utenti - Tutti gli utenti** e quindi fare clic su **+ Invita utente esterno**.

1. Invitare un nuovo utente guest con le impostazioni seguenti (lasciare i valori predefiniti per le altre impostazioni):

    | Impostazione | Valore |
    | --- | --- |
    | E-mail | Il nome dell'entità utente copiato in precedenza in questa attività |
    | Nome visualizzato (scheda Proprietà)  | **az104-01b-aaduser1** |
    | Titolo processo (scheda Proprietà) | **Lab Administrator** |
    | Reparto (scheda Proprietà) | **IT** |
    | Posizione di utilizzo (scheda Proprietà) | **Stati Uniti** |

1. Fare clic su **Invita**. 

1. Tornare nel pannello **Utenti - Tutti gli utenti** e fare clic sulla voce che rappresenta l'account utente guest appena creato.

1. Nel pannello **az104-01b-aaduser1 - Profilo** fare clic su **Gruppi**.

1. Fare clic su **+ Aggiungi l'appartenenza** e aggiungere l'account utente guest al gruppo **IT Lab Administrators**.


## Attività 5: Pulire le risorse

> **Nota**: ricordarsi di rimuovere tutte le risorse di Azure appena create che non vengono più usate. La rimozione delle risorse inutilizzate garantisce che non verranno addebitati costi imprevisti. Anche se, in questo caso, non sono presenti costi aggiuntivi associati ai tenant di Azure Active Directory e ai relativi oggetti, è consigliabile rimuovere gli account utente, gli account di gruppo e il tenant di Azure Active Directory creati in questo lab.

 > **Nota**: non è necessario preoccuparsi se le risorse del lab non possono essere rimosse immediatamente. A volte le risorse hanno dipendenze e l'eliminazione può richiedere più tempo. Si tratta di un'attività comune dell'amministratore per monitorare l'utilizzo delle risorse, quindi è sufficiente esaminare periodicamente le risorse nel portale per verificare il funzionamento della pulizia. 

1. Nel **portale di Azure** cercare **Azure Active Directory** nella barra di ricerca. In **Azure Active Directory** selezionare **Licenze** in **Gestisci**. In **Licenze in** nella sezione **Gestisci** selezionare **Tutti i prodotti** e quindi la voce **Azure Active Directory Premium P2** nell'elenco. Procedere selezionando **Utenti con licenza**. Selezionare gli account utente **az104-01a-aaduser1** e **az104-01a-aaduser2** a cui sono state assegnate le licenze in questo lab, fare clic su **Rimuovi licenza** e, quando viene richiesto di confermare, fare clic su **Sì**.

1. Nel portale di Azure passare al pannello **Utenti - Tutti gli utenti**, fare clic sulla voce che rappresenta l'account utente guest **az104-01b-aaduser1**, quindi nel pannello **az104-01b-aaduser1 - Profilo** fare clic su **Elimina** e, quando viene richiesto di confermare, fare clic su **OK**.

1. Ripetere la stessa sequenza di passaggi per eliminare gli account utente rimanenti creati in questo lab.

1. Passare al pannello **Gruppi - Tutti i gruppi**, selezionare i gruppi creati in questo lab, fare clic su **Elimina** e, quando viene richiesto di confermare, fare clic su **OK**.

1. Nel portale di Azure visualizzare il pannello del tenant di Azure AD Contoso Lab usando il pulsante **Directory e sottoscrizione** immediatamente a destra del pulsante Cloud Shell sulla barra degli strumenti del portale di Azure.

1. Passare al pannello **Utenti - Tutti gli utenti**, fare clic sulla voce che rappresenta l'account utente **az104-01b-aaduser1**, quindi nel pannello **az104-01b-aaduser1 - Profilo** fare clic su **Elimina** e, quando viene richiesto di confermare, fare clic su **OK**.

1. Passare al pannello **Contoso Lab - Panoramica** del tenant di Azure AD Contoso Lab, fare clic su **Gestisci tenant** e quindi nella schermata successiva selezionare la casella accanto a **Contoso Lab**, fare clic su **Elimina** nel pannello **Eliminare tenant "Contoso Labs"?** , fare clic sul collegamento **Ottieni l'autorizzazione per eliminare le risorse di Azure**, nel pannello **Proprietà** di Azure Active Directory impostare **Gestione degli accessi per le risorse di Azure** su **Sì** e fare clic su **Salva**.

1. Tornare nel pannello **Elimina tenant "Contoso Lab"** , fare clic su **Aggiorna** e su **Elimina**.

> **Nota**: se un tenant ha una licenza di valutazione, è necessario attendere la scadenza di tale licenza prima di poter eliminare il tenant. Ciò non comporta costi aggiuntivi.

#### Verifica

In questo lab sono state eseguite le attività seguenti:

- Creazione e configurazione di Azure AD utenti
- Creazione di gruppi di Azure AD con appartenenza dinamica e assegnata
- Creazione di un tenant di Azure Active Directory (AD)
- Gestione di utenti guest di Azure AD 
