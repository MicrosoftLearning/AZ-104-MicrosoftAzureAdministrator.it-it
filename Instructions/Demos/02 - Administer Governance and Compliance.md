---
demo:
  title: 'Dimostrazione 02: Amministrare governance e conformità'
  module: Administer Governance and Compliance
---

# 02 - Amministrare governance e conformità

## Configurare le sottoscrizioni

Per questa area non è disponibile una dimostrazione formale.  

**Riferimento**: [Creare una sottoscrizione di Azure aggiuntiva](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)

## Configurare Criteri di Azure

In questa dimostrazione verranno applicati i criteri di Azure.

**Riferimento**: [Esercitazione: Creare criteri per applicare la conformità - Criteri di Azure](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Assegnare i criteri**

1.  Accedere al portale di Azure.

2.  Cercare e selezionare **Criteri**.

3.  Selezionare **Assegnazioni** e quindi **Assegna criterio**.

5.  Illustrare **l'ambito** che determina le risorse o il raggruppamento delle risorse a cui viene applicata l'assegnazione dei criteri.

6.  Selezionare i puntini di sospensione **definizione dei** criteri per aprire l'elenco delle definizioni disponibili. Esaminare le definizioni di criteri predefinite.

7.  Cercare e selezionare il criterio **Località consentite** . Questi criteri consentono di limitare le posizioni che l'organizzazione può specificare durante la distribuzione delle risorse.

8.  Spostare la scheda **Parametri** e selezionare una o più posizioni consentite nell'elenco a discesa.

9.  Fare clic su **Rivedi e crea** e quindi **su Crea** per creare il criterio.

**Creare e assegnare una definizione di iniziativa**

1.  Tornare alla pagina Criteri di Azure e selezionare **Definizioni** in Creazione.

2.  Selezionare **Definizione iniziativa** nella parte superiore della pagina.

3.  Specificare un **nome** e una **descrizione**.

4.  **Crea nuovo** Categoria.

5.  Nel pannello destro **Aggiungi** il criterio **Località** consentite.

6.  Aggiungere un criterio aggiuntivo di propria scelta.

7.  **Salvare** le modifiche e quindi **Assegnare** la definizione dell'iniziativa alla sottoscrizione.

**Verificare la conformità**

1.  Tornare alla pagina di servizio Criteri di Azure.

2.  Selezionare **Conformità**.

3.  Esaminare lo stato dei criteri e la definizione.

**Verificare se sono presenti attività di correzione**

1.  Tornare alla pagina di servizio Criteri di Azure.

2.  Selezionare **Correzione**.

3.  Esaminare tutte le attività di correzione elencate.

4. Quando si ha tempo, rimuovere il criterio e l'iniziativa. 

## Configurare il controllo degli accessi in base al ruolo

In questa dimostrazione verranno trattate le assegnazioni di ruolo.

**Riferimento**: [Esercitazione: Concedere a un utente l'accesso alle risorse di Azure usando il portale di Azure - Controllo degli accessi in base al ruolo di Azure](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

**Riferimento**: [Guida introduttiva - Controllare l'accesso per un utente alle risorse di Azure - Controllo degli accessi in base al ruolo di Azure](https://docs.microsoft.com/azure/role-based-access-control/check-access)

**Individuare il riquadro Controllo di accesso**

1.  Accedere al portale di Azure e selezionare un gruppo di risorse.  Prendere nota del gruppo di risorse in uso.

2.  Selezionare il  **pannello Controllo di accesso (IAM).** 

3.  Questo pannello sarà disponibile per molte risorse diverse in modo da poter controllare le autorizzazioni.

**Esaminare le autorizzazioni dei ruoli**

1.  Selezionare la scheda **Ruoli** (in alto).

1.  Esaminare il numero elevato di ruoli predefiniti disponibili.

1.  Fare doppio clic su un ruolo e quindi selezionare **Autorizzazioni**  (in alto).

1.  Continuare a eseguire il drill-in del ruolo fino a quando non è possibile visualizzare le azioni **Lettura, Scrittura ed Eliminazione** per tale ruolo.

1.  Tornare al  **pannello Controllo di accesso (IAM).** 

**Aggiungi un'assegnazione di ruolo**

1.  Creare un utente o selezionare un utente esistente.

1.  Selezionare **Aggiungi assegnazione di ruolo** e selezionare un ruolo. Per exmaple, *proprietario*.

1.  Selezionare **Controlla l'accesso**.

1.  Esaminare le autorizzazioni utente.

1.  Si noti che è possibile **negare le assegnazioni**.
