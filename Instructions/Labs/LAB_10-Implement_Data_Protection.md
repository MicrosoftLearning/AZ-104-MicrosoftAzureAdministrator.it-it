---
lab:
  title: 10 - Implementare la protezione dei dati
  module: Administer Data Protection
---

# <a name="lab-10---backup-virtual-machines"></a>Lab 10 - Eseguire il backup delle macchine virtuali
# <a name="student-lab-manual"></a>Manuale del lab per studenti

## <a name="lab-scenario"></a>Scenario del lab

You have been tasked with evaluating the use of Azure Recovery Services for backup and restore of files hosted on Azure virtual machines and on-premises computers. In addition, you want to identify methods of protecting data stored in the Recovery Services vault from accidental or malicious data loss.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same. 

## <a name="objectives"></a>Obiettivi

In questo lab si eseguiranno le attività seguenti:

+ Attività 1: Effettuare il provisioning dell'ambiente lab
+ Attività 2: Creare un insieme di credenziali di Servizi di ripristino
+ Attività 3: Implementare il backup a livello di macchina virtuale di Azure
+ Attività 4: Implementare il backup di file e cartelle
+ Attività 5: Eseguire il ripristino dei file usando l'Agente di Servizi di ripristino di Azure
+ Attività 6: Eseguire il ripristino dei file usando gli snapshot macchina virtuale di Azure (facoltativo)
+ Attività 7: Esaminare la funzionalità di eliminazione temporanea di Servizi di ripristino di Azure (facoltativo)

## <a name="estimated-timing-50-minutes"></a>Tempo stimato: 50 minuti

## <a name="instructions"></a>Istruzioni

### <a name="exercise-1"></a>Esercizio 1

#### <a name="task-1-provision-the-lab-environment"></a>Attività 1: Effettuare il provisioning dell'ambiente lab

In questa attività verranno distribuite due macchine virtuali che verranno usate per testare scenari di backup diversi.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Nel portale di Azure aprire **Azure Cloud Shell** facendo clic sull'icona nell'angolo in alto a destra.

1. Se viene richiesto di selezionare **Bash** o **PowerShell**, selezionare **PowerShell**.

    >**Nota**: se è la prima volta che si avvia **Cloud Shell** e viene visualizzato il messaggio **Non sono state montate risorse di archiviazione**, selezionare la sottoscrizione in uso nel lab e quindi fare clic su **Crea archivio**.

1. Sulla barra degli strumenti del riquadro Cloud Shell fare clic sull'icona **Carica/Scarica file**, nel menu a discesa fare clic su **Carica** e caricare i file **\\Allfiles\\Labs\\10\\az104-10-vms-edge-template.json** e **\\Allfiles\\Labs\\10\\az104-10-vms-edge-parameters.json** nella home directory di Cloud Shell.

1. Edit the Parameters file you just uploaded and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. From the Cloud Shell pane, run the following to create the resource group that will be hosting the virtual machines (replace the <ph id="ph1">`[Azure_region]`</ph> placeholder with the name of an Azure region where you intend to deploy Azure virtual machines). Type each command line separately and execute them separately:

   ```powershell
   $location = '[Azure_region]'
    ```
    
   ```powershell
   $rgName = 'az104-10-rg0'
    ```
    
   ```powershell
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Nel riquadro Cloud Shell eseguire il codice seguente per creare la prima rete virtuale e distribuire in tale rete una macchina virtuale usando il modello e i file di parametri caricati:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-10-vms-edge-template.json `
      -TemplateParameterFile $HOME/az104-10-vms-edge-parameters.json `
      -AsJob
   ```

1. Ridurre a icona il riquadro Cloud Shell, ma non chiuderlo.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the deployment to complete but instead proceed to the next task. The deployment should take about 5 minutes.

#### <a name="task-2-create-a-recovery-services-vault"></a>Attività 2: Creare un insieme di credenziali di Servizi di ripristino

In questa attività verrà creato un insieme di credenziali di Servizi di ripristino.

1. Nel portale di Azure cercare e selezionare **Insiemi di credenziali di Servizi di ripristino** e quindi nel pannello **Insiemi di credenziali di Servizi di ripristino** fare clic su **+ Crea**.

1. Nel pannello **Crea Insieme di credenziali di Servizi di ripristino** specificare le impostazioni seguenti:

    | Impostazioni | Valore |
    | --- | --- |
    | Subscription | Nome della sottoscrizione di Azure usata in questo lab |
    | Resource group | Nome di un nuovo gruppo di risorse **az104-10-rg1** |
    | Nome dell'insieme di credenziali | **az104-10-rsv1** |
    | Region | Nome dell'area in cui sono state distribuite le due macchine virtuali nell'attività precedente |

    >**Nota**: assicurarsi di specificare la stessa area in cui sono state distribuite le macchine virtuali nell'attività precedente.

1. Fare clic su **Rivedi e crea**, assicurarsi che la convalida abbia avuto esito positivo e fare clic su **Crea**.

    >L'attività da eseguire consiste nel valutare l'uso di Servizi di ripristino di Azure per il backup e il ripristino di file ospitati in macchine virtuali di Azure e in computer locali.

1. Al termine della distribuzione, fare clic su **Vai alla risorsa**.

1. Nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** nella sezione **Impostazioni** fare clic su **Proprietà**.

1. Nel pannello **az104-10-rsv1 - Proprietà** fare clic sul collegamento **Aggiorna** sotto l'etichetta **Configurazione di backup**.

1. Si vogliono inoltre identificare i metodi di protezione dei dati archiviati nell'insieme di credenziali di Servizi di ripristino da perdita dei dati accidentale o dannosa.

    >**Nota**: questa impostazione può essere configurata solo se non sono presenti elementi di backup esistenti.

1. Nel pannello **az104-10-rsv1 - Proprietà** visualizzato di nuovo fare clic sul collegamento **Aggiorna** sotto l'etichetta **Impostazioni di sicurezza**.

1. Nel pannello **Impostazioni di sicurezza** si noti che l'opzione **Eliminazione temporanea (per carichi di lavoro in esecuzione in Azure)** è **abilitata**.

1. Chiudere il pannello **Impostazioni di sicurezza** e nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** visualizzato di nuovo fare clic su **Panoramica**.

#### <a name="task-3-implement-azure-virtual-machine-level-backup"></a>Attività 3: Implementare il backup a livello di macchina virtuale di Azure

In questa attività verrà implementato il backup a livello di macchina virtuale di Azure.

   >**Nota**: prima di avviare questa attività, assicurarsi che la distribuzione avviata nella prima attività di questo lab sia stata completata correttamente.

1. Nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** fare clic su **Panoramica** e quindi su **+ Backup**.

1. Nel pannello **Obiettivo del backup** specificare le impostazioni seguenti:

    | Impostazioni | valore |
    | --- | --- |
    | Dove viene eseguito il carico di lavoro? | **Azure** |
    | Di quali elementi si vuole eseguire il backup? | **Macchina virtuale** |

1. Nel pannello **Obiettivo del backup** fare clic su **Backup**.

1. In **Criteri di backup** esaminare le impostazioni per **DefaultPolicy** e selezionare **Crea nuovi criteri**.

1. Definire un nuovo criterio di backup con le impostazioni seguenti e non modificare i valori predefiniti per gli altri criteri:

    | Impostazione | Valore |
    | ---- | ---- |
    | Nome criteri | **az104-10-backup-policy** |
    | Frequenza | **Ogni giorno** |
    | Ora | **12:00** |
    | Fuso orario | Nome del fuso orario locale |
    | Conservare gli snapshot del ripristino istantaneo per | **2** giorni |

1. Fare clic su **OK** per creare il criterio e quindi nella sezione **Macchine virtuali** selezionare **Aggiungi**.

1. Nel pannello **Seleziona macchine virtuali** selezionare **az-104-10-vm0**, fare clic su **OK**, quindi nel pannello **Backup** visualizzato di nuovo fare clic su **Abilita backup**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the backup to be enabled. This should take about 2 minutes.

1. Tornare al pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** e nella sezione **Elementi protetti** fare clic su **Elementi di backup**, quindi sulla voce **Macchina virtuale di Azure**.

1. Nel pannello **Elementi di backup (macchina virtuale di Azure)** selezionare il collegamento **Visualizza dettagli** per **az104-10-vm0** ed esaminare i valori delle voci **Controllo preliminare di backup** e **Stato ultimo backup**.

1. Nel pannello dell'elemento di backup **az104-10-vm0** fare clic su **Esegui backup ora**, accettare il valore predefinito nell'elenco a discesa **Conserva backup fino a** e fare clic su **OK**.

    >**Nota**: non attendere il completamento del backup, ma procedere con l'attività successiva.

#### <a name="task-4-implement-file-and-folder-backup"></a>Attività 4: Implementare il backup di file e cartelle

In questa attività verrà implementato il backup di file e cartelle mediante Servizi di ripristino di Azure.

1. Nel portale di Azure cercare e selezionare **Macchine virtuali** e nel pannello **Macchine virtuali** fare clic su **az104-10-vm1**.

1. Nel pannello **az104-10-vm1** fare clic su **Connetti**, nel menu a discesa fare clic su **RDP**, nel pannello **Connetti tramite RDP** selezionare **Scarica file RDP** e seguire le istruzioni per avviare la sessione di Desktop remoto.

    >                **Nota:** è disponibile una **[simulazione di lab interattiva](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)** che consente di eseguire questo lab in base ai propri tempi.

    >**Nota**: è possibile ignorare eventuali richieste di avviso durante la connessione alle macchine virtuali di destinazione.

1. Quando richiesto, accedere usando il nome utente e la password dello **studente** presenti nel file dei parametri.

    >**Nota:** poiché il portale di Azure non supporta più IE11, è necessario usare il browser Microsoft Edge per questa attività.

1. Nella sessione di Desktop remoto per la macchina virtuale di Azure **az104-10-vm1** avviare un'istanza del Web browser Microsoft Edge, passare al [portale di Azure](https://portal.azure.com) e accedere usando le credenziali.

1. Nel portale di Azure cercare e selezionare **Insiemi di credenziali di Servizi di ripristino** e in **Insiemi di credenziali di Servizi di ripristino** fare clic su **az104-10-rsv1**.

1. Nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** fare clic su **+ Backup**.

1. Nel pannello **Obiettivo del backup** specificare le impostazioni seguenti:

    | Impostazioni | valore |
    | --- | --- |
    | Dove viene eseguito il carico di lavoro? | **Configurazione locale** |
    | Di quali elementi si vuole eseguire il backup? | **File e cartelle** |

    >**Nota**: anche se la macchina virtuale in uso in questa attività è in esecuzione in Azure, è possibile sfruttarla per valutare le funzionalità di backup applicabili a qualsiasi computer locale che esegue Windows Server.

1. Nel pannello **Obiettivo del backup** fare clic su **Preparare l'infrastruttura**.

1. Nel pannello **Preparare l'infrastruttura** fare clic sul collegamento **Scaricare l'agente per Windows Server o Windows Client**.

1. Quando richiesto, fare clic su **Esegui** per avviare l'installazione di **MARSAgentInstaller.exe** con le impostazioni predefinite.

    >**Nota**: nella pagina **Consenso esplicito per Microsoft Update** dell'**Installazione guidata di Agente servizi di ripristino di Microsoft Azure** selezionare l'opzione di installazione **Non utilizzare Microsoft Update**.

1. Si potrebbero notare piccole differenza tra la simulazione interattiva e il lab ospitato, ma i concetti e le idee principali dimostrati sono gli stessi.

1. Passare alla finestra del Web browser che mostra il portale di Azure e nel pannello **Preparare l'infrastruttura** selezionare la casella di controllo **Already downloaded or using the latest Recovery Server Agent** (Download già completato o con la versione più recente di Agente servizi di ripristino), quindi fare clic su **Scarica**.

1. When prompted, whether to open or save the vault credentials file, click <bpt id="p1">**</bpt>Save<ept id="p1">**</ept>. This will save the vault credentials file to the local Downloads folder.

1. Tornare alla finestra della **Registrazione guidata server** e nella pagina **Identificazione insieme di credenziali** fare clic su **Esplora**.

1. Nella finestra di dialogo **Seleziona insieme di credenziali** passare alla cartella **Download**, fare clic sul file delle credenziali dell'insieme di credenziali scaricato e fare clic su **Apri**.

1. Nella pagina **Identificazione insieme di credenziali** visualizzata di nuovo fare clic su **Avanti**.

1. Nella pagina **Impostazione crittografia** della **Registrazione guidata server** fare clic su **Genera passphrase**.

1. Nella pagina **Impostazione crittografia** della **Registrazione guidata server** fare clic sul pulsante **Esplora** accanto a **Immetti un percorso in cui salvare la passphrase**.

1. Nella finestra di dialogo **Sfoglia per cartelle** selezionare la cartella **Documenti** e fare clic su **OK**.

1. Fare clic su **Fine**, esaminare l'avviso di **Backup di Microsoft Azure** e fare clic su **Sì**, quindi attendere il completamento della registrazione.

    >**Nota**: in un ambiente di produzione è consigliabile archiviare il file della passphrase in un percorso sicuro diverso dal server di cui viene eseguito il backup.

1. On the <bpt id="p1">**</bpt>Server Registration<ept id="p1">**</ept> page of the <bpt id="p2">**</bpt>Register Server Wizard<ept id="p2">**</ept>, review the warning regarding the location of the passphrase file, ensure that the <bpt id="p3">**</bpt>Launch Microsoft Azure Recovery Services Agent<ept id="p3">**</ept> checkbox is selected and click <bpt id="p4">**</bpt>Close<ept id="p4">**</ept>. This will automatically open the <bpt id="p1">**</bpt>Microsoft Azure Backup<ept id="p1">**</ept> console.

1. Nella console di **Backup di Microsoft Azure** nel riquadro **Azioni** fare clic su **Pianifica backup**.

1. Nella **Pianificazione guidata backup** nella pagina **Attività iniziali** fare clic su **Avanti**.

1. Nella pagina **Seleziona elementi per backup** fare clic su **Aggiungi elementi**.

1. Nella finestra di dialogo **Seleziona elementi** espandere **C:\\Windows\\System32\\drivers\\etc\\** , selezionare **hosts** e quindi fare clic su**OK**:

1. Nella pagina **Seleziona elementi per backup** fare clic su **Avanti**.

1. Nella pagina **Specificare la pianificazione del backup** assicurarsi che l'opzione **Giorno** sia selezionata e nella prima casella di riepilogo a discesa sotto la casella **Agli orari seguenti (è consentito un massimo di tre orari al giorno)** selezionare **4:30** e quindi fare clic su **Avanti**.

1. Nella pagina **Seleziona criteri di conservazione** accettare le impostazioni predefinite, quindi fare clic su **Avanti**.

1. Nella pagina **Scegliere il tipo di backup iniziale** accettare le impostazioni predefinite, quindi fare clic su **Avanti**.

1. On the <bpt id="p1">**</bpt>Confirmation<ept id="p1">**</ept> page, click <bpt id="p2">**</bpt>Finish<ept id="p2">**</ept>. When the backup schedule is created, click <bpt id="p1">**</bpt>Close<ept id="p1">**</ept>.

1. Nel riquadro Azioni della console di **Backup di Microsoft Azure** fare clic su **Esegui backup**.

    >**Nota**: l'opzione per eseguire il backup su richiesta diventa disponibile dopo la creazione di un backup pianificato.

1. Nella pagina **Seleziona elemento di backup** dell'Esecuzione guidata backup assicurarsi che l'opzione **File e cartelle** sia selezionata e fare clic su **Avanti**.

1. Nella pagina **Conserva backup fino a** accettare l'impostazione predefinita e fare clic su **Avanti**.

1. Nella pagina **Conferma** fare clic su **Backup**.

1. Al termine del backup, fare clic su **Chiudi**, quindi chiudere Backup di Microsoft Azure.

1. Passare alla finestra del Web browser che mostra il portale di Azure, tornare al pannello **Insieme di credenziali di Servizi di ripristino** nella sezione **Elementi protetti** e fare clic su **Elementi di backup**.

1. Nel pannello **az104-10-rsv1 - Elementi di backup** fare clic su **Azure Backup Agent**.

1. Nel pannello **Elementi di backup (Azure Backup Agent)** verificare se è presente una voce che fa riferimento all'unità **C:\\** di **az104-10-vm1.** .

#### <a name="task-5-perform-file-recovery-by-using-azure-recovery-services-agent-optional"></a>Attività 5: Eseguire il ripristino dei file usando l'Agente di Servizi di ripristino di Azure (facoltativo)

In questa attività verrà eseguito il ripristino dei file mediante l'Agente di Servizi di ripristino di Azure.

1. Nella sessione di Desktop remoto per **az104-10-vm1** aprire Esplora file, passare alla cartella **C:\\Windows\\System32\\drivers\\etc\\** ed eliminare il file **hosts**.

1. Open Microsoft Azure Backup and click <bpt id="p1">**</bpt>Recover data<ept id="p1">**</ept> in the <bpt id="p2">**</bpt>Actions<ept id="p2">**</ept> pane. This will start <bpt id="p1">**</bpt>Recover Data Wizard<ept id="p1">**</ept>.

1. Nella pagina **Attività iniziali** del **Ripristino guidato dei dati** assicurarsi che l'opzione **Questo server (az104-10-vm1.)** sia selezionata e fare clic su **Avanti**.

1. Nella pagina **Seleziona modalità di ripristino** assicurarsi che l'opzione **Singoli file e cartelle** sia selezionata e fare clic su **Avanti**.

1. Nella pagina **Seleziona volume e data** nell'elenco a discesa **Seleziona il volume** selezionare **C:\\** , accettare la selezione predefinita del backup disponibile e fare clic su **Monta**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the mount operation to complete. This might take about 2 minutes.

1. Nella pagina **Sfoglia e ripristina file** prendere nota della lettera di unità del volume di ripristino ed esaminare il suggerimento relativo all'uso di robocopy.

1. Fare clic su **Start**, espandere la cartella **Sistema Windows** e fare clic su **Prompt dei comandi**.

1. Dal prompt dei comandi eseguire il codice seguente per copiare il file **hosts** di ripristino nel percorso originale (sostituire `[recovery_volume]` con la lettera di unità del volume di ripristino identificato in precedenza):

   ```sh
   robocopy [recovery_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Tornare al **Ripristino guidato dei dati** e in **Cerca e ripristina file** fare clic su **Smonta** e quando viene richiesta la conferma fare clic su **Sì**.

1. Chiudere la sessione di Desktop remoto.

#### <a name="task-6-perform-file-recovery-by-using-azure-virtual-machine-snapshots-optional"></a>Attività 6: Eseguire il ripristino dei file usando gli snapshot macchina virtuale di Azure (facoltativo)

In questa attività verrà eseguito il ripristino di un file dal backup basato su snapshot a livello di macchina virtuale di Azure.

1. Passare alla finestra del browser in esecuzione nel computer del lab che mostra il portale di Azure.

1. Nel portale di Azure cercare e selezionare **Macchine virtuali** e nel pannello **Macchine virtuali** fare clic su **az104-10-vm0**.

1. Nel pannello **az104-10-vm0** fare clic su **Connetti**, nel menu a discesa fare clic su **RDP**, nel pannello **Connetti tramite RDP** selezionare **Scarica file RDP** e seguire le istruzioni per avviare la sessione di Desktop remoto.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**Nota**: è possibile ignorare eventuali richieste di avviso durante la connessione alle macchine virtuali di destinazione.

1. Quando richiesto, accedere usando il nome utente e la password dello **studente** presenti nel file dei parametri.

   >**Nota:** poiché il portale di Azure non supporta più IE11, è necessario usare il browser Microsoft Edge per questa attività.

1. Nella sessione di Desktop remoto per **az104-10-vm0** fare clic su **Start**, espandere la cartella **Sistema Windows** e fare clic su **Prompt dei comandi**.

1. Dal prompt dei comandi eseguire il codice seguente per eliminare il file **hosts**:

   ```sh
   del C:\Windows\system32\drivers\etc\hosts
   ```

   >**Nota**: più avanti in questa attività verrà eseguito il ripristino di questo file dal backup basato su snapshot a livello di macchina virtuale di Azure.

1. Nella sessione di Desktop remoto per la macchina virtuale di Azure **az104-10-vm0** avviare un'istanza del Web browser Microsoft Edge, passare al [portale di Azure](https://portal.azure.com) e accedere usando le credenziali.

1. Nel portale di Azure cercare e selezionare **Insiemi di credenziali di Servizi di ripristino** e in **Insiemi di credenziali di Servizi di ripristino** fare clic su **az104-10-rsv1**.

1. Nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** nella sezione **Elementi protetti** fare clic su **Elementi di backup**.

1. Nel pannello **az104-10-rsv1 - Elementi di backup** fare clic su **Macchina virtuale di Azure**.

1. Nel pannello **Elementi di backup (Macchina virtuale di Azure)** selezionare **Visualizza dettagli** per **az104-10-vm0**.

1. Nel pannello dell'elemento di backup **az104-10-vm0** fare clic su **Ripristino di file**.

    >**Nota**: è possibile eseguire il ripristino subito dopo l'avvio del backup in base a uno snapshot coerente con l'applicazione.

1. Nel pannello **Ripristino di file** accettare il punto di ripristino predefinito e fare clic su **Scarica eseguibile**.

    >**Nota**: lo script monta i dischi dal punto di ripristino selezionato come unità locali all'interno del sistema operativo da cui viene eseguito lo script.

1. Fare clic su **Scarica** e quando viene richiesto se si vuole eseguire o salvare **IaaSVMILRExeForWindows.exe** fare clic su **Salva**.

1. Nella finestra Esplora file visualizzata di nuovo fare doppio clic sul file appena scaricato.

1. Quando viene richiesto di fornire la password dal portale, copiare la password dalla casella di testo **Password per eseguire lo script** nel pannello **Ripristino di file**, incollarla nel prompt dei comandi e premere **INVIO**.

    >**Nota**: verrà aperta una finestra Windows PowerShell con lo stato di avanzamento del montaggio.

    >**Nota**: se a questo punto viene visualizzato un messaggio di errore, aggiornare la finestra del Web browser e ripetere gli ultimi tre passaggi.

1. Attendere il completamento del processo di montaggio, esaminare i messaggi informativi nella finestra Windows PowerShell, prendere nota della lettera di unità assegnata al volume che ospita **Windows** e avviare Esplora file.

1. In Esplora file passare alla lettera di unità che ospita lo snapshot del volume del sistema operativo identificato nel passaggio precedente ed esaminarne il contenuto.

1. Passare alla finestra **Prompt dei comandi**.

1. Dal prompt dei comandi eseguire il codice seguente per copiare il file **hosts** di ripristino nel percorso originale (sostituire `[os_volume]` con la lettera di unità del volume del sistema operativo identificato in precedenza):

   ```sh
   robocopy [os_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Tornare al pannello **Ripristino di file** nel portale di Azure e fare clic su **Smonta dischi**.

1. Chiudere la sessione di Desktop remoto.

#### <a name="task-7-review-the-azure-recovery-services-soft-delete-functionality"></a>Attività 7: Esaminare la funzionalità di eliminazione temporanea di Servizi di ripristino di Azure

1. Nel portale di Azure nel computer del lab cercare e selezionare **Insiemi di credenziali di Servizi di ripristino** e in **Insiemi di credenziali di Servizi di ripristino** fare clic su **az104-10-rsv1**.

1. Nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** nella sezione **Elementi protetti** fare clic su **Elementi di backup**.

1. Nel pannello **az104-10-rsv1 - Elementi di backup** fare clic su **Azure Backup Agent**.

1. Nel pannello **Elementi di backup (Azure Backup Agent)** fare clic sulla voce che rappresenta il backup di **az104-10-vm1**.

1. On the <bpt id="p1">**</bpt>C:<ph id="ph1">\\</ph> on az104-10-vm1.<ept id="p1">**</ept> blade, select <bpt id="p1">**</bpt>View details<ept id="p1">**</ept> for <bpt id="p2">**</bpt>az104-10-vm1.<ept id="p2">**</ept> .

1. Nel pannello Dettagli fare clic su **az104-10-vm1**.

1. On the <bpt id="p1">**</bpt>az104-10-vm1.<ept id="p1">**</ept> Protected Servers blade, click <bpt id="p1">**</bpt>Delete<ept id="p1">**</ept>.

1. Nel pannello **Elimina** specificare le impostazioni seguenti.

    | Impostazioni | valore |
    | --- | --- |
    | DIGITARE IL NOME DEL SERVER | **az104-10-vm1.** |
    | Motivo | **Riciclo del server di sviluppo/test** |
    | Commenti | **az104 10 lab** |

    >**Nota**: assicurarsi di includere il punto finale quando si digita il nome del server

1. Abilitare la casella di controllo accanto all'etichetta **Sono presenti dati di backup di 1 elemento di backup associati a questo server. Si è consapevoli che facendo clic su "Conferma" verranno eliminati definitivamente tutti i dati di backup cloud. Questa azione non può essere annullata. È possibile inviare un avviso agli amministratori di questa sottoscrizione per notificare l'eliminazione** e fare clic su **Elimina**.

    >**Nota**: Questa operazione avrà esito negativo perché la funzionalità **Eliminazione temporanea** deve essere disabilitata.

1. Tornare al pannello **az104-10-rsv1 - Elementi di backup** e fare clic su **Macchine virtuali di Azure**.

1. Nel pannello **az104-10-rsv1 - Elementi di backup** fare clic su **Macchina virtuale di Azure**.

1. Nel pannello **Elementi di backup (Macchina virtuale di Azure)** selezionare **Visualizza dettagli** per **az104-10-vm0**.

1. Nel pannello dell'elemento di backup **az104-10-vm0** fare clic su **Interrompi backup**.

1. Nel pannello **Interrompi backup** selezionare **Elimina dati di backup**, specificare le impostazioni seguenti e fare clic su **Interrompi backup**:

    | Impostazioni | valore |
    | --- | --- |
    | Digitare il nome dell'elemento di backup | **az104-10-vm0** |
    | Motivo | **Altro** |
    | Commenti | **az104 10 lab** |

1. Tornare al pannello **az104-10-rsv1 - Elementi di backup** e fare clic su **Aggiorna**.

    >**Nota**: la voce **Macchina virtuale di Azure** elenca ancora **1** elemento di backup.

1. Fare clic sulla voce **Macchina virtuale di Azure** e nel pannello **Elementi di backup (Macchina virtuale di Azure)** fare clic sulla voce **az104-10-vm0**.

1. Nel pannello dell'elemento di backup **az104-10-vm0** si noti che è possibile selezionare l'opzione **Annulla eliminazione** per il backup eliminato.

    >**Nota**: questa funzionalità è fornita dalla funzionalità di eliminazione temporanea, che è abilitata per impostazione predefinita per i backup di macchine virtuali di Azure.

1. Tornare al pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** e nella sezione **Impostazioni** fare clic su **Proprietà**.

1. Nel pannello **az104-10-rsv1 - Proprietà** fare clic sul collegamento **Aggiorna** sotto l'etichetta **Impostazioni di sicurezza**.

1. Nel pannello **Impostazioni di sicurezza** disabilitare **Eliminazione temporanea (Per i carichi di lavoro in esecuzione in Azure)** e **Funzionalità di sicurezza (Per i carichi di lavoro in esecuzione nell'ambiente locale)** e fare clic su **Salva**.

    >**Nota**: questo non influisce sugli elementi già in stato di eliminazione temporanea.

1. Chiudere il pannello **Impostazioni di sicurezza** e nel pannello dell'insieme di credenziali di Servizi di ripristino **az104-10-rsv1** visualizzato di nuovo fare clic su **Panoramica**.

1. Tornare al pannello dell'elemento di backup **az104-10-vm0** e fare clic su **Annulla eliminazione**.

1. Nel pannello **Undelete az104-10-vm0** fare clic su **Annulla eliminazione**.

1. Attendere il completamento dell'operazione di annullamento dell'eliminazione, aggiornare la pagina del Web browser, se necessario, tornare al pannello dell'elemento di backup **az104-10-vm0** e fare clic su **Elimina dati di backup**.

1. Nel pannello **Elimina dati di backup** specificare le impostazioni seguenti e fare clic su **Elimina**:

    | Impostazioni | valore |
    | --- | --- |
    | Digitare il nome dell'elemento di backup | **az104-10-vm0** |
    | Motivo | **Altro** |
    | Commenti | **az104 10 lab** |

1. Ripetere i passaggi all'inizio di questa attività per eliminare gli elementi di backup per **az104-10-vm1**.

#### <a name="clean-up-resources"></a>Pulire le risorse

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. Nel portale di Azure aprire la sessione di **PowerShell** all'interno del riquadro **Cloud Shell**.

1. Elencare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*'
   ```

1. Eliminare tutti i gruppi di risorse creati nei lab di questo modulo eseguendo il comando seguente:

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*' | Remove-AzResourceGroup -Force -AsJob
   ```

   >**Nota**: facoltativamente, è possibile prendere in considerazione l'eliminazione del gruppo di risorse generato automaticamente con il prefisso **AzureBackupRG_** . L'esistenza di questo gruppo non comporta alcun addebito aggiuntivo.

    >**Nota**: il comando viene eseguito in modo asincrono, in base a quanto determinato dal parametro -AsJob, quindi, sebbene sia possibile eseguire un altro comando di PowerShell immediatamente dopo nella stessa sessione di PowerShell, i gruppi di risorse verranno rimossi dopo alcuni minuti.

#### <a name="review"></a>Verifica

In questo lab sono state eseguite le attività seguenti:

+ Provisioning dell'ambiente lab
+ Creazione di un insieme di credenziali di Servizi di ripristino
+ Implementazione di un backup a livello di macchina virtuale di Azure
+ Implementazione del backup di file e cartelle
+ Esecuzione del ripristino dei file mediante l'Agente di Servizi di ripristino di Azure
+ Esecuzione del ripristino dei file mediante gli snapshot macchina virtuale di Azure
+ Verifica della funzionalità di eliminazione temporanea di Servizi di ripristino di Azure
