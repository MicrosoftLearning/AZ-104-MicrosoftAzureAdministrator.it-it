---
demo:
  title: 'Dimostrazione 07: Amministrazione ister Archiviazione di Azure'
  module: Administer Azure Storage
---


# 07 - Amministrazione ister Archiviazione di Azure

## Configurare gli account di archiviazione

In questa dimostrazione verrà creato un account di archiviazione.

**Riferimento**: [Creare un account di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. Usare il portale di Azure.

1. Esaminare lo scopo degli account di archiviazione. 
   
1. Cercare e selezionare **Archiviazione Account**. 
 
1. Creare un account di archiviazione di base. 

    - Esaminare i requisiti relativi alla denominazione di un account di archiviazione e alla necessità che il nome sia univoco in Azure. 

    - Esaminare i diversi tipi di archiviazione. Ad esempio, utilizzo generico v2. 

    - Esaminare le selezioni del livello di accesso. Ad esempio, i livelli ad accesso sporadico e frequente. 

    - Altre schede e impostazioni verranno trattate in altre dimostrazioni. 

1. Creare l'account di archiviazione e attendere la distribuzione della risorsa. 


## Configurare l'archivio BLOB

In questa dimostrazione si esaminerà l'archiviazione BLOB.

**Nota:**  questi passaggi richiedono un account di archiviazione.

**Riferimento**: Guida introduttiva: [Caricare, scaricare ed elencare BLOB](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. Passare a un account di archiviazione nel portale di Azure.

1. Esaminare lo scopo dell'archiviazione BLOB. 

1. Creare un contenitore BLOB. Esaminare il livello di accesso per il contenitore. Ad esempio, privato (nessun accesso anonimo). 

1. Caricare un BLOB nel contenitore. Quando si ha tempo rivedere le impostazioni avanzate. Ad esempio, il tipo di BLOB e le dimensioni del BLOB. 

## Configurare la sicurezza dell'archiviazione

In questa dimostrazione verrà creata una firma di accesso condiviso.

**Nota:**  questa dimostrazione richiede un account di archiviazione, con un contenitore BLOB e un file caricato.

**Riferimento**: [Creare token di firma di accesso condiviso per i contenitori di archiviazione](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. Selezionare un BLOB o un file da proteggere. 

1. Generare una firma di accesso condiviso. Esaminare le autorizzazioni, l'ora di inizio e la scadenza e i protocolli consentiti.

1. Usare l'URL della firma di accesso condiviso per assicurarsi che la risorsa venga visualizzata. 


## Introduzione alla configurazione di File di Azure 

In questa dimostrazione verranno usati snapshot e condivisioni di file.

**Nota:**  questi passaggi richiedono un account di archiviazione.

**Riferimento**: [Guida introduttiva per la gestione delle condivisioni file di Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. Esaminare lo scopo delle condivisioni file. 

1. Accedere a un account di archiviazione e fare clic su **File**.

1. Creare una condivisione file. Esaminare le quote, caricare i file e aggiungere directory per organizzare le informazioni. 

1. Creare uno snapshot di condivisione file. Esaminare quando usare gli snapshot e come sono diversi dai backup. Quando si ha tempo, caricare un file, creare uno snapshot, eliminare il file e ripristinare lo snapshot.


## Archiviazione Tools (facoltativo)

In questa dimostrazione verranno esaminati diversi strumenti comuni di archiviazione di Azure. 

**Riferimento**: [Introduzione a Esplora Archiviazione](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. Installare Archiviazione Explorer o usare Archiviazione Browser.

1. Esaminare come esplorare e creare risorse di archiviazione. Ad esempio, aggiungere un contenitore BLOB. 

**Riferimento**: [copiare o spostare i dati in Archiviazione di Azure usando AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. Discutere quando usare AzCopy. Visualizzare la Guida, **azcopy /?**.

1. Scorrere verso il basso la **sezione Esempi** . Come si ha tempo, provare uno degli esempi. 
    



